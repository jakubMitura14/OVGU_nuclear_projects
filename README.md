# OVGU Nuclear Medicine - AI Projects Showcase

This repository contains the source code for a static website showcasing the Artificial Intelligence (AI) projects and expertise of the **OVGU Nuclear Medicine Clinic**. The website is built with simple HTML and CSS and is hosted on GitHub Pages.

The primary goal of this repository is to provide a clean, professional, and easily maintainable platform to display the clinic's innovative work in the field of AI and nuclear medicine.

## Website Structure

The website is organized as follows:

- `docs/`: This directory contains all the files for the GitHub Pages website.
  - `index.html`: The main landing page, which serves as a directory for all projects.
  - `projects/`: This directory contains the individual HTML subpages for each project.
    - `synthetic-ct.html`
    - `thyroid-app.html`
    - `...` (etc.)
  - `assets/`: This directory holds all static assets.
    - `style.css`: The stylesheet for the website.
    - `(images)`: Project images should be stored here.

## How to Contribute

The website is designed to be easily updated. Here's how you can add new projects.

### Adding a New Project

1.  **Create a new project HTML file:**
    -   In the `docs/projects/` directory, create a new HTML file for your project (e.g., `new-cool-project.html`).
    -   You can copy the structure from an existing project file to get started.
    -   Update the `title` and content with the new project's information.

2.  **Add a link to the main page:**
    -   Open `docs/index.html`.
    -   Find the `<ul>` with the class `project-list`.
    -   Add a new list item `<li>` that links to your new project file. For example:
        ```html
        <li><a href="projects/new-cool-project.html">Name of Your New Cool Project</a></li>
        ```

3.  **Add any images (optional):**
    -   Place your image file (e.g., a `.png` or `.jpg`) into the `docs/assets/` directory.
    -   Reference the image in your project's HTML file using an `<img>` tag. For example:
        ```html
        <img src="../assets/your-image.png" alt="A descriptive alt text" style="width:100%; max-width:600px;">
        ```
    -   Note the `../` in the path, which is necessary to go up one level from the `projects` directory.

## Deployment

The website is automatically deployed to GitHub Pages. Any changes pushed to the `main` branch will trigger a GitHub Actions workflow that builds and deploys the site.

The live website can be accessed at the URL provided by GitHub Pages once the repository is set up.

---

This project was initiated by the OVGU Nuclear Medicine Clinic to highlight its commitment to research and innovation in medical technology. For more information about the clinic, please visit the [official website](https://www.med.ovgu.de/krn/en/).