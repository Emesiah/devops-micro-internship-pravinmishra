# Assignment 6 — Building an AI-Assisted Git Safety Net (PR Ready Check)

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In Week 2 you built Claude Code hooks that block a dangerous action *before* it happens (`PreToolUse`), and a restricted skill that could look but not touch (`allowed-tools` without `Write`). In this assignment you will discover that Git has the exact same idea, decades older: a **pre-commit hook** that blocks a commit before it's created.

You will build both halves of a real "PR Ready" workflow:

1. A **Git hook that follows fixed rules** — scans staged changes for hardcoded secrets and oversized files and refuses the commit. No AI involved, no guessing, just a rule that gives the same answer every time.
2. A **restricted Claude Code skill** (`/pr-ready`) that reads your staged diff and drafts a Pull Request title, description, and a short list of things worth a second look — the kind of judgment a fixed rule can't make (mixed changes, missing context, unclear intent). The skill never commits, pushes, or opens the PR. You do that yourself, using its draft as a starting point.

This mirrors the Agentic Loop from Week 3's Linux triage assignment: **Gather → Analyze → Human Act → Verify**. The hook and the skill both gather and analyze; only you act.

---

# Task 0 — Confirm Your Fork and Create a Feature Branch

## Goal

Confirm you are working in your own fork, then create a dedicated branch for this assignment.

### Evidence

#### Screenshot 1 — Output of git remote -v and git branch showing the new branch

![Screenshot 1](week-04-Assignmen-06-screenshot1.png)

---

### Notes

**1. Why create a dedicated branch instead of doing this work on main?**

I use a dedicated branch so changes can be developed, tested, and reviewed safely before they reach main.

---

# Task 1 — Stage a Change With Realistic Risk

## Goal

