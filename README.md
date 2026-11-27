# Bare-Bone Realtek ALC257 + Intel SST Audio Driver

A stripped-down, bloat-free audio driver package for **Lenovo LOQ (83GS)** laptops using the **Realtek ALC257** codec and **Intel Smart Sound Technology (SST)** controller.

## Why This Exists

* **Eliminates the Shutdown Pop:** Microsoft's generic `HdAudio.sys` driver lacks Realtek's proprietary shutdown (S5) depop/clamp sequences, causing a sharp metallic pop in sensitive IEMs when the motherboard cuts power. This package uses Realtek's native kernel driver (`RTKVHD64.sys`) to clamp the output cleanly on shutdown.
* **100% Raw Audio & Mic:** Strips out all OEM DSP processing, audio enhancements, and background services so you get a flat, uncolored signal (ideal for Equalizer APO) and raw microphone array input.
* **BIOS-Compatible Bus:** Because the Lenovo LOQ BIOS locks the Intel audio controller into SST mode (`CC_0401`), Microsoft's generic `HDAudBus.sys` fails with Code 10. This package includes only the bare minimum Intel SST Bus and OED drivers required to initialize the codec.

---

## Tested Hardware

| Component | Details / Hardware ID |
| :--- | :--- |
| **Laptop Model** | Lenovo LOQ (`83GS`) |
| **Audio Controller** | `PCI\VEN_8086&DEV_7AD0&SUBSYS_380817AA&REV_11` |
| **Audio Codec** | Realtek ALC257 (`INTELAUDIO\FUNC_01&VEN_10EC&DEV_0257&SUBSYS_17AA3CB2`) |
| **OS** | Windows 11 Pro (x64) |

---

## Package Contents

### Included (Bare-Bone Stack)
* `Intel_SST\IntcAudioBus.inf` — Intel SST Bus driver (`10.29.0.12181`)
* `Intel_SST\IntcOED.inf` — Intel SST OED driver & required firmware/topology dependencies
* `Realtek_Codec\HDXLVSST.inf` — Bare Realtek ALC257 kernel driver (`RTKVHD64.sys`, `Codec_9879.1`)
* `install.bat` — Portable, self-elevating `pnputil` installer (no hardcoded paths)

### Stripped Out (Zero Bloat)
* ❌ Nahimic / A-Volute APO & Services
* ❌ AISpeech APO & Voice Services
* ❌ Realtek APO (`RltkAPOU64.dll`) & Audio Console (HSA)
* ❌ Realtek Background Services (`RtkAudUService64.exe`)
* ❌ OEM Extension INFs (`ExtRtk`) & Unused SST Sub-Drivers

---

## Installation

1. Download the latest `.zip` archive from the **[Releases](../../releases)** page.
2. Extract the `Realtek ALC257 Audio Driver` folder anywhere on your PC.
3. Double-click **`install.bat`** and accept the Administrator (UAC) prompt.
4. Wait for all three `.inf` packages to report `Driver package added successfully`.
5. **Reboot your PC** to load the new driver stack.

---

## Post-Install Recommendations

* **Verify Clean Install:** Open **Device Manager** and confirm **Realtek(R) Audio** appears under *Sound, video and game controllers*, and no Nahimic, AISpeech, or Realtek APO entries appear under *Software components*.
* **Equalizer APO:** Open Equalizer APO Configurator and attach it to your **Realtek(R) Audio** playback device (use **Install as SFX/EFX** under Troubleshooting options if needed).
* **Block Windows Update Bloat (Windows 11 Pro):** To stop Windows Update from silently reinstalling Nahimic or OEM extensions:
  1. Press `Win + R`, type `gpedit.msc`, and press Enter.
  2. Navigate to `Computer Configuration → Administrative Templates → Windows Components → Windows Update → Manage updates offered from Windows Update`.
  3. Set **Do not include drivers with Windows Updates** to **Enabled**.

---

## License & Disclaimer

The `install.bat` script and documentation in this repository are licensed under the **MIT License**. 

All driver binaries (`RTKVHD64.sys`, `IntcAudioBus.sys`, `IntcOED.sys`) and associated firmware files belong to their respective copyright holders (**Realtek Semiconductor Corp.**, **Intel Corporation**, and **Lenovo**) and are not covered by this license. This repository is maintained strictly for personal backup and troubleshooting purposes—use at your own risk.
