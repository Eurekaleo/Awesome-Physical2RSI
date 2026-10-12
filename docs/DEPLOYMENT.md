# GitHub Pages deployment

Repository: <https://github.com/Eurekaleo/Awesome-Physical2RSI>

Website: <https://eurekaleo.github.io/Awesome-Physical2RSI/>

## Initial setup

1. In the repository's **Settings > Pages**, set **Source** to **GitHub Actions**.
2. Keep the default branch named `main`, or update both workflow branch filters.
3. Push the reviewed public files to `main`, or run **Deploy GitHub Pages** from the Actions tab.
4. Confirm that the workflow builds and uploads `dist/`, then inspect the deployed homepage and reference filters.

The quality workflow runs on pull requests and pushes. The deployment workflow builds the site independently and deploys only after validation passes. Repository content access is read-only; only the deployment job receives Pages and OpenID Connect write permissions. Concurrent deployments are serialized.

## Deployment boundary

The build selects these public paths:

- `index.html`, `404.html`, `robots.txt`, `sitemap.xml`, and `site.webmanifest`
- Approved web assets under `assets/`
- CSS and JavaScript under `site/`
- `data/references.json`

The output also receives an empty `.nojekyll` marker. Repository documentation, workflow files, import tools, raw bibliography files, manuscript files, and archives are never copied. The script rejects symlinks and unsupported extensions under the selected directories.

## Verify a release

```sh
npm run build
npm run preview
```

After deployment, check the site at its `/Awesome-Physical2RSI/` project path, including a hard reload, a nonexistent URL, mobile navigation, and at least one reference link. Preserve the repository name's capitalization in Pages URLs. Browser caches can retain an older asset briefly after deployment.

## Repository renames

GitHub redirects repository links after a rename, but that does not establish a redirect for the previous GitHub Pages path. Keep the canonical URLs and project identity synchronized, redeploy Pages, and verify any separately configured website redirect. Do not create a repository at the previous name solely for a redirect: reusing that name stops GitHub's repository redirect.

Both previous repository names, `awesome-physical-ai` and `awesome-physical-ai-rsi`, use GitHub's automatic redirect to `Awesome-Physical2RSI`. Their previous website paths are handled separately by `awesome-physical-ai/index.html` and `awesome-physical-ai-rsi/index.html` in the account's `Eurekaleo.github.io` repository. Both pages redirect directly to the current site while preserving query parameters and anchors.

After a future rename, update the URLs in `tools/public-files.mjs`, page metadata, `404.html`, `package.json`, `robots.txt`, `sitemap.xml`, issue links, and deployment documentation. Run `npm run readme` to regenerate the README and survey links, then build and redeploy. Update every historical redirect to the new canonical URL and verify those entries in a browser; a successful repository redirect does not verify a Pages redirect.

## Roll back

Revert the problematic source commit through a reviewed pull request and let the Pages workflow rebuild. Do not change the upload directory to the repository root to work around a deployment failure.
