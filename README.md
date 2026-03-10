# A Little Bit of Everything in Biostatistics for Health Science Students

An open-source applied biostatistics textbook for HS 4510 at the University of the People.

Built with [Jupyter Book](https://jupyterbook.org) · Dataset: `Snow.deaths` (HistData, R)

---

## Repository Structure

```
biostats-book/
├── _config.yml          # Book settings (title, GitHub links, LaTeX extensions)
├── _toc.yml             # Navigation sidebar definition
├── index.md             # Landing page
├── references.bib       # BibTeX citations (optional)
└── chapters/            # All chapter content
    ├── 01_vitals.md
    ├── 02_middle.md
    ├── 03_margin.md
    ├── 04_chance.md
    ├── 05_hypothesis.md
    ├── 06_twogroups.md
    ├── 07_scaling.md
    ├── 08_future.md
    └── 09_review.md
```

---

## Building the Book Locally

**Prerequisites:** Python 3.8 or later.

```bash
# 1. Install Jupyter Book
pip install jupyter-book

# 2. Build the HTML website
jupyter-book build biostats-book/

# 3. Open locally — double-click the file below in your file explorer,
#    or run this in your terminal:
open biostats-book/_build/html/index.html
```

The compiled website will appear in `biostats-book/_build/html/`.

---

## Publishing to GitHub Pages

After building locally and confirming it looks correct:

```bash
# Install the GitHub Pages publishing tool
pip install ghp-import

# Push the built HTML to the gh-pages branch
ghp-import -n -p -f biostats-book/_build/html
```

Your book will be live at:
`https://yourusername.github.io/your-repo-name`

Update `repository_url` in `_config.yml` to match your actual GitHub repository URL.

---

## Editing the Book

Every chapter is a standard Markdown (`.md`) file in the `chapters/` folder.
Open any file in any text editor, make changes, and re-run `jupyter-book build` to update the website.

**Math formulas** use standard LaTeX syntax:
- Inline: `$\bar{x} = \frac{\sum x_i}{n}$`
- Display: `$$SE = \frac{s}{\sqrt{n}}$$`

---

## Licence

This textbook is released under the [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/) licence.
You are free to share and adapt the material for any purpose, provided appropriate credit is given.
