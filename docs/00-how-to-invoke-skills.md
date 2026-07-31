# How to Invoke Skills — The Complete Guide

Understanding the `@skill-name` invocation system is the key to unlocking everything in this library.

---

## How It Works

When you type `@skill-name` in a prompt to your AI assistant, the agent:

1. **Locates** the `SKILL.md` file in your skills directory (e.g. `~/.agents/skills/skill-name/SKILL.md`)
2. **Reads** the full instructions, guidelines, and persona defined in that file
3. **Adopts** the expert persona and applies those methodologies to your task
4. **Executes** your request as that specialist would — with domain-appropriate patterns, tools, and standards

Think of each `@skill-name` as instantly putting on an expert's hat. `@security-auditor` makes your AI think like a security engineer. `@database-architect` makes it reason about data the way a 10-year database veteran would.

---

## Syntax Patterns

### Single skill
```
@skill-name your request here
```
**Example:**
```
@backend-architect Design the service layer for a multi-tenant billing system
```

### Multiple skills (chain in one prompt)
```
@skill-a @skill-b your request here
```
**Example:**
```
@database-design @prisma-expert Create a schema for a college event management system with users, events, registrations, and payments
```

### Skill + context
```
@skill-name [context: your project description] your request
```
**Example:**
```
@react-best-practices [context: Next.js 15 App Router, TypeScript, Tailwind] Review this component for performance issues
```

### Sub-skills (nested styles)
Some skill families have sub-variants accessed with `/`:
```
@design-it/glassmorphism Design a login card component
@design-it/neo-brutalism Create a dashboard layout
@game-development/multiplayer Design the lobby system architecture
@super-code/typescript Write a generic retry utility
```

---

## Invocation by IDE

### Antigravity IDE
Simply type `@skill-name` anywhere in your prompt. The IDE auto-discovers skills from `~/.agents/skills/`.

```
@api-design-principles review my routes/api/ directory
```

### Claude Code
Use the slash command prefix or `@` mention:
```bash
/brainstorming help me plan a task scheduler feature
@security-auditor scan my authentication middleware
```

### Cursor
Use `@` in the chat panel:
```
@clean-code refactor the UserService class
```

### Gemini CLI
Type `@skill-name` in your chat:
```
@debugging-strategies my API is returning 500 errors intermittently
```

---

## Chaining Skills — Prompt Chains

The real power comes from **sequencing** skills across a workflow. Each skill's output becomes the next skill's input.

### Example: Feature Development Chain
```
Step 1:  @brainstorming         → Plan the feature scope
Step 2:  @database-design       → Design the data model
Step 3:  @api-design-principles → Design the API contract
Step 4:  @backend-architect     → Implement the service
Step 5:  @react-best-practices  → Build the frontend
Step 6:  @tdd                   → Write tests
Step 7:  @code-review-excellence → Review and polish
Step 8:  @create-pr             → Package into a PR
```

### How to write a chained prompt:
```
@brainstorming We're adding a real-time notification system to our app.
Users need to see alerts when someone registers for their event.
Plan the feature: what components do we need, what are the edge cases,
and what should we build first?
```
Then after reviewing the output:
```
@database-design Based on the plan above, design the database schema.
We use PostgreSQL with Prisma. Include: notifications table, user_notification_preferences,
notification_channels (email, in-app, push).
```

---

## Handling Context Window Limits

If your AI agent warns about context overload when too many skills are loaded:

### Option 1: Targeted install
```bash
# Install only development-related skills
npx antigravity-awesome-skills --category development --risk safe

# Install only frontend skills
npx antigravity-awesome-skills --category frontend --tags react,nextjs

# Install only backend skills
npx antigravity-awesome-skills --category backend,database
```

### Option 2: Use skills selectively
Don't load all skills at once. Invoke only what you need per task:
- ❌ Avoid: Loading 20 skills in one prompt
- ✅ Do: Use 1-3 tightly related skills per prompt

### Option 3: Use the skill router
```
@skill-router I need to build a payment integration with Stripe. 
What skills should I use?
```

---

## When NOT to Use a Skill

- **Simple, one-off questions** — Just ask directly. No need for `@skill-name`.
- **Already have context** — If you've been working on a feature and the AI has full context, adding a skill may distract.
- **Speed matters** — Skills add a small overhead. For quick fixes, type directly.

---

## Discovering Skills

### Search by keyword
```
@skill-scanner search: "rate limiting"
@skill-suggester I need to implement webhook delivery with retries
```

### Browse by category
All skills are documented in this repository's role files. Navigate to your role guide:
- [All 20 role guides →](../README.md#-quick-reference--navigate-by-role)

### Check what's installed
```bash
ls ~/.agents/skills/          # macOS/Linux
dir C:\Users\<you>\.agents\skills\  # Windows
```
