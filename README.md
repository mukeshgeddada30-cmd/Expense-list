# Stride — Step Counter

A responsive step-counter website built with React and Vite.

## Features
- Daily step dashboard with animated goal progress ring
- Manual step entry (adjustable increments)
- Optional phone motion-sensor tracking where supported
- Daily step goal editor
- Estimated distance, calories, and active time
- Local browser storage so today's count is saved on the device
- Responsive layout for mobile and desktop

## Run locally
1. Install [Node.js](https://nodejs.org/).
2. Open this project folder in a terminal.
3. Run:

   ```bash
   npm install
   npm run dev
   ```

4. Open the local URL printed by Vite.

## Build
```bash
npm run build
```

## Publish on GitHub Pages
1. Create a new GitHub repository and upload/push all files in this folder.
2. In the repository, open **Settings → Pages**.
3. Under **Build and deployment**, choose **GitHub Actions**.
4. Create `.github/workflows/deploy.yml` with the workflow below, then commit it.

```yaml
name: Deploy React app to GitHub Pages
on:
  push:
    branches: [main]
  workflow_dispatch:
permissions:
  contents: read
  pages: write
  id-token: write
concurrency:
  group: pages
  cancel-in-progress: true
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: npm
      - run: npm ci
      - run: npm run build
      - uses: actions/upload-pages-artifact@v3
        with:
          path: ./dist
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - id: deployment
        uses: actions/deploy-pages@v4
```

The project uses a relative Vite base path, so it works when hosted under a GitHub Pages repository URL.

## Notes about motion tracking
Motion tracking requires a compatible phone/browser and usually a secure context (HTTPS). Some browsers ask for motion permission after you tap **Start motion tracking**. Sensor step detection is a lightweight estimate and can vary by device. Manual entry works everywhere.

Step data is stored in the current browser's local storage; it is not synced between devices.
