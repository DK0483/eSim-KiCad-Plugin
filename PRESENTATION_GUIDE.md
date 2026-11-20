# Presentation & Video Demonstration Guide (5–10 Minutes)
## eSim KiCad Plugin Suite: Smart Manufacturability Analyzer & Security Linter

**Task 6: KiCad Plugin Development (CSE and Related Fields)**  
**eSim Semester Long Internship - Autumn 2026**  
**Presenter:** Dhyey Kabra  

---

## Overview & Timing Breakdown

| Section | Topic | Duration | Target Goal |
| :---: | :--- | :---: | :--- |
| **Part 1** | Introduction & Problem Statement | 1:00 min | Explain the gap between schematic & layout in eSim/KiCad. |
| **Part 2** | CS & Electronics Synergy | 1:30 min | Detail the 2D spatial binning, Joule heating, and EMI models. |
| **Part 3** | Architecture & Module Breakdown | 1:30 min | Present the modular architecture & KiCad 9 API integration. |
| **Part 4** | Live Demo: Manufacturability & Thermal Analyzer | 2:00 min | Walk through heatmaps, histograms, DFM scoring & fixes. |
| **Part 5** | Live Demo: Hardware Security Linter & CI/CD | 1:30 min | Show boundary distance auditing, 'X' markers & GitHub push. |
| **Part 6** | Conclusion & Internship Roadmap | 0:30 min | Summarize deliverables and future FEM/impedance extensions. |
| **Total** | | **8:00 min** | Well within the recommended 5–10 minute window. |

---

## Detailed Script & Demonstration Walkthrough

### Part 1: Introduction & Problem Statement (0:00 – 1:00)
**Visual Cue:** Title Slide showing *eSim KiCad Plugin Suite*, FOSSEE logo, and presenter name.

> *"Hello everyone! My name is Dhyey Kabra, and today I am excited to present my project for Task 6 of the eSim Semester Long Internship: the **eSim KiCad Plugin Suite**.*
>
> *In the FOSSEE eSim ecosystem, engineers design circuit schematics and transition into KiCad for physical PCB layout. However, traditional KiCad Design Rule Checks only verify basic geometric clearances—such as whether two copper traces overlap. They completely miss critical electro-thermal dissipation issues, electromagnetic interference hotspots, manufacturing yield bottlenecks, and physical hardware security vulnerabilities.*
>
> *To solve this, I designed and developed an intelligent, dual-module KiCad 9 plugin suite that bridges Computer Science algorithms with physical Electronics principles."*

---

### Part 2: Synergy Between Computer Science & Electronics (1:00 – 2:30)
**Visual Cue:** Slide with theoretical concept comparison table & formula highlights.

> *"This project strongly couples Computer Science and Electronics concepts:*
>
> 1. *First, **2D Spatial Discretization**: In Computer Science, we use spatial partitioning and 2D histogram binning (`numpy.histogram2d`) to map thousands of continuous vector traces into a discrete Cartesian grid matrix.*
> 2. *Second, **Electro-Thermal Modeling**: From Electronics, we apply Joule's Law ($P = I^2 R$). Dense trace clusters experience higher aggregate current, and thinner traces present higher electrical resistance ($R \propto 1 / \text{Width}$). Combining these in matrix operations allows us to simulate localized temperature rise without needing a multi-hour simulation.*
> 3. *Third, **Electromagnetic Interference (EMI)**: We model parasitic capacitive and inductive crosstalk through an 8-neighborhood spatial kernel convolution, highlighting high-speed routing corridors at risk of signal distortion.*
> 4. *Finally, **Computational Geometry**: In our security module, we implement point-to-segment Euclidean distance projections with boundary clamping to audit physical clearances along the board outline."*

---

### Part 3: Architecture & KiCad 9 Modernization (2:30 – 4:00)
**Visual Cue:** System Architecture Diagram (from `DESIGN_DOCUMENT.md`).

