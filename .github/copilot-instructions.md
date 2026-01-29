# AI Coding Agent Instructions for Math-3180-UConn

## Project Overview

This is an **undergraduate machine learning course repository** (Math 3180, University of Connecticut) that teaches mathematical foundations using Python, Jupyter notebooks, and academic publishing workflows.

**Key Structure:**

- **Topic Modules**: Each ML topic (LinearRegression, GradientDescent, NaiveBayes, PCA, SVM, etc.) has a consistent structure: `notes/`, `slides/`, `lab/`, `src/`, `data/`, and `img/`
- **Purpose**: Educational content combining mathematical theory with computational practice
- **Primary Audiences**: Undergraduate students and course instructors

## Architecture & Data Flow

### Topic Module Structure

Each topic directory follows this pattern:

- **notes/**: Markdown source files converted to HTML/PDF using Pandoc + LaTeX
  - Files use mathematical notation with custom LaTeX commands (e.g., `\newcommand{\df}[1]{\frac{\partial}{\partial #1}}`)
  - Generated artifacts: `main.html`, `main.tex`, `main.pdf`
- **lab/**: Jupyter notebooks (`.ipynb`) for hands-on student exercises
  - Example: [LinearRegression/lab/RegressionLab.ipynb](../LinearRegression/lab/RegressionLab.ipynb)
  - Often paired with solution versions: `RegressionLabWithSolution.ipynb`
- **src/**: Python visualization and example scripts
  - Use Bokeh for interactive plots (e.g., [DistanceToPlane.py](../LinearRegression/src/DistanceToPlane.py) generates HTML + PNG)
  - Scripts often demonstrate visualization techniques rather than core algorithms
- **slides/**: HTML-based presentations from Markdown sources
- **data/**: Topic-specific datasets referenced in labs (if needed)

### Course Content Flow

`docs/` → Published course website (Jekyll-based, hosted at jeremy9959.net/Math-3180-UConn)

- `docs/schedule.md` - Course calendar
- `docs/topics.md` - Topic outline
- `docs/project.md`, `docs/project2.md` - Project specifications
- `docs/daily/` - Daily lecture PDFs and review materials (Quarto/markdown-to-PDF)

`data/` - Shared datasets used across topics

- `auto-mpg/`, `penguins/`, `home_sales/`, `jester/`, etc.
- Each has a README describing source and usage

### External Dependencies

- **Pandoc + LaTeX filters**: `pandoc-secnos`, `pandoc-fignos`, `pandoc-eqnos`, `pandoc-citeproc` for academic document generation
- **Bokeh**: For interactive visualizations (not matplotlib)
- **Jupyter**: For notebook-based labs
- **Quarto** (implied from `.qmd` files): For dynamic document generation
- **Bibliography**: Uses CSL (Citation Style Language) with BibTeX references

## Critical Workflows

### Document Generation (notes/slides)

All `.md` → `.html`/`.pdf` conversion uses Makefile-based Pandoc pipelines:

```bash
cd LinearRegression/notes/
make html    # → main.html using mathjax for math rendering
make pdf     # → main.tex then main.pdf using pdflatex
make paper   # → main.tex only
```

**Key Pattern**: LaTeX filters ensure proper cross-referencing for equations, figures, and citations. When editing `.md` files, preserve filter directives and citation format.

### Daily Lecture Notes (Daily/ directory)

- **Raw lecture notes**: `Daily/` contains instructor notes from each class session
- **Date formats**: Two naming conventions exist:
  - **New format** (preferred): `YYYYMMDD.md` or `.qmd` (e.g., `20260122.md` for Jan 22, 2026)
  - **Legacy format**: `YYYY-MM-DD.md` (e.g., `2022-01-17.md`)
- **File types**:
  - `.md` files: Plain markdown lecture notes (outline style, informal)
  - `.qmd` files: Quarto markdown with YAML frontmatter for PDF generation
- **Content style**: Notes are typically bullet-point outlines with mathematical notation, not polished documents
- **Generation**: `quarto render <file>.qmd` → produces `.html` and `.pdf` handouts for students
- **Publishing**: Selected/polished notes republished to `docs/daily/` with README index

### Publishing

- Course website hosted via Jekyll (`_config.yml` uses jekyll-theme-minimal)
- Manual deployment of `docs/` folder to web

## Project-Specific Conventions

### File Organization

- **Case sensitivity matters**: Dates use format `YYYYMMDD` (e.g., `20260122.md`)
- **Math notation**: Heavy use of LaTeX; all notes assume MathJax/pandoc math filters
- **Images**: Stored in `img/` relative to each topic; `.py` scripts export to PNG via Bokeh
- **Data naming**: CSV files in `data/<topic_name>/` with README for provenance

### Notebook Conventions

- Labs named with subject (`RegressionLab.ipynb`, not `lab.ipynb`)
- Solution versions suffixed: `RegressionLabWithSolution.ipynb`
- Expected to be self-contained examples for students (not production code)

### Code Style

- **Visualization-first**: Python scripts focus on Bokeh visualization setup, not algorithmic complexity
- **Example pattern** (from [DistanceToPlane.py](../LinearRegression/src/DistanceToPlane.py)):
  1. Create Bokeh figure with explicit dimensions & titles
  2. Add data (scatter, lines) with semantic colors
  3. Export to PNG for handouts
  4. Show interactive version

### Mathematical References

- Bibliography: `references/references.bib` (single shared BibTeX file)
- Citation style: `resources/stat.csl` (statistical formatting)
- Custom LaTeX macros defined per-document in YAML frontmatter or `.md` header

## Coding Priorities

**When implementing or extending:**

1. **Educational clarity over efficiency**: Code should demonstrate concepts; assume notebooks are teaching tools
2. **Reproducibility**: All lab notebooks should run without external paths; use bundled datasets from `data/`
3. **Pandoc/LaTeX compatibility**: If editing `.md` files, preserve filter markers and math syntax
4. **Consistent date formats**: Use `YYYYMMDD` for filenames
5. **Self-contained scripts**: Python `.py` files should be runnable standalone with `python script.py`

## Key Files to Know

- **Starting point**: [docs/README.md](../docs/README.md) - course overview
- **Topic lookup**: [topics.md](../topics.md) - complete list of covered topics
- **Assignment specs**: [docs/project.md](../docs/project.md) (midterm), [docs/project2.md](../docs/project2.md) (final)
- **Build reference**: Any `Makefile` in `notes/` or `slides/` subdirectories
