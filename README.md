# SaveGuard Pro

**Smart auto-save and backup plugin for Adobe Photoshop**

SaveGuard Pro protects your work by automatically saving and backing up your Photoshop documents based on customizable settings and intelligent activity detection. Never lose progress again.

> Requires Photoshop 23.3.0 or later

---

## Features

- **AutoSave** — automatically saves your document at a configurable interval
- **Smart Delay** — dynamically adjusts the save interval based on how long saves actually take, keeping Photoshop responsive
- **Cohesion Delay** — waits for you to pause editing before triggering a save, avoiding interruptions mid-stroke
- **AutoBackup** — creates timestamped backup copies of your document at a separate interval
- **Multiple formats** — save and back up as PSD, PSB, PNG, or JPG (with quality control)
- **Custom filenames** — optionally save to a custom filename instead of overwriting the original
- **Ignored Operations** — define lists of Photoshop history operations that should not trigger a save or backup
- **Per-document settings** — each open document has its own independent configuration
- **Retry logic** — automatically retries failed saves up to 3 times before stopping
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
| Smart Delay | On | — | Automatically raises the interval if saves are taking too long |
| Wait after edit (s) | 2 s | 1 s – 300 s | How long to wait after the last edit before saving |
| Format | Overwrite | Overwrite / PSD / PSB / PNG / JPG | Format used for auto-save |
| Custom filename | Off | — | Save to a custom filename instead of overwriting |

### Smart Delay

When Smart Delay is enabled, SaveGuard monitors how long each save actually takes and automatically adjusts the interval so that saving never consumes more than ~25% of the interval. This keeps Photoshop responsive on large files.

- Minimum smart interval: 2 s
- Maximum smart interval: 20 s (overridden by observed save duration up to 2 min)

---

## AutoBackup

| Setting | Default | Range | Description |
|---|---|---|---|
| Save every (min) | 15 min | 5 min – 120 min | How often a new backup copy is created |
| Wait after edit (s) | 2 s | 1 s – 300 s | Waits for editing to pause before creating a backup |
| Format | PSD | PSD / PSB / PNG / JPG | Format used for backup files |
| Backup folder | Default | — | Where backup files are stored |
| Max copies (default folder) | 15 | 1 – 15 | Oldest backups are deleted when limit is exceeded |
| Storage limit (default folder) | 5 GB | — | Oldest backups are deleted when folder exceeds this size |
| Max copies (custom folder) | 20 | 1 – 20 | — |
| Storage limit (custom folder) | 8 GB | — | — |

---

## Ignored Operations

You can define lists of Photoshop history state names that SaveGuard should ignore when deciding whether to save. For example, operations like "Zoom" or "Scroll" don't change the document content — adding them to an ignored list prevents unnecessary saves.

- Create multiple named lists and switch between them
- Assign separate lists to AutoSave and AutoBackup independently
- Lists are stored as JSON files on disk and can be shared between machines

---

## Supported File Formats

### Full save support (overwrite)
PSD, PSDT, PDD, PSB, TIF/TIFF, PNG, JPG/JPEG, PDF/PDP

### Formats that need to be flatten before save
BMP, TGA, EPS, AVIF, JXL, PCX, and others

### Formats that cannot be auto-saved
WEBP, RAW, DCM, DDS, KTX/KTX2, MPO

---

## Privacy

SaveGuard Pro does not collect, transmit, or store any data outside your local machine. All operations are performed locally. Settings are saved to a `pluginSettings.json` file in the plugin's local storage.

---

## Support

- **Email:** xpewix@gmail.com
- **Issues & Questions:** [GitHub Issues](https://github.com/pewi21/SaveGuard-Pro/issues)

---

## License

© Paweł Prygiel. All rights reserved. This plugin is distributed through Adobe Exchange.
