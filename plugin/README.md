# Pocket Pulse plugin

Finds subscriptions, cost increases, bills and spending habits in your bank
and card transactions, inside your AI agent. It works best with the ledger
export from a free [pocket-pulse.com](https://pocket-pulse.com) account,
which keeps your categories and corrections. It also reads bank and card
CSVs, and can connect through SimpleFIN.

It is a standard MCP server, so it works with agents that support MCP, such
as Claude Code, Claude Desktop, Cursor, VS Code, Codex CLI and Gemini CLI.
It needs Node 20 or later.

## Claude Code

```
/plugin install pocket-pulse --marketplace fighterz8/pocket-pulse
```

From a shell: `claude plugin install pocket-pulse --marketplace fighterz8/pocket-pulse`.
Then run `/pocket-pulse:money-review` for a guided review.

## Other AI agents

1. Download it: `git clone https://github.com/fighterz8/pocket-pulse`
2. Add an MCP server to your agent's settings that runs Node with the
   downloaded `plugin/dist/server.mjs`:

   ```json
   { "command": "node", "args": ["/full/path/to/pocket-pulse/plugin/dist/server.mjs"] }
   ```

3. Ask your agent to review your Pocket Pulse ledger export, or a bank CSV.

## Privacy

Pocket Pulse runs on your computer and keeps your transactions in a local
folder. It goes online only to fetch your transactions from SimpleFIN, and
only if you connect it. What it finds (merchants, amounts and dates) becomes
part of your conversation with your AI agent, like anything else you share
there.

SimpleFIN setup tokens are read from a file you save, never from the
conversation. The access they unlock is stored in the data folder and never
shown to your agent.

Most agents keep it in `~/.pocketpulse/`, or the folder named by
`POCKET_PULSE_DATA`. In Claude Code it is the plugin's data folder
(`~/.claude/plugins/data/pocket-pulse-pocket-pulse/`), which Claude Code
deletes when the plugin is uninstalled. The folder and its files are
readable only by you.

## Tools

| Tool | What it does |
|---|---|
| `import_csv` | Imports a bank or card CSV, or a ledger export from pocket-pulse.com (keeping its categories). Skips rows already imported. |
| `leak_report` | Subscriptions, possible duplicates, new charges, cost increases, usage-based charges, bills, ended charges, spending habits and charges to check. |
| `list_recurring` | Every current recurring charge with its monthly cost. |
| `explain_finding` | The reasons, price history and transactions behind one finding. |
| `set_decision` | Saves the user's call on a charge: needed, leak, hide or undo. |
| `categorize_merchant` | Sets a merchant's category for current and future imports. |
| `cancel_help` | Where to cancel a subscription: the service's own help page and account page, or the store that bills it. It never cancels anything. |
| `connect_simplefin` | Claims a SimpleFIN setup token saved in a file, stores the access and imports up to a year of history. |
| `sync_simplefin` | Fetches new transactions from SimpleFIN, at most once a day. |
| `disconnect_simplefin` | Deletes the stored SimpleFIN access; imported transactions stay. |

## What runs

`dist/server.mjs` loads `dist/engine.mjs` beside it and nothing else. Both
files are readable JavaScript, and the only network calls are SimpleFIN's.
