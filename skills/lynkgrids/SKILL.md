---
name: lynkgrids
description: Work in the user's Lynkgrids workspace through the Lynkgrids MCP. Find and import LinkedIn leads, look up and enrich people and companies, manage lists, pipelines, and tasks, run campaigns and workflows, and triage replies. Use when the user asks about leads, prospects, LinkedIn outreach, connection requests, campaigns, replies, follow-ups, pipeline, or their Lynkgrids CRM. Confirm before any write, import, or anything that reaches a real person.
---

# Lynkgrids

Lynkgrids is a LinkedIn prospecting and outreach CRM. The `lynkgrids` MCP tools act on the user's live workspace at `https://mcp.lynkgrids.com/mcp`.

Call only tools the server actually exposes. Do not invent a tool name, field, or filter. If a name below is missing from the connected server, skip it and use the tools that are present. For any route without a dedicated tool, read the `lynkgrids://api-guide` resource and use `lynkgrids_request`.

## How the pieces fit

- A **person** belongs to a **company**, can carry **tags**, sit in **lists**, and sit in a **pipeline stage**.
- A **LinkedIn seat** (`list_linkedin_accounts`) is the account that sends. Its `linkedinAccountId` is the `account_id` for send tools.
- Sends need the recipient's LinkedIn **`provider_id`**. Enriched people carry it as `linkedinProviderId`; otherwise use `resolve_provider_id` with the username from `linkedin.com/in/<username>`.
- A **campaign** runs the standard sequence: connection request → accepted → message 1 → wait 3 days → message 2 → wait 4 days → message 3 → wait 4 days → message 4. A reply stops it for that person. For people already connected, pass `skipConnect`.
- A **workflow** is a custom drip of `connect` / `message` / `inmail` / `email` / `enrich` / `comment` steps, each with `waitDays`.
- **In-app agents** prospect and draft replies on their own. Their drafts wait in `pending_replies`.
- People who replied are tagged **New Response** until someone handles them.

## Start of a session

Call `whoami`, then `setup_status`. If setup is incomplete, ask the next question it returns (one at a time: about you and who you sell to, working hours and timezone, LinkedIn seat, ideal customer profile) and save each answer with `save_profile` or `save_icp` as soon as you have it.

## Read freely

These tools don't change anything. Use them to answer questions:

- `whoami`, `setup_status`, `setup_checklist`, `workspace_overview`, `todays_plan`
- `list_linkedin_accounts`, `list_team_members`
- `search_people`, `get_person`, `lookup_person`, `search_companies`, `get_company` (searches are paginated; page until `pagination.totalPages`)
- `find_leads`, `list_connections`: return candidate rows, nothing is imported
- `review_leads`, `leads_summary`, `leads_link`
- `list_watchlist`, `latest_posts`
- `list_stages`, `list_custom_fields`, `list_tasks`
- `campaign_progress`, `agent_list`, `get_agent`, `agent_settings`
- `pending_replies`, `conversation_history`, `list_follow_ups`, `waiting_for_me`, `unanswered_questions`
- `get_subscription_link`: the user's billing link and plan status, for subscribe / upgrade / renew
- `search_knowledge`, `list_knowledge`: call `search_knowledge` before drafting any message or reply, and state only facts it returns

## Confirm before you change anything

Show the exact change and wait for an explicit yes before calling:

- `import_leads` (each pick needs a `fitScore` 1–5 and a one-line `reason`), `approve_leads`, `bulk_import_people`, `linkedin_search_import`, `retry_failed_leads`
- `create_person`, `update_person`, `create_company`, `update_company`, `add_note`, `enrich_person`, `score_existing_contacts`
- `create_list`, `add_to_list`, `create_pipeline`, `create_stage`, `create_custom_field`, `create_task`, `complete_task`
- `save_profile`, `save_company_profile`, `save_icp`, `add_knowledge`, `learn`, `answer_question`, `resolve_watchlist_item`
- `create_agent`, `update_agent`, `run_agent`, `pause_agent`, `resume_agent`, `allow_autonomy`
- `create_workflow` (create as `draft`; activating it is a separate yes)
- `pause_campaign`, `resume_campaign`, `run_campaign_now`, `stop_campaign_run`
- `lynkgrids_request` with any method other than GET

## These reach a real person: show the exact text first

- `launch_campaign`, `launch_outreach`
- `send_connection_request` (note at most 300 characters)
- `send_linkedin_message` (1st-degree only), `send_inmail` (needs a subject), `send_email`
- `send_reply_draft` (only after a yes on the exact draft text)
- `comment_on_post`, `create_post`

Never send a message, launch a campaign, activate a workflow, or import leads because the task implied it. Draft it, show it, and stop until the user confirms. If the user asks for something no tool can do, call `cannot_do` with their words and say so plainly.

## Gotchas

- **`update_person` `tags` replaces the whole array.** To add or remove one tag, `get_person` first, edit the list, and send the full new list.
- **"This user is not a relation"** means you aren't 1st-degree connected. Offer a connection request instead; don't retry the message.
- **"Invalid parameters" on a connection request** is usually the weekly invite cap, not a bad payload. Stop sending invites and tell the user.
- **Seat ownership.** You can only send from a seat the user owns or is assigned. Acting as a teammate needs `actAsUserId` through `lynkgrids_request`, and only for admins.
- **Daily limits.** Check `todays_plan` before adding volume.
- **Plans.** If a tool says the workspace has no plan, its plan has ended (read-only), a limit was reached, or a feature isn't on the plan, show that message and its link to the user, and stop changing things; don't retry. When the user asks to subscribe, upgrade, renew, or see their plan, call `get_subscription_link` and share the link. Payment always happens in the browser: never ask for, accept, or enter card details.

## Setup

The tools are missing, or calls fail with an auth error, when the user hasn't signed in. Sign-in happens in the browser:

- **Claude Code:** run `/mcp`, pick **lynkgrids**, and choose **Authenticate**.
- **claude.ai / Claude Desktop:** Settings → Connectors → Lynkgrids → **Connect**.

On the page that opens, the user clicks **Continue to Lynkgrids**, signs in or creates an account, picks a plan if the workspace has none (paid in the browser), and clicks **Allow**. Lynkgrids then creates the connector's key itself. Someone who already has a Read & write API key can choose **Use an API key instead** on that page and paste it there, never into chat. If the user pastes a key into chat anyway, don't repeat or store it, and suggest rotating it.
