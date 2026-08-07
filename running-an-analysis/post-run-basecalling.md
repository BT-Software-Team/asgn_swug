# Post-run Basecalling

Sequencing data generated with **MinKNOW Core versions 24.02.8 – 25.03.9** must be reanalyzed post-run in MinKNOW before it can be analyzed with the AmplideX Nanopore Carrier Plus Analysis Module v2.0.0 or later. Rebasecalling applies the latest basecalling model, producing a dataset this Software can analyze.

See the Software Release Notes for the basecaller model requirements of your specific pipeline version.

## Rebasecall and Import

1. **Follow MinKNOW's post-run basecalling guide.**

   - At **step 4 of that guide** — *Select output folder and file type* — create a new folder with a unique name at a **different directory level**. It must not sit inside the folder structure holding your original sequencing run data. This keeps the rebasecalled FASTQ files importable as a dataset distinct from the one already imported from the default MinKNOW data source (for example `/data`).
   - For the required MinKNOW sequencing configuration settings, see **Table 18** in the *AmplideX Nanopore Carrier Plus Kit Protocol Guide* (doc 00004909v3).

2. **Check the output folder name.** Post-run basecalling writes `fastq.gz` files to a subdirectory named `fastq_pass` by default. This must match the import pattern of the data source you will import from — which also defaults to `fastq_pass`. If the names differ, rename the directory to match, or adjust the pattern. See [Configure Data Sources](../configuration/data-sources.md) to review the import patterns on the MinKNOW data source.

3. ***(Optional)* Copy the original `report.json`.** Post-run basecalling does **not** generate a new `report.json`. That file is produced by MinKNOW during the original run, and the Software uses it to display compatibility information between AmplideX One Reporter and MinKNOW. Copying it from the original run directory into the rebasecalled folder enables that compatibility status check.

   > **You will see MinKNOW Warnings either way.** After post-run basecalling, the Software reports warnings about basecaller model incompatibility whether or not `report.json` is present. These warnings can be ignored — you may proceed with the analysis. See [MinKNOW Warnings](create-an-analysis.md#minknow-warnings).

4. **Import the rebasecalled dataset.** Return to [Create an Analysis](create-an-analysis.md) and choose **Import Dataset...** in the **Dataset** dropdown to import it.
