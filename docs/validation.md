# Validation notes

Checked with **KiCad 10.0.5** on **7 November 2026**.

## Project packaging

- The schematic and main project settings retain their original contents; Git normalizes text line endings to LF.
- PCB geometry, routing, footprints, and electrical data are unchanged. Only the ESP32-S3 3D model path was made project-relative.
- The ESP32-S3 STEP model and footprint library were recovered from the local `Line_follower2` project. The original absolute model path was unavailable; the supplied local model is used for visualization.
- Custom symbol libraries were extracted from the current schematic's embedded definitions; custom footprint libraries were copied from the project and local KiCad libraries.
- Every custom footprint referenced by the PCB is present in the repository. No absolute machine paths remain in the uploaded design and library files.
- Existing fabrication exports and original mechanical assets were copied without modification.
- Both PCB SVG previews, the schematic SVG, and the schematic PDF exported successfully.

## Rule-check results

These are the findings in the supplied design. Upload preparation did not attempt electrical or layout corrections.

| Check | Severity | Type | Count |
| --- | --- | --- | ---: |
| DRC | warning | `lib_footprint_mismatch` | 12 |
| DRC | warning | `silk_over_copper` | 44 |
| DRC | warning | `silk_overlap` | 50 |
| ERC | error | `power_pin_not_driven` | 1 |
| ERC | warning | `endpoint_off_grid` | 19 |

DRC reported **0 unconnected items**. Schematic/PCB parity and zone refill were not requested for this snapshot.

Full reports: [DRC](drc.json) and [ERC](erc.json). Review these findings and regenerate fabrication files before manufacturing.

## Reproduce the exports and checks

Run from the repository root with `kicad-cli` available in your terminal:

```powershell
kicad-cli pcb export svg --output docs/pcb-front.svg --layers F.Cu,F.SilkS,Edge.Cuts --page-size-mode 2 --mode-single God_of_cheeseLFR.kicad_pcb
kicad-cli pcb export svg --output docs/pcb-back.svg --layers B.Cu,B.SilkS,Edge.Cuts --page-size-mode 2 --mode-single --mirror God_of_cheeseLFR.kicad_pcb
kicad-cli sch export pdf --output docs/schematic.pdf God_of_cheeseLFR.kicad_sch
kicad-cli sch export svg --output docs/ God_of_cheeseLFR.kicad_sch
kicad-cli pcb drc --format json --output docs/drc.json God_of_cheeseLFR.kicad_pcb
kicad-cli sch erc --format json --output docs/erc.json God_of_cheeseLFR.kicad_sch
```
