# Signing In

Once the Software is installed, you access it through your browser at `http://localhost:9000`. This page covers what to expect when you open that URL, and how to sign in.

## Opening the Software

Navigating to `http://localhost:9000` without an active session automatically redirects you to the identity provider — you'll briefly see a **"Grabbing your credentials…"** spinner while this happens. This is expected; you don't need to do anything.

You land on the sign-in form once the redirect completes.

## Sign In

1. Enter your **Username** and **Password**.
2. Click **Sign In**.

> **Don't know your password?** There's no self-service password reset. Click **Need your password? Learn More** below the password field for instructions: contact your organization's admin, who can issue you a one-time password. See [Reset a Password](../managing-your-team/reset-a-password.md).

## First Sign-In (Temporary Password)

If your account was just [created](../managing-your-team/add-a-user.md) or [reset](../managing-your-team/reset-a-password.md), it has status **Pending Login** and a temporary password. Signing in with it takes you to **Reset Your Password** instead of straight into the Software:

1. Enter your **New Password**.
2. Enter it again in **Confirm New Password**.

   > Your new password must be at least 8 characters long and include at least one uppercase letter, one lowercase letter, one number, and one symbol.

3. Click **Reset Password and Sign In**.

Your account status changes to **Active**, and future sign-ins use your new password directly.

## Troubleshooting Sign-In

| Message | Cause | Fix |
|---------|-------|-----|
| `Invalid username or password` | The username or password entered doesn't match an account. | Double-check for typos. If you're not sure of your password, use **Need your password? Learn More** (see above). |
| `Account is locked out` | Too many failed sign-in attempts in a short period. | Wait and try again shortly. If it persists, contact your organization's admin. |
| `This account has been deactivated. Please contact your administrator.` | An admin has [deactivated](../managing-your-team/manage-a-user.md#deactivate-an-account) this account. | Ask an admin to [reactivate](../managing-your-team/manage-a-user.md#reactivate-an-account) it — you can't sign in until then. |
| A server-error page instead of the sign-in form | The Software's identity service isn't reachable (for example, it's still starting up after an install or restart). | Wait a moment and retry. If it persists, see [Troubleshooting](../troubleshooting/common-errors.md). |

## What's next

- [Quick Start](quick-start.md) — run your first analysis.
- [Roles & Permissions](../managing-your-team/roles-and-permissions.md) — what account statuses like **Pending Login** and **Deactivated** mean.
