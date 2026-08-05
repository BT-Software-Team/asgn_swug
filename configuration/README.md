# Configuration Overview

**System Configuration** is where you set up everything an analysis depends on before you run it: where the Software looks for sequencing data, which variants it reports on, and whether other computers on the network can reach it.

These settings are set up once — typically during installation or when a new instrument is added — and then reused by every analysis. You do not configure them each time you run a sample.

> System Configuration is separate from the account **Settings** menu, which covers your own profile, password, and team members. If you are looking for user accounts, see [Managing Your Team](../managing-your-team/add-a-user.md).

---

## What You Can Configure

| Setting | What it controls | Where to read more |
|---------|-----------------|-------------------|
| **Data Sources** | Where the Software looks for sequencing run data — a folder on this computer, or a GridION or other machine reached over the network. Every configured data source appears as an option when you set up a new analysis. | [Configure Data Sources](data-sources.md) |
| **Panel Filters** | Which variants are included in your analysis results, at both the variant level (Analyze) and the gene and sample level (Summarize). The Software ships with a default filter, and you can create custom configurations for specific use cases. | [Panel Filter Configurations](panel-filters.md) |
| **Remote Access** | Whether other computers on the local network can connect to the Software, or whether it is reachable only from the machine it is installed on. | [Set Up Remote Access](data-sources.md#remote-access) |

---

## Open System Configuration

1. Go to the **Analysis Dashboard** — the main screen listing your analyses.
2. In the table toolbar above the list, click the **gear icon**.
3. System Configuration opens with a menu on the left-hand side. Select the area you want: **Data Sources**, **Panel Filters**, or **Remote Access**.

Every configuration procedure in this guide begins from this screen.

---

## Where to Start

If you are setting up the Software for the first time:

1. **Check your data source.** A default data source pointing at MinKNOW's run folder is created during installation. If your runs are stored somewhere else, or on another machine, add a data source for it — see [Configure Data Sources](data-sources.md).
2. **Review panel filters.** The default filter is suitable for most users. Create a custom configuration only if your use case calls for a different set of reported variants — see [Panel Filter Configurations](panel-filters.md).
3. **Decide on remote access.** Leave it off unless colleagues need to reach the Software from other computers on the network. Read the security note in [Set Up Remote Access](data-sources.md#remote-access) before enabling it.

Once these are in place, you are ready to run an analysis — see [Create an Analysis](../running-an-analysis/create-an-analysis.md).
