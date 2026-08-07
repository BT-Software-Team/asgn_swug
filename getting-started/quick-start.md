# Quick Start

This walkthrough takes you from a freshly installed Software to your first set of results in about five minutes of hands-on time (analysis run time varies). It uses the default data source and default panel filter, so there's nothing to configure first.

> **Before you begin:** The Software must be [installed](installation-linux.md), and you must be [signed in](signing-in.md) at `http://localhost:9000`. You also need a **completed** sequencing run in the default MinKNOW data location — a default data source pointing there is created for you during installation.

## 1. Create the analysis

1. On the **Analysis Dashboard**, click the **+ (plus) icon** in the table toolbar.
2. Type a name in **Analysis Id** (letters, numbers, `-`, and `_` only).
3. Leave the preselected pipeline as-is.
4. In the **Dataset** dropdown, choose **Import Dataset...**, click your completed run in the table that appears, then click **Create Analysis**.

*Full details: [Create an Analysis](../running-an-analysis/create-an-analysis.md).*

## 2. Configure it

1. Double-click your new analysis (status **New**) to open it.
2. In the **Panel Filter** dropdown, select the default filter. It applies as soon as you pick it — no need to build your own configuration for a first run.
3. Under **Import Sample Sheet**, click **Click to Browse Files** and select your `.txt` sample sheet. It validates immediately.
4. Fix any errors flagged in the **Errors** column (click the pencil icon to edit a row, then the save icon).

*Full details: [Configure an Analysis](../running-an-analysis/configure-an-analysis.md).*

## 3. Run it

Click **Start Analysis**. The analysis moves to **In Queue**, then **Running**.

## 4. Watch for completion

Back on the **Analysis Dashboard**, refresh and watch the **Status** column until it reads **Complete**.

*Status meanings: [Start & Monitor Execution](../running-an-analysis/start-and-monitor.md).*

## 5. Review your results

Double-click the **Complete** analysis (or click its **⋯** menu and choose **Sample Summary**) to open the **Sample Summary**. From there, drill into **Genotype Summary** → **Variant Results** → **View Variant Figure**.

*Full details: [Review Results](../analysis-results/review-results.md).*

## What's next

- Point the Software at your own run data by adding a [data source](../configuration/data-sources.md) — needed when your runs are not in the default MinKNOW location, or live on a GridION or other machine.
- Control which variants get reported by creating a [panel filter](../configuration/panel-filters.md).
- See what else is configurable in the [Configuration Overview](../configuration/README.md).
- Learn to [download and archive](../analysis-results/download-results.md) result files.
- Hit a snag? See [Troubleshooting](../troubleshooting/common-errors.md).
