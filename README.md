# Dependability Project — Group 5

This repository contains the LaTeX source of the **Dependability Project** report, written for the Dependability course at the University of Pisa.

The report analyses a set of malicious Android APKs from the **Malbus** family — repackaged transportation applications masquerading as legitimate bus-information services for South Korean cities. Each sample is examined through a multi-tool pipeline covering static analysis, automated scanning, and dynamic emulation.

---

## Analysed Samples

| Chapter | Sample | SHA-256 (prefix) |
|---------|--------|------------------|
| 1 | `Malbus_v1.0.3.apk` | `3252fbce...` |
| 2 | `Malbus_v3.3.7.apk` | `19162b06...` |
| 3 | `Malbus_v3.6.5.apk` | `bed3e665...` |

---

## Analysis Methodology

Each APK is analysed with the following tools:

- **VirusTotal** — multi-engine antimalware scan, permission inspection, contacted domains and behaviour tab
- **Static Analysis** — manual review of decompiled Java classes (`MainActivity`, `BaseMainActivity`, `EnvConstant`, `BusApiWeb`, `AndroidManifest.xml`)
- **MobSF** — automated mobile security framework scan (certificate analysis, high/medium/low severity findings)
- **Genymotion** — dynamic analysis in Android emulator with runtime observation

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
│   ├── apk_1/             # Chapter 1 — Malbus v1.0.3
│   │   ├── analysis.tex
│   │   └── filelist.tex
│   ├── apk_2/             # Chapter 2 — Malbus v3.3.7
│   │   ├── second.tex
│   │   └── filelist.tex
│   ├── apk_3/             # Chapter 3 — Malbus v3.6.5
│   ├── comparison.tex     # Cross-sample comparison
│   └── final_part.tex     # Conclusions
└── img/
    ├── apk_1/             # Figures for chapter 1
    │   └── screenshots/   # Genymotion screenshots
    ├── apk_2/             # Figures for chapter 2
    │   └── screenshots/   # Genymotion screenshots
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
