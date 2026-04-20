---
name: micro-lesson
description: Extract a compact learning note from the current session so the user walks away having learned something. Use when the user invokes /micro-lesson, asks to "summarize what we learned", wants a takeaway from the conversation, or asks to turn recent work into a lesson.
---

# Micro-Lesson

Turn the current session into a short, personal lesson the user can learn from. The goal is teaching, not documenting — skip filler, skip recap-for-its-own-sake.

## When to use

- User types `/micro-lesson` (optionally with a focus topic, e.g. `/micro-lesson railway deployment`)
- User asks for a takeaway, recap, or "what did we just learn"
- User wants to distill a debugging session, refactor, or exploration into reusable knowledge

## Process

1. **Pick the subject.** If the user supplied an argument, focus on that aspect. Otherwise, identify the single most interesting thing from the session — the thing the user most likely did not already know. Skip trivial steps (file reads, obvious edits). Favor: non-obvious tool behavior, a gotcha, a pattern, a design decision, a piece of domain knowledge.

2. **Write the lesson.** Use the template below. Keep it tight — a micro-lesson should read in under 60 seconds.

3. **Output directly to the conversation.** Do not create files unless the user asks you to save it.

## Template

```markdown
# Micro-Lesson: {{topic}}

## Essence
{{2–3 sentences — the core idea, distilled. Lead with the "aha", not the setup.}}

## Analogy
{{One comparison to something the user already understands. Concrete beats clever.}}

## Business value
{{Why this matters in $ or time terms. Speed, cost, risk reduced, or new capability unlocked. 1–2 bullets.}}

## Future uses
{{2–3 concrete scenarios where the user would reach for this again. Name the trigger, not just the tool.}}

## Alternatives
{{What else could have been done. For each: one-line tradeoff — why you'd pick it or skip it.}}
```

## Style rules

- **Lead with the insight.** The first line of "Essence" should be the lesson itself, not "In this session we…".
- **Be specific.** "Use `railway up` with `--service`" beats "configure the Railway CLI properly."
- **One idea per lesson.** If the session covered three things, pick the best one or ask the user which to focus on.
- **Analogies must be concrete.** "Like git stash but for environment variables" is good. "Like a container" is not.
- **Business value in business terms.** Engineers sometimes need reminding that "saves 20 min per deploy" matters more than "is more elegant".
- **Alternatives with tradeoffs.** Listing alternatives without tradeoffs is trivia. Always include the "why pick/skip".
- **No filler.** Skip "Hope this helps!", "In summary…", or restating the template headings.

## Length

- Entire output: ~150–250 words.
- Essence: 2–3 sentences max.
- Each bullet: one line.

## Example trigger patterns

- `/micro-lesson` → lesson on the single most interesting thing from the session
- `/micro-lesson caching` → lesson focused on the caching aspect specifically
- "teach me what we just did" → treat as `/micro-lesson`
- "give me the takeaway" → treat as `/micro-lesson`
