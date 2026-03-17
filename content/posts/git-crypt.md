+++
date = '2026-03-17T00:00:00-04:00'
draft = false
title = 'Git-Crypt Notes'
tags = ['git', 'dev', 'devops', 'devsecops']
categories = ['engineering']
image = "/images/git-Crypt.png"
featuredImage = "/images/git-Crypt.png"
+++

# How to Use git-crypt to Store Secrets Safely in Git

## What it is

`git-crypt` lets you keep secrets in your Git repository while only decrypting them for authorized users.

- Plaintext in the working tree is stored in Git as encrypted blobs.
- Authorized collaborators can automatically decrypt files after checkout.
- Everyone else sees ciphertext only.

## Why use it

- API tokens, credentials, and service configs in version control
- Shared environment templates and deployment secrets
- Team workflows where full-file encryption is too heavy for ad-hoc secret management

## Install

```bash
brew install git-crypt       # macOS
sudo apt install git-crypt   # Debian/Ubuntu
```

## Initialize and set up a repo

```bash
git init
git-crypt init
```

`git-crypt init` creates cryptographic keys in `.git/git-crypt` and starts tracking repository-wide encryption state.

## Configure encrypted files

Use `.gitattributes` to mark paths for encryption:

```bash
echo "secrets/*.env filter=git-crypt diff=git-crypt" >> .gitattributes
echo "config/prod.json filter=git-crypt diff=git-crypt" >> .gitattributes
```

Commit the attribute file:

```bash
git add .gitattributes
git commit -m "Track sensitive files with git-crypt"
```

## Add authorized users

### GPG recipients (most common)

```bash
git-crypt add-gpg-user "user@example.com"
git commit -m "Add git-crypt recipient"
```

`git-crypt add-gpg-user` requires that the target public key is already available in the local GPG keyring (import the key before running it).

### Included benefits of GPG

GPG is the practical default for `git-crypt` teams because it ties access to a real identity you already manage in your keyring.

- New developers can start decrypting secrets as soon as their GPG key is approved; no shared team secret has to be copied around.
- Access is easier to reason about than a single symmetric key because you can add and remove users without reworking everyone’s local setup.
- Rotation is safer: remove a key or replace a recipient instead of rewriting all secrets around one shared password.
- Auditing is cleaner because you can trace who got access via key approval history and repository commits.

### Symmetric key (single/team-owned)

```bash
git-crypt export-key ./key.txt
```

> Prefer GPG recipients whenever possible; symmetric keys are harder to rotate and audit.  
> Distribute the exported key file securely; never edit `.git-crypt` internals manually.

## Encrypted status and file handling

- Files in matching `.gitattributes` paths are automatically encrypted on commit.
- To force re-encryption after changing patterns:

```bash
git-crypt status -f
git commit -m "Re-encrypt tracked files"
```

If the repo is locked or using a symmetric key, ensure it is unlocked first (`git-crypt unlock [<keyfile>]`) so `status -f` can stage encrypted versions.

To check status:

```bash
git-crypt status
```

## Unlock in a new clone

After cloning:

```bash
git-crypt unlock
```

With GPG recipients configured, unlock uses your local keyring automatically when your private key is available.  
With a symmetric key, use:

```bash
git-crypt unlock /path/to/keyfile
```

## Common gotchas

- Ensure `.gitattributes` is committed before sensitive files, or they may already be in history as plaintext.
- Keep `.git-crypt/` metadata out of the repo root (it is internal; do not edit manually).
- Rotate/adjust access when team membership changes, and re-encrypt current files:

```bash
git-crypt add-gpg-user "new-user@example.com"
git-crypt lock
git-crypt unlock
git add -u
git commit -m "Re-encrypt after recipient change"
```

- `git-crypt` does not provide a `remove-gpg-user`/`del-gpg-user` command, so there is no built-in recipient revocation. This applies to both GPG and symmetric workflows.
- `git-crypt` does not rewrite past commits, so removing access cannot retroactively protect data already in history. Ensure no sensitive plaintext was ever committed before `.gitattributes` matched those paths.

## Useful references

- Official project: https://github.com/AGWA/git-crypt
- `man git-crypt` (if installed)
