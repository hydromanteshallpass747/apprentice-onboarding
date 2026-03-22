# The Guild Learning Path

**A structured path from zero to agentic AI practitioner.**

You don't need to learn to code. You don't need to memorize syntax. You don't need a CS degree. Those things are the cuneiform of our era: once essential, now automated.

What you *do* need is to **think clearly**, **communicate intent**, **verify results**, and **refine through iteration**. These are human skills. They always have been.

This learning path is built on a methodology called **Verification Driven Development via Iterative Adversarial Refinement (VDD/IAR)**. In plain language: define what success looks like *before* you build, use AI agents to build it, stress-test the results, and improve through honest critique. Every phase practices that loop.

The guild's default language is **Rust**. Not because it's the easiest, but because its compiler enforces correctness in ways that align with our methodology. The agent handles the syntax. You handle the thinking.

---

## How This Path Works

This is a guild, not a school. You advance by **showing your work**, not by passing tests.

There are three tiers:

**Apprentice** - You learn to direct agents and verify their output. You build real things with guidance, submit your work for adversarial review, and learn from the feedback.

**Journeyman** - You run full VDD cycles on your own. You architect solutions, set verification criteria upfront, stress-test through adversarial refinement, and handle complex multi-step workflows. Your portfolio shows not just what you built but *how* you built it.

**Master** - You contribute back. You mentor apprentices, develop new techniques, push into new territory. Your body of work speaks for itself.

This repository contains the **Apprentice Learning Path**: everything you need to go from "I've never touched code" to "I can build real things with AI agents and prove it."

---

## The Apprentice Path

### Phase 0: Foundations
*What you need to understand before you start building.*

1. [The New Literacy](00-foundations/01-the-new-literacy.md) - Why directing agents is the skill that matters now
2. [What Code Actually Is](00-foundations/02-what-code-actually-is.md) - A mental model, not a syntax lesson
3. [The Language Landscape](00-foundations/03-the-language-landscape.md) - Programming languages in conceptual terms
4. [Your Workspace](00-foundations/04-your-workspace.md) - Setting up the tools you'll use every day
5. [Git: Just Enough](00-foundations/05-git-just-enough.md) - How to save, share, and track your work

### Phase 1: Talking to Agents
*How to communicate with AI so it builds what you actually want.*

1. [How Agents Think](01-talking-to-agents/01-how-agents-think.md) - What's happening on the other side
2. [The Art of Intent](01-talking-to-agents/02-the-art-of-intent.md) - Turning vague ideas into clear directions
3. [Context Is Everything](01-talking-to-agents/03-context-is-everything.md) - What the agent needs to know and why
4. [Breaking Problems Down](01-talking-to-agents/04-breaking-problems-down.md) - The most important skill you'll learn
5. [Your First Build](01-talking-to-agents/05-first-build.md) - A guided project from design doc to working software

### Phase 2: The Methodology
*The discipline that makes the output actually good.*

1. [How We Build](02-the-methodology/01-how-we-build.md) - VDD and the adversarial refinement loop
2. [Tracking Your Work](02-the-methodology/02-tracking-your-work.md) - Issue tracking, building your own, then using Chainlink

### Phase 3: Real-World Skills
*Working the way professionals work.*

1. [The Workshop](03-real-world-skills/01-the-workshop.md) - Contributing to the guild toolkit, an existing codebase
2. [Working with APIs](03-real-world-skills/02-working-with-apis.md) - MCP servers, skills, and talking to external services
3. [Project Architecture](03-real-world-skills/03-project-architecture.md) - Multi-file projects, god files, and managing complexity
4. [Security](03-real-world-skills/04-security.md) - Why agent code is insecure and how to fix it
5. [Shipping It](03-real-world-skills/05-shipping-it.md) - Testing, verification, cross-platform, and delivery

### Phase 4: Proving It
*No guides, no hand-holding. The capstone.*

1. [On Your Own](04-proving-it/01-on-your-own.md) - Your capstone project, fully self-directed
2. [Giving Back](04-proving-it/02-giving-back.md) - Mentoring a newer apprentice
3. [The Portfolio Review](04-proving-it/03-the-portfolio-review.md) - Assembling your work and the journeyman transition

---

## Projects

Each phase includes hands-on builds of increasing complexity:

| # | Project | Language | What It Teaches |
|---|---|---|---|
| 1 | Bookmark Manager | HTML/JS | Prompting, verification, the build-verify loop |
| 2 | Issue Tracker | Rust | State machines, CLI design, structured data, first Rust project |
| 3 | Guild Toolkit | Rust | Reading existing code, working within established patterns, PRs |
| 4 | API Client | Rust | Network requests, error handling, external dependencies |
| 5 | *Architecture project* | Rust | Multi-file structure, modules, managing complexity at scale |

The bookmark manager is the one exception to the Rust default. It's your very first build, and seeing something appear in a browser is the fastest path to "I made a thing." From project 2 onward, everything is Rust.

---

## Portfolio Requirements

At the end of the Apprentice path, you should have:

- **Completed projects** demonstrating increasing complexity, tracked in git with full history
- **Design documents** for each project (generated with the agent)
- **Process documentation** showing how you directed the agent, what went wrong, and how you fixed it
- **Adversarial review history**: your submissions to the roast channel, the feedback you received, and how you responded to it
- **Workshop contributions**: PRs to the shared codebase, reviewed and merged
- **3-5 self-directed projects**: things you chose to build for yourself, designed and executed on your own. These don't need to be large. They need to be yours: your idea, your design doc, your process. This is where you prove you can work without a guide telling you what to build next

Your portfolio isn't just the finished code. It's the *entire journey*: the thinking, the failures, the iterations, and the final result. That's what makes it real.

---

## Philosophy

This path exists because the world is changing and most education hasn't caught up. Coding bootcamps teach you to write syntax that AI can now generate in seconds. Universities teach theory that takes years to reach practical application. Neither prepares you for the skill that actually matters: **directing intelligent systems to build things that work**.

We don't test you. We don't certify you. We don't hand you a credential you can cram for and forget. We ask you to build things, submit them for honest critique, and improve. The portfolio you build here is proof that can't be faked, because the whole process is visible.

Welcome to the guild.
