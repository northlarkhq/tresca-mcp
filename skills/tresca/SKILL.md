---
name: tresca
description: Find an MCP server or agent skill for engineering software (CAD, BIM, FEA, CFD, geotechnical, EDA, PLC and similar), check its facts (official, drives the running app or files only, can change models, operating systems, licence, last activity) and install it with the user's permission, using the Tresca directory (tresca.app). Also answers whether a vendor has its own AI assistant. Use when the user asks whether an MCP server or skill exists for an engineering tool ("is there an MCP server for Revit?", "a Claude skill for KiCad"), wants to connect their AI client to engineering software, or asks how to install one.
---

# Tresca: MCP servers and agent skills for engineering software

Tresca (https://tresca.app) lists MCP servers, agent skills and vendor AI assistants for engineering software. Every listing has sources and the date it was checked. "unknown" means the project's own documentation doesn't say; never fill it in by guessing. Tresca records facts and doesn't audit code. AI-generated models, drawings and results must be checked by a qualified engineer before use.

## Tools

When the Tresca MCP server is connected, use its tools:

| Tool | Use it for |
|---|---|
| `get_tool` | "Is there a server or skill for {tool}, and is one official?" Counts, official listings, the vendor's AI assistant, scripting interfaces, the tool's Tresca page |
| `search_listings` | Text search, or browse by `tool`, `discipline` or `client`; filters `type`, `official`, `os`, `access` |
| `get_listing` | All facts for one listing, with its `sources` and `checkedOn` |
| `get_install_steps` | The install steps, copied exactly from the project's documentation, each with its `sourceUrl`, plus warnings |
| `get_vendor_assistant` | A vendor's built-in AI assistant: status per product in the vendor's words, and how it relates to MCP |
| `list_disciplines` | Disciplines and their tools, to browse |

If the server isn't connected, read the public pages instead: `https://tresca.app/tools/{tool}/` (for example `https://tresca.app/tools/revit/`), each listing's page, and `https://tresca.app/search/?q={words}`. To connect the server, see "Connect Tresca" below.

## Workflow

1. **Find.** For a named tool, call `get_tool` first. If `found` is false, try its `suggestions`, then `search_listings` with a `query`. For a task across tools ("FEA results to a report"), use `search_listings` with a `query`, or `discipline`.
2. **Present the facts, not a verdict.** For each option, show the user:
   - name and type (MCP server, skill or plugin), and whether it is `official` (published by the software's vendor, or by an open-source tool's own developers);
   - `connection`: `live_app` drives the running program, `files_only` works on files without it, `cloud_api` uses the vendor's cloud service, `docs_only` answers documentation questions and doesn't operate the software;
   - `access` (read_only, read_write, configurable) and `os`;
   - `license`, `lastPush`, and the Tresca `url`.

   Results follow the site's order (official first), not a ranking. Don't rank the options or call one better or safer; say what the facts show and let the user choose.
3. **Check before installing.** Call `get_listing` for the chosen one and tell the user, with the source link, if:
   - `github.archived` or `github.notFound` is true, or `lastPush` is old;
   - `access` is `read_write` (it can change models or files) or `unknown`;
   - `requiresToolLicence` is true (it needs a licensed copy of the software);
   - its `os` doesn't include the user's operating system, or `toolVersions` doesn't cover their version;
   - it isn't `official` and the user expected the vendor's own.
4. **Install only with permission.** Call `get_install_steps`, with the user's AI client as `client` if you know it. Show the steps and their `sourceUrl`, and ask before running any command or editing any file. Keep keys and paths as placeholders and tell the user what to fill in; never ask them to paste a secret into the chat. If `available` is false, give the `reason` and the project's own page.
5. **Nothing found.** Say so plainly: Tresca not listing something doesn't mean none exists. Point to https://tresca.app/wanted/ (tools with no integration yet) and https://tresca.app/submit/ (to suggest a project). If the tool has scripting interfaces (`get_tool` → `scripting`), mention them: an integration can only do what those allow.

## Connect Tresca

The Tresca MCP server is remote and read-only, with no sign-in: `https://tresca.app/mcp` (streamable HTTP). For example, in Claude Code:

```
claude mcp add --transport http tresca https://tresca.app/mcp
```

Other clients: see [reference/clients.md](reference/clients.md) for where each one keeps its MCP config and its skills. Setup page: https://tresca.app/agents/
