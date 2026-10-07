---
description: Show replies waiting in Lynkgrids (agent drafts and New Response leads), with what each prospect wrote and a suggested reply. Sends nothing.
argument-hint: "[optional: owner or campaign]"
---

Use the `lynkgrids` skill. Scope: $ARGUMENTS

1. `pending_replies` for agent drafts, and `search_people` with `tags: ["New Response"]` for everyone else who replied (all pages).
2. For each person: quote their last message, say what they want in one line, and show the agent draft or your own. Call `search_knowledge` before writing and state only facts it returns.
3. Interested people first. Anyone who asked to stop: mark them, no draft.
4. Send nothing. After the user approves an exact text, send it with `send_reply_draft` or `send_linkedin_message`, then remove the New Response tag (read the tags with `get_person` first, then write the full remaining list).
