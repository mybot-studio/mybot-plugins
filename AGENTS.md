# AGENTS.md — MyBot Community Plugins

Rules for AI agents (and humans) contributing to this plugins index. When a rule here and the engine disagree, the engine wins: the discovery contract is `discover_installed_plugins()` in `mybot-studio/mybot-studio` (`backend/app/api/plugins.py`), and the i18n contract is its `AGENTS.md` + `scripts/check_i18n.py`.

## 1. The manifest is `manifest.json` — the one rule that matters

The engine's plugin scanner reads a file named **`manifest.json`** at the root of each plugin folder, and it only understands this field set:

| Field | Required | Notes |
| --- | --- | --- |
| `key` | yes | snake_case identifier; falls back to the folder name if absent. |
| `name` | no | English display name. |
| `name_fa` | no | Persian display name. |
| `description` | no | English. |
| `description_fa` | no | Persian. |
| `version` | no | Defaults to `1.0.0`. |
| `author` | no | Defaults to `MyBot Community`. |
| `type` | no | `toolkit` (default) or `admin`. |
| `icon` | no | Lucide icon name; defaults to `Puzzle`. |

Anything else — `plugin.json`, `id`, `category`, `entrypoint`, `type: "canvas_node"` — is **dead metadata the panel never reads**. A plugin authored against that schema will not appear in the panel's Plugins list. If you add a field to this table, first confirm `discover_installed_plugins()` actually reads it; do not invent schema.

## 2. No hardcoded language

A plugin that ships user-facing copy ships it in **both** `name`/`description` (English) and `name_fa`/`description_fa` (Persian) in the manifest. The engine renders the pair against the panel's language; an empty `_fa` field is the only "not translated yet" state, and a missing `name_fa` falls back to `name` — so both keys should exist for real copy.

- Do not hardcode a language code anywhere in plugin code: read it from the request / engine context the way the engine does.
- Node labels, tooltips and emitted message text are user-visible copy. If the plugin renders text, it goes through the engine's translation path (`t(key, language_from_request(request))`) with keys in the plugin's own locale pack, not as string literals in four languages inline.
- A plugin that speaks to a *language model* (prompt text, tool definitions) follows the engine's "Prompt language" rule: those literals are model instructions, not UI copy, and are not translated.

## 3. Layout

```
plugins/<category>/<key>/
├── manifest.json        # required, the schema above
├── node.py / plugin.py  # backend logic (importable by the engine)
├── locales/             # optional, plugin-owned locale pack
└── README.md            # one-paragraph what + how-to-configure
```

`categories.json` is the single source of truth for category ids; a folder's category directory must match an id there.

## 4. Security invariants (same as the engine)

- **No outbound URLs without the SSRF guard.** Anything that fetches a remote endpoint must go through the engine's `validate_outbound_url` (scheme, host, private/loopback IP, `ALLOW_PRIVATE_URLS`), not raw `httpx`/`requests` to user-supplied hosts.
- **No secrets in the manifest or repository.** API keys are operator-side configuration; a manifest containing a real token, private key or `.env` value is rejected.
- **No telemetry, no obfuscation, no remote-executable bytecode**, no token exfiltration, no backdoors.
- **Permissions are declared, not assumed.** A plugin that touches a user's database, balance, or Telegram session says so in `manifest.json` description and its README, and keeps that surface as small as possible.

## 5. Quality bar

- Every plugin folder ships a `README.md`: what it does, how to configure it, and the engine version it was built against.
- Node plugins are validated the way the engine validates flows: unknown node types, dangling edges, and over-length `callback_data` are authoring bugs, not features.
- Non-commercial only: commercial or proprietary extensions live in the author's own repository, not in this index.
- PRs must be reviewable in one sitting: one plugin per PR, no lockfile churn, no generated artifacts.
