# Post-run Basecalling

If your sequencing run was generated with an older or incompatible MinKNOW version, rebasecall the raw data before importing it for analysis. This produces a dataset the Software can analyze without basecaller-model warnings.

See the Software Release Notes for the specific basecaller model requirements for your pipeline version.

1. Follow MinKNOW's post-run basecalling guide. At step 4, create a new output folder at a **different directory level** from the original run — not nested inside it — so the rebasecalled dataset can be imported as a distinct dataset.

2. The default output subdirectory for rebasecalled `fastq.gz` files is `fastq_pass`. Confirm this matches the import pattern configured in your data source (see [Configure Data Sources](../configuration/data-sources.md)).

3. *(Optional)* Copy the original run's `report.json` file into the rebasecalled folder to enable MinKNOW compatibility checks. If omitted, the Software will show basecaller model warnings — these can be ignored when using post-run basecalled data.

4. Return to [Create an Analysis](create-an-analysis.md) and select **Import Dataset…** to import the rebasecalled dataset.
