# Configure Data Sources

A **data source** defines a path or endpoint where sequencing run data is stored and accessible by the Software.

A default data source is created during installation pointing to where MinKNOW stores run data:

| OS | Default path |
|----|-------------|
| Linux | `/var/lib/minknow/data` |
| Windows | `C:\data` (stored internally as `/mnt/c/data`) |

Additional data sources can be added for networked devices (e.g., a GridION, another sequencing workstation, or a remote fileserver) using SSH on port 22. All configured data sources appear as options when setting up a new analysis.

Every data source is one of two types:

| Type | What it means | Use it when |
|------|---------------|-------------|
| **Local Computer** | The run data sits on a drive that the computer running the Software can reach directly — an internal disk, an attached USB or external drive, or a network share already mounted by the operating system. No credentials are needed; the Software reads the path as-is. | The files are on this machine, or on storage this machine already has mounted. |
| **Remote (SFTP)** | The run data sits on a *different* computer, and the Software reaches it over the network via SSH/SFTP on port 22. Requires a host name, SSH username, and password. | The files are on a GridION, another sequencing workstation, or a fileserver that is not mounted locally. |

If you can open the folder in the file browser on the computer running the Software, it is a **local** data source. If you would have to log in to another machine to see the folder, it is a **remote** data source.

After a dataset is imported, it is copied to a managed location:

| OS | Imported dataset path |
|----|----------------------|
| Linux | `/var/lib/asuragen/datasets` |
| Windows | `[User Data Path]\datasets` (default: `C:\ProgramData\asuragen\datasets`) |

---

## Open System Configuration

Data source setup lives in **System Configuration**, not the account **Settings** menu.

1. From the **Analysis Dashboard**, click the **gear icon** in the table toolbar.
2. In the left-hand menu, select **Data Sources**.

## Add a Local Data Source

Use this method when the run data is on a disk the computer running the Software can already reach — an internal drive, an attached external drive, or an OS-mounted network share.

1. Open **System Configuration** → **Data Sources**.

2. Under **Add new data source**, click **Create New**.

