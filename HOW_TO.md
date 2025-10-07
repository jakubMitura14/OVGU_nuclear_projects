# How to Contribute to the AI Projects Showcase

This document explains how to add new projects or update existing information on the OVGU Nuclear Medicine AI Projects website. The site is built using GitHub Pages and Jekyll, which makes content updates straightforward.

## Website Structure

The website is organized as follows:

- `_config.yml`: The Jekyll configuration file. This sets the base theme for the site.
- `README.md`: The main landing page for the website.
- `HOW_TO.md`: This file, containing the contribution instructions.
- `projects/`: This directory contains the individual Markdown subpages for each project.
  - `synthetic-ct.md`
  - `...` (etc.)
- `assets/`: This directory holds all static assets.
  - `css/style.scss`: The custom stylesheet that overrides the base theme to create the site's professional look.

## How to Add a New Project

### 1. Create a New Project Markdown File

- In the `projects/` directory, create a new Markdown file for your project (e.g., `new-cool-project.md`).
- Add your project's content using Markdown. You can copy the structure from an existing project file.
- Include a link back to the main page at the bottom: `[Back to all projects](../README.md)`

### 2. Add a Link to the Main Page

- Open the root `README.md` file.
- Find the `<ul>` list under the "Our Projects" section.
- **Important:** Add a new list item using the HTML `<li>` tag to ensure it renders correctly.

  ```html
  <li><a href="./projects/new-cool-project.html">Name of Your New Cool Project</a></li>
  ```

### 3. Add Any Images (Optional)

- If it doesn't already exist, create an `assets/images/` directory.
- Place your image file (e.g., a `.png` or `.jpg`) into the `assets/images/` directory.
- Reference the image in your project's Markdown file.

  ```markdown
  ![A descriptive alt text](../assets/images/your-image.png)
  ```

## Customizing the Style

The website uses the `jekyll-theme-minimal` as a base, but its appearance is customized.

- **Base Theme:** The theme is set in the `_config.yml` file.
- **Custom Styles:** All custom styles, colors, and layout adjustments are located in the `assets/css/style.scss` file. If you need to make design changes, you should edit this file.

## Deployment

The website is automatically built and deployed by GitHub Pages whenever changes are pushed to the `main` branch, as long as the source is set to the repository root in the repository settings. No special configuration is needed.