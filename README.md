# LogDuck plugin for Claude Code

[LogDuck](https://logduck.com) is event logging and notifications for apps: post events, get pushed when the ones that matter happen. This plugin gives Claude Code the LogDuck MCP server and a skill that sets LogDuck up for an app.

## Install

```
claude plugin marketplace add Log-Duck/logduck-plugin
claude plugin install logduck@logduck
```

Then run `/mcp` and sign in to `logduck` with your LogDuck account. Nothing else to configure: there is no token to paste and no client id to register.

## What you get

- **The LogDuck MCP server** at `https://api.logduck.com/mcp`. Sixty-odd tools covering everything the dashboard configures (projects, environments, API keys, event types, categories, webhooks, alert rules, scheduled summaries) and everything it reads (events, usage, telemetry, analytics, geo distribution). Tools respect your role in each organization, and the server never deletes a project or an environment.
- **`/logduck:setup`**, a skill that takes a description of an app, proposes the event types worth tracking, creates them, mints an API key, prints the SDK snippet for your language, posts a first test event and shows it arriving.

## What it sends and fetches

Every tool call goes to `https://api.logduck.com/mcp` with the OAuth token Claude Code obtains when you sign in. Sign-in happens in your browser at `https://logduck.com/oauth/authorize`; the plugin itself holds no credentials. The skill also reads the server's `logduck://docs` resource for the event format and SDK snippets. Nothing goes anywhere else. You can see and revoke the connection at any time from your LogDuck account page.

## Other hosts

The same server works in claude.ai (add it as a custom connector by URL), Cursor, ChatGPT and any MCP client that speaks streamable HTTP. See [the documentation](https://logduck.com/docs#connect-an-agent).

## License

MIT.
