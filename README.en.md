# MyBot Community Plugins Directory

Welcome to the official open-source plugins catalog for **MyBot Studio** — the self-hosted, visual no-code Telegram bot engine.

This repository is a central index where creators, developers, and the community publish, discover, and maintain modular extensions for MyBot.

> 🌐 **Languages:** [English](README.en.md) · [فارسی](README.fa.md) · [Русский](README.ru.md) · [العربية](README.ar.md)

---

## 🧩 How It Works

Plugins extend the visual MyBot studio canvas with **custom node families**, admin toolkits, payment gateways, analytics, and external API connectors.

A plugin is a self-contained folder under `plugins/<category>/<key>/` containing:

1. `manifest.json` — metadata the panel's plugin scanner reads (see the schema below).
2. Backend Python (a DAG node family or a toolkit the engine can import).
3. Optional dashboard/canvas metadata for the panel.

> ⚠️ **The manifest filename is `manifest.json`, not `plugin.json`.** The engine's
> discovery routine (`discover_installed_plugins()` in `backend/app/api/plugins.py`)
> scans each plugin folder for a `manifest.json` and reads the fields listed below.
> A folder with any other manifest name or the old `id`/`entrypoint`/`category`
> schema will **not** appear in the panel's Plugins list.

### Manifest Schema (`manifest.json`)

```json
{
  "key": "live_weather",
  "name": "Live Weather Provider",
  "name_fa": "ارائهدهنده آبوهوای زنده",
  "description": "Adds a canvas node that fetches real-time weather for the user's location.",
  "description_fa": "افزودن نودی که آبوهوای لحظه‌ای بر اساس موقعیت کاربر را دریافت می‌کند.",
  "version": "1.0.0",
  "author": "YourName",
  "type": "toolkit",
  "icon": "Cloud"
}
```

Field reference (what `discover_installed_plugins()` actually reads):

| Field | Required | Notes |
|---|---|---|
| `key` | yes | Unique snake_case identifier; falls back to the folder name if missing. |
| `name` | no | Display name (English default). |
| `name_fa` | no | Persian display name. |
| `description` | no | English description. |
| `description_fa` | no | Persian description. |
| `version` | no | Defaults to `1.0.0` when absent. |
| `author` | no | Defaults to `MyBot Community`. |
| `type` | no | `toolkit` (default) or `admin`. |
| `icon` | no | Lucide icon name; defaults to `Puzzle`. |

---

## 📁 Folder Structure

Plugins are organized by category (`categories.json` is the single source of truth):

| Folder | Category | Examples |
|---|---|---|
| `plugins/payments/` | Payment gateways & crypto | ZarinPal, TRON, wallet connectors |
| `plugins/analytics/` | Analytics, tracking & dashboards | user counters, event logs |
| `plugins/services/` | External services & APIs | AI, weather, webhooks |
| `plugins/admin/` | Admin, anti-spam & moderation | user bans, channel subscriptions, admin alerts |
| `plugins/messengers/` | Messengers & tickets | live support, cross-platform bridges |
| `plugins/community/` | Games & utilities | calculators, fun nodes |

---

## 🤝 Contribution & Maintenance Rules

To keep the ecosystem reliable and transparent:

1. **Pull Request Submissions**: to add a plugin, create a folder under `plugins/<category>/<key>/` with a valid `manifest.json` and submit a PR with clear documentation and tests.

2. **Open Source & Non-Commercial**: all plugins hosted here are **free and open-source** under GNU AGPL-3.0 (or a compatible OSI license). **Commercial, paid, or proprietary** plugins are **not accepted** in this public index. Authors who wish to distribute commercial extensions or monetize their work must run their own service, platform, or website.

3. **Community Maintenance**: submitting a plugin grants the community permission to bug-fix, preserve, and update it against newer MyBot engine versions if the original author becomes inactive. The core MyBot team does not endorse, guarantee, or take legal liability for third-party services, advertisements, APIs, or content delivered through community plugins.

4. **Security & Quality Standards**:
   - No obfuscated, encrypted, or remote-executable bytecode (beyond permitted server-side APIs).
   - No telemetry, unauthorized data scraping, token exfiltration, or backdoors. Every PR is reviewed by automated security analysis and code review.

5. **No hardcoded language**: a plugin that ships user-facing copy keeps it bilingual (`name`/`description` + `name_fa`/`description_fa`) in the manifest, mirroring the MyBot Studio engine's i18n contract. See `AGENTS.md`.

---

## 📦 Installing a Plugin in MyBot Studio

From your MyBot dashboard:
1. Go to **Plugins** in the sidebar.
2. Search for the plugin title or paste its GitHub repository / folder link.
3. Click **Install / Activate**. The node appears instantly on your canvas.

---

## 📄 License

This repository and its extensions are licensed under the **GNU Affero General Public License v3.0** (AGPL-3.0). See [LICENSE](LICENSE).
