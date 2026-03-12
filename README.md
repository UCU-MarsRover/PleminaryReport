# Preliminary Report

LaTeX template for the **European Rover Challenge 2026 Preliminary Report**.

## Repository structure

```
.
├── main.tex                          # Main document entry point
├── preamble.tex                      # Shared LaTeX preamble (packages, styles)
├── sections/
│   ├── cover.tex                     # Cover page
│   ├── matrix_of_compliance.tex      # Section 1 – Matrix of Compliance
│   ├── preliminary_test_plan.tex     # Section 2 – Preliminary Test Plan
│   ├── preliminary_design_description.tex  # Section 3 – Design Description
│   ├── rio_analysis.tex              # Section 4 – RIO Analysis
│   └── project_budget.tex            # Section 5 – Project Budget
├── teams/
│   ├── drone/design.tex              # Drone Team
│   ├── arm/design.tex                # Arm Team
│   ├── suspension/design.tex         # Suspension Team
│   ├── science/design.tex            # Science Team
│   ├── ground_station/design.tex     # Ground Station Team
│   ├── navigation/design.tex         # Navigation Team
│   └── electronics/design.tex        # General Electronics Team
└── figures/                          # Place all figures/images here
```

## How to use

1. **Set team metadata** – open `main.tex` and edit `\teamname`, `\projectname`,
   `\submissiondate`, and `\revisionnum`.

2. **Fill in your content** – replace all `[placeholder]` text in `sections/`
   and `teams/` files with your actual content.

3. **Add figures** – place all images in the `figures/` directory and reference
   them with `\includegraphics{filename}` (no extension needed for common formats).

4. **Compile**:
   ```bash
   pdflatex main.tex   # run twice to resolve references
   # or
   latexmk -pdf main.tex
   ```

5. **Embed the MoC `.xlsx`** – after compiling to PDF, use a PDF editor (e.g.
   [Foxit PDF Editor](https://www.foxit.com/)) to embed the `MoC_<TeamName>_ERC2026.xlsx`
   file as an attachment to earn full marks.

6. **Name your output file** as per ERC rules:
   `<TeamName>_PreliminaryReport_ERC2026.pdf`

## Requirements (ERC 2026)

| Requirement | Value |
|---|---|
| Format | A4, searchable PDF |
| Max pages | 25 (excl. cover, TOC, Appendix 3) |
| Language | English |
| Min font size | 10 pt |
| Margins | ≥ 2.54 cm (1 inch) all sides |

## Team sections

Each team fills in their corresponding file under `teams/`:

| Team | File |
|---|---|
| Drone | `teams/drone/design.tex` |
| Arm | `teams/arm/design.tex` |
| Suspension | `teams/suspension/design.tex` |
| Science | `teams/science/design.tex` |
| Ground Station | `teams/ground_station/design.tex` |
| Navigation | `teams/navigation/design.tex` |
| General Electronics | `teams/electronics/design.tex` |
This repository is created to organize team's work on the Preliminary Report.
