# Dex Tweaks v2.1.0 - Windows 10 and Windows 11 Control Panel

Dex Tweaks 2.1.0 expands the managed administration panel introduced in version 2.0.0 with official 64-bit Windows 10 compatibility while preserving the native Windows 11 experience. The project continues to provide diagnostics, maintenance, privacy, hardware configuration and performance tools from a single `Dex-Tweaks.bat` file.

This release focuses on operating-system compatibility, safer version-aware behavior and more accurate preflight validation. Windows 10 users now have access to the same panel and nearly the same service catalog available on Windows 11.

## What's new

- Official Windows 10 compatibility starting with build 19044.
- Recommended Windows 10 target: version 22H2, build 19045.
- Windows 10 build 19044 remains accepted for version 21H2 and LTSC 2021 installations.
- Windows 11 continues to be supported starting with build 22000.
- Centralized operating-system detection identifies Windows family, build, client type and 64-bit architecture.
- Native mode for Windows 11 and compatibility mode for Windows 10.
- Dashboard and exported reports now display the detected operating system and compatibility mode.
- Managed-profile preflight now reports Windows edition, build and known action compatibility.
- Incompatible managed actions are blocked instead of being applied blindly.
- Windows Server and 32-bit Windows installations are rejected.

## Version-aware behavior

- Windows 11 Copilot and shell-specific registry values are only applied when Windows 11 is detected.
- Windows 10 uses News and Interests settings instead of Windows 11 Widgets settings where appropriate.
- Meet Now, search, Task View and shared taskbar settings continue to be handled on both systems.
- Windows 11-only shell actions are automatically skipped on Windows 10.
- Hardware Accelerated GPU Scheduling actions are capability-gated and still require a compatible GPU and WDDM driver.
- Windows version reporting no longer depends on the deprecated WMIC command.

## Hardware and optional features

- System Guard no longer rejects Windows 10 based only on its build number.
- System Guard compatibility now checks TPM, Secure Boot, UEFI and firmware virtualization.
- Hyper-V management verifies whether the feature exists in the installed Windows edition.
- WSL management verifies feature availability before offering changes.
- The custom optional-feature manager reports unavailable features without modifying the system.
- Feature availability remains dependent on Windows edition, hardware, firmware and installed drivers.

## Windows Update safety

- Removed the hard-coded `ProductVersion=Windows 11` policy from the privacy workflow.
- Removed the hard-coded Windows 11 `24H2` target-release policy.
- Existing Windows Update product and feature-release targeting is preserved.
- Windows 10 is no longer pointed toward Windows 11 by a privacy operation.
- Windows 11 is no longer locked to an obsolete release by that workflow.
- Recommended managed profiles continue to keep Windows Update and BITS available.

## Safety and recovery

- Managed profiles continue to preview their action queue before execution.
- A managed snapshot is created before profile changes are applied.
- Applied actions are verified and recorded as successful, failed or blocked.
- High-risk service, BCD, debloat, privacy and Microsoft Defender operations are protected by Expert Mode and additional confirmation.
- Complete Defender component removal is blocked; supported optimization and passive-mode paths remain available.
- Expert workflows stop when the managed snapshot cannot be created.
- Interactive input is validated and no longer expanded through unsafe CMD percent syntax.
- Cleanup preserves Prefetch, Windows Update state, event logs, crash dumps, saved credentials, Windows.old and Recycle Bin contents.
- Physical deletion or renaming of protected System32, SystemApps, WindowsApps and DriverStore files is blocked.
- Discord and Fortnite cleanup is limited to disposable caches and logs, preserving application modules and user configuration.
- Downloaded executables must pass signature validation before installation or automatic startup.
- The unavailable NVIDIA Profile Inspector preset import is skipped instead of executing with a failed download.
- Chocolatey installation now opens the official instructions instead of executing a remote script as administrator.
- Registry, service, power, Defender and BCD state can be captured and selectively restored.
- System Restore Point, full Registry backup and BCD backup workflows remain available.
- Browser security updates, Safe Browsing, SSL and Windows SmartScreen remain enabled in protected workflows.

## Included toolset

- System dashboard, managed profiles and Change Center.
- Windows cleanup, DISM, SFC and system-health diagnostics.
- CPU, memory, GPU, network, storage, USB, audio and display tools.
- Desktop and laptop power plans.
- Hardware inventory, storage-health and security reports.
- NVIDIA and AMD driver tools.
- Windows services, optional features, Search, Explorer and Defender management.
- Privacy, permissions, telemetry, advertising and local-data controls.
- Browser configuration for Edge, Chrome, Firefox, Opera GX, Brave and Vivaldi.
- DNS and hosts configuration for ad and tracker blocking.
- Game boosters, MSI Mode, affinity, DirectX, OBS and streaming tools.
- Dex Toolbox for application discovery, installation, updates and removal.
- Managed backups, benchmarks, global search, history and Restart Center.

## Tests

- Structural self-test continues to validate duplicate labels, navigation targets and required modules.
- Structural self-test rejects unsafe input expansion, remote-script bootstrap patterns and destructive protected-file commands.
- Smoke test now simulates supported Windows 10 and Windows 11 builds.
- Smoke test validates the Windows-specific capability flags.
- Builds below the supported minimum are confirmed to be rejected.
- Runtime, dashboard queries, managed queues, snapshot creation and snapshot path containment remain covered without applying system tweaks.

Run the internal tests from Command Prompt:

```bat
Dex-Tweaks.bat --self-test
Dex-Tweaks.bat --smoke-test
```

## Upgrade notes

1. Download the new `Dex-Tweaks.bat`.
2. Run it as administrator on a supported 64-bit Windows client.
3. Windows 10 users should use build 19044 or later; build 19045 is recommended.
4. Accept the initial notice and create the offered backup.
5. Start with the `Safe` or `Balanced` managed profile.
6. Review the compatibility preflight and action preview.
7. Restart Windows when the panel reports a pending reboot.

Existing results may vary depending on Windows edition, build, hardware, firmware, drivers and installed software. Review every action before applying it and use Expert Mode only when you understand the security and recovery impact.

## Windows 10 servicing notice

Windows 10 Home and Pro have reached the end of regular Microsoft support. Continued Windows 10 use should be paired with Extended Security Updates (ESU) or an eligible LTSC edition that still receives security updates.

Dex Tweaks compatibility does not replace operating-system security updates or Microsoft support.

## Files

- `Dex-Tweaks.bat`: complete Windows 10 and Windows 11 control panel.
- `README.md`: project overview, requirements and quick-start guide.
- `DOCUMENTACAO.md`: detailed functional and compatibility documentation.
- `RELEASE_NOTES_v2.1.0.md`: changes and upgrade information for this release.

**Release history:** https://github.com/mxndex7/Dex-Tweaks/releases
