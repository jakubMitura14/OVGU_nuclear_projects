# How to Contribute to the AI Projects Showcase

This document explains how to add new projects or update existing information on the OVGU Nuclear Medicine AI Projects website. The site is built using GitHub Pages and Jekyll, which makes content updates straightforward.

## Website Structure

The website is organized as follows:

- `_config.yml`: The Jekyll configuration file. This sets the base theme for the site.
- `README.md`: The main landing page for the website.
- `HOW_TO.md`: This file, containing the contribution instructions.
- `projects/`: This directory contains the individual Markdown subpages for each project.
- `assets/`: This directory holds all static assets.

## How to Add a New Project

### 1. Create a New Project Markdown File

- In the `projects/` directory, create a new Markdown file for your project (e.g., `new-cool-project.md`).
- Add your project's content using Markdown. You can copy the structure from an existing project file.
- Include a link back to the main page at the bottom: `[Back to all projects](../README.md)`

### 2. Add a Link to the Main Page

- Open the root `README.md` file.
- Find the `projects-section` `<div>`. The structure looks like this:

  ```html
  <div class="projects-section">

  <h2>Our Projects</h2>

  <ul>
    <li><a href="...">...</a></li>
    <!-- Add new projects here -->
  </ul>

  </div>
  ```

- **Important:** Add a new list item for your project using the HTML `<li>` tag inside the `<ul>`. This is critical to ensure it renders correctly.

  ```html
  <li><a href="./projects/new-cool-project.html">Name of Your New Cool Project</a></li>
  ```

**Note on Rendering:** To avoid issues with how GitHub Pages renders Markdown inside HTML, we use standard HTML tags (`<h2>`, `<ul>`, `<li>`) for the project list section in the `README.md`. Please maintain this structure.

### 3. Add Any Images (Optional)

- If it doesn't already exist, create an `assets/images/` directory.
- Place your image file (e.g., a `.png` or `.jpg`) into the `assets/images/` directory.
- Reference the image in your project's Markdown file.

  ```markdown
  ![A descriptive alt text](../assets/images/your-image.png)
  ```

## Customizing the Style

The website's appearance is controlled by the files in the `assets/css/` directory and the theme set in `_config.yml`. If you need to make design changes, you should edit these files.

## Deployment

The website is automatically built and deployed by GitHub Pages whenever changes are pushed to the `main` branch. No special configuration is needed.