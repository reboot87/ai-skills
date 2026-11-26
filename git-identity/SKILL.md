---
name: git-identity
description: Use when git commit or push asks for GitHub credentials, asks which account to use, fails with "could not read Username for 'https://github.com'", or commits under the wrong author; sets the repository-local author and credential username to the account that owns origin, then proves a push works with no prompt.
---

# Git identity and credentials

Two annoyances, one fix: the wrong commit author, and a credential helper that asks which
GitHub account to use on every push.

## When to use

- A login or account-selection dialog appears on commit or push.
- `fatal: could not read Username for 'https://github.com'` from a non-interactive shell.
- Commits are attributed to the wrong person.

Do not use it to rewrite history, or on a repository you only read.

## Diagnose (read-only)

```sh
git remote get-url origin                  # the owner is the first path segment
git config --get user.name; git config --get user.email
git config --global --get user.name
git config --get-all credential.helper     # manager = Git Credential Manager
```

On Windows, list the accounts that are already stored:

```powershell
cmdkey /list | Select-String -Pattern "github" -Context 0,2
```

Each entry shows `target=git:https://github.com` and `User: <login>`. Two different logins
for the same host are exactly what makes the helper ask every time.

macOS uses `security find-internet-password`, Linux `secret-tool search`; the fix is the same.

## Fix (repository-local, written to .git/config)

Use the account that owns `origin`, not the author of the last commit.

```sh
git config --local user.name  <login>
git config --local user.email "<login>@users.noreply.github.com"
git config --local credential.https://github.com.username <login>
```

The noreply address needs no verification and keeps a private address out of public
history. The `<id>+<login>@users.noreply.github.com` form survives a later rename; `id` is
`.id` from `GET https://api.github.com/users/<login>`.

The `username` line removes the ambiguity: the helper is asked for one specific account
instead of choosing between all stored ones. It works with GCM, osxkeychain and libsecret.

## Prove it without interaction

```sh
GCM_INTERACTIVE=Never GIT_TERMINAL_PROMPT=0 git push --dry-run origin HEAD
```

PowerShell: set the two environment variables, then run the same command.

- `Everything up-to-date`, or the refs it would update: done, no dialog will appear.
- `Cannot prompt because user interactivity has been disabled` / `could not read Username`:
  that account's credential is not stored yet. One interactive login is unavoidable: run a
  normal `git push` once, then repeat this check.
- An unborn branch has nothing to push; commit first.

Optionally confirm the identity git will use:

```sh
git var GIT_AUTHOR_IDENT
```

## Make it the default for every repository

Only when every repository on this machine is pushed as the same GitHub account:

```sh
git config --global user.name  <login>
git config --global user.email "<login>@users.noreply.github.com"
git config --global credential.https://github.com.username <login>
```

Do not set the global `username` when some repositories use a second account: those pushes
would authenticate as the wrong user and fail.

## Never

- Amend or rewrite commits that are already pushed just to change their author.
- Print, copy or store a token; redact any `Authorization` header to `<redacted>`.
- Delete credential-manager entries; leaving another account stored is harmless.
- Commit `.git/config`; the change is local and must stay untracked.
