# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Dex Tweaks is a single-file Windows Batch application (`Dex-Tweaks.bat`, ~17,200 lines) that acts as an
administrative panel for Windows 10/11: diagnostics, maintenance, privacy, hardware configuration, and
performance tweaks. There is no build step, no package manager, and no separate source tree — the entire
executable logic lives in that one `.bat` file. `DOCUMENTACAO.md` (Portuguese) is the authoritative functional
reference for every menu and routine; `README.md` is the shorter public-facing overview (also Portuguese).

## Commands

Run from a Command Prompt (not PowerShell) in the repo root:

```bat
Dex-Tweaks.bat --self-test
Dex-Tweaks.bat --smoke-test
```

- `--self-test`: static validation only, no admin rights needed. Uses PowerShell to parse the batch file
  itself and checks for duplicate `:labels`, `goto`/`call` targets that don't resolve, presence of the
  required top-level modules (`menu`, `dashboardcenter`, `profilescenter`, `changecenter`, `backupmanager`,
  `healthcenter`, `benchmarkcenter`, `globalsearch`, `historycenter`, `restartcenter`, `destruct`), and a
  blocklist of unsafe patterns (`iex`/`DownloadString`, destructive `bcdedit` flags, deletions under
  `System32`/`WindowsApps`/`DriverStore`, and unsanitized interactive input tokens reused in dangerous
  contexts). Exit code 1 on any violation.
- `--smoke-test`: exercises the managed runtime end-to-end (OS detection for simulated Windows 10/11 builds,
  dashboard cache refresh, profile queue build/validation, snapshot creation, path-traversal checks) against
  temp data in `%TEMP%\DexTweaksSmoke_*`, without applying any real tweaks. Cleans up after itself.

Both entry points are handled at the very top of the script (`if /I "%~1"=="--self-test" goto ...`) before the
admin check, so they run without Administrator privileges. Normal (no-arg) execution requires Administrator —
the script checks via `net session` and redirects to `:NotAdmin` otherwise.

There is no linter, formatter, or automated CI config in this repo — `--self-test` is the closest thing to a
lint pass and should be run after any structural edit (new labels, renamed labels, new `goto`/`call` targets).

## Architecture

### Single-file, label-based control flow

The script is one large state machine using Batch `:label` sections and `goto`/`call`. There are two kinds of
navigation:
- `goto :Label` — jumps and does not return (used for menu transitions).
- `call :Label` — subroutine-style, returns via `exit /b [code]`, used for reusable logic (validation, logging,
  applying/verifying a single change, etc).

Menu areas map to single-letter top-level labels reachable from `:menu` (`:TweaksMenu` for `3` Optimizations,
`:HardwareMenu` for `4`, `:WindowsMenu` for `5`, `:PrivacyMenu` for `6`, `:AdvancedMenu` for `7`, etc — see the
`choice /C 123456789ABCDEX` block at `:menu`). Sub-features inside each area are large label blocks named after
what they do (e.g. `:NVIDIAGPU`, `:WiFiOptimization`, `:HardwareSecurityCenter`). When adding a new feature,
follow this pattern: add a menu entry, a label block, and — if it's destructive/system-level — route it through
the managed layer described below rather than writing directly.

### Two layers of functionality

1. **Legacy/traditional tweaks** (Optimizations, Hardware, Windows, Privacy, Advanced menus): direct, often
   aggressive `reg add` / `sc config` / `bcdedit` / PowerShell calls grouped by topic (GPU vendor, CPU vendor,
   browser, service category, etc). These predate the managed layer and are generally not restorable via the
   managed snapshot — only via Full Registry Backup or Windows System Restore.
2. **Managed layer** (Profiles, Change Center, Backup/Restore): a newer, safer subsystem built around a fixed
   vocabulary of change codes validated in `:ValidateChangeCode` (`DEFENDER_ON`, `UPDATES_ON`, `SEARCH_ON`,
   `SEARCH_OFF`, `GM_ON`, `HAGS_ON`, `HAGS_OFF`, `PWR_BAL`, `PWR_HIGH`, `VFX_AUTO`, `VFX_PERF`,
   `TELEMETRY_SAFE`, `ADS_OFF`, `ACTIVITY_OFF`, `DELIVERY_OFF`, `BGAPPS_OFF`). Every managed change follows the
   same pipeline: preview → compatibility check (`:CheckChangeCompatibility`) → snapshot
   (`:CreateManagedSnapshot`) → apply one action at a time (`:ApplyChange`) → verify resulting state
   (`:VerifyManagedChange`) → log (`:LogEvent`) → flag reboot if needed (`:MarkRebootRequired`). Adding a new
   managed code means updating `:ValidateChangeCode`, `:ApplyChange`, and `:VerifyManagedChange` together —
   these three must stay in sync or the smoke test and self-test can pass while the feature is broken.
   `Profiles` (`Safe`, `Balanced`, `Competitive`, `Streaming`, `Laptop`, `Privacy`) are just named, pre-built
   queues of these codes (`:BuildProfile`).

