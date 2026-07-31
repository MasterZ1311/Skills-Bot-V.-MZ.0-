# 🚀 Antigravity Awesome Skills — Complete Guide

> The definitive reference for using the world's largest agentic skills library across every role and workflow.

---

## What Is This?

**Antigravity Awesome Skills** is a community-maintained library of **1,900+ agentic skills** designed to work across every major AI coding assistant:

| Agent | Install Command |
|---|---|
| **Antigravity IDE** | `npx antigravity-awesome-skills -- antigravity` |
| **Claude Code** | `npx antigravity-awesome-skills -- claude` |
| **Cursor** | `npx antigravity-awesome-skills -- cursor` |
| **Gemini CLI** | `npx antigravity-awesome-skills -- gemini` |
| **Codex CLI** | `npx antigravity-awesome-skills -- codex` |
| **Custom Path** | `npx antigravity-awesome-skills -- path ./my-skills` |

📊 **22,000+ GitHub stars · 3,800+ forks · v13.13.0 (July 2026)**

Each skill is a self-contained `SKILL.md` file — a battle-tested instruction set that turns your AI assistant into a domain expert for a specific task.

---

## ⚡ Quick Install (30 Seconds)

### Step 1 — Install Node.js (if not already installed)

Download from [nodejs.org](https://nodejs.org) or use a version manager:
```bash
# Windows (winget)
winget install OpenJS.NodeJS.LTS

# macOS / Linux (nvm)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
nvm install --lts
```

### Step 2 — Run the installer for your IDE

```bash
# Antigravity IDE (installs to C:\Users\<you>\.agents\skills\)
npx antigravity-awesome-skills -- antigravity

# Claude Code (installs to ./.claude/skills/ in your project)
npx antigravity-awesome-skills -- claude

# Cursor (installs to ./.cursor/skills/)
npx antigravity-awesome-skills -- cursor

# Gemini CLI (installs to ~/.gemini/skills/)
npx antigravity-awesome-skills -- gemini

# Install EVERYTHING to a custom path
npx antigravity-awesome-skills -- path ./my-skills
```

> **⚠️ Context Overload Warning:** Installing ALL ~1,900 skills at once may hit context window limits.
> Use targeted installs with filters if your agent struggles:
> ```bash
> npx antigravity-awesome-skills --category development,backend --risk safe,none
> npx antigravity-awesome-skills --category frontend --path ./.agents/skills
> npx antigravity-awesome-skills --tags react,nextjs,typescript
> ```

### Step 3 — Use a skill in your AI assistant

Type `@skill-name` in your prompt and your AI will load that skill's instructions:

```
@brainstorming help me plan the data model for a multi-tenant SaaS

@security-auditor review the authentication flow in src/auth/

@api-design-principles review the REST endpoints in routes/
```

---

## 🎯 Role-Based Starter Bundles

Rather than loading all 1,900 skills, install curated bundles by role:

| Bundle | Skills Included |
|---|---|
| **Essentials** | `@brainstorming`, `@architecture`, `@debugging-strategies`, `@doc-coauthoring`, `@create-pr` |
| **Web Wizard** | `@frontend-design`, `@api-design-principles`, `@lint-and-validate`, `@create-pr` |
| **Full Stack** | `@backend-architect`, `@react-best-practices`, `@database-design`, `@e2e-testing` |
| **Security Engineer** | `@security-auditor`, `@lint-and-validate`, `@debugging-strategies` |
| **DevOps** | `@docker-expert`, `@github-actions-advanced`, `@kubernetes-architect`, `@terraform-specialist` |

---

## 📂 How This Documentation Is Organized

```
📁 Skills Bot/
├── README.md                          ← You are here
├── docs/
│   ├── 00-how-to-invoke-skills.md    ← How @skill-name invocation works
│   ├── roles/
│   │   ├── 01-full-stack-developer.md
│   │   ├── 02-frontend-developer.md
│   │   ├── 03-backend-developer.md
│   │   ├── 04-mobile-app-developer.md
│   │   ├── 05-devops-cicd-engineer.md
│   │   ├── 06-cloud-infrastructure-architect.md
│   │   ├── 07-database-engineer.md
│   │   ├── 08-security-engineer.md
│   │   ├── 09-qa-testing-engineer.md
│   │   ├── 10-ai-ml-engineer.md
│   │   ├── 11-data-engineer.md
│   │   ├── 12-systems-programmer.md
│   │   ├── 13-game-developer.md
│   │   ├── 14-product-manager.md
│   │   ├── 15-ui-ux-designer.md
│   │   ├── 16-technical-writer.md
│   │   ├── 17-seo-growth-marketer.md
│   │   ├── 18-agent-orchestration-engineer.md
│   │   ├── 19-web3-blockchain-developer.md
│   │   └── 20-saas-startup-operator.md
│   └── workflows/
│       ├── wf-01-build-a-full-stack-app.md
│       ├── wf-02-build-a-mobile-app.md
│       ├── wf-03-launch-saas-mvp.md
│       ├── wf-04-secure-and-audit-codebase.md
│       ├── wf-05-add-ai-features.md
│       ├── wf-06-setup-cicd-devops.md
│       └── wf-07-debug-and-optimize.md
```

---

## 🌟 The 10 Skills Everyone Should Know First

| Skill | What It Does | When to Use |
|---|---|---|
| `@brainstorming` | Structured planning before writing code | Start of any new feature or project |
| `@architecture` | System design and component structure | Before writing the first line of code |
| `@debugging-strategies` | Systematic troubleshooting playbooks | When you're stuck on a bug |
| `@api-design-principles` | API shape, consistency, versioning | Designing any REST/GraphQL API |
| `@security-auditor` | Security-focused code review | Before any production release |
| `@lint-and-validate` | Lightweight quality checks | After writing any code |
| `@create-pr` | Packages work into clean pull requests | End of every feature branch |
| `@doc-coauthoring` | Structured technical documentation | After building any feature |
| `@clean-code` | Refactoring and code quality patterns | During and after development |
| `@tdd` | Test-driven development workflow | When you want bulletproof code |

---

## 📖 Quick Reference — Navigate by Role

- 👨‍💻 [Full Stack Developer](docs/roles/01-full-stack-developer.md)
- 🎨 [Frontend Developer](docs/roles/02-frontend-developer.md)
- ⚙️ [Backend Developer](docs/roles/03-backend-developer.md)
- 📱 [Mobile App Developer](docs/roles/04-mobile-app-developer.md)
- 🔧 [DevOps / CI-CD Engineer](docs/roles/05-devops-cicd-engineer.md)
- ☁️ [Cloud Infrastructure Architect](docs/roles/06-cloud-infrastructure-architect.md)
- 🗄️ [Database Engineer](docs/roles/07-database-engineer.md)
- 🔒 [Security Engineer](docs/roles/08-security-engineer.md)
- ✅ [QA & Testing Engineer](docs/roles/09-qa-testing-engineer.md)
- 🤖 [AI / ML Engineer](docs/roles/10-ai-ml-engineer.md)
- 📊 [Data Engineer](docs/roles/11-data-engineer.md)
- 🖥️ [Systems Programmer](docs/roles/12-systems-programmer.md)
- 🎮 [Game Developer](docs/roles/13-game-developer.md)
- 📋 [Product Manager](docs/roles/14-product-manager.md)
- 🎭 [UI/UX Designer](docs/roles/15-ui-ux-designer.md)
- ✍️ [Technical Writer](docs/roles/16-technical-writer.md)
- 📈 [SEO & Growth Marketer](docs/roles/17-seo-growth-marketer.md)
- 🤝 [Agent Orchestration Engineer](docs/roles/18-agent-orchestration-engineer.md)
- ⛓️ [Web3 / Blockchain Developer](docs/roles/19-web3-blockchain-developer.md)
- 🚀 [SaaS Startup Operator](docs/roles/20-saas-startup-operator.md)

---

## 🔄 End-to-End Workflow Guides

- [Build a Full Stack App from Scratch](docs/workflows/wf-01-build-a-full-stack-app.md)
- [Build a Mobile App (React Native / iOS / Android)](docs/workflows/wf-02-build-a-mobile-app.md)
- [Launch a SaaS MVP](docs/workflows/wf-03-launch-saas-mvp.md)
- [Secure & Audit a Codebase](docs/workflows/wf-04-secure-and-audit-codebase.md)
- [Add AI Features to an Existing App](docs/workflows/wf-05-add-ai-features.md)
- [Set Up CI/CD & DevOps Pipeline](docs/workflows/wf-06-setup-cicd-devops.md)
- [Debug, Profile & Optimize a Slow App](docs/workflows/wf-07-debug-and-optimize.md)

# Skills-Bot-V.-MZ.0-
