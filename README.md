# ScaleSKUs plugin for Claude

Audit and optimize your Amazon Ads account with Claude, using your ScaleSKUs data. The plugin connects Claude to the ScaleSKUs MCP server and teaches it how to use it well: which tools answer which question, how to read the numbers correctly, how to judge whether a search term fits a product, and how to propose changes that you approve before anything changes on Amazon.

## What's included

**Skill**

- `scaleskus-optimizer`: a product-aware audit of one advertising account. It maps what the account sells, then looks for wasted spend, under-funded winners, category search terms the account doesn't advertise yet, and bid, budget and placement efficiency. The result is an owner-readable report in which every recommendation names a product and a keyword. The skill also carries the ground rules Claude follows for any ScaleSKUs question.

**Commands.** In Claude Code and Cowork, run them as `/scaleskus:<command>`. In Claude chat they load as skills and Claude applies them when your request fits.

| Command | What it does |
|---|---|
| `weekly-review` | One prioritised plan for the week: ads on broken products, waste to cut, search terms to harvest, capped winners to unlock |
| `cut-waste` | Search terms and targets that spend without selling, with fit-checked negatives |
| `grow-sales` | Converting search terms you don't target yet, placed as exact keywords on the right product |
| `fix-bids` | Bid cuts and raises computed from your target ACoS, with the maths behind each |
| `budget-caps` | Efficient campaigns that run out of budget, with safe budget raises |
| `sales-drop` | Why sales or ad sales dropped, with causes ranked by money lost |

**Connector.** The ScaleSKUs MCP server at `https://scaleskus.com/mcp` (Streamable HTTP, OAuth sign-in). If you have already added the ScaleSKUs connector, this is the same server, so you see one set of tools.

## Requirements

- A ScaleSKUs account with your Amazon Ads account connected. Sales, traffic, inventory and profit questions also need your Seller Central or Vendor Central account connected in ScaleSKUs.
- Sign-in through ScaleSKUs' own OAuth flow. When you connect the ScaleSKUs connector, you sign in on ScaleSKUs' authorization page with your ScaleSKUs login and approve the access shown on the consent screen. The plugin contains no API keys, tokens or passwords.

## Install and connect

- **Claude (web, desktop) and Cowork:** add the plugin from **Customize > Plugins**, then open the plugin's **Connectors** tab and connect ScaleSKUs. To try a local copy, zip this folder and use **Customize > Plugins > Add > Upload plugin**.
- **Claude Code:** a plugin added on claude.ai syncs to Claude Code. To load a local copy for one session, run `claude --plugin-dir ./scaleskus-plugin`. Then run `/mcp` to sign in to ScaleSKUs.

## Try it

- "Give me a briefing on my Amazon Ads account."
- "Audit my account and write an optimization report."
- "Which search terms wasted the most spend last month?" or `/scaleskus:cut-waste`
- "Which campaigns ran out of budget this week, and what did it cost me?"
- "Why are my sales down this week?"

## How changes work

- **Nothing changes unless you ask.** Recommendations come back as a numbered plan.
- **Queued by default.** When you pick items ("apply 1 and 3"), Claude queues them in ScaleSKUs as pending tasks. Nothing changes on Amazon until they are approved, either by you under Tasks in ScaleSKUs or by an explicit instruction to Claude.
- **Live only on your explicit go-ahead.** Claude applies a change live only when you explicitly ask for it, and it states exactly what will change first. ScaleSKUs requires two-factor verification on your login for live changes; without it, changes stay queued. A daily limit on applied changes per Amazon account also applies.
- **Audited.** Every change goes through ScaleSKUs' tasks pipeline and is recorded in your account's audit log. Automation rules and day-parting schedules are created switched off and run only after you enable them.
- **Read-only connections.** If your ScaleSKUs connection has no write tools, Claude still analyses and recommends, and gives you the changes as a checklist.

## Data: what the plugin sends and fetches

- **The plugin itself** is Markdown instructions plus the address of one MCP server. It contains no code, scripts, hooks or credentials, runs nothing on your computer, and collects nothing itself.
- **Claude to ScaleSKUs.** When you ask a question, Claude calls ScaleSKUs tools over HTTPS at `https://scaleskus.com/mcp`. Each call sends the inputs that tool needs: typically the account, date ranges and filters, identifiers of campaigns, keywords, targets or search terms, and the details of any change you ask Claude to queue or apply. Calls are made with your ScaleSKUs sign-in and are limited to the organization and Amazon accounts your login can access.
- **ScaleSKUs to Claude.** ScaleSKUs answers from the Amazon Ads and Amazon Seller Central or Vendor Central data your ScaleSKUs account already syncs. The results come back into your Claude conversation, where they are handled under your agreement with Anthropic.
- **What ScaleSKUs keeps.** ScaleSKUs logs which tools your assistant called, with the inputs and a preview of the results; its privacy policy sets how long.
- **ScaleSKUs to Amazon.** Reads come from data ScaleSKUs has already synced through Amazon's APIs. Changes reach Amazon only through the approval pipeline described above.
- **Team memory, only on request.** Claude saves a note to your organization's shared team memory in ScaleSKUs only when you ask it to, or agree when it offers. You can remove notes at any time.
- **Google Sheets, only on request.** If you ask for a large export, ScaleSKUs can write it to a Google Sheet in your own Google Drive. This works only if you have connected your Google account in ScaleSKUs.
- **Privacy.** ScaleSKUs' privacy policy states that it does not sell personal data and does not use your Amazon account data to train AI models. Read the [privacy policy](https://scaleskus.com/privacy-policy) and the [terms](https://scaleskus.com/terms).

## Support

- Documentation: [scaleskus.com/docs/mcp](https://scaleskus.com/docs/mcp)
- Support: [support@scaleskus.com](mailto:support@scaleskus.com)
- Security reports: email support@scaleskus.com with "Security report" in the subject.

## About

Published by Artallur Technologies India LLP (ScaleSKUs), an Amazon Ads Verified Partner. Amazon, Amazon Ads, Seller Central and Vendor Central are trademarks of Amazon.com, Inc. or its affiliates. This plugin is not made or endorsed by Amazon or Anthropic.

## License

MIT. See [LICENSE](LICENSE).
