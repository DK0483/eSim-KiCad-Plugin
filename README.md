# eSim KiCad Plugin Suite
### Smart PCB Manufacturability, Thermal & EMI Risk Analyzer & Hardware Security Linter

[![KiCad Version](https://img.shields.io/badge/KiCad-9.0%20%7C%208.0-blue?style=for-the-badge&logo=kicad)](https://kicad.org)
[![Python Version](https://img.shields.io/badge/Python-3.9%20%7C%203.10%20%7C%203.11%20%7C%203.12-yellow?style=for-the-badge&logo=python)](https://python.org)
[![FOSSEE eSim](https://img.shields.io/badge/FOSSEE-eSim-orange?style=for-the-badge)](https://esim.fossee.in/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)

An intelligent, multi-module plugin suite developed for **KiCad 9** and the **FOSSEE eSim** ecosystem. The suite bridges Computer Science algorithms with physical Electronics principles to provide real-time Design-for-Manufacturability (DFM), electro-thermal, EMI risk analysis, and physical IoT hardware security auditing.

---

## 🌟 Modules Overview

| Module | Core Purpose | Key Techniques | Primary Location |
| :--- | :--- | :--- | :--- |
| **Smart PCB Manufacturability Analyzer** | Evaluates trace density, Joule heating hotspots, EMI cross-talk, and DFM compliance. | 2D Spatial Binning, Power Law Thermal Modeling, 8-Neighbor Spatial Convolution, wxPython + Matplotlib embedded canvas. | [`pcb_manufacturability_risk_analyzer/`](pcb_manufacturability_risk_analyzer/) |
| **eSim Hardware Security Linter** | Audits edge clearances of sensitive debug buses (UART, JTAG, SWD, SPI) to prevent physical tampering & EMFI glitching. | Computational Geometry (Point-to-Segment Projections), Regex Lexical Filtering, 1-Click CI/CD GitHub Actions Engine. | [`esim_hw_linter/`](esim_hw_linter/) |

---

## 🚀 Key Features

### 1. Smart PCB Manufacturability & Thermal/EMI Risk Analyzer
- **Key Manufacturing Metrics:** Calculates average track width, minimum track width, standard deviation, and via/capacitor statistics.
- **Embedded Multi-Heatmap:** 4 synchronized heatmaps rendered directly inside KiCad:
  - *Density Heatmap* (`viridis` colormap)
  - *Thermal Heatmap* (`inferno` colormap)
  - *EMI Risk Heatmap* (`coolwarm` colormap)
  - *Temperature Map* (`hot` colormap with ambient scaling)
- **Automatic Hotspot Framing:** Identifies and draws bounding boxes around critical congestion zones ($\ge 60\%$ peak density).
- **Statistical Analysis:** Interactive Track Width Histogram and Density vs. Thermal Correlation Scatter Plot with calculated Pearson correlation coefficient ($r$).
- **DFM Score & Fixes:** Generates an actionable 0–100 design rating with educational guidance for students and researchers.

### 2. eSim Hardware Security Linter
- **Perimeter Attack Surface Scan:** Detects exposed debug interfaces placed dangerously close to `Edge.Cuts` boundaries.
- **Trace-Level EMFI Glitch Detection:** Scans copper tracks on sensitive nets (`TX`, `RX`, `SWDIO`, `SWCLK`, etc.) routed along the perimeter.
- **Visual Board Annotation:** Draws $2\text{ mm} \times 2\text{ mm}$ red **'X'** cross markers on the `User.Drawings` layer for immediate visual feedback.
- **1-Click CI/CD Deployment:** Automatically configures a GitHub Actions `.github/workflows/security.yml` pre-push hardware firewall.
- **Dual Mode (GUI & Headless):** Runs interactively inside `pcbnew` or headlessly in cloud containers (`xvfb-run`) with exit codes to block compromised pull requests.

---

## 📋 Deliverables & Documentation

This repository contains all required submission deliverables for **Task 6: KiCad Plugin Development (CSE and Related Fields)** for the **eSim Semester Long Internship - Autumn 2026**:

1. 📖 **[Architectural Design Document](DESIGN_DOCUMENT.md):** In-depth technical architecture, module interactions, theoretical models, and KiCad 9 API specifications.
2. 💻 **Code Implementation:** Clean, verified, modular Python plugins located in [`pcb_manufacturability_risk_analyzer/`](pcb_manufacturability_risk_analyzer/) and [`esim_hw_linter/`](esim_hw_linter/).
3. ⚙️ **[Execution & Testing Instructions](INSTRUCTIONS.md):** Complete cross-platform installation and validation steps for Windows, Linux, and macOS.
4. 🎥 **[Presentation & Video Guide](PRESENTATION_GUIDE.md):** 5–10 minute script and slide cues explaining concept, design decisions, and live demo walkthrough.

---

## 🛠️ Quick Installation Guide

1. Copy the plugin folders into your KiCad 9 scripting directory:
   - **Windows:** `%USERPROFILE%\Documents\KiCad\9.0\scripting\plugins\`
   - **Linux:** `~/.local/share/kicad/9.0/scripting/plugins/`
   - **macOS:** `~/Documents/KiCad/9.0/scripting/plugins/`

2. Open KiCad 9 and launch the PCB Editor (`pcbnew`).

3. Access the plugins from the top toolbar or via **Tools** → **External Plugins**.

*For detailed platform instructions and troubleshooting, see [INSTRUCTIONS.md](INSTRUCTIONS.md).*

---

## 📊 Directory Structure

```text
eSim-KiCad-Plugin/
├── DESIGN_DOCUMENT.md                     # Comprehensive Architectural Design Document
├── INSTRUCTIONS.md                        # Cross-platform installation and testing guide
├── PRESENTATION_GUIDE.md                  # 5-10 minute presentation & demo walkthrough script
├── README.md                              # Repository overview & quick start
├── LICENSE                                # MIT License
│
├── pcb_manufacturability_risk_analyzer/   # Module 1: DFM, Thermal & EMI Analyzer
│   ├── __init__.py                        # KiCad ActionPlugin registration
│   ├── plugin.py                          # Main wxPython GUI & embedded Matplotlib canvas
│   ├── heatmap.py                         # 2D Spatial binning, thermal & EMI algorithms
│   ├── analysis_utils.py                  # Geometric data extraction & statistical metrics
│   ├── README.md                          # Detailed module documentation
│   └── Images/                            # Screenshots of plugin inside KiCad
│
└── esim_hw_linter/                        # Module 2: Hardware Security & CI/CD Linter
    ├── __init__.py                        # KiCad ActionPlugin registration
    ├── linter.py                          # Geometry distance engine, CI/CD generator & CLI
    ├── icon.png                           # Action toolbar icon
    └── README.md                          # Security model documentation
```

---

## 👥 Author & Acknowledgements

- **Developer:** Dhyey Kabra
- **Project:** Task 6: KiCad Plugin Development (CSE and Related Fields)
- **Organization:** FOSSEE, IIT Bombay (eSim Semester Long Internship - Autumn 2026)
- **License:** Distributed under the MIT License. See `LICENSE` for details.
