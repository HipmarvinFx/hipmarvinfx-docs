# HipMarvinFX — Research Log Push Guide

How to push any research file (weekly, daily update, week-end review,
position ledger) from your Downloads folder into the correct repo and
folder in one command.

---

## The script

`update-file.ps1` lives in the root of `hipmarvinfx-research-log`. It:
1. Creates the target week folder if it doesn't exist
2. Pulls latest from origin
3. Copies the file from your source path
4. Commits and pushes with your message
5. Shows the last 3 commits to confirm

---

## Weekly Research File (new week)

```powershell
cd C:\Users\user\hipmarvinfx-research-log
New-Item -ItemType Directory -Path "week-35" -ErrorAction SilentlyContinue
.\update-file.ps1 `
  -SourcePath "$env:USERPROFILE\Downloads\WEEK35_WEEKLY_RESEARCH.md" `
  -TargetRelativePath "week-35\WEEK35_WEEKLY_RESEARCH.md" `
  -CommitMessage "Week 35: weekly research file"
```

*(The script creates the folder automatically if it doesn't exist, but
`New-Item` above is a safe fallback if you prefer to be explicit.)*

---

## Daily Update File

```powershell
cd C:\Users\user\hipmarvinfx-research-log
.\update-file.ps1 `
  -SourcePath "$env:USERPROFILE\Downloads\WEEK35_DAILY_UPDATE.md" `
  -TargetRelativePath "week-35\WEEK35_DAILY_UPDATE.md" `
  -CommitMessage "Week 35: daily update — Day N closed"
```

---

## Week-End Review

```powershell
cd C:\Users\user\hipmarvinfx-research-log
.\update-file.ps1 `
  -SourcePath "$env:USERPROFILE\Downloads\WEEK35_WEEK_END_REVIEW.md" `
  -TargetRelativePath "week-35\WEEK35_WEEK_END_REVIEW.md" `
  -CommitMessage "Week 35: week-end review"
```

---

## Position Ledger (after any status change)

```powershell
cd C:\Users\user\hipmarvinfx-research-log
.\update-file.ps1 `
  -SourcePath "$env:USERPROFILE\Downloads\POSITION_LEDGER.md" `
  -TargetRelativePath "POSITION_LEDGER.md" `
  -CommitMessage "Position Ledger: [brief description of what changed]"
```

---

## Docs repo (methodology / template updates)

For `hipmarvinfx-docs` (templates, prompts, standing protocol), use the
same pattern but point at the docs repo:

```powershell
cd C:\Users\user\hipmarvinfx-docs
.\update-file.ps1 `
  -SourcePath "$env:USERPROFILE\Downloads\WEEKLY_RESEARCH_TEMPLATE_v4.md" `
  -TargetRelativePath "WEEKLY_RESEARCH_TEMPLATE_v4.md" `
  -CommitMessage "Add WEEKLY_RESEARCH_TEMPLATE_v4"
```

*(Note: `update-file.ps1` does not currently exist in `hipmarvinfx-docs`
— copy it there once if you want the same convenience there.)*

---

## Commit message conventions

| File type | Convention |
|---|---|
| Weekly research | `Week N: weekly research file` |
| Daily update | `Week N: daily update — Day N closed` |
| Week-end review | `Week N: week-end review` |
| Position ledger | `Position Ledger: [what changed]` |
| Template / prompt update | `[filename]: [one-line description of change]` |
| Schema migration | `chore: [migration description]` |
| Code fix | `fix: [what was broken and what fixed it]` |
