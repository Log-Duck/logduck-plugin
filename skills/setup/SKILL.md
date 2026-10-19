---
name: setup
description: Set up LogDuck - event logging and notifications - for an app from a description of what it does, using the LogDuck MCP server. Proposes the event types and categories worth tracking, creates them, mints an API key, prints the SDK snippet for the app's language, posts a first test event and confirms it arrived. Use when someone wants LogDuck wired into a new or existing app, or asks what they should be logging.
---

# Set up LogDuck for an app

You need the LogDuck MCP server connected (`https://api.logduck.com/mcp`) with a token or sign-in that allows configuration. Everything below is a LogDuck tool call; the tools enforce the user's role, so if a step is refused for lack of access, stop and report it rather than working around it.

Keep each tool's result; later steps use the ids it returns. Say what you are about to create before you create it.

1. **Orient.** Call `whoami`. If the user belongs to one organization, use it; if several, ask which one before creating anything. Read the `logduck://docs` resource once - it has the event wire format, the SDK snippets and the validation rules.

2. **Create the project.** Propose a short project name from the description, confirm it, then `create_project`. Only the organization's owner can create projects: if refused as not the owner, tell the user who to ask; if refused for the plan's project limit (one on the free plan), offer to set up in an existing project (`list_projects`). The project comes with a Production environment: `list_environments`, note the id of the one with `isProduction: true`.

3. **Propose event types.** From the description, list 4-8 things worth knowing when they happen (a signup, a payment failing, an export finishing). For each, propose a `name` in wire form - lowercase, dotted, such as `user.signup` or `payment.failed` - a display name, an emoji, what it means for the business (`archetype`: `signup`, `activity`, `conversion`, `revenue` or `error` - leave it out when none fits; LogDuck never infers this from a name), and whether it should push a notification to the team's phones (reserve that for things someone should act on). Show the list; let the user confirm, add or drop before creating anything.

4. **Create them.** `list_event_categories` for the presets; `create_event_category` for any the proposal needs; then `create_event_type` for each, passing the category names in `categoryIds`. Check with `list_event_types`.

5. **Mint the key.** `create_api_key` with `sourcetype` `server` for a backend or `client` for users' devices. A client key is readable by anyone who has the page or the app, so give it `allowedOrigins` (`https://*.example.com` covers the apex and every subdomain) for a website, or `allowsMissingOrigin: true` for a native app or CLI, which send no Origin header. The response carries the key exactly once: hand it to the user verbatim, tell them to store it as a secret, and never repeat it later. Refused means the user is not the owner - say so.

6. **Print the snippet.** From the docs resource, give the install command and the smallest working example for the app's language, with the key placeholder and one of the event types created above.

7. **Prove it works.** `post_test_event` with one of the new types and a short `message`, then `list_events` in Production and show the event back.

8. **Close.** Summarise what exists - project, environment, event types, categories, the key's prefix - and suggest the next step: a Slack or Discord webhook (`create_webhook`, `enable_event_type_webhook`) or, on PRO, an alert rule (`create_action_group`, then `create_alert_rule`) for the type that matters most.
