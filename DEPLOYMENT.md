# 🚀 Deploying to GitHub Pages

Since this project uses **Docsify** (a client-side documentation renderer), hosting on GitHub Pages requires no build step.

## Prerequisites

1.  A GitHub account.
2.  This repository pushed to GitHub.

## One-Time Setup

1.  **Push your code to GitHub.**
    ```bash
    git init
    git add .
    git commit -m "Initial commit"
    git branch -M main
    git remote add origin https://github.com/YOUR_USERNAME/learning-waddle.git
    git push -u origin main
    ```

2.  **Enable GitHub Pages.**
    *   Go to your repository **Settings**.
    *   Click on **Pages** in the left sidebar.
    *   Under **Build and deployment**, select **Source** as `Deploy from a branch`.
    *   Under **Branch**, select `main` and folder `/ (root)`.
    *   Click **Save**.

3.  **Wait for deployment.**
    *   GitHub will automatically build and deploy your site.
    *   Wait a minute or two, then refresh the page to see your live URL (e.g., `https://your-username.github.io/learning-waddle/`).

## ⚠️ Important Note: The `.nojekyll` File

This repository includes a hidden file called `.nojekyll`. **Do not delete it.**

*   **Why?** GitHub Pages uses Jekyll by default, which ignores any files starting with an underscore (like `_sidebar.md`).
*   **Fix:** The `.nojekyll` file tells GitHub to treat this as a raw static site, ensuring your sidebar and other assets load correctly.

## 🧪 Running Locally

To preview the site on your machine:

```bash
npx docsify-cli serve .
```

Then open `http://localhost:3000` in your browser.
