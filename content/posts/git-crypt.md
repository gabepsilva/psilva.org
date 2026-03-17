+++
date = '2026-03-17T00:00:00-04:00'
draft = false
title = 'Git-Crypt Notes'
tags = ['git', 'dev', 'devops', 'devsecops']
categories = ['engineering']
image = "/images/git-Crypt.png"
featuredImage = "/images/git-Crypt.png"
+++

# Git-Crypt Notes

## What it is

`git-crypt` lets you keep secrets in your Git repository while only decrypting them for authorized users.

- Plaintext is committed to Git as encrypted blobs.
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

### Included benefits of GPG

GPG is the practical default for `git-crypt` teams because it ties access to a real identity you already manage in your keyring.

- New developers can start decrypting secrets as soon as their GPG key is approved; no shared team secret has to be copied around.
- Access is easier to reason about than a single symmetric key because you can add and remove users without reworking everyone’s local setup.
- Rotation is safer: remove a key or replace a recipient instead of rewriting all secrets around one shared password.
- Auditing is cleaner because you can trace who got access via key approval history and repository commits.

### Symmetric key (single/team-owned)

```bash
git-crypt export-key ./key.txt
echo "my-key-file" > .git-crypt/keys/...
```

> Prefer GPG recipients whenever possible; symmetric keys are harder to rotate and audit.

## Encrypted status and file handling

- Files in matching `.gitattributes` paths are automatically encrypted on commit.
- To force re-encryption after changing patterns:

```bash
git add -u
git commit -m "Re-encrypt tracked files"
```

To check status:

```bash
git-crypt status
```

## Unlock in a new clone

After cloning:

```bash
git-crypt unlock
```

If you have the right GPG keys in your keyring, files decrypt automatically.

## Common gotchas

- Ensure `.gitattributes` is committed before sensitive files, or they may already be in history as plaintext.
- Keep `.git-crypt/` metadata out of the repo root (it is internal; do not edit manually).
- Rotate keys when team membership changes:

```bash
git-crypt lock
git-crypt unlock
git add -u
git commit -m "Rotate git-crypt keys"
```

## Useful references

- Official project: https://github.com/AGWA/git-crypt
- `man git-crypt` (if installed)
