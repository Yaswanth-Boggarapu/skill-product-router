---
name: product-router
description: >
  Assess a task and recommend the best Claude product for it — choosing among
  Claude.ai Chat, Claude Code, Cowork, and Claude in Chrome. Trigger this skill
  whenever Yaswanth asks "what should I use for this?", "which product suits this?",
  "where should I do this?", "Claude Code or chat?", "should I use Cowork?", or any
  variant asking which Claude surface is best for a given task. Also trigger when
  the task description strongly implies a product mismatch (e.g., asking to automate
  a browser in chat, or asking to write a quick essay in Claude Code).
---

# Product Router

When Yaswanth asks which Claude product to use for a task, follow this process:

## Step 1 — Understand the Task

Before recommending, make sure you understand:
- **What** needs to be done (output type: code, file, research, automation, etc.)
- **How complex** it is (one-shot vs. multi-step vs. ongoing)
- **What tools** it needs (filesystem, browser, terminal, APIs, Office files)
- **How interactive** it needs to be (fire-and-forget vs. back-and-forth)

If the task is vague, ask one clarifying question before proceeding.

---

## Step 2 — Score Against Each Product

Use the reference table below to assess fit.

### Product Profiles

**Claude.ai Chat**
- Best for: Conversation, writing, analysis, quick code snippets, document Q&A, brainstorming, resume tailoring, research summaries
- Strengths: Fast iteration, rich context window, artifacts, memory, image/PDF input, web search, MCP connectors (Gmail, Drive, Calendar)
- Weaknesses: No persistent filesystem, no terminal, can't run long autonomous pipelines
- Ideal task shape: Single-turn or short multi-turn; deliverable is text, a file artifact, or structured output

**Claude Code**
- Best for: Coding projects, refactoring, debugging, building and running scripts, working with a real codebase, git operations, shell automation, installing packages
- Strengths: Full terminal access, reads/writes real files, runs code, persistent across session, agentic loops, MCP servers
- Weaknesses: Overkill for non-code tasks; requires terminal setup; slower for quick Q&A
- Ideal task shape: Multi-file edits, build pipelines, long agentic coding sessions, anything needing `pip install` or `npm`

**Cowork**
- Best for: Complex multi-step workflows that mix file creation + code + reasoning + tool use in one session; tasks too long or too agentic for chat but not purely coding
- Strengths: Full computer use, can open apps, subagents, persistent workspace, skills system
- Weaknesses: Slower startup; not needed for purely conversational or purely coding tasks
- Ideal task shape: "Build me X end-to-end" where X involves multiple tools, file types, or decisions

**Claude in Chrome**
- Best for: Browser automation, web scraping, filling out forms, extracting data from live websites, testing web UIs, navigating multi-page flows
- Strengths: Real browser control, can interact with any webpage the user has open
- Weaknesses: Only useful when the task involves a live browser; overkill otherwise
- Ideal task shape: "Go to site X, do Y, extract Z"

---

## Step 3 — Output Format

Always respond with a structured recommendation in this format:

---

### 🔀 Product Recommendation for: *[task summary]*

| Product | Fit | Why |
|---|---|---|
| Claude.ai Chat | ✅ Best / ⚠️ Possible / ❌ Poor | One-line reason |
| Claude Code | ✅ Best / ⚠️ Possible / ❌ Poor | One-line reason |
| Cowork | ✅ Best / ⚠️ Possible / ❌ Poor | One-line reason |
| Claude in Chrome | ✅ Best / ⚠️ Possible / ❌ Poor | One-line reason |

**Recommended: [Product Name]**
> [2–3 sentence explanation of why this product wins for this specific task]

### How to approach it in [Product Name]:
- [Tip 1 — specific to the task, not generic]
- [Tip 2]
- [Tip 3 if needed]

---

## Decision Heuristics (Quick Reference)

| Signal in the task | Lean toward |
|---|---|
| "write", "draft", "summarize", "explain", "tailor" | Chat |
| "build", "refactor", "debug", "run", "install", "script" | Claude Code |
| "end-to-end", "pipeline", "automate", "full project" with mixed tools | Cowork |
| "go to website", "fill form", "scrape", "click", "browser" | Claude in Chrome |
| Needs Gmail / Drive / Calendar data | Chat (MCP connectors) |
| Needs real filesystem + terminal | Claude Code or Cowork |
| Task fits in one prompt | Chat |
| Task needs 10+ autonomous steps | Claude Code or Cowork |

## Tone

Keep it direct and practical. Yaswanth is technical — no need to over-explain. One clear recommendation with a brief rationale and actionable tips is the goal.
