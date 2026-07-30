# Dex Tweaks v2.1.0 - Windows 10 Compatibility

Dex Tweaks 2.1.0 expands the managed control panel to 64-bit Windows 10 clients while preserving native Windows 11 behavior.

## Compatibility

- Added Windows 10 support starting with build 19044.
- Recommended Windows 10 target: version 22H2, build 19045.
- Included Windows 10 21H2 and LTSC 2021, build 19044.
- Windows 11 remains supported starting with build 22000.
- Windows Server and 32-bit Windows installations are rejected.
- Dashboard reports show the detected operating system and compatibility mode.

## Version-aware behavior

- Windows 11 Copilot and shell registry values are only applied on Windows 11.
- Windows 10 News and Interests replaces the Windows 11 Widgets path where appropriate.
- System Guard no longer rejects Windows 10 by version alone. Compatibility now requires TPM, Secure Boot, UEFI and firmware virtualization.
- Optional-feature management reports features unavailable in the current edition.
- HAGS actions are capability-gated and still require a compatible GPU and WDDM driver.

## Windows Update safety

The comprehensive privacy workflow no longer writes a hard-coded Windows product or feature release. It preserves existing Windows Update product and target-release settings, preventing Windows 10 from being pointed at Windows 11 and avoiding an obsolete Windows 11 release lock.

## Tests

The smoke test now simulates supported Windows 10 and Windows 11 builds, verifies their capability flags and confirms that builds below the minimum are rejected.

## Windows 10 servicing notice

Windows 10 Home and Pro have reached the end of regular Microsoft support. Use Extended Security Updates (ESU) or an eligible LTSC edition when continued Windows 10 operation is required.
