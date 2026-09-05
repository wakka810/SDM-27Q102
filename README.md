# Sony INZONE M10S II (SDM-27Q102) ddcutil UDF

User-defined VCP feature definitions for the Sony INZONE M10S II / SDM-27Q102 (`SNY`, `SDM27Q102*30`, product code `2104`).

## Validation basis

- Firmware: **M004**, released 2026-08-18. `SDM-27Q102.bin` is 2,809,244 bytes; SHA-256 `bcef74b7a9b9b6415619411c62afd843714c32edcefeb4d59a557452e9c4a729`.
- INZONE Hub: **1.0.19.0**, released 2026-04-15. Extracted `INZONEHub.dll` SHA-256 `77082a578f25b2ef6722e256fd7f53de0d69678a8c852a7aaf4e8f569c542a5f`.
- Managed-code decompilation shows `PCWidget.Communication.MonitorDeviceConfig.SDM_27Q102Info` contains exactly **54** model-specific VCP entries. The 54 VCP codes in this UDF match that set 54/54, with no duplicates or omissions.
- M004 contains the monitor capability string three times (offsets `0x10E51`, `0x20E51`, `0x30E51`); it declares `mccs_ver(2.1)`. The same capability string is byte-identical in M003 and M004, so no firmware-declared VCP capability change was observed.
- M004 identifies itself as `M004` at offset `0x4119A` and carries a main-image build stamp of `Aug  3 2026` / `17:55:08`.
- The firmware's native capability string does not enumerate Sony's private VCP codes. Those private mappings (`xE0`, `xE5`, `xFE`, etc.) therefore remain sourced from INZONE Hub 1.0.19.0, while `MCCS_VERSION 2.1` is sourced from M004 itself.

Sony release information: [M004 firmware](https://support.sony.jp/electronics/support/monitors-inzone-m10-series/sdm-27q102/software/00408700) and [INZONE Hub 1.0.19.0](https://support.sony.jp/electronics/support/software/00384248).

## Install

```sh
sudo install -Dm644 SNY-SDM27Q102_30-2104.mccs /usr/local/share/ddcutil/SNY-SDM27Q102_30-2104.mccs
```

`ddcutil` uses the UDF's `MCCS_VERSION 2.1` instead of relying on the monitor's xDF VCP-version query.

## Safety

The UDF intentionally marks reset, OLED maintenance, and USB firmware-update commands as write-only. Do not use `setvcp` on those entries merely to probe support, especially Panel Refresh (`xF8`) and USB Software Update (`xF4`).

Vendor installer and firmware binaries are analysis inputs only and are not stored in this repository.
