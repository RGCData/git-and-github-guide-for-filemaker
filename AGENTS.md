# AGENTS.md

This file is a reusable companion to the public guide, **From Project Folder to GitHub**. It is meant to be copied into the root of a project and then adjusted for that project.

Guide author: Alex Seidler, RGC Data LLC  
Contact: alex@rgcdata.com  
Website: https://rgcdata.com/

## How to use this file

This is guidance for Codex and other folder-based AI agents. It is not a credential file and it does not grant GitHub access.

Before doing project work:

1. Read this file.
2. Read the project's README and package manifest.
3. Check the current Git branch and working tree.
4. Ask the project owner to confirm the requested scope.
5. Use the guide's order: install and authenticate, create the repository, prepare the folder, commit, connect the remote, push, then use branches for ongoing work.

The project owner must complete GitHub sign-in in their own browser or terminal. Never ask them to paste a password, token, private key, or secret environment file into chat.

## Project details to fill in

- Project name: <!-- fill in -->
- Local project path: <!-- fill in -->
- GitHub owner: <!-- personal account or organization -->
- GitHub repository: <!-- owner/repository -->
- Repository visibility: private by default
- Default branch: \x60main\x60
- Package manager: <!-- pnpm, npm, yarn, or other -->
- Build command: <!-- for example, pnpm build -->
- Test command: <!-- for example, pnpm test -->
- Lint command: <!-- for example, pnpm lint -->

## Step 1: Check the computer and the login

Do not install software or change accounts without the owner's direction.

Useful checks:

\x60\x60\x60powershell
git --version
gh --version
gh auth status
\x60\x60\x60

If Git is missing, direct the owner to the official Git for Windows page:

https://git-scm.com/install/windows

GitHub CLI is optional:

https://cli.github.com/

If GitHub CLI is installed, the owner can authenticate with:

\x60gh auth login\x60

The recommended choices are GitHub.com, HTTPS, and browser authentication. If the owner uses more than one GitHub account, they can use:

\x60\x60\x60powershell
gh auth switch
\x60\x60\x60

The agent must not handle the credentials.

## Step 2: Create the correct GitHub thing

A repository is the online home for the project. A GitHub App, OAuth App, or other application is a separate integration and is not needed just to upload project files.

If the owner is using the GitHub website:

1. Open https://github.com/new.
2. Select the correct owner.
3. Enter the repository name and description.
4. Choose \x60Private\x60 unless the owner has explicitly approved publication.
5. For an existing local project, do not pre-create a README, \x60.gitignore\x60, or license unless the owner understands the resulting merge situation.
6. Create the repository.
7. Copy the HTTPS repository URL from the Quick Setup page.

If the owner asks the agent to create a repository through GitHub CLI, confirm the owner and visibility first:

\x60\x60\x60powershell
gh repo create OWNER/REPOSITORY --private
\x60\x60\x60

Do not create a repository, GitHub App, organization, team, or collaborator without explicit approval.

## Step 3: Inspect the project before staging

Run:

\x60\x60\x60powershell
git status --short --branch
Get-ChildItem -Force
\x60\x60\x60

Look for:

- \x60.env\x60 and \x60.env.*\x60 files
- private keys such as \x60.pem\x60, \x60.key\x60, \x60.p12\x60, or \x60.pfx\x60
- passwords, tokens, API keys, certificates, and database exports
- customer or production data
- \x60node_modules\x60, \x60dist\x60, \x60build\x60, coverage, and other generated folders
- large binary files
- local IDE settings that should not be shared

A normal web project \x60.gitignore\x60 often includes:

\x60\x60\x60text
node_modules/
dist/
build/
.env
.env.*
!.env.example
*.pem
*.key
*.p12
*.pfx
\x60\x60\x60

Never add a real secret to \x60.env.example\x60. If a secret has already been committed, stop, tell the owner, and recommend rotating it before publication. Removing it from the latest file does not remove it from Git history.

## Step 4: Run project checks

Use the package manager and scripts already defined by the project. Do not invent a package manager when a lockfile makes the choice clear.

Examples:

\x60\x60\x60powershell
pnpm install
pnpm build
pnpm lint
pnpm test
\x60\x60\x60

Report commands that do not exist instead of treating that as a failure.

## Step 5: Initialize and make the first commit

Only do this after the owner has reviewed the files that will be staged:

\x60\x60\x60powershell
git init -b main
git config user.name "Owner Name"
git config user.email "owner@example.com"
git status
git add .
git status
git diff --cached --check
git diff --cached
git commit -m "Initial project version"
\x60\x60\x60

The second \x60git status\x60 and the staged diff are the review points. If a sensitive or unexpected file appears, stop and remove it from staging:

\x60\x60\x60powershell
git restore --staged path/to/file
\x60\x60\x60

Do not use \x60git add -A\x60 or \x60git commit -am\x60 to hide what is being staged.

## Step 6: Connect and push

For a new remote:

\x60\x60\x60powershell
git remote add origin https://github.com/OWNER/REPOSITORY.git
git remote -v
git push -u origin main
\x60\x60\x60

If \x60origin\x60 already exists:

\x60\x60\x60powershell
git remote set-url origin https://github.com/OWNER/REPOSITORY.git
git remote -v
\x60\x60\x60

The correct command is \x60git push -u origin main\x60. The \x60origin\x60 part names the remote repository and \x60main\x60 names the branch. The \x60-u\x60 remembers the relationship so later \x60git push\x60 is usually enough.

Do not change an SSH remote to HTTPS or HTTPS to SSH without checking what authentication method the owner intends to use. HTTPS with browser authentication is usually simpler for a beginner. A \x60403\x60 means the authenticated GitHub account does not have the required access to the repository. A \x60Permission denied (publickey)\x60 error means SSH is not using a key GitHub recognizes.

Only push when the owner explicitly asks. A pull request is preferred for meaningful work after the initial upload.

## Step 7: Permissions and privacy

Private repositories should remain private unless the owner deliberately approves publication.

For an organization-owned repository, access is normally managed in the repository's Settings under Collaborators and teams or Manage access. Explain roles before changing them:

- Read: view and clone
- Write: push branches and contribute
- Maintain: manage much of the repository without the most sensitive administration
- Admin: manage access and repository settings

Use the smallest role that allows the work. Do not grant Admin just because someone needs to push code.

Do not change visibility, add collaborators, create teams, or alter branch protection without explicit approval.

## Step 8: Ongoing work with branches

Keep \x60main\x60 stable. Before starting work:

\x60\x60\x60powershell
git switch main
git pull --rebase
git switch -c feature/short-description
\x60\x60\x60

After making and testing changes:

\x60\x60\x60powershell
git status
git add .
git diff --cached
git commit -m "Describe the change"
git push -u origin feature/short-description
\x60\x60\x60

Open a pull request, explain the change, review it, and merge it into \x60main\x60. If another person is working on a different area, each person should use a separate branch. If the same lines are edited, resolve the conflict together and run the checks again.

## Step 9: Agent reporting standard

At the end of a task, report:

- What changed
- Which files changed
- Which commands and checks ran
- Whether the checks passed
- Any secrets, permissions, or merge risks noticed
- Whether anything was committed or pushed

Do not claim that a push succeeded unless the command actually completed successfully.

The owner remains responsible for GitHub account access, repository permissions, credentials, and the decision to publish.

