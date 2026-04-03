# SaveGuard Pro

**Smart auto-save and backup plugin for Adobe Photoshop**

SaveGuard Pro protects your work by automatically saving and backing up your Photoshop documents based on customizable settings and intelligent activity detection. Never lose progress again.

> Requires Photoshop 23.3.0 or later

---

## Features

- **AutoSave** — automatically saves your document when changes are detected, at a configurable check interval
- **Smart Delay** — dynamically adjusts the check interval based on observed save duration, keeping Photoshop responsive
- **Cohesion Delay** — waits for you to pause editing before triggering a save, avoiding interruptions mid-stroke
- **Interval Load Warning** — alerts you when saves are consuming a large fraction of the check interval (when Smart Delay is off)
- **AutoBackup** — creates timestamped backup copies of your document at a separate interval
- **Multiple formats** — save and back up as PSD, PSB, PNG, or JPG; AutoSave also supports overwriting the original
- **Format options** — per-format settings: JPG quality (1–12), PNG interlacing, PSD/PSB Maximize Compatibility
- **Custom filenames** — optionally save to a custom filename instead of overwriting the original
- **Custom save folder** — optionally direct AutoSave output to a folder other than the document's original location
- **Ignored Operations** — define named lists of Photoshop history state names that suppress saves or backups
- **Per-document settings** — each open document has its own independent configuration
- **Retry logic** — automatically retries failed saves up to 3 times, with per-error-code delays
- **Crash-safe storage** — settings and ignored lists are written atomically with `.tmp`/`.bak` crash recovery
- **Storage limits** — automatically removes oldest backups when a folder exceeds a set size or copy count
- **11 languages** — EN, PL, DE, FR, ES, IT, PT, RU, ZH, JA, KO

---

## Requirements

| | |
|---|---|
| Application | Adobe Photoshop |
| Minimum version | 23.3.0 |
| Platform | Windows, macOS |

---

## Installation

1. Open **Adobe Photoshop**
2. Go to **Plugins → Browse Plugins** (or visit [Adobe Exchange](https://exchange.adobe.com/))
3. Search for **SaveGuard Pro**
4. Click **Get** and follow the prompts
5. After installation, open the panel via **Plugins → SaveGuard Pro**

---

## Quick Start

1. Open a document that has already been saved to disk (File → Save As… at least once)
2. In the SaveGuard Pro panel, set your desired **check interval** (in seconds)
3. Click **AutoSave OFF** to toggle it on — the button turns green
4. Optionally enable **AutoBackup** and select a backup folder

> **Note:** The plugin cannot save documents that have never been saved to disk. Save manually first.

---

## AutoSave

| Setting | Default | Range | Description |
|---|---|---|---|
| Check every (s) | 2 s | 2 s – 3600 s | How often SaveGuard checks for unsaved changes |
| Smart Delay | On | — | Automatically adjusts the interval so saves stay near 25% of the interval duration |
| Wait after edit (s) | 2 s | 1 s – 300 s | How long to wait after the last edit before saving |
| Format | Overwrite | Overwrite / PSD / PSB / PNG / JPG | Format used for auto-save |
| JPG Quality | 10 | 1 – 12 | JPEG compression quality (visible when JPG is selected) |
| PNG Interlaced | Off | — | Enables interlaced PNG encoding (visible when PNG is selected) |
| Max. Compatibility | Off | — | Enables PSD/PSB Maximize Compatibility (visible when PSD or PSB is selected) |
| Custom filename | Off | — | Save to a custom filename instead of overwriting the original |
| Save folder | Document folder | — | Folder where the auto-saved file is written when using a non-overwrite format |

### Smart Delay

When Smart Delay is enabled, SaveGuard monitors how long each save actually takes and automatically adjusts the interval so that saving stays near 25% of the interval. This keeps Photoshop responsive on large files.

- Minimum smart interval: 2 s
- Nominal maximum smart interval: 20 s
- Can exceed 20 s if observed save duration requires it (hard ceiling: 2 min)

### Interval Load Warning

When Smart Delay is **disabled**, SaveGuard monitors save duration relative to your configured interval and shows an in-panel warning:

- **Warning** (orange): save consumes ≥ 40% of the interval
- **Critical** (red): save consumes ≥ 70% of the interval

Warnings can be dismissed individually and reset automatically when the ratio improves or the interval is changed.

### Save Folder

When using a non-overwrite format (PSD, PSB, PNG, JPG), a **Select save folder** button appears in the panel. For the **Overwrite** format on multi-layer PNG or JPG documents (which cannot be directly overwritten), SaveGuard prompts you to choose a save location before enabling.

### Pause State

When SaveGuard is waiting for overwrite confirmation or folder selection, the AutoSave button displays **PAUSED**. A separate **✕** button clears the paused state and disables AutoSave immediately.

---

## AutoBackup

| Setting | Default | Range | Description |
|---|---|---|---|
| Save every (min) | 15 min | 5 min – 120 min | How often a new backup copy is created |
| Wait after edit (s) | 2 s | 1 s – 300 s | Waits for editing to pause before creating a backup |
| Format | PSD | PSD / PSB / PNG / JPG | Format used for backup files |
| JPG Quality | 10 | 1 – 12 | JPEG compression quality (visible when JPG is selected) |
| PNG Interlaced | Off | — | Enables interlaced PNG encoding (visible when PNG is selected) |
| Max. Compatibility | Off | — | Enables PSD/PSB Maximize Compatibility (visible when PSD or PSB is selected) |
| Backup folder | Default | — | Where backup files are stored |
| Max copies (default folder) | 15 | 1 – 99 | Oldest backups are deleted when limit is exceeded |
| Storage limit (default folder) | 5 GB | 1 – 20 GB | Oldest backups are deleted when folder exceeds this size |
| Max copies (custom folder) | 20 | 1 – 99 | — |
| Storage limit (custom folder) | 8 GB | 1 – 20 GB | — |
| Custom filename | Off | — | Save backups under a fixed filename (available when a custom folder is selected) |
| All AutoBackup documents | Off | — | When enabled, storage limits apply across all documents backed up to the same custom folder |

---

## Ignored Operations

You can define named lists of Photoshop history state names that SaveGuard should ignore when deciding whether to save or back up. For example, operations like "Lasso" or "Move Selection" don't change the document content — adding them to an ignored list prevents unnecessary saves.

- Create multiple named lists and switch between them
- Assign separate lists to AutoSave and AutoBackup independently
- Use the **Add to ignore** button to capture the most recent history state name directly from Photoshop
- Lists are stored as JSON files in the plugin data folder and can be shared between machines

---

## Supported File Formats

### Full save support (overwrite)
PSD, PSDT, PDD, PSB, TIF/TIFF, PNG, JPG/JPEG/JPE, PDF/PDP

### Formats that must be flattened before saving
BMP, TGA, EPS, AVIF, JXL, PCX, and others

### Formats that cannot be auto-saved
WEBP, RAW, DCM, DDS, KTX/KTX2, MPO

---

## Privacy

SaveGuard Pro does not collect, transmit, or store any data outside your local machine. All operations are performed locally. Settings are saved to a `pluginSettings.json` file in the plugin's local data folder.

---

## Support

- **Email:** xpewix@gmail.com
- **Issues & Questions:** [GitHub Issues](https://github.com/pewi21/SaveGuard-Pro/issues)

---

## License

© Paweł Prygiel. All rights reserved. This plugin is distributed exclusively through Adobe Exchange.
