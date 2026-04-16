# Add Service SOP

This page reads service data from `services.json` and renders cards dynamically in `index.html`.

## 1. Add the service to `services.json`

`services.json` is a top-level array of groups.

Each group looks like:

```json
{
  "title": "HomeLab",
  "services": [
    {
      "name": "Grafana",
      "icon": "./icons/grafana.svg",
      "url": "http://devhome.home-env.nxw.name:6842"
    }
  ]
}
```

Rules:

- `title`: Environment name shown on the page.
- `services`: Array of service entries for that environment.
- `name`: Display name of the service.
- `url`: Full target URL.
- `icon`: Optional local file path under `./icons/`.

Notes:

- `host`, `scheme`, and `endpoint` are not stored in JSON anymore.
- The page derives those values from `url` automatically.
- If `icon` is omitted, the card falls back to a generated monogram.

## 2. Decide whether to add to an existing group or a new group

- If the environment already exists, append the new service object to that group's `services` array.
- If it is a new environment, add a new group object with a new `title`.

Example new service:

```json
{
  "name": "ExampleApp",
  "url": "https://example.home-env.nxw.name:1234"
}
```

## 3. Find an icon

Preferred order:

1. Official repo or official site asset
2. Existing open icon source already used here
3. No icon, leave it blank and use the generated fallback

Good places to check:

- GitHub repo contents
- Repo `public/`, `assets/`, `docs/`, `app/images/`, `web/public/`
- Official site favicon, logo, or SVG assets

Typical asset types:

- `.svg`
- `.png`
- `.ico`

Prefer:

- SVG when available
- Clean square-ish logo
- Non-white-on-transparent assets for this light theme

## 4. Download the icon locally

Store all icons in the repo under `icons/`.

Example:

```bash
curl -fL https://raw.githubusercontent.com/example/project/main/public/logo.svg -o icons/example.svg
```

Then reference it from `services.json`:

```json
{
  "name": "ExampleApp",
  "icon": "./icons/example.svg",
  "url": "https://example.home-env.nxw.name:1234"
}
```

## 5. If no good icon exists

- Omit the `icon` field
- Do not add a broken or low-quality asset just to fill the slot

Example:

```json
{
  "name": "InternalTool",
  "url": "http://internal.home-env.nxw.name:9999"
}
```

The page will render a fallback monogram automatically.

## 6. Preview locally

From the repo root:

```bash
python3 -m http.server 8000
```

Open:

```text
http://localhost:8000
```

Check:

- The new card appears in the right environment section
- The icon loads correctly
- The host/protocol/endpoint text derived from `url` looks right
- Mobile layout still looks acceptable

## 7. Commit changes

Typical files changed:

- `services.json`
- `icons/<file>`
- `index.html` only if rendering behavior or styling changes

Example:

```bash
git add services.json icons/example.svg
git commit -s -m "Add ExampleApp service"
git push origin main
```

## 8. Quick checklist

- Service added to the correct group in `services.json`
- URL is correct
- Icon saved locally under `icons/` or intentionally omitted
- Page renders correctly in preview
- Commit includes only intended files
