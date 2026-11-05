# NeatListing: Google Business Profile MCP server

NeatListing lets AI assistants manage Google Business Profiles (formerly Google My Business, or GMB). It works with Claude, Claude Code, ChatGPT, Codex, Cursor and any other MCP client. Your assistant can update hours, publish posts, add photos, reply to reviews and read up to 18 months of performance data. You sign in with Google once and decide what it's allowed to change. You don't need your own Google API approval, a Google Cloud project or a credit card.

- MCP server: `https://app.neatlisting.com/mcp` (Streamable HTTP, OAuth 2.1)
- Website and setup guide: [neatlisting.com/setup](https://neatlisting.com/setup)
- Official MCP registry: `com.neatlisting/neatlisting`

This repository holds the NeatListing plugin for Claude Code, Claude, Codex, Cursor and Grok Build, plus the server's registry manifest. The server itself is hosted by NeatListing, so there is nothing to run.

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
| Profile (one tool per setting) | `update_regular_hours`, `update_special_hours`, `update_more_hours`, `update_description`, `update_services`, `update_labels`, `update_open_status`, `update_website`, `update_phone`, `update_additional_phones`, `update_business_name`, `update_address`, `update_primary_category`, `update_secondary_categories`, `update_service_area`. Also `get_attributes`, `update_attributes`, `search_categories` |
| Posts, offers and events | `list_posts`, `create_post`, `update_post`, `delete_post` |
| Reviews | `list_reviews`, `reply_to_review`, `delete_review_reply` |
| Photos | `list_photos`, `add_photo`, `delete_photo` |
| Booking and order links | `list_action_links`, `create_action_link`, `update_action_link`, `delete_action_link` |
| Menus and hotels | `get_food_menus`, `update_food_menus`, `get_lodging`, `update_lodging` |
| Performance | `get_performance` (calls, direction requests, website clicks, bookings and views for up to 18 months), `get_search_keywords` |
| Verification | `get_verification_status`, `start_verification`, `complete_verification` |
| Its own changes | `list_my_changes`, `get_change`, `undo_my_change` |
| Older waiting changes | `list_pending_changes`, `approve_change`, `reject_change` (the owner says yes or no in the chat) |
| Orientation and setup | `get_my_permissions`, `list_locations`, `get_location`, `get_google_updates`, `list_admins`, `preview_change`, `get_setup_status`, `finish_setup`, `set_location_access` |

Things to ask once you're connected:

- "Set holiday hours for Christmas Eve on all my locations: open 6 AM to 2 PM."
- "Reply to this week's unanswered reviews rated 4 or 5 stars."
- "Post our new seasonal drink on the Downtown location."
- "How many calls and direction requests did each location get last month?"
- "Which searches showed my business on Google last month?"

## Safety

- Each profile setting has its own tool, so you decide in your AI app which ones need your OK (in Claude: Always allow, Needs approval or Blocked, per tool; ChatGPT and Codex have the same kind of per-tool approval).
- When you connect, you choose which locations the assistant can reach, or give it read-only access.
- High-risk fields (business name, address, categories, phones, website, service area, open/closed status) can get a profile flagged or suspended by Google, so the assistant tells you that and checks before changing one.
- Pace limits and keyword-stuffing checks guard against edits that can get a profile suspended.
- Agents can't add or remove managers, transfer profiles or delete them.
- Every change is logged. Profile edits, attributes, menus and review replies can be undone.

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

Only if you want it to. Those changes can trigger a re-verification or a suspension, so the assistant warns you first. To stop it entirely, set `update_business_name` and `update_address` to blocked in your AI app's tool settings, or connect read-only.

### What happens to my data?

See the [privacy policy](https://neatlisting.com/privacy). NeatListing stores the changes it makes so you can review and undo them. It doesn't sell data or use it for advertising.

## Repository layout

- `.claude-plugin/marketplace.json`: Claude Code marketplace
- `.cursor-plugin/marketplace.json`: Cursor marketplace
- `plugin/`: the plugin (Claude Code manifest in `.claude-plugin/`, Cursor manifest in `.cursor-plugin/`, Grok Build manifest in `.grok-plugin/`, Agent Plugins manifest for ChatGPT and Codex in `plugin.json`, the MCP server config and the `google-business-profile` skill)
- `server.json`: the official MCP registry manifest

## Support

- Questions and bug reports: [GitHub issues](https://github.com/neatlisting/neatlisting/issues). For anything about your own account or business, email hello@neatlisting.com rather than posting publicly.
- Security reports: hello@neatlisting.com
- [Privacy policy](https://neatlisting.com/privacy) · [Terms of service](https://neatlisting.com/terms)

NeatListing is a product of Gronda LLC. It isn't affiliated with or endorsed by Google. Google Business Profile is a trademark of Google LLC.
