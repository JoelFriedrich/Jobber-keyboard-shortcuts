# Jobber Shortcuts

A Chrome extension that brings keyboard shortcuts to [Jobber](https://getjobber.com) — so you can save records, navigate the schedule, and create clients without reaching for your mouse.

![Jobber Shortcuts popup](screenshots/popup.png)

---

## Shortcuts

### Everywhere on Jobber

| Shortcut | Action |
|---|---|
| `⌘ ⇧ S` | Save the current record |
| `⌘ ⇧ E` | Edit the current record or modal |
| `⌘ ⇧ O` | Click the primary action button |
| `⌘ ⇧ C` | Quick create a new client |
| `⌘ ⇧ R` | Quick create a new request |

### Schedule page only (`/schedule`)

| Shortcut | Action |
|---|---|
| `⌘ ⇧ M` | Switch to month view |
| `⌘ ⇧ L` | Switch to week view |
| `⌘ ⇧ D` | Switch to day view |
| `⌘ ⇧ U` | Toggle unscheduled appointments |
| `⌘ ⇧ K` | Toggle map view |

> On Windows/Linux, replace `⌘` with `Ctrl`.

---

## Installation

### From the Chrome Web Store
*(Coming soon)*

### Manual install (Developer mode)

1. Download or clone this repository
2. Open Chrome and navigate to `chrome://extensions`
3. Enable **Developer mode** (top right toggle)
4. Click **Load unpacked** and select the `jobber-shortcuts` folder
5. Navigate to `secure.getjobber.com` — shortcuts are active immediately

---

## How it works

All shortcuts are handled via a `keydown` listener scoped exclusively to `secure.getjobber.com`. Nothing runs outside of Jobber.

**DOM targeting strategy:**
- `Save` and `Edit` use text matching against visible buttons and spans
- `Primary action` targets Jobber's primary button fingerprint (`amVSJ50CiOo- + TrzCxs3OEpM-`), with a safety exclusion list for destructive actions (delete, archive, void, etc.)
- Schedule view toggles use `input[type="radio"][value="..."]` and `aria-label` selectors — stable regardless of CSS class changes
- `New client` and `New request` navigate directly to Jobber's quick-create URLs

**Popup UI** shows active shortcuts grouped by scope, dims schedule shortcuts when you're not on the schedule page, and shows a green status dot when active on a Jobber tab.

A subtle toast confirms each action so you always know a shortcut fired.

---

## Adding shortcuts

The codebase is designed to make adding shortcuts trivial:

1. Add an entry to `shortcuts.js`
2. Add the key combo to `KEY_MAP` in `content.js`
3. Done — the popup renders from the config automatically

```js
// shortcuts.js
newJob: {
  label: "New job",
  keys: "⌘ ⇧ J",
  keysWin: "Ctrl + Shift + J",
  description: "Quick create a new job",
  action: "navigate",
  target: "https://secure.getjobber.com/jobs/new",
  scope: "global",
},
```

```js
// content.js — KEY_MAP
"meta+shift+j": "newJob",
```

---

## Privacy

Jobber Shortcuts runs entirely in your browser. It reads nothing, sends nothing, and stores nothing. It only activates on `secure.getjobber.com`.

**Permissions used:**
- `activeTab` — to interact with the current Jobber tab
- `scripting` — to inject the content script on page load
- `tabs` — to read the current tab URL for the popup UI

---

## Project structure

```
jobber-shortcuts/
├── manifest.json      # Extension config, permissions
├── shortcuts.js       # All shortcut definitions (single source of truth)
├── content.js         # Keydown listener + DOM action handlers
├── background.js      # Service worker entry point (MV3 requirement)
├── popup.html         # Cheat sheet UI markup
├── popup.js           # Popup rendering logic
└── icons/             # Extension icons (16, 48, 128px)
```

---

## Suggesting a shortcut

Have a workflow you'd like to shortcut? [Send a suggestion](mailto:joelfriedrichdev+jobberextension@gmail.com?subject=Shortcut%20Suggestion&body=Hey%20Joel%2C%20here's%20a%20shortcut%20I'd%20love%20to%20see%3A)

---

## License

MIT
