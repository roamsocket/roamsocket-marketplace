# RoamSocket Marketplace

**Official** catalog for [RoamSocket](https://github.com/roamsocket/roamsocket-mobile):
connectors, skills, plugins, and on-device Metal (MLX) models.

Clients (iOS + desktop) fetch this repo’s `catalog.json`, cache it, and fall
back to bundled defaults when offline. **Anyone can also add their own
marketplace repos** in RoamSocket → Settings → Marketplace.

## Live catalog URL

```
https://raw.githubusercontent.com/roamsocket-ai/roamsocket-marketplace/main/catalog.json
```

Edit `catalog.json` on `main` and push — no App Store / desktop release required
for catalog-only updates.

---

## What’s in `catalog.json`

| Section | Controls |
|---------|----------|
| **connectors** | Composer + Connectors UI names, ids, availability |
| **skills** | Browse / featured skill listings |
| **plugins** | Skill bundles + product-area packs |
| **pluginCategories** | Composer Plugins flyout labels |
| **metalModels** | Recommended Metal / MLX hub ids (iPhone + Mac) |

---

## How to make your own marketplace

### 1. Create a GitHub repo

Any name works (e.g. `my-roamsocket-marketplace`). Public repos need no token.
Private repos need a PAT with `contents:read` for client fetches.

### 2. Add `catalog.json`

Recommended layouts:

**Root (same as this official repo):**

```
my-marketplace/
├── README.md
└── catalog.json
```

**Nested (also supported):**

```
my-marketplace/
└── marketplace/
    └── catalog.json
```

### 3. Minimal valid catalog

```json
{
  "schemaVersion": 1,
  "updatedAt": "2026-08-07",
  "name": "My team marketplace",
  "description": "Internal connectors and Metal picks.",
  "connectors": [],
  "skills": [],
  "plugins": [],
  "pluginCategories": [],
  "metalModels": []
}
```

Empty arrays are fine. Clients ignore unknown fields.

#### Connector

```json
{
  "id": "notion",
  "name": "Notion",
  "description": "Search Notion workspaces.",
  "icon": "doc.text",
  "available": true,
  "category": "productivity"
}
```

- `id` — stable (composer toggles key off this)
- `available: false` — show greyed / off by default

#### Skill listing

```json
{
  "id": "pr-review",
  "name": "pr-review",
  "description": "Review pull requests with a fixed checklist.",
  "category": "engineering",
  "author": "Your team",
  "source": "community",
  "featured": true
}
```

Marketplace skills control **discovery**. Full `SKILL.md` bodies still live in
the user’s skills repo or desktop skills store.

#### Plugin

```json
{
  "id": "eng-pack",
  "name": "Engineering pack",
  "description": "PR review helpers.",
  "category": "engineering",
  "skillIds": ["pr-review", "mcp-builder"],
  "featured": true
}
```

#### Metal model

```json
{
  "hubID": "lmstudio-community/Qwen3-1.7B-MLX-4bit",
  "displayName": "Qwen 3 1.7B",
  "approxSize": "~1.0 GB",
  "blurb": "Balanced on-device chat.",
  "tags": ["recommended", "new"],
  "platforms": ["ios", "desktop"]
}
```

| Tag | UI |
|-----|-----|
| `recommended` / `best` | Featured |
| `thinking` | Thinking chip |
| `vision` | Vision (desktop chat drops vision primaries) |
| `new` | New chip |
| `experimental` | Experimental |
| `legacy` | Legacy |

Prefer 4-bit MLX hubs. Keep phone picks roughly ≤ 4B unless marked `legacy` /
`experimental`. Omit `platforms` (or include both) for iOS + desktop.

### 4. Publish and add in the app

1. Commit + push to the branch clients track (usually `main`).
2. Copy the **raw** catalog URL, e.g.  
   `https://raw.githubusercontent.com/<owner>/<repo>/main/catalog.json`
3. RoamSocket → **Settings → Marketplace → Add marketplace**
4. Tap **Refresh**

You can enable **multiple** marketplaces. Order: official default first, then
user sources. Same connector `id` or Metal `hubID` → later enabled source wins.

### 5. Validate

```bash
node -e "JSON.parse(require('fs').readFileSync('catalog.json','utf8')); console.log('ok')"
```

Local draft:

```bash
npx --yes serve . -p 8799
# add http://127.0.0.1:8799/catalog.json in Settings
```

---

## Related (not the same)

| Mechanism | Who | Purpose |
|-----------|-----|---------|
| **This marketplace** | Project owner + community forks | Public discovery catalog |
| User **skills** repo | End user | Their `SKILL.md` files |
| User **MCP** repo | End user | Their `.mcp.json` connectors |
| RoamSoxket app | Product | Clients that consume catalogs |

Marketplace = what people can **discover**. Skills/MCP repos = what they **own**.

## License

MIT — same spirit as RoamSocket Catalog entries may reference third-party models
(e.g. Hugging Face hubs); those remain under their own licenses.
