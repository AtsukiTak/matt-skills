---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
---

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled: the questions you can ask _now_ without guessing at answers you haven't heard yet. Ask the whole frontier in one round: number each question and give your recommended answer. Then wait for the user's answers before the next round.

Ask every question in Japanese.

Prioritise clarity over brevity. Make each question as long as it needs to be to explain its context, terminology, options, tradeoffs, and why the decision matters, so the user can answer without guessing. Include concrete examples wherever they make the question or its implications easier to grasp.

Put each round to the user through the harness's tool for asking the user questions when it has one (`AskUserQuestion` in Claude Code, `request_user_input` in Codex): one entry per question, keeping its number and title, with your recommended answer as the first option, labelled as the recommendation, and the alternatives after it. If the tool caps how many questions one call can carry, split the round across consecutive calls rather than dropping any. Where no such tool exists, write the round as text, formatted like so:

```
❓ **Q1** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>

---

❓ **Q2** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>
```

Word each question so "yes" accepts your recommended answer.

If the user answers `unanswered` to any question, stop the grilling there. Do not answer that question on their behalf, ask a replacement question, or go on to the next round.

Each round the user answers reshapes the tree: settled decisions push the frontier outward and unblock questions that depended on them. Recompute the frontier and ask the next round. A question whose answer depends on another question still open in this round belongs to a _later_ round, not this one.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent with a fresh context window to find it; don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent to report; ask the rest of the frontier now. The _decisions_ are the user's: put each to them and wait.

The session is done when the frontier is empty: every branch of the design tree visited, nothing left silently assumed. Do not act on it until the user confirms you have reached a shared understanding. In particular, do not move into implementation or execution because the discussion feels resolved: stop, and wait for the user's explicit instruction to implement.
