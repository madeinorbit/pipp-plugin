# Pipp for Grok Bot

**Customer support, minus the fuss, right where you already talk to Grok.**

[Pipp](https://pipp.so) is a simple shared email inbox for small teams. Your customer emails land in one place, the whole team can see them, and you reply together. This plugin lets Grok Bot work in that inbox with you: set Pipp up in a conversation, find out what's waiting, catch up on a conversation, and prepare replies your team can review. Grok Bot sends a reply only when you tell it to.

Status: not yet listed in Grok Bot. Until the listing is approved, Pipp does not appear in **Connect apps**.

## How Grok Bot helps

**Setting up support for the first time.** Say "Set up Pipp for support@mycompany.com." Grok Bot creates your workspace and inbox, walks you through forwarding from Gmail or your email provider, lists the DNS records for your sending domain and checks each step as you go. You only do the steps that need you: a click in Gmail, a change at your DNS host.

**Answering customers.** Ask "What needs a reply?" or "Catch me up on the Acme conversation." Grok Bot reads the conversation and Pipp's saved summary, drafts a reply in the shared draft your teammates see in Pipp, and adds an internal note when you want to hand something over. Assign, snooze or close a conversation without switching apps.

**Running the team.** If you're an Admin and allow it, Grok Bot can invite teammates, manage inboxes and sending domains, and block a sender who keeps causing trouble.

**In a team chat.** A shared Team Bot, for example in Slack, uses the Pipp account of whoever is talking to it. Everyone acts as themselves, with their own access.

**Always under your control.** You connect your own account and choose exactly what Grok Bot may do. Teammates see in Pipp which actions a Bot took for you, and customers get your reply with no added footer. Disconnect any time in Pipp under **Connected agents**.

## Install

1. In Grok Bot, open **Connect apps**, search for **Pipp** and choose **Add**.
2. When Grok Bot asks you to authenticate, sign in to Pipp or create an account in your browser, then choose on Pipp's consent page what the Bot may do. The page names Cursor because Grok Bot connects plugins through Cursor's connector service.
3. Ask the Bot, for example: "Set up Pipp for my team's support email."

If a task needs access you did not grant, the Bot tells you. Choose **Reconnect** on Pipp in **Connect apps** and select the extra access.

Each person connects their own Pipp account. A shared Team Bot uses the account of the person talking to it. The Bot never becomes a Pipp member or workspace Owner.

## What the Bot can do, exactly

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
