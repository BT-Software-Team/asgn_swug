# Sample Sheet Errors

Every sample sheet is validated when you upload it during [Configure an Analysis](../running-an-analysis/configure-an-analysis.md). Errors are reported in three places, and where a message appears tells you how much of the sheet is affected:

| Where it appears | Scope | Effect |
|------------------|-------|--------|
| The **Errors** column of a row | That row only | Other rows still load. Fix the row and the error clears. |
| A message **above the file card**, no table shown | The whole file | Nothing is loaded. The file must be corrected and re-uploaded. |
| An alert **above the table** | The whole sheet | The table loads, but the analysis is blocked until it is resolved. |

Rows with no problems show `N/A` in the Errors column.

Find your message below, or check the [sample sheet requirements](../running-an-analysis/configure-an-analysis.md#sample-sheet-requirements) if you are building a sheet from scratch. Once resolved, return to [Configure an Analysis](../running-an-analysis/configure-an-analysis.md#assign-a-sample-sheet) to continue.

---

## Row Errors

These appear in the **Errors** column of the sample sheet table. Several errors on the same row are shown together, separated by spaces. Most can be corrected in place — click the **pencil icon** in that row's **Actions** column, edit the value, then click the **save icon**.

### Missing values

| Message | Cause | Fix |
|---------|-------|-----|
| `SourceID is blank.` | The row has no sample name. | Enter a sample name. Every row needs one. |
| `BarcodeId is blank.` | The row has no barcode. | Enter the barcode used for that sample, in `BCXX` form. |
| `The calibrators column is blank.` | The Calibrators cell is empty. | Enter `n` if the sample is not a calibrator, or the mix letters it calibrates (`a`, `c`, or `ac`). The column is never allowed to be empty. |

### Barcode problems

| Message | Cause | Fix |
|---------|-------|-----|
| `BarcodeID is not in the format 'BCXX'.` | The barcode is not two digits preceded by `BC` — for example `BC1`, `barcode01`, or `1`. | Rewrite it as `BC` plus a two-digit number: `BC01`, not `BC1`. |
| `BarcodeID does not have corresponding barcode directory in the fastq folder'.` | The barcode is well-formed, but the imported dataset has no matching `barcodeXX` folder. | Confirm the barcode was actually sequenced, and that it is spelled the way MinKNOW wrote the folder. If the folder should exist, the dataset may have imported incompletely — see [Missing Datasets](common-errors.md#missing-datasets). |

> **This one is not fixable by editing the row.** The row is checked against the dataset on disk, so correcting the sheet only helps if the barcode was genuinely wrong. If the sequencing run truly has no data for that barcode, remove the row from the file and upload it again.

### Formatting problems

| Message | Cause | Fix |
|---------|-------|-----|
| `Line is not tab-delimited.` | The row's columns are separated by something other than tabs — most often commas, from a file saved as `.csv` and renamed. | Re-export the sheet as **tab-delimited text**. In Excel: *Save As* → *Text (Tab delimited) (\*.txt)*. |
| `Line contains spaces.` | A value contains a space — for example a sample name like `Patient 01`. | Remove the spaces. Use `-` or `_` instead: `Patient_01`. Values may only contain letters, numbers, `-`, and `_`. |

> Both of these affect the file rather than a single value, so they are usually faster to fix in the source file and re-upload than to correct row by row.

### Mix and calibrator problems

| Message | Cause | Fix |
|---------|-------|-----|
| `Line has an incorrect number of mixes.` | The Mixes cell is empty or lists more than four mixes. | Enter between one and four mix letters with no separators — `a`, `ac`, or `abcd`. |
| `Mixes contain the invalid mix {X}.` | The Mixes cell contains a letter other than `a`, `b`, `c`, or `d`. | Use only `a`, `b`, `c`, `d`. The `{X}` in the message names the offending character. |
| `The calibrators column contains calibrator {X} which is not in the mixes column.` | A sample is marked as a calibrator for a mix it was not run with — for example Calibrators `ac` with Mixes `ab`. | Make the calibrator letters a subset of the mix letters. Either add the mix to the Mixes cell or drop the letter from Calibrators. |

---

## File Errors

These appear as a red message above the file card, and **no table is shown** — the whole sheet is rejected. They cannot be fixed in the interface; correct the file and upload it again.

| Message | Cause | Fix |
|---------|-------|-----|
| `Samplesheet doesn't contain one or more of the required headers (...).` | The header row is missing a required column. The message lists the headers the Software expects. | Add the missing column. Headers are **case-sensitive** — see [Sample Sheet Requirements](../running-an-analysis/configure-an-analysis.md#sample-sheet-requirements). |
| `Header row in samplesheet is not tab delimited.` | The header row uses commas or another separator. | Re-export as tab-delimited text. If the header row is comma-separated, the data rows almost certainly are too. |
| `Header row in samplesheet contains spaces.` | A column name contains a space. | Remove the spaces from the header names. |

> **All three point at the same root cause most of the time:** the file was created or saved in the wrong format. Before hunting for a subtle header problem, confirm the file is genuinely tab-delimited `.txt` and not a renamed `.csv`.

---

## Calibrator Alert

| Message | Cause | Fix |
|---------|-------|-----|
| `Samplesheet contains mix {X} but no calibrator` | The sheet uses a mix somewhere, but no row anywhere in the sheet is designated as a calibrator for it. | Mark at least one sample as a calibrator for that mix, by putting its letter in that row's Calibrators cell. |

This appears above the sample sheet table and applies to the sheet as a whole, not to one row — no individual row will show an error for it. It clears automatically when you upload a corrected sheet.

> Mixes A and C always require at least one designated calibrator. See the [Protocol Guide](../README.md#additional-resources) for calibrator recommendations.

---

## Warnings

A yellow alert above the table is a **warning**, not an error. Warnings come from checking the sequencing dataset — not from the sample sheet — and they do **not** block the analysis. The most common is a MinKNOW compatibility warning; see [MinKNOW Warnings](../running-an-analysis/create-an-analysis.md#minknow-warnings).

---

## Still Stuck?

- Check the file against [Sample Sheet Requirements](../running-an-analysis/configure-an-analysis.md#sample-sheet-requirements).
- If the sheet looks correct but barcodes are not found, the dataset may be the problem — see [Missing Datasets](common-errors.md#missing-datasets).
- If nothing here matches your message, see [Contact Support](contact-support.md).
