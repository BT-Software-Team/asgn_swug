# Configure an Analysis

Before an analysis can run, it needs a **panel filter** (which variants to report) and a **sample sheet** (which samples and barcodes to expect). This page walks through assigning both and resolving any sample-sheet errors.

> **Prerequisite:** You must have already [created the analysis](create-an-analysis.md) and imported its dataset.

Configuration has three parts, done in order on a single page:

1. [Open the Analysis](#open-the-analysis)
2. [Assign a Panel Filter](#assign-a-panel-filter)
3. [Assign a Sample Sheet](#assign-a-sample-sheet)

Then [start the analysis](#start-the-analysis).

---

## Open the Analysis {#open-the-analysis}

From the **Analysis Dashboard**, double-click an analysis with status **New**, or click its **⋯** menu and choose **Configure Analysis**.

The page header shows the analysis name and its dataset, so you can confirm you opened the right one.

---

## Assign a Panel Filter {#assign-a-panel-filter}

The panel filter determines which variants the analysis reports.

1. Open the **Panel Filter** dropdown at the top of the page. It lists the default filter and every custom configuration.
2. Select the one you want.

The selection is applied as soon as you make it — there is no separate apply or save button.

> No suitable configuration in the list? See [Panel Filter Configurations](../configuration/panel-filters.md) to create one, then return to this page.

---

## Assign a Sample Sheet {#assign-a-sample-sheet}

The sample sheet tells the analysis which samples and barcodes to expect.

### Sample sheet requirements {#sample-sheet-requirements}

The sample sheet must be a tab-delimited `.txt` file with the following required headers (case-sensitive):

| Column | Description |
|--------|-------------|
| `SourceID` | Sample name. Displayed as **Sample ID** in the sample sheet table. |
| `BarcodeID` | Barcode in format `BCXX` (e.g., `BC01`) |
| `Mixes` | Letters for each Mix included (e.g., `abc` for Mix A, B, and C) |
| `Calibrators` | `n` for non-calibrators; `a`, `c`, or `ac` for calibrators |

Additional rules:

- Values may only contain letters, numbers, `-`, and `_`. Spaces and other whitespace are not accepted.
- Each `SourceID` + `BarcodeID` combination must be unique.
- For each `BarcodeID`, a matching `barcodeXX` folder must exist in the imported dataset directory.
- If Mix A or C is used, at least one calibrator sample must be designated for those mixes.

> **Default User Data Path is a hidden folder.** To view it in File Explorer on Windows, enable hidden files in View settings. Default path: `C:\ProgramData\asuragen\`

### Upload the file

1. Under **Import Sample Sheet**, click **Click to Browse Files**.
2. Select your `.txt` sample sheet. It uploads and validates immediately.

Once uploaded, the file appears as a card showing its name and size. To replace it, click the **✕** on that card and browse for another file.

### Review the results

A table of the sample sheet's contents appears below the file card. Check the **Errors** column: rows that passed validation show `N/A`, and any other text means that row failed.

If no table appears at all and a red message shows above the file card, the whole file was rejected rather than individual rows.

> **Got an error?** See [Sample Sheet Errors](../troubleshooting/sample-sheet-errors.md) for every message, what causes it, and how to fix it.
>
> A **yellow warning banner** above the table is not an error and does not block the analysis — see [MinKNOW Warnings](#minknow-warnings) below.

### Correct a row

1. Click the **pencil icon** in that row's **Actions** column.
2. Edit the values in place.
3. Click the **save icon** to keep the change, or the **cancel icon** to discard it.

Identifier columns cannot be edited. To change those, correct the source file and upload it again.

---

## Start the Analysis {#start-the-analysis}

Once a panel filter is selected and every sample sheet row is free of errors, click **Start Analysis**.

The button stays disabled until all three conditions are met: a panel filter is selected, a sample sheet is loaded, and no errors remain. **Cancel** returns to the Analysis Dashboard without starting anything.

> If a red banner reads *No pipelines were found. Starting an analysis is disabled until pipelines are available*, the Software has no pipeline to run against and the analysis cannot be started. Contact support if this persists.

---

## MinKNOW Warnings {#minknow-warnings}

A yellow warning banner above the sample sheet table means the run data may not be fully compatible with the selected analysis pipeline. Common causes include an unsupported MinKNOW version, a basecaller model mismatch, barcode trimming settings, an unvalidated barcode kit, or a missing `report.json` file.

Warnings **do not block the analysis** — you can select a panel filter, load a sample sheet, and click **Start Analysis** as normal. Review the warning first so you know what it may mean for your results.

If the warning relates to a basecaller model or an incompatible MinKNOW version, see [Post-run Basecalling](post-run-basecalling.md). Data that has already been rebasecalled reports these warnings regardless, and they can be ignored.

## What's next

- [Start & Monitor Execution](start-and-monitor.md) — run the analysis and track its status.
