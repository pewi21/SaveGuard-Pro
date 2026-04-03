# SaveGuard Pro — Help & Troubleshooting

## Table of Contents

1. [The plugin panel is not showing](#the-plugin-panel-is-not-showing)
2. [AutoSave is not saving](#autosave-is-not-saving)
3. [AutoSave stopped with a critical error](#autosave-stopped-with-a-critical-error)
4. [AutoSave is slowing down Photoshop](#autosave-is-slowing-down-photoshop)
5. [AutoSave shows an interval load warning](#autosave-shows-an-interval-load-warning)
6. [AutoSave asks me to select a folder](#autosave-asks-me-to-select-a-folder)
7. [AutoBackup is not creating backups](#autobackup-is-not-creating-backups)
8. [Backup folder says "unavailable"](#backup-folder-says-unavailable)
9. [File was saved in the wrong format](#file-was-saved-in-the-wrong-format)
10. [My document has too many layers error](#my-document-has-too-many-layers-error)
11. [PSD file exceeds 2 GB](#psd-file-exceeds-2-gb)
12. [Ignored Operations list not found](#ignored-operations-list-not-found)
13. [Settings are not remembered between sessions](#settings-are-not-remembered-between-sessions)
14. [Error messages reference](#error-messages-reference)
15. [Contact support](#contact-support)

---

## The plugin panel is not showing

**Symptoms:** You installed SaveGuard Pro but cannot find the panel in Photoshop.

**Fix:**
1. Go to **Plugins** in the menu bar
2. Look for **SaveGuard Pro** in the list
3. If not listed, go to **Plugins → Plugin Manager** and confirm the plugin is installed
4. Restart Photoshop if needed

---

## AutoSave is not saving

**Possible causes and fixes:**

### The document has never been saved to disk
SaveGuard Pro requires the document to already exist on disk. The button will show a warning if this is the case.

**Fix:** Go to **File → Save As…** in Photoshop to save the document to disk first, then enable AutoSave.

### AutoSave is waiting for you to stop editing
This is normal behavior. The **Cohesion Delay** setting ("Wait after edit") makes SaveGuard wait until you pause editing before saving. While waiting, the AutoSave button stays green (**AutoSave ON**) and the status area shows **Waiting for Save...**. The save triggers automatically once you stop editing.

**Fix:** If this happens too often, lower the "Wait after edit" value in settings.

### AutoSave button shows PAUSED (orange)
The **PAUSED** state appears when SaveGuard is waiting for your input — either for overwrite confirmation (a file with the same name already exists) or for a save folder to be selected. A **✕** button appears next to the PAUSED button to immediately cancel and disable AutoSave.

**Fix:** Confirm the overwrite, select the requested folder, or press **✕** to cancel.

### The document has no unsaved changes
SaveGuard only saves when there are actual unsaved changes. If nothing has changed since the last save, no save is performed.

### AutoSave interval is very long
The default check interval is 2 seconds, but if you changed it to a high value, saves will be infrequent.

**Fix:** Lower the "Check every (s)" value in AutoSave settings.

---

## AutoSave stopped with a critical error

When AutoSave encounters an unrecoverable error, it stops automatically to prevent data corruption. The panel will display the error message.

**Common causes:**

| Error | Meaning | Fix |
|---|---|---|
| File unavailable or does not exist | The file was moved, renamed, or the drive disconnected | Save the file again manually from Photoshop, then re-enable AutoSave |
| File is empty (0 bytes) | The last save produced a 0-byte file — possible disk issue | Check your disk health; save manually |
| PSD file exceeds 2GB limit | The document is too large for PSD format | Change format to **PSB** in AutoSave settings |
| Document state is corrupted | Photoshop's internal document state became unrecoverable | Save the document manually in Photoshop to restore it, then re-enable AutoSave |
| AutoSave stopped after N consecutive history tracking failures | SaveGuard could not read the document's history states | Restart Photoshop; if it persists, [contact support](#contact-support) |
| Critical error: [message] | An unexpected error occurred | Note the error message and [contact support](#contact-support) |

After fixing the underlying problem, click the AutoSave button to re-enable it.

---

## AutoSave is slowing down Photoshop

If saving a large document takes several seconds, running AutoSave on a short interval can cause Photoshop to feel sluggish.

**Recommended fix: Enable Smart Delay**

Smart Delay is enabled by default. It monitors actual save duration and automatically increases the interval so that saving does not consume more than ~25% of the cycle time. For example, if a save takes 4 seconds, the interval is raised so Photoshop has plenty of time between saves.

If you disabled Smart Delay:
1. Open AutoSave settings
2. Enable **Smart Delay**

Alternatively, manually increase the "Check every (s)" value.

---

## AutoSave shows an interval load warning

Even with Smart Delay disabled, SaveGuard monitors how long saves take and shows an in-panel warning when saves consume too much of your configured interval.

| Color | Threshold | Meaning |
|---|---|---|
| Orange (warning) | ≥ 40% of interval | Save is taking a significant portion of the cycle time |
| Red (critical) | ≥ 70% of interval | Save is taking most of the cycle time; Photoshop will feel sluggish |

**Fix:** Increase the "Check every (s)" value, or enable **Smart Delay** and let SaveGuard manage the interval automatically. You can dismiss the warning individually — it will reset on its own if the ratio improves or if you change the interval.

---

## AutoSave asks me to select a folder

This happens in two situations:

1. **Non-overwrite format selected (PSD, PSB, PNG, JPG):** SaveGuard needs a destination folder for the output file. A **Select save folder** button appears in the AutoSave settings section.

2. **Overwrite format on a multi-layer PNG or JPG document:** These documents cannot be overwritten directly — Photoshop requires a Save As operation. SaveGuard asks you to confirm the original folder before enabling.

**Fix:** Click **Select save folder** (or confirm the prompted folder) to continue. AutoSave will enable automatically once a valid folder is selected.

---

## AutoBackup is not creating backups

### No backup folder is set
If you haven't selected a custom folder, SaveGuard uses a default backup folder. This should work automatically. If there's a problem with the default folder, select a custom one.

**Fix:** Open AutoBackup settings → click **Select backup folder** → choose a folder on your local drive.

### The interval hasn't elapsed yet
AutoBackup has a minimum effective interval of 5 minutes, regardless of what value you set. The first backup is created only after the configured interval has elapsed from when you enabled AutoBackup.

### The document hasn't changed since the last backup
AutoBackup uses Cohesion Delay to wait for editing activity. If no edits were detected, no backup is created.

---

## Backup folder says "unavailable"

This means the previously selected backup folder no longer exists or is not accessible (external drive disconnected, folder deleted, etc.).

**Fix:**
1. Open AutoBackup settings
2. Click **Select backup folder**
3. Choose a new folder
4. Or click **Reset to default** to use the default backup location

---

## File was saved in the wrong format

If AutoSave format is set to **Overwrite**, SaveGuard saves in the document's original format. If you want a specific format, change it in settings.

**Note:** Some formats are not compatible with auto-save:
- **WEBP, RAW, DDS, DCM** and others cannot be saved automatically due to Photoshop API limitations
- Multi-layer documents cannot be saved as **BMP, TGA, EPS**, and similar single-layer formats — SaveGuard will warn you and stop
- **GIF files** with multiple layers cannot be overwritten unless the document is in Indexed Color mode — SaveGuard will warn you and stop

**Fix:** Change the AutoSave format to **PSD** or **PNG** in settings.

---

## My document has too many layers error

**Error:** `[format] files can only have 1 layer. This document has X layers.`

This happens when AutoSave format is set to a format that does not support multiple layers (e.g. BMP, TGA, EPS).

**Fix:** Change the AutoSave format to **PSD**, **PSB**, or **PNG** in settings.

---

## PSD file exceeds 2 GB

**Error:** `PSD file exceeds 2GB limit. Please change format to PSB.`

PSD is limited to 2 GB. Large documents with many layers or high resolution can exceed this.

**Fix:** In AutoSave (or AutoBackup) settings, change the format from **PSD** to **PSB** (Large Document Format). PSB supports files up to 4 EB.

---

## Ignored Operations list not found

**Error:** `List "name" doesn't exist on disk. Set to default list.`

The JSON file for a custom ignored operations list was deleted or moved.

**Fix:**
1. Open the **Ignored Operations** section
2. Create a new list with the same name, or switch to the **Default** list
3. Re-add the operations you want to ignore

---

## Settings are not remembered between sessions

SaveGuard Pro saves all settings per-document in a local file (`pluginSettings.json`). If settings are lost, it may mean:

- The plugin's local storage was cleared (e.g. plugin reinstalled or Photoshop reset)
- The document was opened from a different location

Settings are tied to the document ID, not the filename, so renaming or moving a file may cause SaveGuard to treat it as a new document.

---

## Error messages reference

| Message | Meaning |
|---|---|
| `File unavailable or does not exist - save to disk first!` | Document has not been saved to disk yet |
| `AutoSave took Xs. "Check every (s)" is set to Ys. Consider increasing this value.` | Save duration is ≥ 40% of the interval — performance warning (orange) |
| `AutoSave took Xs. "Check every (s)" is set to Ys. Increase this value to improve performance.` | Save duration is ≥ 70% of the interval — critical warning (red) |
| `Folder unavailable` | Selected save or backup folder is not accessible |
| `Wrong folder selected. Please choose the document's original folder.` | When format requires saving next to the original, you must select the correct folder |
| `Max attempts reached to select the original folder. AutoSave stopped.` | Too many wrong folder selections — AutoSave stopped |
| `Document state is corrupted. Please save the document manually to restore it.` | Photoshop internal state became unrecoverable; manual save required |
| `AutoSave stopped after N consecutive history tracking failures.` | SaveGuard could not read document history reliably — restart Photoshop |
| `AutoBackup stopped after N consecutive history tracking failures.` | Same as above, for AutoBackup |
| `File is empty (0 bytes). AutoSave stopped.` | The saved file is empty — disk or permission issue |
| `PSD file exceeds 2GB limit. Please change format to PSB.` | Document too large for PSD; switch to PSB in settings |
| `[format] files can only have 1 layer.` | Selected format does not support layers — switch to PSD or PNG |
| `GIF files cannot be overwritten in non-Indexed Color mode.` | Convert to Indexed Color or change format |
| `Same format as original - file will be overwritten` | AutoSave will overwrite the document's original file; check the **Allow overwriting** checkbox to confirm |
| `File "{filename}" already exists - will be overwritten` | A file with the target filename already exists in the selected folder |
| `Could not delete some files: [files]` | Old backups could not be deleted; they remain in the folder |
| `Could not delete any of N files. AutoBackup stopped.` | All retention-cleanup deletions failed — check folder permissions |
| `Failed to save ignored list settings` | The ignored operations list could not be written to disk — list works only for this session |
| `Failed to update ignored list: file is busy. Try again.` | The list JSON file is locked by another process |
| `Cannot delete list: file is busy. Try again.` | The list file could not be deleted — try again |
| `List "{name}" doesn't exist on disk. Set to default list.` | A previously assigned ignored list file was removed externally |

---

## Contact support

If you're experiencing an issue not covered here, please reach out:

- **Email:** xpewix@gmail.com
- **GitHub Issues:** [github.com/pewi21/SaveGuard-Pro/issues](https://github.com/pewi21/SaveGuard-Pro/issues)

When reporting a bug, please include:
- Photoshop version
- Operating system (Windows / macOS)
- What you were doing when the error occurred
- The exact error message shown in the panel
