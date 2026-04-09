# hitchhiker-s-guide-to-vlctechfest

Informal presentation to the VLCTechFest whether you are a visitor, a potential speaker or just curious.

The presentation uses [presenterm](https://github.com/mfontanini/presenterm).

[View the slides online](https://llyorshch.github.io/hitchhiker-s-guide-to-vlctechfest/)

## Publishing to GitHub Pages

presenterm requires a real TTY to export, so this must be done locally.

1. Export the slides from a terminal:
   ```bash
   presenterm --export-html slides.md --output docs/index.html
   ```

2. Commit and push the `docs/` folder.

3. In the GitHub repo, go to **Settings → Pages** and set:
   - **Source**: Deploy from a branch
   - **Branch**: `master`, folder: `/docs`