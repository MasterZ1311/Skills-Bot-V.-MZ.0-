# Antigravity Awesome Skills — Complete Guide

> A comprehensive reference for utilizing the agentic skills library across development roles and workflows.

---

## Overview

**Antigravity Awesome Skills** is a community-maintained library of over 1,900 agentic skills designed for seamless integration across major AI coding assistants.

| Assistant / Agent | Installation Command |
|---|---|
| **Antigravity IDE** | `npx antigravity-awesome-skills -- antigravity` |
| **Claude Code** | `npx antigravity-awesome-skills -- claude` |
| **Cursor** | `npx antigravity-awesome-skills -- cursor` |
| **Gemini CLI** | `npx antigravity-awesome-skills -- gemini` |
| **Codex CLI** | `npx antigravity-awesome-skills -- codex` |
| **Custom Location** | `npx antigravity-awesome-skills -- path ./my-skills` |

**22,000+ Stars · 3,800+ Forks · Version 13.13.0**

Each skill is encapsulated in a self-contained `SKILL.md` instruction set that enables your AI assistant to perform domain-specific tasks with high accuracy and consistency.

---

## Quick Installation

### Step 1: Install Node.js

Ensure Node.js (LTS version recommended) is installed:

```bash
# Windows (winget)
winget install OpenJS.NodeJS.LTS

# macOS / Linux (nvm)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
nvm install --lts
```

### Step 2: Run the Installer

Select the command corresponding to your target environment:

```bash
# Antigravity IDE (installs to C:\Users\<user>\.agents\skills\)
npx antigravity-awesome-skills -- antigravity

# Claude Code (installs to ./.claude/skills/ in your project)
npx antigravity-awesome-skills -- claude

# Cursor (installs to ./.cursor/skills/)
npx antigravity-awesome-skills -- cursor

# Gemini CLI (installs to ~/.gemini/skills/)
npx antigravity-awesome-skills -- gemini

# Custom target directory
npx antigravity-awesome-skills -- path ./my-skills
```

> **Note on Context Limits:** Installing the full library of ~1,900 skills simultaneously may exceed AI context window boundaries. It is recommended to use filtered installations:
> ```bash
> npx antigravity-awesome-skills --category development,backend --risk safe,none
> npx antigravity-awesome-skills --category frontend --path ./.agents/skills
> npx antigravity-awesome-skills --tags react,nextjs,typescript
> ```

### Step 3: Invoke Skills in AI Assistants

Reference skills directly in your prompt using `@skill-name`:

```
@brainstorming help me plan the data model for a multi-tenant SaaS application

@security-auditor review the authentication flow in src/auth/

@api-design-principles review the REST endpoints in routes/
```

---

## Role-Based Starter Bundles

To optimize context usage, install targeted bundles tailored to specific engineering roles:

| Bundle | Included Skills |
|---|---|
| **Essentials** | `@brainstorming`, `@architecture`, `@debugging-strategies`, `@doc-coauthoring`, `@create-pr` |
| **Web Development** | `@frontend-design`, `@api-design-principles`, `@lint-and-validate`, `@create-pr` |
| **Full Stack** | `@backend-architect`, `@react-best-practices`, `@database-design`, `@e2e-testing` |
| **Security Engineering** | `@security-auditor`, `@lint-and-validate`, `@debugging-strategies` |
| **DevOps & Infrastructure** | `@docker-expert`, `@github-actions-advanced`, `@kubernetes-architect`, `@terraform-specialist` |

---

## Repository Structure

```
Skills Bot/
├── README.md                          # Main repository documentation
└── docs/
    ├── 00-how-to-invoke-skills.md    # Skill invocation reference
    ├── roles/                         # Role-specific guide documents (01-20)
    └── workflows/                     # End-to-end operational workflows (wf-01 to wf-07)
```

---

## Core Skills Summary

Below are ten foundational skills recommended for general software development tasks:

| Skill Name | Function | Primary Use Case |
|---|---|---|
| `@brainstorming` | Structured technical planning | Project initialization and feature ideation |
| `@architecture` | System design and component structuring | Architectural planning before implementation |
| `@debugging-strategies` | Systematic troubleshooting procedures | Resolving complex software bugs |
| `@api-design-principles` | API schema design and versioning | RESTful and GraphQL API specification |
| `@security-auditor` | Security vulnerability analysis | Pre-release code auditing |
| `@lint-and-validate` | Code quality verification | Post-implementation code review |
| `@create-pr` | Pull request compilation | Packaging changes for code review |
| `@doc-coauthoring` | Technical documentation generation | Post-feature documentation |
| `@clean-code` | Refactoring guidelines | Code readability and maintenance |
| `@tdd` | Test-driven development | Standardized test creation |

---

## Role-Based Documentation

- [Full Stack Developer](docs/roles/01-full-stack-developer.md)
- [Frontend Developer](docs/roles/02-frontend-developer.md)
- [Backend Developer](docs/roles/03-backend-developer.md)
- [Mobile App Developer](docs/roles/04-mobile-app-developer.md)
- [DevOps / CI-CD Engineer](docs/roles/05-devops-cicd-engineer.md)
- [Cloud Infrastructure Architect](docs/roles/06-cloud-infrastructure-architect.md)
- [Database Engineer](docs/roles/07-database-engineer.md)
- [Security Engineer](docs/roles/08-security-engineer.md)
- [QA & Testing Engineer](docs/roles/09-qa-testing-engineer.md)
- [AI / ML Engineer](docs/roles/10-ai-ml-engineer.md)
- [Data Engineer](docs/roles/11-data-engineer.md)
- [Systems Programmer](docs/roles/12-systems-programmer.md)
- [Game Developer](docs/roles/13-game-developer.md)
- [Product Manager](docs/roles/14-product-manager.md)
- [UI/UX Designer](docs/roles/15-ui-ux-designer.md)
- [Technical Writer](docs/roles/16-technical-writer.md)
- [SEO & Growth Marketer](docs/roles/17-seo-growth-marketer.md)
- [Agent Orchestration Engineer](docs/roles/18-agent-orchestration-engineer.md)
- [Web3 / Blockchain Developer](docs/roles/19-web3-blockchain-developer.md)
- [SaaS Startup Operator](docs/roles/20-saas-startup-operator.md)

---

## End-to-End Workflow Guides

- [Build a Full Stack App from Scratch](docs/workflows/wf-01-build-a-full-stack-app.md)
- [Build a Mobile App (React Native / iOS / Android)](docs/workflows/wf-02-build-a-mobile-app.md)
- [Launch a SaaS MVP](docs/workflows/wf-03-launch-saas-mvp.md)
- [Secure & Audit a Codebase](docs/workflows/wf-04-secure-and-audit-codebase.md)
- [Add AI Features to an Existing App](docs/workflows/wf-05-add-ai-features.md)
- [Set Up CI/CD & DevOps Pipeline](docs/workflows/wf-06-setup-cicd-devops.md)
- [Debug, Profile & Optimize a Slow App](docs/workflows/wf-07-debug-and-optimize.md)
