<!---![FastMAPO Logo](img/fastmapol1.png)--->
<img src="img/fastmapol1.png" alt="drawing" width="500"/>

# FastMAPOL ATBD Builder

This repository contains the source files for the **FastMAPOL Algorithm Theoretical Basis Document (ATBD)**. The ATBD documents the scientific methodology, forward models, retrieval algorithms, and data products for the FastMAPOL system used with PACE polarimeter observations (HARP2 and SPEXone).

The document is written in **Markdown** and compiled into a structured report (HTML/PDF) using **Quarto**.

---

## FastMAPOL in a Nutshell

FastMAPOL is a physics-based inversion framework that retrieves aerosol and ocean properties from multi-angle polarimetric measurements. It combines:

- Polarized radiative transfer modeling  
- Neural network forward model emulation  
- Nonlinear least-squares inversion  
- Uncertainty quantification  

The ATBD provides a complete description of these components and their implementation.

---

## Repository Structure

    mapoldoc/
    ├── atbd_v1/        # Quarto build directory
    ├── chapters/       # Chapter files (Markdown)
    ├── figure/         # Figures
    ├── bib/            # Bibliography files (optional)
    ├── logo/           # Logo
    └── README.md

---

## Writing and Updating Chapters

All content is maintained in the `chapters/` directory using **Markdown**.

### General rules

- Each chapter should start with a level-2 heading:

    ## Chapter Title

- Keep content modular and focused on a single topic

---

### Citations

Use BibTeX-style citation keys:

    @Gao:2021aa

Ensure the key exists in the `.bib` file referenced in `_quarto.yml`.

---

### Figures

Define figures with captions and labels:

    Based on the synthetic data, retrieval results are shown in @fig-validation_harp2_l2.

    ![Retrieval performance: AOD (550 nm) and ALH showing retrieval, truth, and uncertainty (a–c, d–f). Figure adapted from @Gao:2023aa](../img/validation_harp2_l2.png){#fig-validation_harp2_l2 width=80%}

Guidelines:

- Use clear, concise captions  
- Always include a figure label (`#fig-...`)  
- Reference figures using `@fig-...`  

---

## Building the ATBD

Generate the document using Quarto:

    cd atbd_v1
    quarto render

Output will be located in:

    atbd_v1/_site/

---

## Contributing

Contributions are welcome.

### Workflow

1. Edit or add files in `chapters/`
2. Follow formatting rules (headings, citations, figures)
3. Keep content clear and scientifically accurate
4. Test locally:

        quarto render

5. Submit a pull request

---

### Best Practices

- Use clear, technical ATBD-style writing  
- Avoid duplication across chapters  
- Keep terminology consistent  
- Use publication-quality figures  
- Cite all scientific content  

---

## Notes

- Do **not** edit files in `_site/`
- Use relative paths for images (`../img/...`)
- Keep figures optimized for HTML/PDF output

---

## Future Extensions

- Additional chapters (uncertainty, NN model, validation)
- Expanded bibliography support
- Automated figure generation and validation tools