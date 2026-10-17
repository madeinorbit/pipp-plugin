# Pipp for Grok Bot

[Pipp](https://pipp.so) is a shared email inbox for small support teams. This plugin connects Grok Bot to your own Pipp account so a Bot can set up Pipp with you and work your support email.

Status, 2026-10-17: submitted for listing, not yet listed in Grok Bot. Until the listing is approved, Pipp does not appear in **Connect apps**.

## Install

1. In Grok Bot, open **Connect apps**, search for **Pipp** and choose **Add**.
2. When Grok Bot asks you to authenticate, sign in to Pipp or create an account in your browser, then choose on Pipp's consent page what the Bot may do. The page names Cursor because Grok Bot connects plugins through Cursor's connector service.
3. Ask the Bot, for example: "Set up Pipp for my team's support email."

If a task needs access you did not grant, the Bot tells you. Choose **Reconnect** on Pipp in **Connect apps** and select the extra access.

Each person connects their own Pipp account. A shared Team Bot uses the account of the person talking to it. The Bot never becomes a Pipp member or workspace Owner.

## What the Bot can do

With the permissions you grant, the Bot can:

- create or resume a workspace and inbox, and guide email forwarding and sending-domain DNS setup;
- read cases and saved Pipp summaries, and summarize messages itself in Grok Bot;
- prepare shared reply and note drafts, assign, snooze and close cases;
- send replies you authorize and report their delivery status;
- if you are an Admin and grant it: manage members and invitations, inboxes, sending domains, blocked senders and Pipp AI settings.

Receiving and drafting work before DNS setup. Sending requires a verified custom domain assigned to the inbox; Pipp has no shared sender fallback. Limits: three new workspaces per account per UTC day and 100 To and Cc recipient deliveries per workspace per UTC day.

Some steps stay with you. Changes in your email provider or DNS host need your own access there, Gmail's forwarding confirmation needs your click, and deleting a workspace or transferring ownership needs the Owner's confirmation in Pipp.

The Bot cannot start Pipp AI summaries or change their results. Pipp decides AI access; the Bot can only submit your request after showing you Pipp's disclosure.

## Privacy and access

Content the Bot reads through this plugin, including customer email, is sent to Grok Bot and processed under its terms. In a shared conversation, such as a Slack channel or group chat, anything the Bot reads through your connection can be shown to everyone in that conversation. Teammates see in Pipp which actions a Bot took for you. Customer email sent by a Bot carries no extra footer.

Review or disconnect Bot access at any time in Pipp under **Connected agents** (`https://app.pipp.so/connected-agents`). Disconnecting there stops the Bot's access even if the plugin stays installed in Grok Bot.

Privacy notice: https://pipp.so/privacy/. Contact: hello@pipp.so.

## For developers

The plugin contains an MCP server reference (`mcp.json`, `https://app.pipp.so/api/agent`, OAuth 2.1 with discovery and dynamic client registration) and two skills: `pipp-setup` and `pipp-cases`. It has no local commands, hooks or stored credentials.
