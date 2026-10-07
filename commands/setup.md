---
description: Check the Lynkgrids connection and workspace setup, and fill in anything missing one question at a time.
---

Use the `lynkgrids` skill.

1. Call `whoami`. If the Lynkgrids tools are missing, explain how to connect (README: set `LYNKGRIDS_API_KEY` in Claude Code, or add the custom connector in claude.ai / Claude Desktop) and stop.
2. Call `setup_status`. If it is incomplete, ask the one question it returns, save the answer (`save_profile` / `save_icp`), and repeat until complete.
3. When complete, say which workspace and LinkedIn seat are connected and list five things the user can ask for.
