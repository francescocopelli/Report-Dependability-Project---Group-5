# Dependability Project — Group 5

This repository contains the LaTeX source of the **Dependability Project** report, written for the *Dependability and Secure Systems* course at the University of Pisa (A.Y. 2024–2025), supervised by Prof. Maurizio Palmieri.

The report analyses three Android APK samples from the **Malbus** family — repackaged public-transport applications masquerading as legitimate bus-information services for South Korean cities (**Changwon**, **Gwangju**, **Jeonju**). The samples, internally labelled *cwbus*, *gjbus*, and *jjbus*, are examined through a multi-tool pipeline covering anti-malware scanning, static analysis, and dynamic emulation, followed by a cross-version comparative analysis.

---

## Analysed Samples

| Chapter | Internal label | APK version | SHA-256 (prefix) |
|---------|---------------|-------------|------------------|
| 1 | cwbus | `Malbus_v1.0.3.apk` | `3252fbce...` |
| 2 | gjbus | `Malbus_v3.3.7.apk` | `19162b06...` |
| 3 | jjbus | `Malbus_v3.6.5.apk` | `bed3e665...` |

All three samples share the package name `a.privacy.MalBuslmh` and are signed by the same certificate (`CN=Myoung-Hun Lee`, SHA-1/RSA v1 scheme), making them vulnerable to the **Janus attack** on Android 5.0–8.0.

---

## Analysis Methodology

Each APK is analysed through the following pipeline:

1. **VirusTotal** — multi-engine antimalware scan (75+ AV engines), permission inspection, contacted domains and behaviour tab
2. **Static Analysis** — manual review of decompiled Java classes (`MainActivity`, `BaseMainActivity`, `EnvConstant`, `BusApiWeb`, `AppInfoActivity`, `AndroidManifest.xml`) via JD-GUI / Java Decompiler
3. **MobSF** — automated mobile security framework scan: certificate analysis, high/medium/low severity findings, risk scoring, privacy tracker detection
4. **Genymotion** — dynamic analysis in a virtual Android device with runtime observation

---

## Key Findings

### Anti-Malware (VirusTotal)

- All three samples flagged as `trojan.andr/apkdownloader`
- Detection rates: **cwbus** 40/75 · **gjbus** 40/75 · **jjbus** 38/75
- Notable misses: Microsoft Defender, McAfee, Malwarebytes failed to detect any sample

### Static Analysis

- Identical dropper architecture across all three variants: `System.loadLibrary("Audio3.0")` + native methods `startUpdate()` / `updateApplication()`
- Multi-stage infection chain: dropper → Malware Distribution Server → payload → C2 server + Upload server
- Hardcoded advertising/analytics keys (`AdMob`, `AdLib`, `Facebook`, `Google Analytics UA-56668425-6`) in `EnvConstant`
- Unencrypted HTTP in `BusApiWeb`, exposing data to MITM interception
- Janus vulnerability (SHA-1 v1 signing), debug config enabled, Task Hijacking / StrandHogg across all variants

### Comparative Analysis (cwbus → gjbus → jjbus)

| Capability | cwbus | gjbus | jjbus |
|---|:---:|:---:|:---:|
| Obfuscation + Java Reflection | ✓ | ✓ | ✓ |
| VM / emulator detection | — | ✓ | ✓ |
| CPU name check (sandbox detection) | — | ✓ | ✓ |
| Process name manipulation | — | ✓ | ✓ |
| `CALL_PHONE` / `RECORD_AUDIO` permissions | — | — | ✓ |
| `libAudio3.0.so` without stack canary | — | — | ✓ |
| StrandHogg 2.0 | — | — | ✓ |
| Crashlytics SDK integration | — | — | ✓ |

- All three samples match the `IcedID` structural YARA rule (CAPEv2 repository, author: `kevoreilly/threathive`), confirming an advanced dropper/loader architecture.
- **jjbus** MobSF risk score: **38/100 — Grade C, HIGH RISK**

---

## Repository Structure

```
.
├── main.tex               # Entry point, loads all chapters
├── settings.tex           # Shared macros and APK name commands
├── filelist.tex           # Chapter include list
├── page/
│   ├── abstract.tex
│   ├── intro.tex
│   ├── apk_1/             # Chapter 1 — cwbus (Malbus v1.0.3)
│   │   ├── analysis.tex
│   │   └── filelist.tex
│   ├── apk_2/             # Chapter 2 — gjbus (Malbus v3.3.7)
│   │   ├── second.tex
│   │   └── filelist.tex
│   ├── apk_3/             # Chapter 3 — jjbus (Malbus v3.6.5)
│   ├── comparison.tex     # Cross-sample comparative analysis
│   └── final_part.tex     # Conclusions
└── img/
    ├── apk_1/             # Figures for chapter 1 (cwbus)
    │   └── screenshots/   # Genymotion dynamic analysis screenshots
    ├── apk_2/             # Figures for chapter 2 (gjbus)
    │   └── screenshots/   # Genymotion dynamic analysis screenshots
    └── common/            # Shared assets (logo, frontpage)
```

---

## How to Compile

Requires a full LaTeX distribution (e.g. TeX Live or MiKTeX).

```bash
pdflatex main.tex
pdflatex main.tex   # run twice for correct cross-references
```

Or use **Overleaf** by uploading the repository as a ZIP archive.

---

## Authors

Francesco Copelli — University of Pisa