3. Fill in the following fields:

   | Field | Description | Required |
   |-------|-------------|----------|
   | **Name** | Unique display name for this data source | ✅ |
   | **Type** | Choose `Local Computer` | ✅ |
   | **Source Path** | Path to the folder containing sequencing runs (Linux-style) | ✅ |
   | **Import patterns** | RegEx patterns that identify run files (defaults work for most users) | ✅ |
   | **Exclude folders** | Folder names to hide from the import list | Optional |

   **Converting Windows paths to Linux-style:**

   On Windows, the Software stores paths in Linux style, so a path copied from File Explorer has to be rewritten before you paste it into **Source Path**. There are three changes.

   Start with the path as Windows shows it — for example `C:\Archive\Nanopore\runs`.

   | Step | What to do | Result |
   |------|-----------|--------|
   | 1 | Replace the drive letter and colon (`C:`) with `/mnt/` followed by that letter in **lowercase** (`/mnt/c`). A `D:` drive becomes `/mnt/d`, an `E:` drive becomes `/mnt/e`, and so on. | `/mnt/c\Archive\Nanopore\runs` |
   | 2 | Turn every backslash `\` into a forward slash `/`. | `/mnt/c/Archive/Nanopore/runs` |
   | 3 | Type the folder names in lowercase. | `/mnt/c/archive/nanopore/runs` |

   The finished path always starts with `/mnt/`, uses only forward slashes, and has no drive letter followed by a colon.

   More examples:

   | Windows path | Linux-style equivalent |
   |-------------|----------------------|
   | `C:\Archive\Nanopore\runs` | `/mnt/c/archive/nanopore/runs` |
   | `D:\data` | `/mnt/d/data` |
   | `C:\Users\labuser\Documents\runs` | `/mnt/c/users/labuser/documents/runs` |
   | `E:\ONT Runs\2026` | `/mnt/e/ont runs/2026` |

   **To find the path for a folder:** open it in File Explorer, click once in the address bar at the top of the window (the text turns into an editable path such as `C:\Archive\Nanopore\runs`), press **Ctrl+C** to copy it, then apply the three steps above.

   > Spaces in folder names are fine — keep them, and do not add quotation marks.

   **Default import patterns:**

   ```
   .*fastq_pass/.*\.gz$           # Compressed FASTQ files inside fastq_pass/
   .*report_.*\.(html|json)$      # MinKNOW report files
   ```

   To create a custom import pattern, identify the actual file path in a completed run and translate it to a RegEx. For example:

   ```
   .*output/reads/.*\.fastq\.gz$      # Custom compressed FASTQ location
   .*reports/.*\.(html|json)$         # Custom report location
   ```

   Use `.*` for variable path segments, escape periods as `\.`, and end patterns with `$` to avoid partial matches.

4. Click **Preview and apply changes** to see the folders the source resolves to. Verify expected folders appear before saving.

5. Click **Save**. The new data source will appear in the **Available Data Sources** list, and as an option when setting up a new analysis.

> **Runs not showing up?** Double-check the Source Path and Import patterns — a misconfiguration is the most common cause.

---

## Add a Remote Data Source (SSH/SFTP) {#add-a-remote-data-source}

Use this method when the run data is on a different computer that the Software must log in to over the network — a GridION device, another sequencing workstation, or a network file server.

> **Requirement:** SSH access on port 22 (enabled by default on GridION).

1. Open **System Configuration** → **Data Sources**.

2. Under **Add new data source**, click **Create New**.

3. Set **Type** to `Remote (SFTP)` and fill in:

   | Field | Description |
   |-------|-------------|
   | **Name** | Unique display name |
   | **Host name** | IP address or hostname of the remote computer |
   | **Username** | SSH username |
   | **Password** | Password for the SSH user |
   | **Source Path** | Full path to the run data folder on the remote machine |
   | **Import patterns** | Same as local — see above |
   | **Exclude folders** | Optional |

   > **Use a local system account** (not a personal or domain account) for the SSH credentials. This prevents access interruptions caused by password changes or account deactivation.

   **Run data on a non-C drive (Windows remote)?** Create a symlink on the remote machine first:

   ```cmd
   mklink /D C:\sftp\data D:\data
   ```

   Then enter `/sftp/data` as the Source Path in the Software. The symlink resolves to `D:\data` transparently — no data is moved.

4. Click **Preview and apply changes** to see the top-level directories. Confirm expected folders are visible.

5. Click **Save**.

---

## Modify a Data Source

Use this when sequencing data has moved to a different path, or when SSH credentials change.

> **Credential changes:** If the account configured for a data source is disabled, sequencing runs at that endpoint will disappear from the import list. Update the data source with a working account to restore access.

1. Open **System Configuration** → **Data Sources**, then click the data source in the **Available Data Sources** list to open its details.
2. Click the **pencil (edit)** icon, update the relevant fields using the same instructions above, then click **Preview and apply changes** → **Save**.

---

## Remove a Data Source

1. Open **System Configuration** → **Data Sources**, then click the data source in the **Available Data Sources** list to open its details.
2. Click the **trash (delete)** icon.
3. In the **Remove data source?** confirmation, click **Remove**.

---

## Set Up Remote Access {#remote-access}

Remote access controls whether other computers on the network can connect to the Software.

> **Security note:** The Software does not include authentication or authorization controls. Secure remote access with a firewall or network access controls before enabling.

1. Open **System Configuration** (gear icon in the Analysis Dashboard toolbar).
2. Select **Remote Access** in the left-hand menu.
3. Toggle **Allow connection from other computers** on to allow access from other computers on the local network, or off to limit access to the local machine only.

Applying the change can take up to 5 minutes, during which the user interface may be unavailable. Refresh the page afterward to see the updated status.
