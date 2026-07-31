# Quick Start

This walkthrough takes you from a freshly installed Software to your first set of results in about five minutes of hands-on time (analysis run time varies). It uses the default data source and default panel filter, so there's nothing to configure first.

> **Before you begin:** The Software must be [installed](installation-linux.md) and open in your browser at `http://localhost:9000`, and you need a **completed** sequencing run in the default MinKNOW data location. A default data source pointing there is created for you during installation.

## 1. Create the analysis

1. On the **Analysis Dashboard**, click **New Analysis**.
2. Type a name in **Analysis Id** (letters, numbers, `-`, and `_` only).
3. In **Select Dataset**, choose **Import Dataset…**, pick your completed run, and click **Create**.

*Full details: [Create an Analysis](../running-an-analysis/create-an-analysis.md).*

## 2. Configure it

1. Double-click your new analysis (status **New**) to open it.
2. Under **Assign a Panel Configuration**, select `default_filter.json` and click **Add Panel Configuration**.
3. Under **Import Sample Sheet**, click **Choose File**, select your `.txt` sample sheet, then click **ADD SAMPLESHEET**.
4. Fix any errors flagged in the **Errors** column (click the pencil icon to edit a row inline).

*Full details: [Configure an Analysis](../running-an-analysis/configure-an-analysis.md).*

## 3. Run it

Click **Analyze**. The analysis moves to **In Queue**, then **Running**.

## 4. Watch for completion

Back on the **Analysis Dashboard**, refresh and watch the **Status** column until it reads **Complete**.

*Status meanings: [Start & Monitor Execution](../running-an-analysis/start-and-monitor.md).*

## 5. Review your results

Click the **Complete** analysis and double-click it (or click **Results**) to open the **Sample Summary**. From there, drill into **Genotype Summary** → **Variant Results** → **View Variant Figure**.

*Full details: [Review Results](../analysis-results/review-results.md).*

## What's next

- Set up your own [data sources](../configuration/data-sources.md) and [panel filters](../configuration/panel-filters.md).
- Learn to [download and archive](../analysis-results/download-results.md) result files.
- Hit a snag? See [Troubleshooting](../troubleshooting/common-errors.md).
