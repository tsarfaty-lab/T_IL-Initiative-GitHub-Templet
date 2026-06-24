# Essential Claude Code Skills for the T_IL Initiative

This document extends the T_IL initiative tool table with a dedicated **Claude Code layer**, focusing on capabilities that can be installed, configured, and used by students across T_IL repositories.

Claude Code supports skills via `SKILL.md`, invocable with `/skill-name`. Custom commands have effectively merged into skills, and bundled skills include `/code-review`, `/batch`, `/debug`, `/loop`, `/claude-api`, plus runtime-verification skills such as `/run` and `/verify`.

Reference: [Claude Code Skills documentation](https://docs.anthropic.com/en/docs/claude-code/skills)

---

## Claude Code Skills and Components for T_IL

| # | Claude Code skill / component | T_IL type | Role in the initiative | How it improves T_IL work |
|---:|---|---|---|---|
| 26 | **T_IL `CLAUDE.md` Project Memory** | Skill + Harness | Store repository-specific rules: architecture, build commands, coding standards, data restrictions, branch policy, naming conventions, and service map. | Gives every student and AI coding session persistent project context. Claude Code uses `CLAUDE.md` files and auto memory to carry instructions across sessions. |
| 27 | **Claude Code `/code-review` Skill** | Harness | Run structured reviews of student PRs: bugs, unclear logic, missing tests, unsafe file operations, poor modularity, and documentation gaps. | Adds a consistent AI review layer before human supervision. |
| 28 | **Claude Code `/debug` Skill** | Skill | Analyze failing scripts, Docker builds, tests, notebooks, API endpoints, and logs. | Helps students recover from runtime errors without waiting for the PI or senior developer. |
| 29 | **Claude Code `/loop` Skill** | Loop | Iteratively run, inspect, fix, and re-run until a target condition is met, such as passing tests or successful container startup. | Useful for repetitive repair cycles in microservices and student-generated code. |
| 30 | **Claude Code `/batch` Skill** | Loop + Skill | Apply similar changes across many repositories, for example README updates, dependency changes, issue-template edits, or service naming normalization. | Supports T_IL’s multi-repository structure without requiring repetitive manual editing. |
| 31 | **Claude Code `/run` Skill** | Harness | Launch an app, service, notebook workflow, or local Docker Compose environment and inspect whether the change works in practice. | Goes beyond static code review by testing whether a T_IL service actually runs. |
| 32 | **Claude Code `/verify` Skill** | Harness + Goal | Build and run the project to confirm that a code change behaves correctly. | Provides a concrete acceptance check before PR merge or student report submission. |
| 33 | **Claude Code `/run-skill-generator`** | Skill | Teach Claude how to build, launch, and verify a specific T_IL service. | Each repo can define its own run/verify behavior: FastAPI service, Nextflow workflow, Snakemake pipeline, QuPath export, MLflow experiment, or Docker service. |
| 34 | **T_IL SOW-to-GitHub Skill** | Skill + Goal | Custom `.claude/skills/sow-to-github/SKILL.md` that converts SOW sections into GitHub Issues, milestones, labels, dependencies, and Gantt Mermaid diagrams. | Automates the original use case: SOW → GitHub execution plan. |
| 35 | **T_IL Repo Auditor Skill** | Loop + Harness | Custom skill that checks all T_IL repositories for commits, stale branches, open PRs, CI failures, missing README sections, and overdue issues. | Enables the weekly “check every GitHub repo and email participants” loop. |
| 36 | **T_IL Weekly Update Skill** | Skill + Loop | Generate a weekly status report from GitHub Projects, Issues, PRs, commits, MLflow/DVC logs, and student notes. | Produces the PI-level summary plus student-specific action lists. |
| 37 | **Claude Code GitHub Action** | Hook + Loop | Run Claude Code inside GitHub Actions for PR review, issue responses, code changes, and automated workflow tasks. | Adds AI assistance directly inside the T_IL GitHub workflow. |
| 38 | **Claude Code Hooks: PreToolUse / PostToolUse** | Hook + Harness | Add deterministic controls before and after Claude uses tools: block unsafe commands, log actions, require tests after edits, and prevent accidental access to restricted paths. | Critical for student safety, clinical-data governance, and reproducibility. |
| 39 | **Claude Code Subagents** | Skill + Harness | Create specialized T_IL subagents: `pathomics-reviewer`, `radiomics-reviewer`, `cellomics-reviewer`, `omics-pipeline-reviewer`, `docker-k8s-reviewer`, and `documentation-reviewer`. | Keeps complex reviews isolated and domain-specific. |
| 40 | **Claude Code MCP Connectors for T_IL Tools** | Skill + Harness | Connect Claude Code to external systems: GitHub, MLflow, DVC storage, MongoDB metadata, issue trackers, dashboards, and internal APIs through MCP. | Lets Claude act on real project systems instead of relying on pasted context. |

---

## Recommended Claude Code Installation Pattern for Each T_IL Repository

| Repository file / folder | Purpose |
|---|---|
| `.claude/CLAUDE.md` | Project instructions, architecture, coding rules, build commands, data restrictions. |
| `.claude/skills/sow-to-github/SKILL.md` | Convert SOW into GitHub Issues, milestones, labels, and Gantt. |
| `.claude/skills/repo-audit/SKILL.md` | Check repo status, stale branches, missing docs, failed tests, overdue tasks. |
| `.claude/skills/weekly-update/SKILL.md` | Generate weekly student/PI update report. |
| `.claude/skills/readme-check/SKILL.md` | Validate README completeness and generate missing sections. |
| `.claude/skills/test-and-verify/SKILL.md` | Run pytest, Docker build, API smoke tests, notebook validation. |
| `.claude/agents/code-reviewer.md` | Dedicated PR/code review subagent. |
| `.claude/agents/data-validator.md` | Dedicated metadata, schema, and data-ingestion review subagent. |
| `.claude/settings.json` | Shared project-level permissions, hooks, MCP servers, and plugin settings. |
| `.github/workflows/claude.yml` | Claude Code GitHub Action workflow for PRs, issues, and scheduled audits. |

---

## Suggested `.claude/` Directory Structure

```text
.claude/
├── CLAUDE.md
├── settings.json
├── skills/
│   ├── sow-to-github/
│   │   └── SKILL.md
│   ├── repo-audit/
│   │   └── SKILL.md
│   ├── weekly-update/
│   │   └── SKILL.md
│   ├── readme-check/
│   │   └── SKILL.md
│   └── test-and-verify/
│       └── SKILL.md
└── agents/
    ├── code-reviewer.md
    ├── data-validator.md
    ├── pathomics-reviewer.md
    ├── radiomics-reviewer.md
    ├── cellomics-reviewer.md
    ├── omics-pipeline-reviewer.md
    ├── docker-k8s-reviewer.md
    └── documentation-reviewer.md
```

---

## Updated Priority Set for T_IL

For T_IL, Claude Code should be added in this order:

1. **Repository-level `CLAUDE.md`**
2. **`/code-review`, `/debug`, `/run`, `/verify`**
3. **Custom `sow-to-github` skill**
4. **Custom `repo-audit` skill**
5. **Custom `weekly-update` skill**
6. **Claude Code GitHub Action**
7. **PreToolUse / PostToolUse hooks**
8. **Domain-specific subagents**
9. **MCP connectors**
10. **Agent SDK automation for cross-repository reports**

---

## Practical T_IL Operating Loop

```text
SOW
  ↓
Claude Code SOW skill
  ↓
GitHub Issues / Projects / Milestones
  ↓
Student work
  ↓
Claude Code review / debug / verify
  ↓
GitHub Action audit
  ↓
Weekly update
  ↓
PI supervision
```

---

## Recommended First Implementation

The first practical implementation should include these files in one pilot repository, for example `TL_Pathomics` or `T_IL_Pathomics9`:

1. `.claude/CLAUDE.md`
2. `.claude/skills/sow-to-github/SKILL.md`
3. `.claude/skills/repo-audit/SKILL.md`
4. `.claude/skills/weekly-update/SKILL.md`
5. `.claude/skills/test-and-verify/SKILL.md`
6. `.claude/agents/code-reviewer.md`
7. `.github/workflows/claude.yml`

This creates a reproducible pattern that can later be cloned into the full T_IL repository ecosystem.
