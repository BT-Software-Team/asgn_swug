# Review Results

When an analysis completes, the Software presents results in three progressively more detailed views — sample, genotype, and variant — plus interactive figures. This page shows how to drill down through them and share what you find.

> **Prerequisite:** The analysis must have status **Complete**.
>
> **Research Use Only.** Not for use in diagnostic procedures.

## View Results

1. From the **Analysis Dashboard**, double-click an analysis with status **Complete**, or click its **⋯** menu and choose **Sample Summary**.

2. This opens the **Sample Summary** — a high-level view of key results per sample.

3. Click a sample row to select it, then click **View Genotype Summary** in the toolbar. Double-clicking the row does the same thing.

   Genotype Summary shows per-gene genotype results, filtered to the sample and barcode you came from. Clear the filters to view all entries.

4. Click a row there, then click **View Variant Results** in the toolbar — or double-click the row.

   Variant Results shows variant-level detail, filtered to the sample, barcode, and gene you came from. Clear the filters to view all entries.

5. In Variant Results, click the **chart icon** at the left of a row to open an interactive figure for that variant in a new browser tab. The icon appears only on rows that have a figure.

> The **View Genotype Summary** and **View Variant Results** buttons stay greyed out until you select a row.

For the columns and contents of each view and output file, see [Results Description](../reference/results-description.md).

### Move Between Views

Each results view has **breadcrumbs** in its toolbar showing where you are:

```
Home / Sample Summary / Genotype Summary / Variant Results
```

Click any earlier step to go back to it — **Home** returns to the Analysis Dashboard. Going back this way keeps the filters you arrived with, so you can return to a narrowed view without redoing the drill-down.

### Share Results

URLs for Sample Summary, Genotype Summary, and Variant Results are **permalinks** — they preserve the current filter state and are unique to each analysis and page. Share them directly with collaborators who have access to the Software.

### Customize the View

Every results view uses the same table toolbar as the Analysis Dashboard — search, column visibility, filters, export, sorting, and column pinning. See [Table Controls](../running-an-analysis/start-and-monitor.md#table-controls) for how each works.

## What's next

- [Download Results](download-results.md) — export result files and browse the output folder structure.
