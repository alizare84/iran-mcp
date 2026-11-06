**English** · [فارسی](README.fa.md)

# iran-mcp

72 read-only MCP servers and 896 tools for public Iranian websites. Prices are in Toman. The full list is in [TOOLS.md](TOOLS.md) and [servers.json](servers.json).

This tree tracks [sepehr071/iran-mcp](https://github.com/sepehr071/iran-mcp). New sites are added here by pull request.

## Add a site

Write only the site name. The prompt builds a read-only server in this repo's shape and opens a pull request.

1. Fill the `سایت:` line in [prompt.md](prompt.md) and paste the whole block into an agent.
2. The agent adds `servers/<name>-mcp/` and opens a pull request on [alizare84/iran-mcp](https://github.com/alizare84/iran-mcp).
3. The pull request is reviewed before the server joins the catalog.

The contract for names, files, tools, and the HTTP client is [format.md](format.md). If the site is already listed, or the data sits behind a login, the agent does not open a pull request.

## Run a server

```json
{
  "mcpServers": {
    "digikala": { "command": "uvx", "args": ["digikala-mcp"] }
  }
}
```

`uvx` installs the package on first run. Two servers are hosted by their sites instead: Pasavard at `https://pasavard.ir/api/mcp`, and SwapWallet at `https://swapwallet.app/mcp` (sign-in required).

## License

[MIT](LICENSE). Site names and marks belong to their owners.
