# Collibra Dashboard (Viewer + Admin)

A lightweight, client-side dashboard system for Collibra that lets you:
- Render multiple custom HTML widgets in a responsive grid
- Configure layout and widgets from an admin page
- Persist configuration in browser localStorage
- Export/import config JSON between users or environments

This folder contains two pages that work together:
- `dashboards.html` - end-user dashboard viewer
- `dashboards-admin.html` - admin/configuration interface

Both pages share the same stylesheet:
- `style.css`

## Features

- Responsive widget grid with configurable:
  - Column count
  - Row height
  - Gap size
- Widget controls:
  - Reload iframe
  - Open in new tab
  - Maximize in overlay
- Theme modes:
  - Light
  - Dark
  - System
- Greeting in top bar, with best-effort user profile fetch from Collibra
- Local-only persistence (no server component required)
- Config import/export for portability

## How It Works

The app stores dashboard settings in localStorage under:
- `collibra-dashboard-config`

The dashboard builds widget URLs with:
- Base URL + `/resources/images/` + widget path

By default, base URL is current origin (`window.location.origin`) unless overridden in admin settings.

## Project Files

- `dashboards.html`
  - Renders the dashboard grid
  - Loads saved config
  - Fetches current session user to personalize greeting
- `dashboards-admin.html`
  - Add/edit/delete/reorder widgets
  - Configure grid and base URL
  - Import/export/reset config
- `style.css`
  - Shared theme tokens and UI styling for both pages

## Collibra Endpoints Used

Dashboard page uses:
- `GET /rest/2.0/auth/sessions/current?include=user`
  - Used to read user info for greeting/session display

Widget iframes resolve to:
- `/resources/images/<your-widget-path>`

## Configuration Shape

Example config stored in localStorage:

```json
{
  "version": 1,
  "theme": "system",
  "baseUrl": "",
  "gridColumns": 12,
  "rowHeight": 180,
  "gap": 20,
  "widgets": [
    {
      "id": "widget-1",
      "title": "Data Quality",
      "path": "widgets/dq.html",
      "colSpan": 6,
      "rowSpan": 2,
      "order": 0,
      "enabled": true,
      "showHeader": true
    }
  ]
}
```

## Local Development

1. Serve this folder from any static web server.
2. Open `dashboards-admin.html` and configure widgets.
3. Open `dashboards.html` to view the result.

Notes:
- Running from `file://` is not recommended for realistic iframe/API behavior.
- For Collibra API requests, use a valid session in the same browser context.

## Publishing to GitHub

Recommended structure in your repository:

- `uberdash/dashboards.html`
- `uberdash/dashboards-admin.html`
- `uberdash/style.css`
- `uberdash/README.md`

If you publish with GitHub Pages:
- Static UI will load as expected.
- Collibra API calls will only work when served in an environment that has access/authentication to your Collibra instance.

## Deploying in Collibra

1. Upload this folder's assets to your Collibra images/resources location.
2. Open `dashboards-admin.html` and set widget paths (relative to `/resources/images/`).
3. Save widgets and layout.
4. Open `dashboards.html` for consumer view.

## Security Notes

- Widget paths are loaded into iframes. Only reference trusted internal HTML resources.
- Keep admin access restricted if this is exposed to broad audiences.

## Browser Support

Modern Chromium, Firefox, and Safari versions are recommended.

## License

MIT (or your preferred project license)
