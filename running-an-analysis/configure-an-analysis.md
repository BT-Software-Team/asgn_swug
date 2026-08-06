# Configure an Analysis

Before an analysis can run, it needs a **panel configuration** (which variants to report) and a **sample sheet** (which samples and barcodes to expect). This page walks through assigning both and resolving any sample-sheet errors.

> **Prerequisite:** You must have already [created the analysis](create-an-analysis.md) and imported its dataset.

## Assign a Panel Filter and Sample Sheet

1. From the **Analysis Dashboard**, double-click an analysis with status **New**, or click its **⋯** menu and choose **Configure Analysis**. The page header shows the analysis name and its dataset.

2. Select a configuration from the **Panel Filter** dropdown. It is applied as soon as you select it — there is no separate apply button. To create a configuration first, see [Panel Filter Configurations](../configuration/panel-filters.md).

3. Under **Import Sample Sheet**, click **Click to Browse Files** and select your `.txt` sample sheet. It uploads and validates immediately.

   Once uploaded, the file appears as a card showing its name and size. To replace it, click the **✕** on that card and browse for another file.

4. Review the sample sheet table that appears below.

   | What you see | What it means |
   |--------------|---------------|
   | A **warning banner** above the table | The run data may not be fully compatible with the pipeline. See [MinKNOW Warnings](create-an-analysis.md#minknow-warnings). This does not block the analysis. |
   | A red error message above the file card | The file could not be processed at all — no table is shown. Correct the file and upload it again. |
   | Text in the **Errors** column | That row failed validation. Check it against [Sample Sheet Requirements](#sample-sheet-requirements) below. |
   | `N/A` in the **Errors** column | That row is valid. |

   To fix a row, click the **pencil icon** in its **Actions** column, edit the values in place, then click the **save icon** to keep the change or the **cancel icon** to discard it. Identifier columns cannot be edited — to change those, correct the source file and upload it again.

5. Once every row is free of errors, click **Start Analysis**.

   The button stays disabled until a panel filter is selected, a sample sheet is loaded, and no errors remain. **Cancel** returns to the Analysis Dashboard.

   > If a red banner reads *No pipelines were found. Starting an analysis is disabled until pipelines are available*, the Software has no pipeline to run against and the analysis cannot be started. Contact support if this persists.

## Sample Sheet Requirements {#sample-sheet-requirements}

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

## What's next

- [Start & Monitor Execution](start-and-monitor.md) — run the analysis and track its status.
