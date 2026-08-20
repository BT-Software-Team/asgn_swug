# Troubleshooting

This page collects the most common problems — the Software won't start, an installation or analysis fails, results look wrong, or a dataset won't import — with the checks and fixes for each. If nothing here resolves the issue, contact Asuragen Technical Support.

---

## Software Won't Start After Restarting (Windows)

The Software runs inside WSL and is started automatically via a Windows Scheduled Task. If it doesn't start after a restart:

### Start the Scheduled Task

1. Open **Task Scheduler** (Start → search "Task Scheduler" → **Run as administrator**).
2. Click **Task Scheduler Library** and locate the task named **AmplideX One Reporter (WSL)**.
3. Right-click the task and click **Run**.
4. In the Actions pane, click **Refresh** and confirm the status changes to **Running**.

If the status remains **Ready**, the task failed — continue to the next section.

### Modify the Scheduled Task

Windows may be blocking WSL from running on behalf of another user. To fix this, update the task to run under the Administrators group:

1. Open Task Scheduler and locate **AmplideX One Reporter (WSL)**.
2. Right-click the task → **Properties**.
3. On the **General** tab, click **Change User or Group…**
4. Type `Administrators` in the text box and click **Check Names**. The computer name will be prepended (e.g., `COMPUTERNAME\Administrators`). Click **OK**.
5. Confirm `BUILTIN\Administrators` now appears in Security options, and that **Run with highest privileges** is checked.
6. Click **OK** to save.
7. Right-click the task → **Run** → **Refresh** and confirm the status is **Running**.

> **Why `BUILTIN\Administrators`?** Windows only allows this specific group to execute WSL, and only for users who belong to it. Substituting a different group or a named admin account will not work.

> **Note:** Only administrator accounts can operate the Software when the task is configured this way.

If the Software still does not start, contact Asuragen Technical Support.

---

## Investigating a Failed Installation

### Verify System Requirements

**Confirm your OS version:**

Ubuntu 22.04:
```bash
cat /etc/os-release
```

Windows 11: Start → Settings → System → About → Windows specifications.