On your own fork of this repository (the one you've been submitting your DMI work in since onboarding), create a new branch and stage a change that a real reviewer should catch: a hardcoded-looking secret and a leftover debug statement.

### Evidence

#### Screenshot 1 — Output of  `git status` showing the staged file on feature/ai-pr-ready

![ Output of  `git status` showing the staged file on feature/ai-pr-ready](week-04-Assignmen-06-screenshot2-1.png)

---

### Notes

**1. Why does this assignment use an obviously fake key instead of a real one?**

They use a fake key intentionally because the assignment is designed to teach us how to detect and prevent secrets from being committed to Git without putting a real credential at risk.

---

# Task 2 — Write a Real Git Pre-Commit Hook

## Goal

Create a tracked, shareable pre-commit hook that blocks a commit containing secret-like patterns or files over 1MB.

### Evidence

#### Screenshot 2 — `hooks/pre-commit` open in VS Code showing the full script

![ Screenshot 2](week-04-Assignmen-06-screenshot2-2.png)

---

#### Screenshot 3 —git` confirming it points to `hooks`

![Screenshot 3](week-04-Assignmen-06-screenshot3.png)

---

### Notes

**1. Why is `hooks/pre-commit` tracked in the repo instead of living only in `.git/hooks/`?**

Because .git/hooks/ is intentionally local to each Git clone and is not tracked by Git.

---

**2. Compare this to `PreToolUse` from Week 2 Assignment 6. What does each one intercept, and what do they have in common?**

Git pre-commit intercepts commits before they are recorded in Git history, while Claude PreToolUse intercepts Claude's tool calls before they are executed; both provide a programmable safety checkpoint that can prevent an action from proceeding.

---

# Task 3 — Prove the Hook Blocks the Risky Commit

## Goal

Attempt to commit the staged file from Task 1 and show the hook rejecting it.

### Evidence

#### Screenshot 4 — Terminal showing `git commit` rejected with the hook's "BLOCKED" message naming the exact file

![Screenshot4](week-04-Assignmen-06-screenshot4.png)

---

### Notes

**1. Which line in `hooks/pre-commit` matched your fake key, and why did it match?**

The line containing the GitHub-token regex matched the fake ghp_... key because the fake key had the same structural pattern as a real GitHub Personal Access Token.

---

**2. Could this hook have caught a poorly-named variable that stores a secret without the `AKIA` prefix? What does that tell you about the limits of a fixed rule like this?**

Yes. A fixed rule like this could miss a secret if the value does not contain the specific pattern the hook is looking for, such as the AKIA prefix used by AWS access-key IDs.

---

# Task 4 — Build the `/pr-ready` Skill

## Goal

Create a manually invoked Claude Code skill that reads your staged changes and produces a PR-readiness report and a draft PR description — without writing, committing, or pushing anything itself.

### Evidence

#### Screenshot 5 — `SKILL.md` frontmatter showing `allowed-tools: Bash, Read, Grep` (no `Write`) and `disable-model-invocation: true`

![Screenshot 5](week-04-Assignmen-06-screenshot5.png)

---

#### Screenshot 6 — `/pr-ready` output while the risky file is still staged, showing it flagged the secret and/or debug statement

![Screenshot 6](week-04-Assignmen-06-screenshot6.png)

---

### Notes

**1. Why does `/pr-ready` have `Bash` and `Read` but not `Write`?**

/pr-ready uses Read and Bash because it only needs to inspect files and run Git/review commands. It does not have Write permission because its job is to review and report issues, not modify project files. Removing Write follows the principle of least privilege and reduces the risk of the review agent making unintended changes.

---

**2. The pre-commit hook and `/pr-ready` both looked at the same staged diff. Did they flag the same things? What did one catch that the other didn't?**

the pre-commit hook checks specific danger signs, while /pr-ready looks at the bigger picture and can spot problems with the code and changes.

---

# Task 5 — Fix the Issues and Re-Verify

## Goal

Remove the secret and debug statement, then prove both gates now pass clean.

### Evidence

#### Screenshot 7 — `git commit` succeeding after the fix (no BLOCKED message)

![Screenshot 7](week-04-Assignmen-06-screenshot7-1.png)

---

#### Screenshot 8 — Second `/pr-ready` run showing a clean risk report and a drafted PR title + description

![Screenshot 8](week-04-Assignmen-06-screenshot8.png)

---

### Notes

**1. What exactly did you change to satisfy the pre-commit hook?**

I removed the hardcoded `AWS_ACCESS_KEY_ID` and the debug `echo` that exposed it. This cleared the secret-pattern check in the pre-commit hook and allowed the staged changes to pass.


---

# Task 6 — Push and Open a Pull Request Using the AI Draft

## Goal

Push your branch and open a real Pull Request, using `/pr-ready`'s drafted title and description as your starting point — read it critically and edit before you use it.

**Important:** Open this Pull Request with base repository set to **your own fork** — not the shared upstream `pravinmishraaws/devops-micro-internship-pravinmishra` repository. This assignment's hook and skill files are your own practice work, not a change meant for the shared class repo.

### Evidence

#### Screenshot 9 — Your Pull Request showing the base repository is your own fork, plus the title and description, with the `/pr-ready` draft visible for comparison (paste it in the PR conversation or your notes below)
![Screenshot 9](week-04-Assignmen-06-screenshot9.png)
---

#### PR Link

https://github.com/Emesiah/devops-micro-internship-interviews/pull/1

---

### Notes

**1. What, if anything, did you edit in the AI's drafted PR description before using it? Why?**

I removed anything that was no longer accurate and kept only details that matched my actual changes. This made the PR description clear, truthful, and easier for a reviewer to understand.

---

**2. If you had blindly copy-pasted the AI's draft without reading it, what could go wrong?**

If I blindly copied the AI draft, it could contain wrong or outdated information. I might submit a PR with incorrect details, missing changes, or claims about work I didn't actually do. That could confuse the reviewer and make the PR look careless.

---

**3. Why does this PR need to target your own fork instead of the shared upstream repository?**

Because the fork is my own copy of the project, I can safely test and review my changes there without directly changing the shared upstream repository. It also lets the maintainer review my PR before anything is merged into the main project.

---

# Task 7 — Map the Workflow to the Agentic Loop

## Goal

Explain this assignment's workflow using the same Gather → Analyze → Human Act → Verify structure from Week 3.

### Notes

**1. Which step(s) represent Gather?**

Gather: Collect the staged files, Git changes, and /pr-ready review results so the agent has the information needed to assess the work.
---

**2. Which step(s) represent Analyze?**

Analyze: /pr-ready examines the staged changes, checks for secrets, debug code, and other problems, then creates a suggested PR title and description.

---

**3. Which step is Human Act, and why must a human — not Claude — run `git commit`, `git push`, and open the PR?**

Human Act: The developer checks the AI’s work and decides whether it is safe and correct before committing, pushing, and creating the PR. This keeps important Git actions under human control.

---

**4. Which step is Verify?**

Verify: The developer checks the final Git status, commit history, pushed branch, and pull request to confirm everything is correct and ready for review.

---

**5. In one or two sentences: why do you need *both* the fixed-rule pre-commit hook and the AI skill? Isn't one enough?**

The pre-commit hook catches specific problems automatically, while the AI skill looks at the changes more generally. Using both gives better protection than using only one.

---

# Task 8 — LinkedIn Post

## Goal

Publish a LinkedIn post summarizing what you built and what you learned about combining fixed-rule safety checks with AI-assisted review.

### Evidence

#### LinkedIn Post URL

https://lnkd.in/p/d_uaHbME

---

## Key Learnings

Add 3-5 bullet points on what you learned this week.

-How to use Git hooks to prevent unsafe changes from being committed.
-How AI can review code and Git changes before creating a Pull Request
-Why developers should always check and approve AI-generated recommendations.
-How to make Git and DevOps workflows more secure through automation.

---

# Submission Instructions

- Ensure `hooks/pre-commit` and `.claude/skills/pr-ready/SKILL.md` are committed to your GitHub repository
- Add all required screenshots to your submission
- All written answers must be in your own words
- Do not use a real secret or credential anywhere in your submission — the fake key in Task 1 is intentional and must stay clearly fake
- Open your Pull Request against your own fork, not the shared upstream repository
- Push your final changes to your forked repository
- Include your PR link and LinkedIn post URL

---

## GitHub Repository URL

https://github.com/Emesiah/devops-micro-internship-interviews

https://github.com/Emesiah/

---

# Completion Checklist

- [ ] Branch `feature/ai-pr-ready` created with a staged file containing a fake secret and a debug statement
- [ ] `hooks/pre-commit` created and tracked in the repo (not only in `.git/hooks/`)
- [ ] `core.hooksPath` configured to point at `hooks/`
- [ ] Pre-commit hook shown blocking the risky commit
- [ ] `.claude/skills/pr-ready/SKILL.md` created with correct `allowed-tools` (no `Write`) and `disable-model-invocation: true`
- [ ] `/pr-ready` run against the risky diff and shown flagging issues
- [ ] Risky file fixed; `git commit` succeeds cleanly
- [ ] `/pr-ready` re-run showing a clean report and drafted PR title/description
- [ ] Pull Request opened using the AI draft as a starting point, with your own fork as the base repository (not upstream), PR link included
- [ ] Agentic Loop mapping (Task 7) completed in your own words
- [ ] LinkedIn post published and URL submitted
- [ ] All required screenshots added
- [ ] GitHub repository URL provided

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## 📌 Resources

- 🌐 DMI Official Website: https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme  
- 🎓 University: https://university.pravinmishra.com?utm_source=github&utm_medium=readme  
- 💬 Discord Community: https://discord.pravinmishra.com?utm_source=github&utm_medium=readme  
- 📝 Blog: https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme  
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*
