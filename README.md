<p align="center"><img src="assets/logo.png" alt="Lynkgrids" width="96"></p>

# Lynkgrids AI for Claude

Official Claude plugin for the hosted Lynkgrids MCP server.

It lets Claude work in your live Lynkgrids workspace: LinkedIn leads, people and companies, lists and pipelines, campaigns and workflows, in-app agents, and replies.

- MCP endpoint: `https://mcp.lynkgrids.com/mcp`
- Works in Claude Code, Claude Desktop, and claude.ai
- Nothing that reaches a real person goes out without your yes

## Get an API key

In Lynkgrids, open **Settings → Workspace → API keys** and create a **Read & write** key. It starts with `lgk_live_`. Keep it out of chat, files, and commits.

## Install in Claude Code

1. Put the key in your environment:

   ```bash
   export LYNKGRIDS_API_KEY=lgk_live_your_key
   ```

   On Windows PowerShell: `setx LYNKGRIDS_API_KEY "lgk_live_your_key"`, then open a new terminal.

2. Install the plugin:

   ```
   /plugin marketplace add lynkread/Lynkgrids-AI-for-Claude
   /plugin install lynkgrids@lynkgrids-ai
   ```

3. Run `/lynkgrids:setup` to check the connection and finish workspace setup.

The plugin's `.mcp.json` sends the key as `Authorization: Bearer ${LYNKGRIDS_API_KEY}`.

### MCP only, without the plugin

```bash
claude mcp add --transport http lynkgrids https://mcp.lynkgrids.com/mcp --header "Authorization: Bearer lgk_live_your_key"
```

## Install in claude.ai or Claude Desktop

1. Open **Settings → Connectors → Add custom connector**.
2. Name it `Lynkgrids` and use the URL `https://mcp.lynkgrids.com/mcp`.
3. Connect. A Lynkgrids page asks for your API key once.

To load the skill as well, upload `skills/lynkgrids/` as a skill in **Settings → Capabilities → Skills**.

## Other MCP clients

Any client that speaks streamable HTTP works:

```json
{
  "mcpServers": {
    "lynkgrids": {
      "type": "http",
      "url": "https://mcp.lynkgrids.com/mcp",
      "headers": {
        "Authorization": "Bearer lgk_live_your_key"
      }
    }
  }
}
```

## What's in the plugin

| Part | What it does |
|---|---|
| `.mcp.json` | Connects the `lynkgrids` MCP server |
| `skills/lynkgrids` | Teaches Claude how Lynkgrids fits together, which tools are safe to call freely, and which need your approval |
| `/lynkgrids:setup` | Checks the connection and walks through workspace setup |
| `/lynkgrids:today` | Read-only brief: today's queue, replies, follow-ups, campaign numbers |
| `/lynkgrids:replies` | Replies waiting, with suggested responses. Sends nothing |

## What you can ask

**Leads**: find people who match your ICP, mine your 1st-degree connections, import a Sales Navigator search, review and approve leads.
**People and companies**: look up, enrich, tag, note, move between pipeline stages.
**Outreach**: draft a connection note and sequence, launch a campaign, build a workflow, send a one-off message.
**Replies**: see agent drafts and New Response leads, edit, and send what you approve.
**Pipeline**: campaign progress, lead counts, follow-ups waiting on you, open tasks.

## Example prompts

- What's queued to go out today, and who replied since yesterday?
- Find 20 people who match my ICP. Show me the list before importing anyone.
- Which of my 1st-degree connections are founders of agencies with 10 to 50 people?
- Draft a connection note for Priya Shah based on her last post. Don't send it.
- Show me the replies waiting for me and suggest a response to each.
- Move everyone who booked a call this week to the After Meeting stage.
- How is my Q4 founders campaign doing? Acceptance and reply rates.

## Safety

Claude reads freely. It asks before it writes, and it shows the exact text before anything reaches a person: connection requests, messages, InMail, email, comments, campaign launches, workflow activation, and reply drafts. LinkedIn's daily limits and weekly invite cap are respected, not retried.

## Related

- [Lynkgrids Sales Engine](https://github.com/lynkread/lynkgrids-sales-engine): a 13-agent outbound team built on this MCP
- [Lynkgrids AI for Jev](https://github.com/lynkread/Lynkgrids-AI-for-Jev): typed lead scoring and reply triage with TypeSafe Jev

## License

MIT
