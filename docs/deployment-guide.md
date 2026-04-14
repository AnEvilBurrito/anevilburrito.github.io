# Deployment Guide

This guide explains how to build, preview, and deploy this website.

## Prerequisites

Before you begin, ensure you have the following installed:

- **Ruby** (version 3.2+ recommended)
- **Bundler**
- **Node.js** (for JavaScript processing)

### Installation

#### Windows (WSL) / Linux
```bash
sudo apt update
sudo apt install ruby-dev ruby-bundler nodejs build-essential
```

#### macOS
```bash
brew install ruby node
gem install bundler
```

## Local Development

1. **Install Dependencies**
   Run this in the root of the repository:
   ```bash
   bundle install
   ```
   *If you encounter permission errors, try installing gems locally:*
   ```bash
   bundle config set --local path 'vendor/bundle'
   bundle install
   ```

2. **Serve Locally**
   To preview the site, run:
   ```bash
   bundle exec jekyll serve -l -H localhost
   ```
   The site will be available at `http://localhost:4000`. Changes to files will trigger an automatic rebuild.

## Using Docker

If you prefer not to install Ruby locally, you can use Docker:

1. **Start the Container**
   ```bash
   docker compose up
   ```
2. **Access the Site**
   The site will be available at `http://localhost:4000`.

## Deployment to GitHub Pages

This site is automatically deployed via GitHub Pages when changes are pushed to the `master` branch.

1. **Commit and Push Changes**
   ```bash
   git add .
   git commit -m "Update website content"
   git push origin master
   ```
2. **Verify Deployment**
   Check the "Actions" tab in your GitHub repository to see the build progress. Once completed, the site will be live at `https://anevilburrito.github.io/`.

## Troubleshooting

- **Gemfile.lock errors:** If `bundle install` fails, try deleting `Gemfile.lock` and running it again.
- **Port 4000 already in use:** You can specify a different port using `--port XXXX`.