> *"Looking at the system architecture: the plugin is structured cleanly into modular layers:*
> - *The **Geometry Extraction Layer** interfaces natively with KiCad 9's Python `pcbnew` API.*
> - *The **Numerical Computation Layer** processes spatial matrices and statistical metrics using NumPy.*
> - *The **Visualization Layer** embeds live Matplotlib charts directly inside custom `wxPython` scrolled panels.*
>
> *The entire codebase is built and tested for **KiCad 9.0**, adopting the latest API standards: integer nanometer coordinate spaces using `VECTOR2I`, modern `PCB_SHAPE` drawing primitives, dynamic layer resolution, and safe net extraction."*

---

### Part 4: Live Demonstration – Manufacturability Analyzer (4:00 – 6:00)
**Visual Cue:** Screen recording of KiCad 9 opening a board, clicking **Smart PCB Manufacturability Analyzer**, and navigating the dashboard.

> *"Let's see the plugin in action! Here I have a sample routed board inside KiCad 9.*
>
> *I open the **Tools** menu and select **Smart PCB Manufacturability Analyzer**.*
>
> *Instantly, the dashboard renders comprehensive diagnostic insights:*
> 1. *At the top, we see the **Key Manufacturing Metrics**: Average Track Width, Minimum Track Width, and Standard Deviation. This tells us immediately if our routing is uniform.*
> 2. *Next, we see **Four Synchronized Heatmaps**:*
>    - *The **Density Heatmap** highlights copper concentration.*
>    - *The **Thermal Heatmap** predicts $I^2R$ power dissipation.*
>    - *The **EMI Risk Map** flags areas where dense parallel traces risk cross-talk.*
>    - *And the **Temperature Map** provides an ambient-scaled temperature estimate.*
>    - *Notice the red bounding boxes dynamically framing detected hotspots.*
> 3. *Further down, we have the **Track Width Histogram** and **Density vs. Thermal Correlation Scatter Plot**, displaying a calculated Pearson correlation coefficient of $0.89$, visually proving how trace concentration drives localized heat.*
> 4. *Finally, the analyzer generates a **Global Manufacturability Score** with actionable educational recommendations for the designer."*

---

### Part 5: Live Demonstration – Hardware Security Linter & CI/CD (6:00 – 7:30)
**Visual Cue:** Switch to running **eSim Hardware Security Linter** in KiCad, showing the settings dialog, the red 'X' markers appearing on the PCB layout, and the CI/CD workflow generation.

> *"Now, let's explore our second module: the **eSim Hardware Security Linter**.*
>
> *In IoT hardware security, exposing debug buses like UART, JTAG, or SWD near the physical edge of a PCB creates severe physical tampering and firmware dumping vulnerabilities.*
>
> *Clicking the shield icon opens the settings dialog where we can configure our critical clearance thresholds—say, 5 mm.*
>
> *When we run the audit, the computational geometry engine identifies all sensitive interfaces and calculates their exact distance to the `Edge.Cuts` boundary.*
>
> *Notice that KiCad's layout canvas now displays **bright red 'X' cross markers** directly on the `User.Drawings` layer, pinpointing exactly which footprints and traces are in the danger zone!*
>
> *Even better: by enabling the **Push to GitHub Actions** checkbox, the plugin automatically creates a `.github/workflows/security.yml` file, commits the board, and initiates a cloud audit that acts as a pre-push hardware firewall!"*

---

### Part 6: Conclusion & Internship Roadmap (7:30 – 8:00)
**Visual Cue:** Final slide with GitHub repository link, feature summary, and future roadmap.

> *"To summarize: this submission delivers two fully functional, verified KiCad 9 plugins with clean modular code, comprehensive design documentation, and complete execution guides.*
>
> *During the semester-long internship, I plan to further expand this base version by implementing a true 2D Poisson FEM thermal diffusion solver and high-speed differential pair impedance verification.*
>
> *Thank you to the FOSSEE and eSim team for this opportunity, and I look forward to contributing to eSim! Thank you."*

---

## Presentation Checklist & Tips

1. **Resolution & Audio:** Record in 1080p (1920x1080) with clear microphone audio.
2. **KiCad Setup:** Have KiCad 9 pre-opened with a test PCB loaded so the demo launches without delay.
3. **Cursor Visibility:** Enable mouse pointer highlights in your screen recorder (e.g., OBS Studio).
4. **Time Enforcement:** Rehearse once to ensure your delivery falls comfortably between 6 and 8 minutes.
