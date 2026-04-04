# SaveGuard Pro

**Smart auto-save and backup plugin for Adobe Photoshop**

SaveGuard Pro protects your work by automatically saving and backing up your Photoshop documents based on customizable settings and intelligent activity detection. Never lose progress again.

> Requires Photoshop 23.3.0 or later

---

## Features

- **AutoSave** — automatically saves your document when changes are detected, at a configurable check interval
- **Smart Delay** — dynamically adjusts the check interval based on observed save duration, keeping Photoshop responsive
- **Cohesion Delay** — Waits for a pause in editing activity before triggering a save.
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

### Primary use case

AutoSave is designed for workflows where you need changes in Photoshop to appear in another application as quickly as possible — for example, editing a texture that a 3D viewport or a web browser reloads automatically, or updating a source file that a design tool watches live. It creates a near-live bridge between Photoshop and external tools without manual saves.

> **AutoSave is not recommended for large or complex documents** (high resolution, many Smart Objects, many layers). These documents can take several seconds to save, and with a short check interval this can noticeably reduce editing comfort. If you work with such files, enable **Smart Delay** (on by default) — it automatically raises the interval to keep save time below ~25% of the cycle — or increase the interval manually. For protecting large documents against data loss, **AutoBackup** is the better tool.

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

Smart Delay's primary role is to enforce a **minimum safe interval floor** — it raises the interval when saves are taking too long, but never lowers it below what you have set. You can always increase the interval yourself at any time; Smart Delay will not override a manually set value that is already high enough.

When Smart Delay is enabled, SaveGuard monitors how long each save actually takes and automatically adjusts the interval so that saving stays near 25% of the interval. This keeps Photoshop responsive on large files.

- Smart Delay only raises the interval, never lowers it below your manually configured value
- Minimum smart interval: 2 s
- Nominal maximum smart interval: 20 s
- Can exceed 20 s if observed save duration requires it (hard ceiling: 2 min)

### Interval Load Warning

When Smart Delay is **disabled**, SaveGuard monitors save duration relative to your configured interval and shows an in-panel warning:

- **Warning** (orange): save consumes ≥ 40% of the interval
- **Critical** (red): save consumes ≥ 70% of the interval

Warnings can be dismissed individually and reset automatically when the ratio improves or the interval is changed.

### Format Change Stops AutoSave

Changing the Format setting while AutoSave is running stops it immediately and resets it to OFF. This is intentional: each format may require a different save destination, a different change-detection mode, or new overwrite confirmation. Continuing to save automatically mid-change could write to the wrong path or skip required setup steps. Re-enable AutoSave after selecting the new format — the plugin will guide you through any required folder selection or overwrite confirmation before it starts.

### Save Folder

When using a non-overwrite format (PSD, PSB, PNG, JPG), a **Select save folder** button appears in the panel. For the **Overwrite** format on multi-layer PNG or JPG documents (which cannot be directly overwritten), SaveGuard prompts you to choose a save location before enabling.

### Pause State

When SaveGuard is waiting for overwrite confirmation or folder selection, the AutoSave button displays **PAUSED**. A separate **✕** button clears the paused state and disables AutoSave immediately.

### Change Detection Modes

SaveGuard uses two different methods to decide whether a save is needed, depending on the format and document type:

**Direct save mode** — active when Format is set to **Overwrite** and the document is not a multi-layer PNG or JPG:
- SaveGuard checks Photoshop's native unsaved-changes flag (`doc.saved`)
- A save is triggered as soon as Photoshop reports the document has unsaved changes
- The Ignored Operations list is **not** used in this mode
- Works with any document format that is not in the blocked list, provided the layer requirements are met: PSD, PSB, TIF/TIFF, PDF/PDP, and others save regardless of layer count; PNG/JPG save only when the document has a single layer; BMP, TGA, EPS, AVIF, JXL, PCX, and similar formats also save when the document has a single layer (multi-layer documents in these formats require manual flattening first)

**History-tracking mode** — active when Format is set to PSD / PSB / PNG / JPG, OR when Format is Overwrite but the document is a multi-layer PNG or JPG:
- SaveGuard tracks Photoshop history states to detect changes
- The Ignored Operations list is respected — history states matching the list are not counted as changes
- SaveGuard scans up to 50 history states per check cycle (scan timeout: 5 s)
- When a change is detected, the Cohesion Delay runs before the save is triggered

