# Dex Tweaks v2.0.0 - Managed Control Panel

Dex Tweaks 2.0.0 is a major update to the initial public release. The project is now a complete Windows 11 administration panel while keeping all executable code in a single `Dex-Tweaks.bat` file.

## What's new

- New system dashboard with Defender, network, power plan, Windows build, backup and reboot status.
- Managed profiles: Safe, Balanced, Competitive, Streaming, Laptop and Privacy.
- Change Center with action preview, compatibility checks, custom queues and result validation.
- Managed snapshots with full or selective restore.
- Registry backup, System Restore Point and BCD backup workflows.
- System Health Center with DISM, SFC, storage health, critical events and security audit.
- Before/after benchmark for processes, services, memory, CPU and free disk space.
- Global search, operation history, report export and Restart Center.
- Expanded Dex Toolbox with application installation and update management.
- Persistent preferences, logs, reports and managed state under `%ProgramData%\DexTweaks`.
- Structural self-test and isolated smoke test.

## Safety improvements

- Critical BCD and Microsoft Defender operations require Expert Mode and additional confirmation.
- Managed profiles keep browser updates, Safe Browsing, SSL and Windows SmartScreen protections active.
- Download flows no longer accept a checksum bypass as successful verification.
- NVIDIA installers downloaded by the panel must pass an Authenticode signature check.
- High-impact operations create the available recovery artifacts before execution.

## Included toolset

- Windows cleanup and performance optimizations.
- CPU, memory, GPU, network, storage, USB, audio and display tools.
- Custom power plans and gaming configurations.
- Browser configuration for Edge, Chrome, Firefox, Opera GX, Brave and Vivaldi.
- DNS and hosts configuration for ad and tracker blocking.
- Windows maintenance, privacy, debloat, services, drivers, OBS and streaming tools.

## Upgrade notes

1. Download the new `Dex-Tweaks.bat`.
2. Run it as administrator on Windows 11.
3. Accept the initial notice and create the offered backup.
4. Start with the Safe or Balanced profile.
5. Restart Windows when the panel reports a pending reboot.

Existing results may vary depending on hardware, drivers, Windows build and installed software. Review the action preview before applying changes and use Expert Mode only when you understand the impact.

## Files

- `Dex-Tweaks.bat`: complete executable panel.
- `README.md`: project overview and quick-start guide.
- `DOCUMENTACAO.md`: detailed functional documentation.

**Release history:** https://github.com/mxndex7/Dex-Tweaks/releases
