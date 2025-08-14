# gh-deployment-workflow

A tiny demo that deploys to **GitHub Pages** only when `index.html` changes.

## How it works
- A GitHub Actions workflow triggers on pushes to `main` where the changed files include `index.html` (path filter).
- It uploads the site as a Pages artifact and deploys it to the `github-pages` environment.

Resulting URL (replace `<username>`):  
`https://<username>.github.io/gh-deployment-workflow/`