**Multi-layer PNG / JPG special case:** When Format is **Overwrite** and the document is a PNG or JPG with more than one layer, SaveGuard automatically uses history-tracking mode and performs a Save As (with flattening) to the original folder. This happens transparently — you do not need to change the format setting. The plugin asks you to confirm the destination folder once before it begins.

### Document Switch Delay

After switching between open documents in Photoshop, SaveGuard suppresses saves briefly to prevent an immediate save on the newly activated document before you have time to act:

- **AutoSave:** suppressed for 5 seconds after a document switch
- **AutoBackup:** suppressed for 3 seconds after a document switch

This prevents lag spikes when rapidly cycling through open documents.

---

## AutoBackup

| Setting | Default | Range | Description |
|---|---|---|---|
| Save every (min) | 15 min | 5 min – 120 min | How often a new backup copy is created |
| Wait after edit (s) | 2 s | 1 s – 300 s | Waits for editing to pause before creating a backup. If editing is continuous for 60 seconds without any pause, the backup is triggered immediately regardless of this setting |
| Format | PSD | PSD / PSB / PNG / JPG | Format used for backup files |
| JPG Quality | 10 | 1 – 12 | JPEG compression quality (visible when JPG is selected) |
| PNG Interlaced | Off | — | Enables interlaced PNG encoding (visible when PNG is selected) |
| Max. Compatibility | Off | — | Enables PSD/PSB Maximize Compatibility (visible when PSD or PSB is selected) |
| Backup folder | Default | — | Where backup files are stored |
| Max copies (default folder) | 15 | 1 – 99 | Oldest backups are deleted when limit is exceeded. **Global value — shared across all documents.** |
| Storage limit (default folder) | 5 GB | 1 – 20 GB | Oldest backups are deleted when folder exceeds this size. **Global value — shared across all documents.** |
| Max copies (custom folder) | 20 | 1 – 99 | — |
| Storage limit (custom folder) | 8 GB | 1 – 20 GB | — |
| Custom filename | Off | — | Save backups under a fixed filename (available when a custom folder is selected) |
| All AutoBackup documents | Off | — | When enabled, storage limits apply across all documents backed up to the same custom folder |

> **Max copies** and **Storage limit** for the **default folder** are global settings — they apply to all open documents simultaneously. Changing the value in one document's panel immediately updates all other documents as well. The values are stored in a single shared settings file and persist across Photoshop sessions.
>
> The corresponding limits for a **custom folder** are per-document and independent.

---

## Ignored Operations

You can define named lists of Photoshop history state names that SaveGuard should ignore when deciding whether to save or back up. For example, operations like "Lasso" or "Move Selection" don't change the document content — adding them to an ignored list prevents unnecessary saves.

> **The Ignored Operations section is only visible when the current document and format combination uses history-tracking mode.** When AutoSave is in direct save mode (Format = Overwrite on a non-PNG/JPG document, or a single-layer PNG/JPG), the section is automatically hidden because it has no effect. It reappears as soon as you switch to a format or document type where history tracking is active.

- Create multiple named lists and switch between them
- Assign separate lists to AutoSave and AutoBackup independently
- Use the **Add to ignore** button to capture the most recent history state name directly from Photoshop
- Lists are stored as JSON files in the plugin data folder and can be shared between machines

---

## Supported File Formats

### Full overwrite support
PSD, PSDT, PDD, PSB, TIF/TIFF, PDF/PDP

These formats support direct (in-place) saving regardless of layer count.

### Overwrite support — single-layer documents only
PNG, JPG/JPEG/JPE

When Format is set to **Overwrite** and the document has exactly one layer, SaveGuard saves in place. When the document has multiple layers, SaveGuard automatically switches to Save As (with flattening) to the same folder — the format setting does not need to change.

### Formats that must be flattened manually before saving
BMP, RLE, DIB, TGA, VDA/ICB/VST, EPS, IFF/TDI, AVIF, JXL, PCX, PXR, JP2/JPX/JPC/J2K, JPS, SCT, PBM/PGM/PPM/PNM/PFM/PAM

These formats do not support layers. SaveGuard will warn you and stop if the document has more than one layer. Flatten the document manually in Photoshop first, then re-enable AutoSave.

### Formats that cannot be auto-saved
WEBP, RAW, DCM/DC3/DIC, DDS, KTX/KTX2, MPO

These formats are blocked due to Photoshop API limitations — saving them would open an interactive dialog that cannot be automated.

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
