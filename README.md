# Prompt Master

A Claude skill that writes the perfect prompts for any AI tool. Zero tokens or credits wasted; Full context and memory efficient.


## Installation

### Recommended (clone directly into Claude Code skills directory)

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/nidhinjs/prompt-master.git ~/.claude/skills/prompt-master
```

### Manual install / update (only the skill file)

If you already have this repo cloned (or you downloaded SKILL.md), copy the skill file into Claude Code’s skills directory:

```bash
mkdir -p ~/.claude/skills/prompt-master
cp SKILL.md ~/.claude/skills/prompt-master/
```

### Claude.ai (browser)

1. Download this repo as a ZIP.
2. Go to **claude.ai → Sidebar → Customize → Upload into Custom Skill**

## Usage

In Claude, invoke the skill naturally:

```
Write me a prompt for Cursor to refactor my auth module
```

```
I need a prompt for Claude Code to build a REST API — ask me what you need to know
```

```
Here's a bad prompt I wrote for GPT-4o, fix it: [paste prompt]
```

```
Generate a Midjourney prompt for a cyberpunk city at night
```

Or explicitly invoke it:

```
/prompt-master

I want to ask Claude Code to build a todo app with React and Supabase
```

---

## The Problem This Solves

Every AI user burns credits the same way:

> Write vague prompt → get wrong output → re-prompt → get closer → re-prompt again → finally get what you wanted on attempt 4

That's 3 wasted API calls. And lots of time wasted perfecting the prompt to not waste credits.

---

## 20 Credit-Killing Patterns Detected (with Before/After Examples)

### Task Patterns

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 1 | **Vague task verbs** | "help me with my code" | "Refactor `getUserData()` to use async/await and handle null returns" |
| 2 | **Asking two things at once** | "explain AND rewrite this function" | Split into two sequential prompts |
| 3 | **No success criteria** | "make it better" | "Done when function passes existing unit tests and handles null input without throwing" |
| 4 | **Over-permissive agent** | "do whatever it takes to fix this" | Explicit allowed actions + forbidden actions list |
| 5 | **Missing file path (IDE AI)** | "update the login function" | "Update `handleLogin()` in `src/pages/Login.tsx` line 47" |

### Context Patterns

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 6 | **Assumed knowledge** | "continue where we left off" | Include Memory Block with all prior decisions |
| 7 | **Missing context** | "write a cover letter" | "Write for a PM role at fintech startup, I have 2 years SWE transitioning to product" |
| 8 | **Forgetting prior stack** | New prompt contradicts agreed-upon tech | Always include Memory Block |
| 9 | **Hallucination-prone phrasing** | "what do experts say about X?" | "Cite only sources you are certain of. If uncertain, say so explicitly." |
| 10 | **Undefined audience** | "write something for users" | "Write for non-technical founders, assume zero coding knowledge" |

### Format Patterns

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 11 | **Missing output format** | "explain this concept" | "Explain in exactly 3 bullet points, each under 20 words" |
| 12 | **Implicit length** | "write a summary" | "Write a summary in exactly 3 sentences, no more" |
| 13 | **No role assignment** | (blank system prompt) | "You are a senior backend engineer specializing in Node.js and PostgreSQL" |
| 14 | **Emotional/vague adjectives** | "make it look professional" | "Monochrome palette, 16px base font, 24px line height, no decorative elements" |
| 15 | **No negative prompts (image AI)** | "a portrait of a woman" | Add: "no watermark, no blur, no extra fingers, no distortion" |

### Scope Patterns

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 16 | **No scope boundary** | "fix my app" | "Fix only the login form validation logic in `auth.js`. Touch nothing else." |
| 17 | **No constraints** | "build a React component" | "React 18, TypeScript only, no external libraries, no inline styles" |
| 18 | **No stop condition (agentic)** | "build the whole app" | Explicit stop conditions + human review checkpoints |
| 19 | **Tool mismatch** | GPT-style prompt used in Cursor | Adapted to IDE-specific format with file path + scope |
| 20 | **Re-explaining solved problems** | Paste entire codebase every prompt | Scoped to the relevant function or section only |

---

## 4 Prompt Templates Included

The skill contains ready-to-use templates for every major AI category:

| Template | For | Key Additions |
|----------|-----|---------------|
| **Template A** | Claude, ChatGPT, Gemini | Role + Task + Format + Constraints + Context + Success Criteria |
| **Template B** | Cursor, Windsurf, Copilot | File path + Current behavior + Desired change + Scope boundary |
| **Template C** | Claude Code, Cursor, Antigravity | Objective + Allowed/Forbidden actions + Stop conditions + Checkpoints |
| **Template D** | Midjourney, DALL-E, Sora | Subject + Style + Mood + Lighting + Negative prompts + Aspect ratio |

---

## Memory Block System

When your conversation has history, Prompt Master automatically extracts prior decisions and prepends a Memory Block to prevent the AI from contradicting earlier work:

```
## Memory (Carry Forward from Previous Context)
- Stack: React 18 + TypeScript + Supabase
- Auth uses JWT stored in httpOnly cookies, not localStorage
- Component naming convention: PascalCase, no default exports
- Design system: Tailwind only, no custom CSS files
```

This solves the #1 cause of wasted re-prompts in long sessions.

---

## Full Example

### User Input
```
i want to ask cursor to fix the bug in my login page
```

---

### Prompt Master Asks
```
Two quick things before I write this:
1. What file is the login page in, and what's the buggy function called?
2. What is the bug actually doing wrong right now?
```

---

### User Answers
```
it's in src/pages/Login.tsx, the handleLogin function.
When login fails it should show an error but the page just goes blank
```

---

### Generated Prompt

```
## File
src/pages/Login.tsx

## Current Behavior
The handleLogin function silently fails on bad credentials —
the page goes blank instead of displaying an error message.

## Desired Change
When login fails, display an inline error message below the submit button.
"Incorrect email or password" for a 401.
"Something went wrong" for 500+.

## Scope
Only modify the handleLogin function and the JSX error display section.
Do NOT touch the form layout, styling, or any other component.

## Constraints
- Do not add new libraries
- Maintain existing TypeScript types — do not use `any`
- Keep existing loading state logic untouched

## Done When
- Failed login renders an error message below the submit button
- Successful login still redirects correctly
- Zero TypeScript errors
```

**Target:** Cursor · **Token Estimate:** Light (~180 tokens) · **Strategy:** Scoped to one function, explicit stop boundary, success criteria prevent over-editing.

---

## References

- [Anthropic Prompt Engineering Guide](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview) — Primary source for Claude-specific patterns
- [OpenAI Prompt Engineering Guide](https://platform.openai.com/docs/guides/prompt-engineering) — GPT-4o best practices
- [Cursor Documentation](https://docs.cursor.com) — IDE-specific context patterns
- [Midjourney Prompt Reference](https://docs.midjourney.com/docs/prompts) — Image AI parameter guide

---

## Version History

- **1.0.0** — Initial release

---

## License

MIT
