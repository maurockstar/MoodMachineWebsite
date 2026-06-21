# Mood Machine — Static Website
<!-- redeploy trigger: Actions policy fixed, re-running SWA deploy -->

A single self-contained web page. `index.html` has everything inlined (styles, scripts, fonts-link), so it works on any modern browser — Windows/PC, Mac, iPhone (Safari), and Android (Chrome). It is fully responsive.

Files in this folder:
- `index.html` — the website (this is all you really need)
- `staticwebapp.config.json` — config used by Azure Static Web Apps (safe to keep)

---

## Option A — Azure Static Web Apps (recommended, has a free tier)

You get a free auto-generated URL like `https://<random-name>.azurestaticapps.net`.

### Easiest path (no Git, no build) — Azure CLI

1. Install the Azure CLI: https://learn.microsoft.com/cli/azure/install-azure-cli
   Then install the Static Web Apps CLI:
   ```
   npm install -g @azure/static-web-apps-cli
   ```
2. Sign in:
   ```
   az login
   ```
3. Create a Static Web App (Free SKU):
   ```
   az staticwebapp create \
     --name moodmachine-site \
     --resource-group moodmachine-rg \
     --location "eastus2" \
     --sku Free
   ```
   If the resource group doesn't exist yet:
   ```
   az group create --name moodmachine-rg --location "eastus2"
   ```
4. Get a deployment token:
   ```
   az staticwebapp secrets list --name moodmachine-site --query "properties.apiKey" -o tsv
   ```
5. From INSIDE this `site/` folder, deploy the files:
   ```
   swa deploy ./ --deployment-token <PASTE_TOKEN_HERE> --env production
   ```
6. The CLI prints your live URL: `https://<name>.azurestaticapps.net`. Open it on any device.

### GitHub path (ready-made workflow included)

A workflow file is already bundled at `.github/workflows/azure-static-web-apps.yml`.

1. Push this folder's contents to a GitHub repo's `main` branch (keep `index.html` at the repo root, and the `.github/` folder at the root too).
2. In the Azure Portal, create a **Static Web App** (Plan: **Free**) and connect it to your repo/branch — OR, if you created the app already, just add the deployment token as a repo secret:
   - Azure Portal → your Static Web App → **Manage deployment token** → copy it.
   - GitHub repo → **Settings → Secrets and variables → Actions → New repository secret** → name it `AZURE_STATIC_WEB_APPS_API_TOKEN`, paste the token.
3. Push any commit to `main`. The GitHub Action runs and deploys; your URL appears in the Portal as `https://<name>.azurestaticapps.net`.
4. If `index.html` lives in a subfolder, change `app_location: "/"` to `app_location: "/site"` in the workflow.

### Portal path (Azure auto-generates the workflow)

1. Put this `site/` folder in a GitHub repo.
2. Azure Portal → "Create a resource" → "Static Web App" → Plan type: **Free**.
3. Connect your GitHub repo/branch.
4. Build details: **App location** = `/` (or `/site` if the folder is nested), leave **Api location** empty, **Output location** empty.
5. Create. Azure builds and gives you the `*.azurestaticapps.net` URL.

---

## Option B — Azure Storage static website (also effectively free for this size)

1. Azure Portal → create a **Storage account** (StorageV2, Locally-redundant LRS is fine).
2. In the storage account → **Static website** (left menu) → **Enabled**.
   - **Index document name**: `index.html`
   - **Error document path**: `index.html`
   - Save. It shows a **Primary endpoint** URL like `https://<account>.z13.web.core.windows.net/`.
3. It created a container called **`$web`**. Open it → **Upload** → upload `index.html`.
   (You can skip `staticwebapp.config.json` for this option.)
4. Open the Primary endpoint URL on any device.

---

## Updating the site later
Re-deploy the new `index.html` the same way (re-run `swa deploy`, or re-upload to `$web`). No domain or DNS needed — keep using the free `*.azurestaticapps.net` / `*.web.core.windows.net` URL.

## Notes
- Fonts (Sora, Inter) load from Google Fonts over the internet; the page still renders with system fonts if offline.
- No backend, cookies, or tracking — it's a static marketing page with a looping in-page demo.
