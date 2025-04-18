# Ocre Documentation
[![Publish](https://github.com/project-ocre/project-ocre.github.io/actions/workflows/publish.yml/badge.svg)](https://github.com/project-ocre/project-ocre.github.io/actions/workflows/publish.yml)
[![slack](https://img.shields.io/badge/slack-ocre-brightgreen.svg?logo=slack)](https://lfedge.slack.com/archives/C07F190CC3X)

## Overview
This repository contains the raw source files for building and maintaining documentation for the [Ocre runtime](https://github.com/project-ocre/ocre-runtime) project. While this repository houses the content and structure, it's not intended for direct reading of the documentation. For the most up-to-date and user-friendly version of our docs, please visit our live site at: https://docs.project-ocre.org/

Interested in contributing? Great! You'll find detailed contribution guidelines and setup instructions in the sections below.

## Project Structure and Contribution Guidelines
This documentation site was built using [Jekyll](https://jekyllrb.com/) and the [Just the Docs theme](https://just-the-docs.com/).

### Project File Structure
```
├── docs/                  # Contains all markdown files for documentation
│   └── assets/            # Holds images, CSS, JS, etc. for the documentation
├── _config.yml            # Jekyll configuration file, maintainers manage this
├── Gemfile                # Dependencies for Jekyll site, maintainers manage this
├── Gemfile.lock           # Locked dependencies, maintainers manage this
├── README.md              # You're reading this file
└── _site/                 # Auto-generated, do not edit directly (site build output)
```

### Key Guidelines for Contributors
* **Documentation Location:** All content related to Ocre’s documentation lives in the `docs` folder. If you're contributing, focus your efforts here.
* **Editing Scope:** Contributors (who are not maintainers) should avoid editing files outside the `docs` folder. The rest of the structure is primarily for configuration and assets that support the documentation.
* **Assets:** The assets directory is reserved for resources that support the site's core functionality, including scripts, styles, and any images referenced by configuration files or top-level pages (such as `_config.yml` and `index.md`). All other documentation-specific images should be stored alongside the documents where they are referenced, keeping resources as close as possible to their usage location.

This structure and these guidelines help ensure the integrity and maintainability of the Ocre documentation site while allowing contributions to focus on content updates. Failure to follow these guidelines may result in rejecting your PR. If you have any questions please reach out to us via [Slack](https://lfedge.slack.com/archives/C07F190CC3X).

## Getting Started

### Prerequisites

Ensure you have the following installed on your system:

| Software | Version |
|----------|---------|
| Ruby     | ≥ 3.3.2 |
| Bundler  | ≥ 2.5.4 |
| Jekyll   | ≥ 4.3.0 |

### Setup and Local Development

We encourage contributions to improve the documentation! If you’re looking to update or add content, follow these steps:

1. Fork the repository on GitHub.
2. Clone your forked repo
    ```bash
   git clone https://github.com/your-username/project-ocre.github.io.git
   cd project-ocre.github.io
    ```
3. Install dependencies.
    ```
    bundle install
    ```
4. Create a new branch for your changes.
    ```bash
    git checkout -b feature/your-feature
    ```
5. Make your changes. Rembmer to focus on the `docs` folder for content updates.
6. Preview your changes locally by running:
    ```bash
    bundle exec jekyll serve
    ```
7. Open your browser and visit http://localhost:4000 to see your changes.

8. Once satisfied with your changes, commit them:
    ```
    git add .
    git commit -m "Brief description of your changes"
    ```

9. Push your changes to your fork:
    ```
    git push origin feature/your-feature-name
    ```

10. Go to the main [Ocre docs repository](https://github.com/project-ocre/project-ocre.github.io) and submit a [pull request](https://github.com/project-ocre/project-ocre.github.io/pulls) with a clear description of your changes.

We'll review your contribution and get back to you as soon as possible. Thank you for helping improve the Ocre documentation!

## Contact

If you have any questions, feel free to [reach out](https://lfedge.slack.com/archives/C07F190CC3X) on Slack or open an issue for discussion.
