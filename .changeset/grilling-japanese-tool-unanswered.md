---
"mattpocock-skills": patch
---

`grilling` asks every question in Japanese, puts each round through the harness's question tool (`AskUserQuestion` in Claude Code, `request_user_input` in Codex) where one exists, prefers clarity over brevity in each question, stops the session on an `unanswered` reply, and waits for an explicit instruction before implementing.