**Expert Mode** (`:RequireExpertMode`) gates the highest-risk operations (BCD edits, Defender
disabling/removal). It requires double confirmation, attempts a managed snapshot AND a Windows Restore Point
before proceeding, and refuses to continue if the snapshot can't be created.

### OS detection and compatibility

`:DetectOperatingSystem` (called once at startup, and again per-build in `--smoke-test` via `DEX_TEST_BUILD`)
determines `DEX_OS_FAMILY` (Windows 10 vs 11), rejects non-client/non-64-bit/sub-19044-build systems, and sets
capability flags (`DEX_CAP_HAGS`, `DEX_CAP_WIN11_UI`, `DEX_CAP_SYSTEM_GUARD`) that gate individual features.
Windows 10 (build ≥ 19044) runs in a compatibility mode that reuses the same menu tree; Windows-11-only UI
routines are skipped or substituted rather than duplicated. When adding a feature that behaves differently per
OS, branch on these flags/variables rather than re-checking the build number inline.

### Runtime state and data directory

`:InitializeDexRuntime` sets up `%ProgramData%\DexTweaks` (or a temp root under `--smoke-test`) with
`Backups\`, `Logs\`, `Reports\`, `State\` subfolders, ACLs the folder to Administrators/SYSTEM only, opens a
per-session log (`Logs\Session_<timestamp>.log`), and builds the global search catalog. Almost everything reads
from `DEX_*` environment variables set here (`DEX_LOG`, `DEX_QUEUE`, `DEX_BACKUPS`, `DEX_SNAPSHOT_FILE`, etc) —
grep for `DEX_` prefix to find state wiring. All user-facing events should go through `:LogEvent "<LEVEL>"
"<message>"` (levels used: INFO, OK, FAIL, WARN, BLOCKED, CANCEL, COMPAT, REBOOT, BACKUP, EXPERT).

### Safety-sensitive conventions to preserve

- **Path validation**: any snapshot/restore path must be checked with `:ValidateManagedSnapshotPath`, which
  uses `[IO.Path]::GetFullPath` plus a prefix + filename-pattern check to prevent path traversal outside
  `DEX_BACKUPS`. Never build a restore path by naive string concatenation.
- **Package names**: Toolbox package identifiers are validated with `:ValidatePackageName`
  (`^[A-Za-z0-9][A-Za-z0-9._+-]*$`) before being passed to `choco`.
- **No checksum bypass**: Chocolatey installs never pass `--ignore-checksums`; failures go through
  `:ConfirmChecksumBypass`, which always refuses and logs a `BLOCKED` event. Keep it that way.
- **Driver installs**: the NVIDIA "Tweaked Driver" flow requires a valid Authenticode signature on `setup.exe`
  from a controlled download/extraction path before running it — don't relax this check.
- **Destructive paths**: `--self-test`'s pattern blocklist actively greps the file for dangerous constructs
  (`iex`, `DownloadString`, unsafe `bcdedit` flags, deletions under system directories, unsanitized reuse of
  interactive-input variables). New code that trips these patterns will fail `--self-test` by design — treat
  that as a signal to reconsider the approach, not to special-case the check.
- Registry/service/BCD changes almost always go through `reg add ... /f`, `sc config`, or `bcdedit`, wrapped
  with `>nul 2>&1` and followed by `if errorlevel 1 ...`. Match this style for consistency and to keep output
  clean in the console UI.

### Console UI conventions

- `:SetupConsole` and the color variables block (`:variables`, ANSI escape codes stored in vars like `%c%`,
  `%red%`, `%u%` for reset) are set up once near the top; reuse these variables instead of hardcoding escape
  sequences.
- `choice /C <letters> /N /M "prompt"` plus `if errorlevel N goto ...` (checked from highest to lowest) is the
  standard menu-selection pattern throughout — follow it for new menus rather than `set /p` where a fixed
  option set is being chosen.
