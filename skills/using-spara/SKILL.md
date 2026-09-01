---
name: using-spara
description: Routes questions about a Spara organization's GTM agents to the right Spara tool — read its agents/channels/data model, answer analytics questions about its data, and answer product how-to questions from the documentation. Use whenever the user asks about their Spara GTM agents, channels, lead fields, CRM sync fields, or their Spara metrics/trends.
---

# Using Spara

Spara exposes one organization's setup over read tools, agent-edit tools, an analytics tool, a feedback tool, and Spara's product documentation. Every tool is automatically scoped to the connected organization — never pass an organization id, and you only ever see this org's data.

## Reading the organization's setup

Start broad, then narrow.

| The user asks about… | Call |
| --- | --- |
| What agents and channels exist (chat, email, voice, text, product demo, workflow) | `list_channels` |
| What Agents exist (the containers that hold channels), including empty ones | `list_agents` |
| A channel's prompt/instructions and booking calendar | `get_channel` (identify channels via `list_channels`; follow its argument schema) |
| An agent's full current configuration | `get_active_agent_config` (identify the channel via `list_channels`; follow its argument schema) |
| The data model — the fields Spara stores about a lead, its account, and owner | `list_data_model_fields` |
| The fields a connected CRM (Salesforce / HubSpot / Marketo) exposes, to sync into the data model | `list_crm_fields` |
| Analytics over the organization's data (lead counts, conversation volumes, trends, …) | `analyze` — natural-language question in, written analysis out |

## Editing agents

These save an **unpublished draft** and never publish — the human reviews and publishes in the Spara app (needs a Spara role with agent-edit access):

| The user wants to… | Call |
| --- | --- |
| Change one agent's AI Instructions (a targeted wording tweak) | `propose_instruction_edit` — a unique substring replace; returns a diff + a link to review and publish |
| Create a new Agent to hold channels | `create_agent_folder` — returns the Agent `id` for `create_channel`'s `agent_folder_id` |
| Create a new channel (chat, email, voice, text, product demo) | `create_channel` — creates it empty; fill it in with `update_channel` |
| Change a channel's configuration (instructions, opening message, prompt, voice, steps, …) | `update_channel` — read the current value with `get_channel` first; editing a published channel saves a draft |

Two reads support the edits: `list_voices` (a `voice_id` for voice/product-demo channels) and `list_product_demo_features` (a feature `id` for a product demo's `opening_feature_id`).

Refer to an agent, channel, or voice by its **name** when talking to the user. Every numeric id these tools return — `id`, `channel_id`, `agent_id`, `agent_folder_id`, `voice_id`, a feature `id` — is an internal handle for your own follow-up tool calls and the `editor_url`. It means nothing to the user, so never show or mention any id in your reply.

## Analytics

For questions about the org's own metrics — trends, comparisons, breakdowns, and "why did X change" over its chats, leads, channels, and workflows — call `analyze` with a natural-language `question` and `intent` (why the user is asking, taken from the conversation; it does not change the analysis). It returns a written analysis (never SQL or raw rows). Example: `analyze(question="why did chat conversions drop last week?", intent="user is preparing a weekly pipeline review")`. There is no raw SQL tool on this surface — all analytics goes through `analyze`.

## Product documentation

For "how does Spara…" / "how do I…" questions — about the product, not this org's data — use the documentation tools: `askQuestion`, `searchDocumentation`, `getPage`.

## Choosing between them

- A specific channel, field, or agent of theirs ("our chat agent's prompt") → the setup tools above.
- A metric or trend over their data ("how are conversions trending") → `analyze`.
- "how does Spara…" / setup instructions → the documentation tools.

## Reporting gaps

When something limits how well you could help — a Spara tool errored or returned the wrong result, the user asked for a concrete action no tool here performs, a supported question you couldn't answer, or the user said an answer was wrong — call `submit_feedback` with a `description` of the gap (plus optional `what_it_tried` and `suggestion`). It records the gap for the Spara team; it changes nothing the user sees, so answer the user normally as well. Skip input-validation errors you can fix by retrying with corrected arguments, and requests outside Spara entirely.

## What Spara can't do here

The edit tools (`propose_instruction_edit`, `create_channel`, `update_channel`) only save **unpublished drafts** — this never publishes. A human reviews and publishes in the Spara app. If the user asks to publish, or to change something these tools don't cover, say that's done in the Spara app rather than guessing — and report the gap via `submit_feedback`.

Never say or imply that you will publish, have published, or can make a change go live. After an edit, tell the user the change is saved as a draft and that they publish it in the Spara app (share the `editor_url`) — the going-live step is always theirs, not yours.
