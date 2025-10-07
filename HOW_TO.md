# How to Contribute to the AI Projects Showcase

This document explains how to add new projects or update existing information on the OVGU Nuclear Medicine AI Projects website. The site is built directly from the Markdown files in this repository via GitHub Pages.

## Website Structure

The website is organized as follows:

- `README.md`: The main landing page for the website.
- `HOW_TO.md`: This file, containing the contribution instructions.
- `projects/`: This directory contains the individual Markdown subpages for each project.
  - `synthetic-ct.md`
  - `...` (etc.)
- `assets/`: This directory holds all static assets, such as images (optional).

## How to Add a New Project

### 1. Create a New Project Markdown File

-   In the `projects/` directory, create a new Markdown file for your project (e.g., `new-cool-project.md`).
-   Add your project's content using Markdown. You can copy the structure from an existing project file.
-   Include a link back to the main page at the bottom: `[Back to all projects](../README.md)`

### 2. Add a Link to the Main Page

-   Open the root `README.md` file.
-   Find the list under the "Our Projects" section.
-   Add a new list item that links to your new project file. GitHub Pages will automatically convert the `.md` file to `.html`.

    ```markdown
    *   [Name of Your New Cool Project](./projects/new-cool-project.html)
    ```

### 3. Add Any Images (Optional)

-   Create an `assets/` directory in the root if it doesn't already exist.
-   Place your image file (e.g., a `.png` or `.jpg`) into the `assets/` directory.
-   Reference the image in your project's Markdown file.

    ```markdown
    ![A descriptive alt text](../assets/your-image.png)
    ```

## Deployment

The website is automatically built and deployed by GitHub Pages whenever changes are pushed to the `main` branch, as long as the source is set to the repository root in the repository settings. No special configuration is needed.