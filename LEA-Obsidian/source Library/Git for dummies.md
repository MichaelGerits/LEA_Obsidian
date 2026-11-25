---

Asset type:
  - Coding
---
# Lambach Experimental Aircraft, Beginner’s Git Workflow & Project Guide

![[image.png]]

## What This Guide Is

This is a simplified, beginner-friendly version of the team’s Git workflow. It explains:

- What the project structure looks like
- How to work with Git without losing your work
- Which files you can edit and which are off-limits
- Common mistakes and how to fix them

> **Note:** The repository structure described here is the TARGET state, how the project should be organized as it matures. The current live repo may not yet match this exactly.

---

---

## 0. Prerequisites — Install Git, Log In, and Clone the Repo

### Install Git

- Windows: [https://git-scm.com/download/win](https://git-scm.com/download/win) then `git --version`
- Linux: `sudo apt install git` / `sudo dnf install git` then `git --version`

### Git identity

```bash
git config --global user.name "Your Name"
git config --global user.email "you@email.com"
```

### Log in to GitHub

- HTTPS + personal access token, `gh auth login`, or SSH (`ssh-keygen -t ed25519` + add to GitHub)

### Clone repo (after cd-ing to your folder)

```bash
cd ~/Documents       # Linux/macOS
# OR cd C:\\Users\\YourName\\Documents  # Windows
git clone https://github.com/GuilhermeCLdS/Lambach-Experimental-Aircraft.git
cd Lambach-Experimental-Aircraft
```

> Confirm with `pwd` (Linux/macOS) or `cd` (Windows) that you are inside the repo folder before any `git` command.

## 1. Big Picture: How This Project Is Organized

```plain text
Lambach-Experimental-Aircraft/
├── README.md                 # You are here (project intro)
├── requirements.txt          # Python packages needed by everyone
├── data/                     # Shared datasets (CSV, etc.) — shared by ALL versions
├── docs/                     # Reports, references, write-ups — shared by ALL versions
└── version_1/                # Each version has its own folder
    ├── main.py               # The ONLY file that wires everything together
    ├── figures/              # Plots and images this version produces
    ├── src/                  # All the code
    │   ├── global_parameters.py
    │   ├── drag_estimation/  # Example domain folder (one per feature area)
    │   │   ├── helpers/      # Small reusable functions
    │   │   ├── modules/      # Main phases (e.g., predictor, plotter)
    │   │   └── refactoring_wip/  # TEMPORARY: old unsplit code
    │   └── deprecated/       # Old code kept for reference — DO NOT EDIT
    └── tests/                # Mirrors src/ (same subfolder names, one level deep only)
        └── drag_estimation/  # Test files directly here (no helpers/modules)
```

### Key Rules of the Folder Structure

| Folder / File | What It Does | Who Edits It? |
| --- | --- | --- |
| `README.md` | Project introduction | Only admins (read-only for you) |
| `requirements.txt` | Shared Python dependencies | Only admins |
| `data/` | Shared datasets for ALL versions | Shared — ask before changing |
| `docs/` | Shared reports and references | Shared — ask before changing |
| `version_*/main.py` | Integrates modules for that version | Shared — follow separate-commit rule |
| `version_*/src/global_parameters.py` | Global settings | Shared — separate-commit rule applies |
| `version_*/src/<subfolder>/` | YOUR assigned work area | **Only YOU (within your assigned folder)** |
| `version_*/tests/<subfolder>/` | Tests matching your folder | Only YOU |

---

## 2. The Three-Tier Branch System (Simplified)

Think of branches as layers of a cake:

```plain text
TOP:    main        ← Most recent STABLE release. ONLY admins touch this.
        ↑
MIDDLE: dev         ← Active development for current version. Only merges from feature branches.
        ↑
BOTTOM: drag_estimation, stability, optimise  ← YOUR team branches (one per src/ subfolder)
```

### Branch Rules (Critical)

| Branch | Purpose | You Push To It? |
| --- | --- | --- |
| `main` | Latest stable release | **NO — NEVER** |
| `dev` | Integration and assembly | **NO — NEVER directly** (only via merge) |
| `drag_estimation` (example) | Your team’s feature work | **YES — this is your branch** |

> **Remember:** Each `src/` subfolder gets ONE branch named after it (e.g., `drag_estimation`, `stability`). There is NO branch called `deprecated`. The `deprecated/` folder is read-only and unmaintained.

---

## 3. Your Daily Workflow (As a Regular Contributor)

### Before You Start (Every Session!)

Always sync with BOTH sources before writing any code:

```bash
# 1. Make sure you're on YOUR branch
git checkout drag_estimation

# 2. Pull latest integrated work from dev (other teams' updates)
git pull origin dev

# 3. Pull latest work from teammates on YOUR same branch
git pull origin drag_estimation
```

Then verify:

```bash
git status
```

You should see:

```plain text
On branch drag_estimation
Your branch is up to date with 'origin/drag_estimation'.
nothing to commit, working tree clean
```

> **Safety Rule:** If `git pull` reports a conflict, **DO NOT PANIC**. Back up any uncommitted files, then resolve conflicts before continuing. See Edge Case B below.

---

### Step 2: Making Changes (Inside Your Folder ONLY)

**Golden Rule:** If you are on the `drag_estimation` branch, the ONLY files you should edit are inside:

- `version_1/src/drag_estimation/`
- `version_1/tests/drag_estimation/`

Never edit files in `src/stability/`, `src/optimise/`, or any other folder.

Once your changes are tested locally:

```bash
# Check exactly what changed
git status

# Stage ONLY your folder's changes
git add "version_1/src/drag_estimation/"

# Commit with a clear message
git commit -m "Add drag coefficient calculation for subsonic speed"
```

Verify:

```bash
git status
# Should say: nothing to commit, working tree clean
```

---

### Step 3: Updating Shared/Common Files (Separate Commit!)

Sometimes you need to update shared files like:

- `version_1/main.py` (to import your new module)
- `version_1/src/global_parameters.py` (to add a new parameter)

**Critical Rules for Shared Files:**

1. **Additive-Only Rule:** Only ADD new imports, function calls, or parameters. Never delete or refactor other teams’ code.
2. **Section Isolation:** Only edit within the designated section reserved for your module.
3. **Always commit shared files SEPARATELY from your domain work:**

```bash
# Stage shared/common files separately
git add version_1/main.py version_1/src/global_parameters.py

# Commit them separately
git commit -m "Integration: wire drag_estimation module into main.py and add parameter"
```

**Why separate commits?** If the shared file integration causes a problem, you can revert just that commit without losing all your domain calculations.

---

### Step 4: Submit Your Work

```bash
git push origin drag_estimation
```

You should see output like:

```plain text
To github.com:GuilhermeCLdS/Lambach-Experimental-Aircraft.git
   ... -> drag_estimation
```

---

## 4. Administrator / Moderator Workflow

### Merging a Feature Branch Into `dev`

When a feature branch is approved for integration:

```bash
# 1. Switch to dev and make sure it's current
git checkout dev
git pull origin dev

# 2. Merge the feature branch (e.g., drag_estimation)
git merge drag_estimation
```

**Conflict Resolution Strategy:**

For domain files (`src/drag_estimation/`): Keep incoming changes fully:

```bash
git checkout --theirs -- "version_1/src/drag_estimation/"
```

For OTHER subdirectories (`src/stability/`, `src/optimise/`): Keep `dev` versions intact:

```bash
git checkout --ours -- "version_1/src/stability/" "version_1/src/optimise/"
```

For shared files (`main.py`, `global_parameters.py`): Resolve conflicts MANUALLY in a text editor:

- Open the file
- Look for conflict markers: `<<<<<<< HEAD`, `=======`, `>>>>>>> branch-name`
- Combine both versions, delete the markers, and save
- Then stage and commit:

```bash
git add .
git commit -m "Integrate drag_estimation branch into dev"
git push origin dev
```

**Verification:** After merging, check that:

- `src/drag_estimation/` has the new updates
- Other subdirectories (`stability/`, `optimise/`) are untouched
- `main.py` contains combined imports/calls from all active modules

---

### Promoting `dev` to `main` (Version Release)

When `dev` holds a complete, validated, stable version:

```bash
# 1. Checkout main and pull latest stable
git checkout main
git pull origin main

# 2. Merge completed dev branch into main
git merge dev

# 3. Tag the release
git tag -a v1.0 -m "Release Version 1.0"

# 4. Push both branch and tags
git push origin main --tags
```

> **Only the system owner (admin) performs this step.**

---

## 5. Common Edge Cases & Fixes

### Edge Case A: Two Developers on the Same Branch (Intra-Branch Sync)

**Problem:** Developer A pushes to `drag_estimation`. Developer B pulls from `dev` but doesn’t see A’s updates (because `dev` hasn’t been merged yet).

**Solution:** Always pull from YOUR branch too:

```bash
git pull origin drag_estimation
```

This fetches A’s commits directly without waiting for a moderator merge.

---

### Edge Case B: Merge Conflict During `git pull origin dev`

**Problem:** You and another developer edited the same file. Git shows conflict markers.

**How to Identify:** Look for these markers in the file:

```plain text
<<<<<<< HEAD
  (your current version)
=======
  (incoming version from dev)
>>>>>>> origin/dev
```

**How to Fix:**

4. Open the file in your editor/IDE
5. Read both versions carefully
6. Keep the best of both (or combine them)
7. Delete ALL conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`)
8. Save the file
9. Resolve and commit:

```bash
git add <filename>
git commit -m "Resolved merge conflict with dev on <filename>"
```

---

### Edge Case C: Accidentally Edited Outside Your Folder

**Problem:** You edited `version_1/src/stability/some_file.py` while on `drag_estimation`.

**Fix:** Revert the accidental change BEFORE committing:

```bash
git checkout HEAD -- "version_1/src/stability/some_file.py"
```

This restores the file to its last committed state.

---

### Edge Case D: Uncommitted Changes Blocking Branch Switch

**Problem:** Git refuses to switch branches or pull because you have uncommitted changes.

**Solution — Stash Your Work Temporarily:**

```bash
# Save work temporarily
git stash

# Do your pull or checkout
git pull origin dev

# Re-apply stashed work
git stash pop
```

If `stash pop` creates conflicts, resolve them the same way as Edge Case B.

---

### Edge Case E: Reverting Shared File Edits Without Losing Domain Work

**Problem:** You made a bad edit to `main.py` in its separate commit. You want to undo it but keep your `drag_estimation` work.

**Solution:** Find the commit hash for the common-file commit, then revert it:

```bash
# Find the commit hash
git log --oneline

# Revert only that specific commit
git revert <commit-hash>
```

This creates a new commit that undoes the bad shared-file change while keeping everything else intact.

---

## 6. Naming Conventions (Important!)

Everything uses lowercase with underscores:

- Folders: `drag_estimation` (not `Drag Calc`)
- Files: `global_parameters.py` (not `GlobalParameters.py`)
- Branches: `drag_estimation`, `stability`, `optimise` (not `Drag Calc`, `Stability`)
- Exceptions: `README.md` (capitalized by convention)

> **Note:** The original document used names with spaces (`Drag Calc`) for readability, but Git branches and file paths work best with underscores and no spaces. The target naming is `drag_estimation` to match the repository.

---

## 7. File Rules Recap

### `main.py`

- The ONLY file where modules are assembled
- Never import or wire modules inside `src/` subfolders
- Only edit your designated section
- Always commit separately from your domain folder work

### `global_parameters.py`

- Shared settings for the version
- Only ADD parameters (never delete others’)
- Always commit separately

### `refactoring_wip/`

- **Temporary folder.** Holds existing code that hasn’t been split into one-class/one-function per file yet.
- Once files are properly split into `helpers/` and `modules/`, delete this folder.
- Treat it as a to-do list, not a permanent storage area.

### `deprecated/`

- Old code kept for reference
- Has NO branch of its own
- Read-only; do not edit
- If you need something from it, copy it to your working folder and refactor

### `tests/`

- Mirrors `src/` structure but only ONE level deep
- Example: `tests/drag_estimation/test_drag.py`
- NO `helpers/` or `modules/` folders inside `tests/`

---

## 8. Quick Command Reference

### For Contributors

| Goal | Commands |
| --- | --- |
| Start daily work | `git checkout drag_estimation` → `git pull origin dev` → `git pull origin drag_estimation` |
| Check status | `git status` |
| Save domain work | `git add version_1/src/drag_estimation/` → `git commit -m "message"` |
| Save common files (separate) | `git add version_1/main.py ...` → `git commit -m "Integration: ..."` |
| Submit work | `git push origin drag_estimation` |

### For Moderators

| Goal | Commands |
| --- | --- |
| Integrate feature branch | `git checkout dev` → `git pull origin dev` → `git merge drag_estimation` |
| Resolve domain files | `git checkout --theirs -- "version_1/src/drag_estimation/"` |
| Preserve other folders | `git checkout --ours -- "version_1/src/stability/"` |
| Resolve shared files | Manually edit `main.py`, delete markers, `git add .` → `git commit` |

### For Administrators

| Goal | Commands |
| --- | --- |
| Release to main | `git checkout main` → `git pull origin main` → `git merge dev` → `git tag -a v1.0 -m "Release"` → `git push origin main --tags` |

---

## 9. Potential Issues & How This Guide Fixes Them

The original workflow document contained some inconsistencies that could confuse beginners or cause errors. This guide addresses them:

| Issue in Original | Fix in This Guide |
| --- | --- |
| Branch names used spaces (`"Drag Calc"`) | Uses underscore naming (`drag_estimation`) to match actual repository and avoid Git path issues |
| Confusion about `dev` branch permissions (described as both read-only and allowing merges) | Clearly separates contributor (`main`/feature branches), moderator (`dev` merges), and admin (`main` releases) roles |
| Ambiguous instructions about `main.py` integration | Explicitly states it’s the ONLY integration point and requires a separate commit |
| No mention of `deprecated/` having no branch | Explicitly states `deprecated/` is unmaintained and has no branch |
| Unclear about `tests/` depth limitation | Clearly states `tests/` mirrors `src/` but is only one level deep (no helpers/modules) |
| Missing explanation of why separate commits for shared files are required | Added explanation: allows clean reversion without losing domain work |
| No clarification that `version_*/` folders contain both `src/` and `tests/` | Added in folder structure diagram |

---

## 10. Final Checklist Before Pushing

Before you push any commit:

- [ ] Am I on the correct branch? (`git branch`)
- [ ] Did I only edit files inside my assigned `src/` subfolder?
- [ ] Did I commit domain files separately from shared/common files?
- [ ] Are my commit messages descriptive?
- [ ] Did I pull from both `dev` and my feature branch before starting?
- [ ] Is my working tree clean? (`git status` shows nothing uncommitted in my domain)

If all boxes are checked, you are ready to push safely.

---

*This guide targets beginners and is designed for readability. When in doubt, ask a moderator before pushing to any branch other than your assigned feature branch.*