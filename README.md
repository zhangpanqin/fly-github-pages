### GitHub Pages

name: Build and Deploy
on:
  push:
    branches: ["main"]
  pull_request:
    branches: ["main"]
permissions:
  contents: read
  pages: write
  id-token: write
jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Checkout
        uses: actions/checkout@v3.3.0

      - name: Setup Node.js environment
        uses: actions/setup-node@v3.6.0
        with:
          node-version-file: .tool-versions
          cache: yarn
          cache-dependency-path: yarn.lock
    
      - name: Install dependency and Build
        run: yarn && yarn docs:build
    
      - name: Setup Pages
        uses: actions/configure-pages@v3.0.4
    
      - name: Upload GitHub Pages artifact
        uses: actions/upload-pages-artifact@v1.0.7
        with:
          name: github-pages
          path: docs/.vuepress/dist
    
      - name: Deploy GitHub Pages site
        uses: actions/deploy-pages@v1.2.4
        with:
          artifact_name: github-pages