**Confirm network access:** Verify that outbound HTTPS (TCP port 443) is permitted to all required endpoints. See [System Requirements](../getting-started/system-requirements.md#network-requirements).

**Confirm hardware:** Review the minimum hardware specifications in [System Requirements](../getting-started/system-requirements.md).

**Windows-specific — WSL install error:** If the installer fails with a WSL error, return to [Step 1 of Windows Installation](../getting-started/installation-windows.md) and confirm both `wsl --update` and `wsl --install --no-distribution` were run before retrying.

---

## Investigating Failed Analyses

### Verify Analysis Prerequisites

1. **Docker images not fully downloaded:** If a new Mix is analyzed and fails, check the image-pull log for `Docker image pull completed successfully` — on Windows, `C:\Program Files\asuragen\carrier-plus-images.log`; on Ubuntu Linux, `/var/log/asgn-carrier-plus-images.log`. If missing, confirm internet connectivity, restart the computer, and re-run within 24 hours.
2. **Firewall or network restrictions:** Review [System Requirements](../getting-started/system-requirements.md#network-requirements).
3. **Analysis metadata:** Review the Analysis Configuration page to confirm all calibrators and Mixes are correctly configured.

### Diagnose the Failure

1. Open the failed processes log:
   ```
   [User Data Path]\analyses\{Analysis Id}\results\logs\failed_processes_log.txt
   ```
   The file shows `No failed processes.` if none occurred, or lists failed process names and their hash identifiers.

2. For each failed process, locate its directory in `logs/errors/` using the hash.

3. Open `.command.err` in a text editor to inspect the error. This file can also be sent to Asuragen Technical Support.

4. If the process `COMBINEDEMUXFILES` failed, there is likely an error with the input dataset or sample sheet.

5. Also review the process log for error messages near the bottom:
   ```
   [User Data Path]\analyses\{Analysis Id}\results\logs\process.log
   ```

### Investigating Blank Results Pages

If results pages appear blank after analysis completes:

1. Check `failed_processes_log.txt` (see above) for failed processes.
2. Review the QC failure file:
   ```
   [User Data Path]\analyses\{Analysis Id}\results\quality_control\quality_control_fail.csv
   ```
3. If all samples are present for each selected Mix, the input FASTQ files likely have few or no reads.

---

## Troubleshooting by Observation

| Observation | Potential Cause | Action |
|-------------|----------------|--------|
| **Calibrator-specific low coverage (CalLowCov)** | Wrong calibrator input volume or mass; wrong sample marked as calibrator; incorrect pooling of calibrator PCR product | Add calibrator at the recommended input volume. See [Configure an Analysis](../running-an-analysis/configure-an-analysis.md#sample-sheet-requirements) for marking a sample as a calibrator in the sample sheet, then repeat the analysis. |
| **Incompatible calibrator genotype (CalGT)** | Incompatible user-provided calibrator — most often needed when Mix A/C performance issues are attributed to sample type or isolation method | Use known normal (2-copy) samples in conjunction with the kit calibrator, classified as `calibrator` in the sample sheet for copy number analysis. See [QC Flags Reference](../how-results-are-generated/quality-control.md) for the exact genotype requirements CalGT checks. |
| **Expected QC Fail not observed for NTC** | Sample contamination; reuse of a flow cell in a later batch with the same barcode/mix combination, where cleaning between reuse may have been inadequate | Check NTC fully spanning read depth for NTCs measuring >2 ng/µL after barcoding PCR (should be <100 FSRs or >1000-fold below sample median); apply library to a new/unused flow cell; repeat gene-specific PCR with fresh reagents, ensuring the workflow doesn't contaminate the NTC well before barcode PCR |
| **Amplicon-level LowCov flag** | Evaporation during PCR from improper plate sealing — alters per-amplicon PCR efficiency and bead ratios, changing size selection | Ensure proper plate sealing before PCR, and visually inspect well volumes afterward to identify evaporation |
| **Amplicon-level LowCov flag** | Insufficient sample quantity/quality; unsupported sample type | Confirm sample quantity and quality conform to the Pre-Analytical Steps in the Protocol Guide |
| **Amplicon-level LowCov flag** | Under-sequencing, particularly for large batch sizes (96 samples, Mix A–D) | Confirm Estimated Gb Target was set appropriately for the sample count and Mix configuration — see Start Sequencing in the Protocol Guide for recommended targets. Sequence larger sample sets individually per Mix on a flow cell. |
| **Sample-level QC fail from low coverage** | Mass calculation error causing an imbalance in within-mix or between-mix pooling | Check manual calculations, or use the Bench Workbook to automate them; check the barcode PCR plate's remaining volume to confirm volumetric transfer into the pool |
| **Sample-level QC fail from low coverage** | Insufficient sample quantity/quality; unsupported sample type | Confirm sample quantity and quality conform to the Pre-Analytical Steps in the Protocol Guide |
| **Sample-level QC fail from low coverage** | Read depth in `quality_control.csv` falls just below QC cutoffs | Confirm Estimated Gb Target was set appropriately — see Start Sequencing in the Protocol Guide. Repeat sequencing with increased target data (Gb). |
| **LowConfidence or FC QC flags** | Insufficient sample quantity/quality; unsupported sample type | Repeat sequencing. If the flag persists, repeat sample prep from gene-specific PCR. If multiple samples from a single isolation batch are affected, repeat isolation and analysis. If this occurs frequently with a given isolation method, consider user-defined calibrators: known normal (2-copy) samples classified as `calibrator` in the sample sheet, used in conjunction with the kit calibrator — see [QC Flags Reference](../how-results-are-generated/quality-control.md) for CalGT's full genotype requirements. |
| **Uneven read distribution across samples** | Qubit measurement error (insufficient incubation time or instrument error); manual pooling calculation error; a Mix B sample set with more expanded samples; poor PCR amplification (e.g., enrichment of longer amplicons in Mix D) | Repeat Qubit measurements if an error is suspected; use the Bench Workbook to automate pooling calculations and adjust mass ratios between samples; dilute samples with a Pooled Sample Mass Ratio >1.2 before within-mix pooling. If a single Mix is under-represented after sequencing, repeat sequencing with increased target data, or run the failing Mixes individually. |
| **High frequency of LowCov in one or more Mixes** | Sample pool mass-calculation error; insufficient sample quantity/quality; Qubit measurement error; pooling-ratio error; volumetric-ratio-calculation error for pool combinations; under-sequencing, particularly for large batch sizes | Repeat Qubit measurements if an error is suspected; use the Bench Workbook for pooling calculations; confirm Estimated Gb Target was set appropriately; repeat sequencing with increased target data, or run failing Mixes individually; repeat BC-PCR if yield is insufficient for one or more samples in a pool |
| **"Alpha cluster duplication" reported erroneously** | Excessive sequence deconvolution allele groups from sample contamination or flow cell reuse with same barcode/mix | Apply library to a new or unused flow cell; repeat gene-specific PCR with fresh reagents, ensuring the workflow doesn't contaminate the NTC well before barcode PCR |
| **Super accurate basecalling / barcode trimming warning** | Sequencing didn't use required basecalling settings | Repeat basecalling with the correct settings — see Start Sequencing in the Protocol Guide for MinKNOW setup instructions |
| **Sample sheet will not validate** | Missing values, wrong barcode format, non-tab-delimited file, or mix/calibrator mismatch | See [Sample Sheet Errors](sample-sheet-errors.md) for every message, its cause, and how to fix it |

> **No limit on calibrators.** When investigating CalGT or LowConfidence/FC flags with additional user-defined calibrators, the Software does not cap how many calibrator samples a sample sheet can designate.

---

## Missing Datasets {#missing-datasets}

**Dataset doesn't appear when importing:**

- GridION users: check that MinKNOW's built-in firewall is **off** (when enabled, it may block dataset imports).
- Verify the data source configuration is correct, especially the Run Data Path and import patterns — see [Configure Data Sources](../configuration/data-sources.md).

**Dataset was imported but doesn't appear in Select Dataset dropdown:**

Confirm the dataset was successfully copied to the managed storage location:

- Linux: `/var/lib/asuragen/datasets`
- Windows: `[User Data Path]\datasets` (default: `C:\ProgramData\asuragen\datasets`)

---

## Uninstalling the Software

### Ubuntu 22.04 Linux

```bash
sudo chmod +x /usr/local/bin/asuragen/asgn_onereporter_uninstaller_linux.sh
sudo /usr/local/bin/asuragen/asgn_onereporter_uninstaller_linux.sh
```

### Windows 11

1. Open the installation directory (`C:\Program Files\asuragen` by default) in File Explorer.
2. Right-click `amplidex_one_reporter_uninstaller.exe` → **Run as administrator**.
3. The uninstaller removes all application files, undoes system configurations, and deletes the WSL environment.
4. When prompted, choose whether to delete or keep the uninstaller file.
5. Press any key to close when complete.

> **Analysis data is preserved.** Results and imported datasets remain at `[User Data Path]` (`C:\ProgramData\asuragen` by default) after uninstallation. See [Data Storage & Archival](../reference/data-storage.md) for managing this data.
