---
description: Check the Lynkgrids connection and workspace setup, and fill in anything missing one question at a time.
---

Use the `lynkgrids` skill.

1. Call `whoami`. If the Lynkgrids tools are missing or it fails with an auth error, the user isn't signed in. Tell them: in Claude Code run `/mcp`, pick **lynkgrids**, choose **Authenticate**; in claude.ai / Claude Desktop use Settings → Connectors → Lynkgrids → **Connect**. On the page that opens, click **Continue to Lynkgrids**, sign in or create an account, pick a plan if asked, and click **Allow**. Then run `/lynkgrids:setup` again. Stop here until connected.
2. If `whoami` or `setup_status` says the workspace has no plan, or its plan has ended, call `get_subscription_link` and share the link: the user picks a plan or renews in the browser. Wait for them to say it's done, then run `/lynkgrids:setup` again. Never ask for card details.
3. Call `setup_status`. If it is incomplete, ask the one question it returns, save the answer (`save_profile` / `save_icp`), and repeat until complete.
4. When complete, say which workspace and LinkedIn seat are connected and list five things the user can ask for.
