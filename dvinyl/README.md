# DVinyl on Umbrel

This is a minimal Umbrel Community App Store containing one app: **DVinyl**.

## What's in here

```
dvinyl-umbrel-store/
├── umbrel-app-store.yml      # Identifies this repo as a Community App Store
└── dvinyl/
    ├── umbrel-app.yml        # App manifest (name, tagline, icon links, port)
    └── docker-compose.yml    # Umbrel-flavored compose file (app_proxy + DVinyl + Mongo)
```

## 1. Put this on GitHub

Create a new **public** GitHub repo (e.g. `your-username/umbrel-dvinyl-store`) and push
this folder to it as the repo root — `umbrel-app-store.yml` needs to sit at the top level.

```bash
cd dvinyl-umbrel-store
git init
git add .
git commit -m "DVinyl for Umbrel"
git branch -M main
git remote add origin https://github.com/<your-username>/umbrel-dvinyl-store.git
git push -u origin main
```

## 2. Add your Discogs token (optional but recommended)

Open `dvinyl/docker-compose.yml` and fill in:

```yaml
DISCOGS_TOKEN: "your-token-here"
```

Get a token from Discogs → Settings → Developers → Generate new token. Without it, DVinyl
still works for manual entry, but you lose one-click metadata/cover-art lookup and Discogs
collection import. Commit and push the change before installing (or edit-and-reinstall
later if you'd rather add it after trying the app).

## 3. Add the store to Umbrel

On your Umbrel dashboard: **App Store → ⋮ menu → Community App Stores → Add**, then paste:

```
https://github.com/<your-username>/umbrel-dvinyl-store
```

DVinyl will now appear as an installable app tile. Install it like any other app —
Umbrel handles the port, reverse proxy, and data directory automatically
(`${APP_DATA_DIR}` maps to a persistent folder under `umbrel/app-data/dvinyl-store-dvinyl/`).

## Notes

- `PASSJWT` and `SESSION_SECRET` are auto-generated per-install by Umbrel's `$APP_PASSWORD`
  and `$APP_SEED` variables — you don't need to set these yourself.
- Mongo is pinned to `4.4.18` rather than `latest` since newer Mongo builds can crash-loop
  on ARM devices (Raspberry Pi, Umbrel Home's ARM units). If you're running an x86 Umbrel
  Pro/Home, you can safely change the `mongodb` image to `mongo:latest`.
- Uploaded cover images and the Mongo database both persist under Umbrel's per-app data
  directory, so they survive app updates/restarts.
- To update DVinyl later, bump the `version` and any changed fields in `umbrel-app.yml`,
  commit, and push — umbrelOS will show an update prompt.
