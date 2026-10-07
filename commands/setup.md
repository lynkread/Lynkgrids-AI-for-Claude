---
description: Check the Lynkgrids connection and workspace setup, and fill in anything missing one question at a time.
---

Use the `lynkgrids` skill.

1. Call `whoami`. If the Lynkgrids tools are missing or it fails with an auth error, the user isn't signed in. Tell them: in Claude Code run `/mcp`, pick **lynkgrids**, choose **Authenticate**, and sign in (or create an account) on the Lynkgrids page that opens; in claude.ai / Claude Desktop use Settings → Connectors → Lynkgrids → **Connect**. Then run `/lynkgrids:setup` again. Stop here until connected.
2. Call `setup_status`. If it is incomplete, ask the one question it returns, save the answer (`save_profile` / `save_icp`), and repeat until complete.
3. When complete, say which workspace and LinkedIn seat are connected and list five things the user can ask for.
