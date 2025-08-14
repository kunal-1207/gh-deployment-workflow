# ​ GitHub Pages Deployment with GitHub Actions

This repository demonstrates how to automatically deploy a website to **GitHub Pages** using **GitHub Actions**, but **only when the `index.html` file changes**.

It’s a minimal, focused example that can be adapted to any project — from a single HTML file to a full static site generator like Hugo, Jekyll, or Astro.

---

## ​ Repository Structure

```

gh-deployment-workflow/
├── index.html              # The main HTML page deployed to GitHub Pages
├── README.md               # Documentation for the project
└── .github/
└── workflows/
└── deploy.yml      # GitHub Actions workflow for deployment

````

---

## ​​ index.html

A simple HTML page saying **"Hello, GitHub Actions!"** that serves as our deployed site.

Example content:

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Hello, GitHub Actions!</title>
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <style>
      html,body{height:100%;margin:0;font:16px/1.5 system-ui,Segoe UI,Roboto,Helvetica,Arial}
      .wrap{display:grid;place-items:center;height:100%}
      .card{padding:2rem 3rem;border-radius:16px;box-shadow:0 10px 30px rgba(0,0,0,.08)}
    </style>
  </head>
  <body>
    <main class="wrap">
      <div class="card">
        <h1>Hello, GitHub Actions!</h1>
        <p>If you can read this on Pages, the workflow worked 🎉</p>
      </div>
    </main>
  </body>
</html>
````

---

## &#x20;GitHub Actions Workflow

File: `.github/workflows/deploy.yml`

```yaml
name: Deploy index.html to GitHub Pages

on:
  push:
    branches: [ "main" ]
    paths:
      - "index.html"   # Only trigger when index.html changes
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: true

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Pages
        uses: actions/configure-pages@v5

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: .

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

---

## &#x20;How It Works

1. **Trigger conditions**

   * Runs on `push` to `main`
   * Only runs if `index.html` changes (thanks to `paths` filter)
   * Can also be triggered manually via **Workflow Dispatch**

2. **Jobs**

   * **Build job**

     * Checks out the repo
     * Configures GitHub Pages metadata
     * Uploads the HTML file(s) as a Pages artifact
   * **Deploy job**

     * Deploys the uploaded artifact to GitHub Pages
     * Publishes it to the repository’s Pages environment

3. **Deployment**

   * GitHub Pages picks up the deployed content
   * Accessible at:

     ```
     https://<your-username>.github.io/gh-deployment-workflow/
     ```

---

## &#x20;Setup Instructions

1. **Create the Repository**

   * Name it `gh-deployment-workflow` (or similar)
   * Add `index.html`, `README.md`, and `.github/workflows/deploy.yml`

2. **Push to GitHub**

   ```bash
   git add .
   git commit -m "Initial commit: setup GitHub Pages deployment"
   git push origin main
   ```

3. **Enable GitHub Pages**

   * Go to **Settings → Pages**
   * Set **Source** to **GitHub Actions**

4. **Deploy**

   * Edit `index.html`
   * Commit and push the change
   * Watch the Actions tab for the workflow run
   * Visit your site at:

     ```
     https://<your-username>.github.io/gh-deployment-workflow/
     ```

---

## &#x20;Notes

* **Only deploys when `index.html` changes** — saves workflow runs for unrelated commits.
* To deploy all files in the repo, change:

  ```yaml
  paths:
    - "**"
  ```
* To deploy just the HTML file without other files:

  ```yaml
  with:
    path: index.html
  ```

---

## &#x20;Stretch Goal: Static Site Generators

You can extend this setup for tools like **Hugo**, **Jekyll**, or **Astro**.
Example Hugo workflow snippet:

```yaml
- name: Install Hugo
  uses: peaceiris/actions-hugo@v3
  with:
    hugo-version: 'latest'
    extended: true

- name: Build site
  run: hugo --minify

- name: Upload artifact
  uses: actions/upload-pages-artifact@v3
  with:
    path: ./public
```

---

## &#x20;Related Resources

* 🛠 [roadmap.sh: GitHub Actions Deployment Workflow Guide](https://roadmap.sh/projects/github-actions-deployment-workflow)
* 📚 [GitHub Pages Documentation](https://docs.github.com/pages)
* 📘 [GitHub Actions Documentation](https://docs.github.com/actions)
* ⚙️ [Official Pages Deployment Workflow Example](https://github.com/actions/deploy-pages)

---

## &#x20;License

This project is released under the [MIT License](LICENSE).

