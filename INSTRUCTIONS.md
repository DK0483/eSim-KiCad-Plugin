# Execution & Testing Instructions: eSim KiCad Plugin Suite

This guide provides step-by-step instructions for installing, configuring, running, and verifying the **eSim KiCad Plugin Suite** inside **KiCad 9.0+** across Windows, Linux, and macOS platforms.

---

## 1. Prerequisites & Dependencies

The plugin utilizes standard scientific Python libraries bundled with KiCad's integrated Python runtime. Ensure your system meets the following requirements:

- **KiCad Version:** KiCad 9.0 or later (KiCad 8.0 also supported)
- **Python Runtime:** Python 3.9+ (bundled inside KiCad)
- **Required Libraries:**
  - `pcbnew` (native KiCad Python module)
  - `wxPython` (native KiCad UI module)
  - `numpy` (for matrix computations & spatial binning)
  - `matplotlib` (for visual heatmaps and histograms)

> [!NOTE]
> In official KiCad installations on Windows and macOS, `numpy`, `matplotlib`, and `wxPython` are pre-installed in KiCad's internal Python environment. On Linux, if not already present, install them via your distribution package manager:
> ```bash
> sudo apt-get update && sudo apt-get install python3-numpy python3-matplotlib python3-wxgtk4.0
> ```

---

## 2. Plugin Installation

To install the plugins, copy the plugin folders into your KiCad Scripting Plugins directory.

### Directory Locations by Operating System:

| OS | Target Plugin Directory |
| :--- | :--- |
| **Windows** | `%USERPROFILE%\Documents\KiCad\9.0\scripting\plugins\` |
| **Linux** | `~/.local/share/kicad/9.0/scripting/plugins/` |
| **macOS** | `~/Documents/KiCad/9.0/scripting/plugins/` |

### Step-by-Step Installation:

1. Locate or create your KiCad 9 scripting directory:
   - **Windows (PowerShell):**
     ```powershell
     New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\Documents\KiCad\9.0\scripting\plugins"
     ```
   - **Linux / macOS (Bash):**
     ```bash
     mkdir -p ~/.local/share/kicad/9.0/scripting/plugins/
     ```

2. Copy both plugin folders from this repository into the plugins directory:
   - Copy `pcb_manufacturability_risk_analyzer/`
   - Copy `esim_hw_linter/`

3. Your directory structure should look like this:
   ```text
   plugins/
   ├── pcb_manufacturability_risk_analyzer/
   │   ├── __init__.py
   │   ├── plugin.py
   │   ├── heatmap.py
   │   ├── analysis_utils.py
   │   └── ...
   └── esim_hw_linter/
       ├── __init__.py
       ├── linter.py
       ├── icon.png
       └── ...
   ```

4. Launch or restart **KiCad 9**.

---

## 3. Running & Verifying the Plugins

### Module 1: Smart PCB Manufacturability Risk Analyzer

1. Open any routed PCB design in KiCad's PCB Editor (`pcbnew`).
2. Navigate to the top menu bar:
   - **Tools** → **External Plugins** → **Smart PCB Manufacturability Analyzer**
   - Alternatively, click the plugin icon on the top action toolbar.
3. The interactive **Analysis Dashboard** will open, presenting:
   - **Key Manufacturing Metrics:** Average track width, minimum track width, standard deviation.
   - **Component & Via Counts:** Total vias and capacitors with manufacturing stress insights.
   - **Embedded Multi-Heatmap:** 4 synchronized heatmaps:
     - *Density Heatmap* (copper trace concentration)
     - *Thermal Heatmap* ($I^2R$ power loss modeling)
     - *EMI Risk Map* (spatial 8-neighborhood cross-talk coupling)
     - *Temperature Map* (simulated ambient + thermal rise in °C)
   - **Track Width Histogram:** Graphical spread showing routing consistency.
   - **Density vs. Thermal Correlation:** Scatter plot with calculated Pearson correlation coefficient ($r$).
   - **Risk Distribution & Manufacturability Score:** Global score (0–100) and actionable DFM suggestions.

---

### Module 2: eSim Hardware Security Linter

1. Open your PCB design in KiCad's PCB Editor (`pcbnew`).
2. Ensure an `Edge.Cuts` boundary is drawn around the board perimeter.
3. Click the **Security Shield Icon** on the action toolbar (or select it from **Tools** → **External Plugins**).
4. A **Settings Dialog** will prompt you to set or confirm your physical clearance thresholds:
   - *Critical Risk Distance (mm)*: Default `5.0 mm`
   - *Security Warning Distance (mm)*: Default `15.0 mm`
   - *Optional:* Check **"Push to GitHub Actions (CI/CD)"** to automatically generate a CI/CD workflow and push the audit upstream.
5. Click **Run Audit**:
   - The plugin evaluates all debug footprints (`UART`, `JTAG`, `SWD`, `SPI`, `TEST`) and sensitive traces (`TX`, `RX`, `SWDIO`, etc.).
   - If any interface violates edge clearance rules, a $2\text{ mm} \times 2\text{ mm}$ red **'X'** marker is drawn directly onto the `User.Drawings` layer at the exact coordinates of the vulnerability.
   - A detailed findings summary dialog displays all flagged risks, warnings, and safe interfaces.

---

## 4. Headless CLI / CI/CD Verification

The Hardware Security Linter can also be executed headlessly from the command line or within automated CI/CD pipelines (e.g. GitHub Actions):

```bash
python linter.py --board "path/to/board.kicad_pcb" --crit 5.0 --warn 15.0
```

- Returns **Exit Code 0** if the board is secure.
- Returns **Exit Code 1** if critical hardware vulnerabilities are detected, preventing compromised layouts from merging into production.

---

## 5. Troubleshooting & FAQs

### Q1: The plugin does not appear in the Tools menu after restarting KiCad.
- Open the KiCad Scripting Console (**Tools** → **Scripting Console**).
- Type:
  ```python
  import pcbnew
  print(pcbnew.PLUGIN_DIRECTORIES_SEARCH)
  ```
- Verify that your plugin folder is located within one of the search directories listed.
- In the scripting console, run:
  ```python
  import pcb_manufacturability_risk_analyzer
  ```
  Check if any import errors (e.g. missing `matplotlib` or `numpy`) are reported.

### Q2: "No trace widths detected on this board" message appears.
- Ensure the PCB has active copper tracks routed on copper layers (`F.Cu`, `B.Cu`, etc.). The analyzer requires routed tracks to calculate geometry and density.

### Q3: "No Edge.Cuts drawings found" message appears in Security Linter.
- The Security Linter calculates physical boundary clearances relative to the board outline. Ensure you have closed perimeter lines drawn on the `Edge.Cuts` layer.
