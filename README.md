# NeatListing: Google Business Profile MCP server

NeatListing lets AI assistants manage Google Business Profiles (formerly Google My Business, or GMB). It works with Claude, Claude Code, ChatGPT, Codex, Cursor and any other MCP client. Your assistant can update hours, publish posts, add photos, reply to reviews and read up to 18 months of performance data. You sign in with Google once and decide what it's allowed to change. You don't need your own Google API approval, a Google Cloud project or a credit card.

- MCP server: `https://app.neatlisting.com/mcp` (Streamable HTTP, OAuth 2.1)
- Website and setup guide: [neatlisting.com/setup](https://neatlisting.com/setup)
- Official MCP registry: `com.neatlisting/neatlisting`

This repository holds the NeatListing plugin for Claude Code, Claude and Codex, plus the server's registry manifest. The server itself is hosted by NeatListing, so there is nothing to run.

## Install

### Claude Code

The plugin adds the server and a skill that teaches Claude Code how to use it:

```bash
claude plugin marketplace add neatlisting/neatlisting
```

```bash
claude plugin install neatlisting@neatlisting
```

Or add only the server:

```bash
claude mcp add --transport http --scope user neatlisting https://app.neatlisting.com/mcp
```

Then run `/mcp` in Claude Code and choose **Authenticate**.

### Claude (web, desktop and mobile)

Settings → Connectors → Add custom connector, URL `https://app.neatlisting.com/mcp`.

### Codex

```bash
codex mcp add neatlisting --url https://app.neatlisting.com/mcp
```

```bash
codex mcp login neatlisting
```

### ChatGPT

Turn on developer mode, then create an app with URL `https://app.neatlisting.com/mcp` and OAuth authentication.

### Cursor

Add this to `~/.cursor/mcp.json`:

```json
{ "mcpServers": { "neatlisting": { "url": "https://app.neatlisting.com/mcp" } } }
```

The first time your assistant uses NeatListing, it opens a sign-in page. Sign in with the Google account that manages your Business Profiles, choose which locations the assistant can reach, and click **Allow**.

## What your assistant can do

| Area | Tools |
|---|---|
| Profile | `update_location`: hours, holiday and special hours, more hours, description, services, labels, open status, website, phone, secondary categories. `get_attributes`, `update_attributes`, `search_categories` |
| Posts, offers and events | `list_posts`, `create_post`, `update_post`, `delete_post` |
| Reviews | `list_reviews`, `reply_to_review`, `delete_review_reply` |
| Photos | `list_photos`, `add_photo`, `delete_photo` |
| Booking and order links | `list_action_links`, `create_action_link`, `update_action_link`, `delete_action_link` |
| Menus and hotels | `get_food_menus`, `update_food_menus`, `get_lodging`, `update_lodging` |
| Performance | `get_performance` (calls, direction requests, website clicks, bookings and views for up to 18 months), `get_search_keywords` |
| Verification | `get_verification_status`, `start_verification`, `complete_verification` |
| Its own changes | `list_my_changes`, `get_change`, `undo_my_change` |
| Approvals | `list_pending_changes`, `approve_change`, `reject_change` (the owner says yes or no in the chat) |
| Orientation and setup | `get_my_permissions`, `list_locations`, `get_location`, `get_google_updates`, `list_admins`, `preview_change`, `get_setup_status`, `finish_setup`, `set_location_access`, `get_rules`, `update_rules` |

Things to ask once you're connected:

- "Set holiday hours for Christmas Eve on all my locations: open 6 AM to 2 PM."
- "Reply to this week's unanswered reviews rated 4 or 5 stars."
- "Post our new seasonal drink on the Downtown location."
- "How many calls and direction requests did each location get last month?"
- "Which searches showed my business on Google last month?"

## Safety

- The business name, address, primary category and service area are locked by default.
- Website and phone changes wait for the owner's approval by default. Owners choose what else needs approval, and can approve by telling their assistant or in the NeatListing dashboard.
- Pace limits and keyword-stuffing checks guard against edits that can get a profile suspended.
- Agents can't add or remove managers, transfer profiles or delete them.
- Each assistant sees only the locations the owner allowed, and every change is logged. Profile edits, attributes, menus and review replies can be undone.
- Before an assistant can let a high-risk field go live or approve a high-risk change, it has to read the owner a warning and pass along the owner's own words of approval. Those words are kept in the owner's activity log.

## How it works

NeatListing follows the MCP authorization spec. Your assistant registers itself (OAuth 2.1 dynamic client registration with PKCE), and you approve it on a NeatListing page after signing in with Google. NeatListing checks that you're an owner or manager of each profile and keeps the Google tokens on its side. Assistants never see them. Owners who prefer a key can create an API key and send it as `Authorization: Bearer <key>`.

## FAQ

### Is it free?

Yes.

### Do I need Google API access?

No. NeatListing has its own approved access to the Google Business Profile APIs, so you just sign in.

### Does it work with several locations?

Yes. Agencies and multi-location businesses can let an assistant work on all of their profiles or only some of them.

### Can my assistant change my business name or address?

Not by default. Those fields are locked because changing them can trigger a re-verification or a suspension. If you want your assistant to handle them anyway, tell it. It will read you a warning first, and it can only open them up with your own words of approval.

### What happens to my data?

See the [privacy policy](https://neatlisting.com/privacy). NeatListing stores the changes it makes so you can review and undo them. It doesn't sell data or use it for advertising.

## Repository layout

- `.claude-plugin/marketplace.json`: Claude Code marketplace
- `plugin/`: the plugin (Claude Code manifest in `.claude-plugin/`, Agent Plugins manifest for ChatGPT and Codex in `plugin.json`, the MCP server config and the `google-business-profile` skill)
- `server.json`: the official MCP registry manifest

## Support

- Questions and bug reports: [GitHub issues](https://github.com/neatlisting/neatlisting/issues). For anything about your own account or business, email hello@neatlisting.com rather than posting publicly.
- Security reports: hello@neatlisting.com
- [Privacy policy](https://neatlisting.com/privacy) · [Terms of service](https://neatlisting.com/terms)

NeatListing is a product of Gronda LLC. It isn't affiliated with or endorsed by Google. Google Business Profile is a trademark of Google LLC.
