# Data Source Errors

Errors while adding or editing a data source come from two different stages, and knowing which stage you are in narrows the cause considerably:

| Stage | Where the error appears | What it means |
|-------|------------------------|---------------|
| **Form validation** | Red text beneath the field | The value you typed is not in an acceptable shape. Nothing has been sent anywhere yet. |
| **Preview** | `Preview error: ...` in red inside the preview pane | The Software tried to reach the endpoint and failed. The form is fine; the connection, credentials, or path is not. |
| **Save** | A pop-up dialog | The endpoint was reachable, but the configuration could not be stored. |

Start from [Configure Data Sources](../configuration/data-sources.md) for the full setup procedure. Once the error is resolved, return to it and continue at **Preview and apply changes**.

---

## Form Validation Errors

These appear under the field as soon as you click **Next** (creating) or **Preview and apply changes** (editing). Each names exactly what the field will accept.

| Message | Fix |
|---------|-----|
| `Endpoint name is required.` | Give the data source a name. |
| `Endpoint name can only contain letters, numbers, dashes, and underscores.` | Remove spaces and punctuation — `GridION_1`, not `GridION #1`. |
| `Run data path is required.` | Enter the path to the folder holding the sequencing runs. |
| `Run data path must be a valid Windows or Linux directory path.` | Check the path form. On Windows the path must be converted to Linux style — see [Converting Windows paths](../configuration/data-sources.md#add-a-local-data-source). A common cause is leaving the drive letter in place: `/mnt/c/data`, not `C:\data`. |
| `Host name is required.` | Enter the remote machine's IP address or hostname. |
| `Host name must be a valid server name or IP address.` | Enter just the host — `192.168.1.50` or `gridion-01` — with no `sftp://` prefix, port number, or trailing path. |
| `Username is required.` | Enter the SSH username for the remote machine. |
| `Username can only contain letters, numbers, dashes, dots, and underscores.` | Remove other characters. A domain-style `DOMAIN\user` will not pass — see [Choosing an Account for SSH Credentials](../configuration/data-sources.md#ssh-account). |
| `Password is required.` | Enter the password for the SSH user. Required when creating a data source. |
| `Pattern {n} must be a valid regular expression.` | Import pattern number `{n}` is not valid RegEx — usually an unclosed bracket or parenthesis. See [import patterns](../configuration/data-sources.md#add-a-local-data-source). |
| `Folder name {n} contains invalid characters.` | Exclude-folder entry number `{n}` has characters that are not allowed in a folder name. |

---

## Preview Errors

These appear inside the preview pane as `Preview error: ...`. The form was accepted, so the problem is between the Software and the endpoint. `{host}` and `{path}` are replaced with the values you entered.

### Cannot connect

```
Unable to connect to {host}. The host may be unreachable or the SFTP service
may be unavailable. Verify network connectivity and firewall settings.
```

The Software could not open a connection at all — the host refused it, the attempt timed out, or there is no network route. Work through these in order:

1. **Confirm the host is reachable.** From the machine running the Software, `ping {host}`. If that fails, it is a network problem, not a Software problem.
2. **Confirm SSH is running and listening on port 22.** This is enabled by default on a GridION but not on a stock Windows machine, which needs an SFTP/SSH server installed.
3. **Check firewalls** on the remote machine and anything between the two machines. GridION users: MinKNOW has its own built-in firewall that blocks these connections when enabled — turn it off.
4. **Re-check the host value** for a typo or a stale IP address if the machine uses DHCP.

### Credentials rejected

```
Connection to {host} failed. The User Name or Password may be incorrect,
or {host} may be unreachable
```

The host answered but refused the login.

1. **Re-enter the password.** It is the most common cause, and the field does not show what was typed.
2. **Confirm the account still works** by signing in to the remote machine directly with the same username and password.
3. **Check whether the password expired or the account was disabled.** This is the failure mode that makes a working data source suddenly stop — see [Choosing an Account for SSH Credentials](../configuration/data-sources.md#ssh-account) for why a local system account avoids it.
4. **Check the username form.** Use the plain local account name, not `DOMAIN\user` or `user@domain`.

> This message also fires when the host becomes unreachable mid-attempt, which is why it mentions both causes. If the credentials are definitely correct, treat it as a connectivity problem and work through the previous section.

### Path not found

```
Unable to locate Run Data Path ("{path}") on {host}. Verify the path spelling
and directory structure.
```

The Software connected and authenticated successfully — only the folder is wrong. This is good news: the hard part is working.

1. **Check the spelling** against the folder as it exists on the target machine.
2. **On Windows, check the path conversion.** `C:\data` must be entered as `/mnt/c/data` — see [Converting Windows paths](../configuration/data-sources.md#add-a-local-data-source).
3. **On a Windows remote, check the drive.** Data on a drive other than `C:` is not reachable over SFTP without a symlink — see [Run Data on a Non-C Drive](../configuration/data-sources.md#non-c-drive).
4. **Check permissions.** A folder the SSH user cannot read may report as missing rather than forbidden.

### Any other message

If the preview shows something not listed above, it is the raw error passed straight through from the endpoint. Include the exact text when you [contact support](contact-support.md).

---

## Save Errors

These appear as a pop-up after clicking **Save** in the preview step.

| Message | Cause | Fix |
|---------|-------|-----|
| `Unable to save data source.` | The Software could not reach its own backend service — a network or service problem, not a problem with your entries. | Confirm the Software is running normally, reload the page, and try again. If it persists, see [Common Errors](common-errors.md). |
| Any other message | Passed through from the backend. | Read it for the specific cause; include the exact text if you contact support. |

---

## Related

- The data source saved fine, but no runs appear → [Missing Datasets](common-errors.md#missing-datasets)
- Setting one up from scratch → [Configure Data Sources](../configuration/data-sources.md)
- Nothing here matches → [Contact Support](contact-support.md)
