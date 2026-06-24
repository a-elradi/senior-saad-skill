# 📚 LEARN — Building Your First Claude Code Skill
### From Idea to Open-Source Tool in 48 Hours

**By Abdalla Elradi** · Informatics Engineering Student · University of Technology Bahrain
**Project:** [senior-saad](https://github.com/AbdallaElradi/senior-saad) — System Architecture & Design Skill for Claude Code

---

## 🎯 Who Is This For?

This guide is for **students and developers** who want to:
- Learn what Claude Code Skills are and how they work
- Understand how to design a multi-agent AI pipeline
- Build and publish their own Claude Code Skill as open source
- Apply software engineering thinking to AI tooling

No prior AI experience required. If you can write Markdown, you can build a Skill.

---

## 🧠 What I Built and Why

I'm a final-year Informatics Engineering student who works on robotics, computer vision, and full-stack systems. Every time I started a new project, I spent hours doing the same thing:

- Figuring out the right tech stack
- Drawing system diagrams
- Designing the database schema
- Picking UI components
- Writing the implementation plan

So I asked: **can I automate this with AI?**

The answer became **SAAD** — System Architecture & Design. A Claude Code Skill that takes your project idea and returns a complete, production-ready system design package.

This is what I learned building it.

---

## 📖 Table of Contents

1. [What is Claude Code?](#1-what-is-claude-code)
2. [What is a Claude Code Skill?](#2-what-is-a-claude-code-skill)
3. [How Skills Work Technically](#3-how-skills-work-technically)
4. [The Architecture of SAAD](#4-the-architecture-of-saad)
5. [The LLM Council Pattern](#5-the-llm-council-pattern)
6. [Building the SKILL.md](#6-building-the-skillmd)
7. [Designing the Pipeline](#7-designing-the-pipeline)
8. [Integrating External Sources](#8-integrating-external-sources)
9. [Writing Good Prompts Inside Skills](#9-writing-good-prompts-inside-skills)
10. [Installing and Testing](#10-installing-and-testing)
11. [Publishing as Open Source](#11-publishing-as-open-source)
12. [Key Lessons Learned](#12-key-lessons-learned)
13. [Your Turn — Build Your Own Skill](#13-your-turn--build-your-own-skill)
14. [Resources](#14-resources)

---

## 1. What is Claude Code?

Claude Code is Anthropic's **agentic coding tool** — a command-line application that runs Claude AI directly in your terminal. Unlike chatting with Claude in the browser, Claude Code can:

- Read and write files on your computer
- Run terminal commands
- Browse the web
- Use tools like Figma, GitHub, and more
- Remember context across long coding sessions

You install it with:
```bash
npm install -g @anthropic-ai/claude-code
```

Then run it in any project folder:
```bash
cd my-project
claude
```

Think of it as a senior engineer sitting next to you, with access to your entire codebase.

---

## 2. What is a Claude Code Skill?

A **Skill** is a Markdown file that teaches Claude Code how to do a specific task — with your exact standards, your exact workflow, and your exact preferences.

Skills are stored at:
```
~/.claude/skills/skill-name/SKILL.md    ← applies to all your projects
.claude/skills/skill-name/SKILL.md      ← applies to one project only
```

When Claude Code reads your request, it **automatically detects** which Skill applies and follows the instructions inside.

### Real-World Analogy

Imagine you hire a new developer. Instead of explaining your team's coding standards every single day, you give them a document on day one:
- "We always use compound components"
- "Never use barrel imports"  
- "WCAG AA contrast on every UI element"

That document is a Skill.

---

## 3. How Skills Work Technically

A SKILL.md has two parts:

### Part 1: Frontmatter (YAML)
```yaml
---
name: my-skill
description: >
  Activate this skill when the user asks to [do something].
  Triggers on: "keyword1", "keyword2", "phrase".
  Do NOT activate for [unrelated things].
metadata:
  author: Your Name
  version: "1.0.0"
  argument-hint: <what-to-pass>
---
```

The `description` field is the most important. Claude Code reads it to decide **when to activate** the skill. Write it like you're training a classifier — be specific about what triggers it and what doesn't.

### Part 2: Instructions (Markdown)

Everything after the frontmatter is a detailed instruction set for Claude:

```markdown
# My Skill

## When to Activate
...

## Step-by-Step Process
### Step 1: Do X
...
### Step 2: Do Y
...

## Quality Standards
- [ ] Always check X
- [ ] Never do Y
```

Claude Code reads these instructions literally. The clearer and more structured they are, the better the output.

---

## 4. The Architecture of SAAD

SAAD is a **6-phase sequential pipeline**. Each phase builds on the output of the previous one.

```
User Input (project idea)
         │
         ▼
┌─────────────────────┐
│  Phase 1: Council   │  ← 5 AI advisors analyze the idea
│  Review             │
└─────────┬───────────┘
          │ verdict + findings
          ▼
┌─────────────────────┐
│  Phase 2: System    │  ← Architecture, diagrams, schema, API
│  Architecture       │
└─────────┬───────────┘
          │ full technical spec
          ▼
┌─────────────────────┐
│  Phase 3: Summary   │  ← Present to user, HARD STOP
│  & Confirm          │  ← Wait for color/style input
└─────────┬───────────┘
          │ user confirmation + design prefs
          ▼
┌─────────────────────┐
│  Phase 4: UI/UX     │  ← Design system, components, animations
│  Design             │
└─────────┬───────────┘
          │ complete design system
          ▼
┌─────────────────────┐
│  Phase 5: Figma     │  ← Visual mockup via Figma skill
│  Mockup             │
└─────────┬───────────┘
          │ figma file
          ▼
┌─────────────────────┐
│  Phase 6: Prompt    │  ← SAAD-[Name]-PACKAGE.md
│  Package            │
└─────────────────────┘
          │
          ▼
   Ready for Claude Code
```

### Why This Architecture?

**Sequential phases** prevent a common AI problem: jumping to solutions before understanding the problem.

**The hard stop at Phase 3** is intentional. It forces human review before the AI invests time designing a UI. This is the engineering concept of **fail-fast** applied to AI pipelines.

**The final package** creates a portable artifact — something you can take from SAAD and hand directly to Claude Code to build the actual system.

---

## 5. The LLM Council Pattern

The most interesting part of SAAD is Phase 1: the Council Review.

This is based on a methodology by **Andrej Karpathy** — running the same question through multiple AI advisors with different thinking styles, then synthesizing their perspectives.

### Why Use a Council?

When you ask one AI a question, you get one perspective. That perspective might have blind spots. The council forces **structured adversarial thinking**.

### The 5 Advisors in SAAD

| Advisor | Thinking Lens | What They Catch |
|---------|--------------|----------------|
| **The Architect** | Technical feasibility | Scalability, technical debt, integration complexity |
| **The Strategist** | Market & product value | Competition, user adoption, business model |
| **The Security Analyst** | Attack surface | Vulnerabilities, data privacy, compliance |
| **The UX Advocate** | User experience | Confusion points, accessibility, mobile issues |
| **The Devil's Advocate** | Failure modes | Fatal assumptions, worst-case scenarios |

### The Peer Review Round

After each advisor gives their independent analysis, they review each other's work anonymously:
1. Which analysis is strongest?
2. Which has the biggest blind spot?
3. What did ALL advisors miss?

### The Chairman Synthesis

A final agent reads all advisor outputs + peer reviews and produces:
- Where the council agrees (high-confidence signals)
- Where the council clashes (genuine uncertainty)
- Blind spots surfaced only in peer review
- A clear recommendation
- One concrete next step

### Code Pattern (Simplified)

```markdown
## Council Process

For each of these 5 advisors, analyze the idea independently:

**Advisor 1 — The Architect**
Focus on: technical feasibility, scalability risks, integration complexity
Output: findings + verdict (HIGH/MEDIUM/LOW viability)

**Advisor 2 — The Strategist**
Focus on: market fit, competition, user adoption, business model
Output: findings + verdict

[...3 more advisors...]

After all 5, run peer review:
Each advisor reviews the others' outputs and identifies:
- Strongest point made
- Biggest blind spot
- What everyone missed

Chairman: Synthesize into final verdict with clear recommendation.
```

This pattern is reusable. You can use it for any decision-making skill — not just system design.

---

## 6. Building the SKILL.md

Here's the step-by-step process I followed to write SAAD's `SKILL.md`.

### Step 1: Define the trigger precisely

The description field controls when your skill activates. I started by listing every way a user might ask for system design:

```yaml
description: >
  MANDATORY ACTIVATION on: "SAAD", "senior design",
  "design my system", "architect this", "build from scratch",
  "full system for", "plan my project", "analyze my idea",
  "system structure for", "I have an idea for", "help me design"
```

Then I listed what should NOT trigger it:
```yaml
  Do NOT activate for simple questions, code fixes,
  or tasks with no system design intent.
```

### Step 2: Define the persona

Skills work better when Claude has a clear identity to embody:

```markdown
## Who is SAAD?

You are SAAD — a Senior System Architect operating at elite level.
You carry the combined expertise of:
- Senior Software Architect (15+ years)
- Principal UX/UI Designer
- Principal Engineer
- Product Strategist
- Prompt Engineer

Your output is never vague. Zero tolerance for mediocre output.
```

### Step 3: Write the process as numbered phases

Numbered, titled phases give Claude a clear execution roadmap:

```markdown
## Phase 1: Council Review
**Goal:** Validate the idea before designing anything.
[Detailed instructions...]

## Phase 2: System Architecture  
**Goal:** Design the complete system structure.
[Detailed instructions...]
```

### Step 4: Define quality gates

Quality gates are checklists that the Skill uses to self-check before producing output:

```markdown
## Quality Standards
Every output MUST pass these gates:
- [ ] No single point of failure in core paths
- [ ] Color contrast ≥ 4.5:1 (WCAG AA)
- [ ] No generic fonts (no Inter/Roboto/Arial)
- [ ] All API endpoints documented
```

### Step 5: Add examples

Examples dramatically improve skill accuracy:

```markdown
## Example Triggers
"SAAD — I want to build a marketplace for freelancers"
"Design a complete system for a hospital booking app"
"Architect a real-time collaboration tool like Notion"
```

---

## 7. Designing the Pipeline

The hardest design decision in SAAD was the **hard stop at Phase 3**.

### The Problem

If SAAD runs all 6 phases automatically, it will design a full UI system based on assumptions about what the user wants. That's wasted work.

### The Solution: Human-in-the-Loop

Phase 3 explicitly instructs Claude to stop and ask for input:

```markdown
## Phase 3: Summary & Confirm

[Present the summary...]

Then ask EXACTLY this:
"Before I design the UI, please tell me:
1. Color Theme
2. Visual Style  
3. Anything to add?
4. Anything to change?"

⛔ HARD STOP HERE. Do NOT proceed until the user responds.
```

This is the engineering concept of **checkpointing** — saving state and waiting for validation before committing to the next expensive operation.

### Why Pipelines Beat Single Prompts

A single prompt like "design me a full system" produces generic output. A pipeline:

1. Forces sequential thinking (can't design UI before architecture)
2. Creates feedback loops (human review at Phase 3)
3. Produces structured artifacts (each phase outputs something concrete)
4. Is debuggable (if Phase 4 is bad, check Phase 3's output)

---

## 8. Integrating External Sources

SAAD instructs Claude to fetch real components from three sources:

```markdown
## Component Sources

**KokonutUI — https://kokonutui.com/**
Use WebFetch to browse current components.
Select: glassmorphism cards, animated inputs, dock navigation...

**21st.dev — https://21st.dev/community/components**
Use WebFetch to browse community components.
Select: hero sections, dashboard layouts, auth screens...

**Anime.js v4 — https://animejs.com/**
Include in every project. Use for:
- Page load stagger animations
- Spring physics hover effects  
- Number counters
- Scroll-triggered reveals
```

### Why This Matters

Instead of Claude making up component names, it's instructed to **actually browse these sites** and use real, current components. This keeps the output grounded in what actually exists.

This is an important pattern: **ground AI output in real sources, not hallucinated ones.**

---

## 9. Writing Good Prompts Inside Skills

Skills are essentially structured prompts. Here's what I learned about writing effective ones:

### Be Specific About Format

Vague instruction:
```
Present a summary of the findings.
```

Specific instruction:
```
Present this exact summary format:
╔══════════════════════════════╗
║  PROJECT: [Name]             ║
║  VERDICT: [Decision]         ║
║  TOP RISKS:                  ║
║  1. [Risk + mitigation]      ║
╚══════════════════════════════╝
```

### Use Positive + Negative Constraints

```markdown
# DO:
- Generate a complete Mermaid diagram for every system
- Include Auth, Core, DB, Cache layers minimum
- Label all connections with data flow direction

# DO NOT:
- Use placeholder text like [TODO] or [TBD]  
- Skip the security section
- Proceed to Phase 4 without Phase 3 confirmation
```

### Encode Your Standards, Not Generic Ones

My SKILL.md has specific opinions:
```markdown
Typography: NEVER use Inter, Roboto, Arial, or Space Grotesk.
These are overused in AI-generated designs.
Choose distinctive, characterful typeface pairs.
```

This makes the output specifically mine — not generic AI output.

### Use Output Templates

Templates ensure consistent, structured output:

```markdown
For every API endpoint, use this format:
[METHOD]  /api/v1/[resource]  →  [Description]  →  Auth: [Y/N]

Example:
POST  /api/v1/auth/login  →  Get JWT token  →  Auth: No
GET   /api/v1/users/:id   →  Get user by ID  →  Auth: Yes
```

---

## 10. Installing and Testing

### Installation

```bash
# 1. Clone the skill
git clone https://github.com/AbdallaElradi/senior-saad.git ~/.claude/skills/senior-saad

# 2. Verify
ls ~/.claude/skills/
# Should show: senior-saad/

# 3. Test it
cd any-project-folder
claude
```

### Testing Your Skill

When testing, check these things:

**Activation Test** — does the skill trigger correctly?
```
Input: "SAAD — design a library management system"
Expected: Skill activates, starts Phase 1
Wrong: Skill doesn't activate, Claude answers generically
```

**Output Quality Test** — does each phase produce what you defined?
```
Phase 1: Does the Council produce 5 distinct advisor perspectives?
Phase 2: Does the architecture include DB schema + API map + Mermaid diagram?
Phase 3: Does Claude actually stop and ask for color input?
Phase 4: Does it produce complete CSS variables?
```

**Edge Case Test** — what happens with minimal input?
```
Input: "SAAD"
Expected: Claude asks for the project idea
```

### Iteration Process

My SKILL.md went through 4 full rewrites before I was happy with the output quality. Each time I found:
- A phase that was too vague → made it more specific
- A quality gate that was missing → added it
- A format that Claude ignored → made it a template

This is normal. **Skills are software. They need debugging.**

---

## 11. Publishing as Open Source

### Repository Structure

```
senior-saad/
├── SKILL.md                 ← The skill itself
├── README.md                ← How to install and use
├── LEARN.md                 ← This file — how it was built
├── LICENSE                  ← MIT
├── .gitignore
└── references/
    ├── COMPONENT-SOURCES.md ← Component library guide
    └── ARCHITECTURE-PATTERNS.md ← Common patterns
```

### Writing a Good README

A good README for a Claude Code Skill needs:
1. What it does (1-2 sentences)
2. The pipeline/phases (visual diagram)
3. Install command (copy-paste ready)
4. Trigger examples (how to activate it)
5. Requirements (other skills needed)

### The One-Line Install Pattern

Make installation as simple as possible:
```bash
git clone https://github.com/AbdallaElradi/senior-saad.git ~/.claude/skills/senior-saad
```

The easier it is to install, the more people will use it.

### Choosing a License

For developer tools meant to spread freely: **MIT License**.
- Anyone can use it
- Anyone can modify it
- Anyone can redistribute it
- You get credit

---

## 12. Key Lessons Learned

### 1. The Description Field is Everything

The most important line in your SKILL.md is the `description`. If it's wrong, the skill never activates. Spend 20% of your time on the description.

### 2. Structure Beats Intelligence

A well-structured skill with clear phases, templates, and quality gates will outperform a "smart" but unstructured one. Claude follows structure reliably.

### 3. Hard Stops Are Good UX

The Phase 3 hard stop felt unnatural to design — I wanted the skill to "just work" automatically. But forcing human review at the right moment produces dramatically better final output. Don't automate what should be a human decision.

### 4. Ground Output in Real Sources

Instructing Claude to actually browse real component libraries (KokonutUI, 21st.dev) instead of making up component names produces grounded, actionable output. Always prefer real sources over hallucinated ones.

### 5. Quality Gates Are Self-Review

The checklist at the end of SAAD's SKILL.md isn't for the user — it's for Claude to self-check before finalizing output. This pattern significantly improves consistency.

### 6. Start With Your Own Problem

SAAD exists because I personally spent hours on system design for every project. Building tools for your own problems means you understand the domain deeply and can write better instructions.

### 7. Ship Fast, Iterate

My first version of SAAD was 200 lines. The current version is 1,084 lines. I published the 200-line version, got feedback from using it, and refined from there. Don't wait for perfect.

---

## 13. Your Turn — Build Your Own Skill

Here's a framework for creating your first Claude Code Skill:

### Step 1: Find a Repetitive Task (15 mins)

Answer this question: *"What do I explain or set up the same way every single project?"*

Examples from other students:
- Setting up ESLint + Prettier with your specific config
- Writing academic report sections in your university's format
- Creating database migrations following your team's naming conventions
- Writing unit tests for your specific testing philosophy

### Step 2: Write the Frontmatter (15 mins)

```yaml
---
name: my-skill-name
description: >
  Activate when user says [trigger phrases].
  Handles: [what it does].
  Do NOT activate for [what it doesn't do].
metadata:
  author: Your Name
  version: "1.0.0"
---
```

### Step 3: Write the Instructions (1-2 hours)

Follow this structure:
```markdown
# Skill Name

## Identity (optional — who is the AI when this skill is active?)

## When to Activate

## The Process
### Step 1: [Name]
[Instructions...]

### Step 2: [Name]  
[Instructions...]

## Output Format
[Templates for what the output should look like]

## Quality Standards
- [ ] Check 1
- [ ] Check 2

## Examples
[Show what good triggers look like]
```

### Step 4: Install and Test (30 mins)

```bash
mkdir -p ~/.claude/skills/my-skill
# Copy your SKILL.md into that folder
cd any-project
claude
# Test your trigger phrases
```

### Step 5: Publish (30 mins)

```bash
git init my-skill
cd my-skill
# Copy your SKILL.md + write README.md
git add .
git commit -m "Initial release"
gh repo create my-skill --public
git push -u origin main
```

---

## 14. Resources

### Official Documentation
- [Claude Code Overview](https://docs.anthropic.com/en/docs/claude-code/overview) — Getting started
- [Claude Code Skills](https://docs.anthropic.com/en/docs/claude-code/skills) — Official skills documentation
- [Anthropic Prompt Engineering Guide](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview) — Writing better prompts

### Skill Libraries (Learn by Reading Others')
- [Anthropic Skills Repo](https://github.com/anthropics/skills) — Official starter skills
- [Vercel Agent Skills](https://github.com/vercel-labs/agent-skills) — Production React skills
- [Senior SAAD](https://github.com/AbdallaElradi/senior-saad) — This project

### Component Libraries Used in SAAD
- [KokonutUI](https://kokonutui.com/) — Modern UI components
- [21st.dev Community](https://21st.dev/community/components) — Shadcn-style components
- [Anime.js v4](https://animejs.com/) — Animation engine

### Concepts Referenced
- **LLM Council** — Based on Andrej Karpathy's multi-model council methodology
- **WCAG Accessibility** — [Web Content Accessibility Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)
- **Mermaid Diagrams** — [Mermaid.js documentation](https://mermaid.js.org/)

---

## 📬 Connect

If you build something using this guide or get inspired by SAAD, I'd love to know.

- **GitHub:** [github.com/AbdallaElradi](https://github.com/AbdallaElradi)
- **Project:** [github.com/AbdallaElradi/senior-saad](https://github.com/AbdallaElradi/senior-saad)
- **University:** University of Technology Bahrain — Informatics Engineering

---

> *"The best way to learn how AI works is to build something that teaches AI how to work."*
>
> — What I learned building SAAD

---

*LEARN.md — part of the [Senior SAAD](https://github.com/AbdallaElradi/senior-saad) open-source project*
*MIT License · Free to share, adapt, and build upon*
