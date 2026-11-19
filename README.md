# Tresca for AI agents

[Tresca](https://tresca.app/) is a directory of MCP servers, agent skills and vendor AI assistants for engineering software: CAD, BIM, finite element and CFD, geotechnical, power systems, electronics, PLC and more. Every listing has sources and the date it was checked, and "unknown" stays unknown rather than guessed.

This repository gives AI agents direct access to it (setup for each client: https://tresca.app/agents/):

- **Tresca MCP server**: a remote, read-only server at `https://tresca.app/mcp` (streamable HTTP, no sign-in).
- **tresca skill**: tells an agent when and how to use it. The agent finds a server or skill for an engineering tool, shows the user the facts, and installs it only with their permission.

## Install

**Claude Code (plugin: server and skill together)**

```
/plugin marketplace add northlarkhq/tresca-mcp
/plugin install tresca@tresca
```

**Gemini CLI (extension)**

```
gemini extensions install https://github.com/northlarkhq/tresca-mcp
```

**Any MCP client (server only):** add `https://tresca.app/mcp` as a remote MCP server. For example, in Claude Code:

```
claude mcp add --transport http tresca https://tresca.app/mcp
```

[skills/tresca/reference/clients.md](skills/tresca/reference/clients.md) shows where other clients keep their MCP config.

**Skill only:** copy `skills/tresca/` into your client's skills folder (for example `~/.claude/skills/tresca/`). Without the server, the skill reads Tresca's public pages.

## Tools

| Tool | What it answers |
|---|---|
| `get_tool` | Is there an MCP server or skill for an engineering tool, and is one official? Also the vendor's AI assistant and the tool's scripting interfaces |
| `search_listings` | Search by text, or browse by tool, discipline or AI client; filter by type, official, operating system and whether it can change models |
| `get_listing` | Every fact for one listing, with its sources and checked date |
| `get_install_steps` | Install steps copied exactly from the project's own documentation, each with its source, plus warnings |
| `get_vendor_assistant` | A vendor's built-in AI assistant: status in each product, in the vendor's own words, and how it relates to MCP |
| `list_disciplines` | Disciplines and their tools, with counts |

All tools are read-only. Results follow the site's order, official first, and nothing is ranked. Tresca records what each project's documentation says and does not audit code, so check a project before you install it. AI-generated models, drawings and results must be checked by a qualified engineer before use.

## Privacy

The server doesn't store the questions agents send it. Details: https://tresca.app/privacy/

## Limits

60 requests a minute per IP address.

## Problems

Open an issue in this repository, or use https://tresca.app/submit/ to suggest a project or report a listing.

## Licence

MIT (this repository: the skill, plugin and configuration files). How Tresca's data is checked: https://tresca.app/methodology/
