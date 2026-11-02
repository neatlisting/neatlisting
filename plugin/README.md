# NeatListing plugin

Manage Google Business Profiles (formerly Google My Business) from Claude, Claude Code, ChatGPT and Codex.

NeatListing is a hosted MCP server at `https://app.neatlisting.com/mcp`. The first time you use it, your assistant opens a NeatListing page: sign in with the Google account that manages your Business Profiles, choose which locations the assistant can reach, and click Allow. You don't need your own Google API approval.

## What it can do

- Hours, holiday hours, open/closed status, description, services, attributes, website and phone
- Posts, offers and events
- Photos
- Review replies
- Booking and ordering links, food menus, hotel details
- Calls, direction requests, website clicks and views for up to 18 months, plus search keywords
- Your own change history, with undo

Your business name, address, primary category and service area are locked by default. Changes you choose (by default website, phone, booking links and verification) wait for your approval in NeatListing, and every change is logged. Profile edits, attributes, food menus and review replies support undo.

## What it sends and stores

The plugin has no local code. It adds one remote MCP server, `https://app.neatlisting.com/mcp`, and a skill that tells the assistant how to use it. Your assistant sends NeatListing the requests you ask it to make (for example new holiday hours or a review reply). NeatListing checks them against your rules, sends allowed changes to Google, and keeps a log of every change so you can review or undo it. Your assistant never sees your Google sign-in or tokens. Details: [privacy policy](https://neatlisting.com/privacy).

## Install

- **Claude Code:** `claude plugin marketplace add neatlisting/neatlisting`, then `claude plugin install neatlisting@neatlisting`. Or add only the server: `claude mcp add --transport http --scope user neatlisting https://app.neatlisting.com/mcp`.
- **Codex:** `codex mcp add neatlisting --url https://app.neatlisting.com/mcp`, then `codex mcp login neatlisting`.
- **Claude and ChatGPT apps:** see [neatlisting.com/setup](https://neatlisting.com/setup).

## Contents

- `plugin.json`, `mcp.json`: Agent Plugins manifest (ChatGPT and Codex)
- `.claude-plugin/plugin.json`, `.mcp.json`: Claude Code manifest
- `skills/google-business-profile`: how to use the tools well and within Google's rules

## Support and privacy

- Support: hello@neatlisting.com
- Privacy policy: https://neatlisting.com/privacy
- Terms: https://neatlisting.com/terms

NeatListing is a product of Gronda LLC. It isn't affiliated with or endorsed by Google. Google Business Profile is a trademark of Google LLC.
