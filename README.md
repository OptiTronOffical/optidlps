
# PS5 Package Store

A sleek, standalone web library for browsing a PS5 package catalog — games, DLC, updates, and more. Drop a JSON file next to the HTML, open it in a browser, and browse. No build step, no dependencies, no server required beyond a static file host.

![No build](https://img.shields.io/badge/build-none-brightgreen)
![Single file](https://img.shields.io/badge/HTML-single%20file-blue)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

---

## ✨ Features

- **Fast, searchable catalog** — filter by title, title ID, or category (game, DLC, update, app, theme, demo).
- **Sleek store-style UI** — dark theme, poster cards, responsive layout, keyboard-friendly.
- **Detail modal per title** — cover art, release notes, region, contributor, and every download link in one place.
- **Password manager built in** — all DLPSGAME archive passwords and link-lock passwords in one modal, with one-tap copy.
- **Pippo link-lock detection** — links through Pippo are auto-flagged with the password hint right on the row.
- **Drag-and-drop JSON** — no server? Drop a catalog file onto the page to load it instantly.
- **Mobile-ready** — works on phones, tablets, and desktops.

---

## 📁 Project structure

```
.
├── index.html      # The entire app (single file)
├── dlps.json       # The catalog (see format below)
└── README.md       # This file
```

The page automatically loads **`dlps.json`** from the root. If it's missing, it falls back to `catalog.json`, `ps5-catalog.json`, then `games.json`.

---

## 🚀 Quick start

### 1. Clone or download

```bash
git clone https://github.com/your-username/ps5-package-store.git
cd ps5-package-store
```

### 2. Add your catalog

Place your `dlps.json` file in the project root. See the [Catalog format](#-catalog-format) section below.

### 3. Serve it

Browsers block `file://` requests for local JSON, so serve over HTTP:

```bash
# Python 3
python3 -m http.server 8080

# or Node
npx serve .
```

Then open <http://localhost:8080>.

### Alternative — no server

Just open `index.html` directly in your browser and **drag and drop** your JSON catalog onto the page.

---

## 📦 Catalog format

The catalog is a single JSON file with a `packages` array. Each entry becomes one card in the library.

```json
{
  "name": "DLPSGame",
  "version": 1,
  "packages": [
    {
      "titleId": "PPSA18653",
      "title": "Age of Mythology Retold (DLC)",
      "downloadLinks": [
        {
          "name": "Gofile",
          "url": "https://gofile.io/d/v6eAlalp"
        }
      ],
      "version": "DLC",
      "category": "dlc",
      "posterUrl": "https://cdn.example.com/icon0.webp",
      "description": "Region: USA\nRelease note: Working 13.xx to 10.xx\nContributor: @username",
      "downloadSource": "https://example.com/page/"
    }
  ]
}
```

### Field reference

| Field | Required | Description |
|---|---|---|
| `titleId` | ✅ | PS5 title ID (e.g. `PPSA18653`). Used for search. |
| `title` | ✅ | Display name shown on the card. |
| `downloadLinks` | ✅ | Array of `{ name, url }` objects. Each becomes a row in the detail modal. |
| `category` | ➖ | `game`, `dlc`, `update`, `app`, `theme`, or `demo`. Defaults to `other`. |
| `version` | ➖ | Shown as a badge (e.g. `DLC`, `1.02`). |
| `posterUrl` | ➖ | Cover art. Falls back to a letter placeholder if missing. |
| `description` | ➖ | Parsed line-by-line — supports `Region:`, `Release note:`, `Password:`, `Contributor:` keys. |
| `downloadSource` | ➖ | Adds a **Source page** button to the detail modal. |

### Parsed description fields

The `description` field is split on newlines and matched against known keys:

```
Region: USA
Release note: Working 13.xx to 10.xx Unlocked All DLC
Password: DLPSGAME.COM
Contributor: @High-Speed007
```

Any other `Key: value` pair is shown as-is in the release details grid.

---

## 🔐 Passwords

This library indexes catalogs from **DLPSGAME** sources, so several archive passwords apply. They are always available via the **Passwords** button in the topbar, the footer, or inside any title's detail view.

### DLPSGAME archive passwords

| Password |
|---|
| `DLPSGAME.COM` |
| `downloadgameps3.com` |
| `hako` |
| `[DLPSGAME.COM]` |

### Pippo link-lock password

Some links go through a **Pippo link-lock** page. When one does, the password is:

```
pippo
```

> ⚠️ **It must be entered in lowercase.** Uppercase variants like `Pippo`, `PIPPO`, or `Pippo` will be rejected.

The store automatically detects Pippo hosts in your catalog and shows a pink **🔒 pw: pippo (lowercase)** hint on those link rows, plus a callout at the top of the detail modal.

---

## 🌐 Deployment

### Vercel (recommended)

1. Push the repo to GitHub.
2. Import it in Vercel — no build command needed, output is the root.
3. Add your `dlps.json` to the repo so it deploys with the site.

### Netlify / Cloudflare Pages

Same idea — connect the repo, set the publish directory to the root, and drop your `dlps.json` in.

### Self-hosted

Any static file server works:

```bash
python3 -m http.server 8080
# or
caddy file-server --listen :8080
# or
nginx (point root at the project folder)
```

---

## 💡 Tips

- **Keyboard shortcut** — press `/` anywhere to focus the search bar.
- **Copy all links** — every detail modal has a **Copy all links** button for batch pasting into a download manager.
- **Copy individual links** — the 📋 icon next to each link copies just that URL.
- **Deep link to a catalog** — append `?catalog=myfile.json` to the URL to load a different catalog without editing the page.

---

## 🛠 Customization

All the important configuration lives at the top of the `<script>` block in `index.html`:

```js
var DEFAULT_SOURCES = ['dlps.json', 'catalog.json', 'ps5-catalog.json', 'games.json'];
var ETH_ADDRESS = '0x17E1D7f8A9641749A3f6A932Df09D36FE198df86';
var TG_URL = 'https://t.me/OptiTronOffical';

var PASSWORDS_ARCHIVE = ['DLPSGAME.COM', 'downloadgameps3.com', 'hako', '[DLPSGAME.COM]'];
var PASSWORDS_LOCK = ['pippo'];

var PIPPO_HOSTS = ['pippo', 'pippolink', 'pippo.link', /* ... */];
```

Change the Telegram URL, Ethereum address, catalog filenames, or passwords in one place — everything else updates automatically.

---

## 🙋 Support & custom projects

For any **issues**, **questions**, or to request a **custom project**:

👉 **Message [@OptiTronOffical on Telegram](https://t.me/OptiTronOffical)**

---

## ☕ Donate

If this saved you time, consider buying me a coffee:

**ETH:** `0x17E1D7f8A9641749A3f6A932Df09D36FE198df86`

Accepts ETH and any ERC-20 token. Double-check the address before sending — crypto transfers are irreversible.

---

## ⚖️ Disclaimer

This is an **unofficial fan library** and a **standalone catalog viewer**. It does not host, host-links, or distribute any game files. It only displays metadata and links that are provided in a user-supplied JSON catalog.

All trademarks, game titles, and cover art belong to their respective owners. Use at your own discretion and in accordance with your local laws.

---

## 📄 License

MIT — do whatever you want, no warranty.

```

### Where to place it

Save it as `README.md` in the root of your repo, right next to `index.html` and `dlps.json`. GitHub, GitLab, and most Git hosts will render it automatically on the repo homepage.
