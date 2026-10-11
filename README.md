# Pocket Pulse plugin

Finds subscriptions, cost increases, bills and spending habits in your bank
and card transactions, from CSV exports or SimpleFIN, inside Claude Code. Run `/pocket-pulse:money-review` for
a guided review.

```
/plugin install pocket-pulse --marketplace fighterz8/pocket-pulse
```

From a shell: `claude plugin install pocket-pulse --marketplace fighterz8/pocket-pulse`.
It needs Node 20 or later.

## Privacy

Pocket Pulse runs on your computer and keeps your transactions in a local
folder. It goes online only to fetch your transactions from SimpleFIN, and
only if you connect it. What it finds (merchants, amounts and dates) becomes
part of your conversation with Claude, like anything else you share there.

SimpleFIN setup tokens are read from a file you save, never from the
conversation. The access they unlock is stored in the data folder and never
shown to Claude.

The folder is Claude Code's data folder for the plugin
(`~/.claude/plugins/data/pocket-pulse-pocket-pulse/`), which Claude Code
deletes when the plugin is uninstalled. Other MCP clients use
`~/.pocketpulse/`, or the folder named by `POCKET_PULSE_DATA`. The folder and
its files are readable only by you.

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

## Other MCP clients

The server needs only Node 20 or later: `dist/server.mjs` loads
`dist/engine.mjs` beside it and nothing else. Both files are readable
JavaScript, and the only network calls are SimpleFIN's. Point any MCP
client at it:

```json
{ "command": "node", "args": ["/path/to/plugin/dist/server.mjs"] }
```
