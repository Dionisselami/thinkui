# ThinkUI

**Interface references your coding agent can actually use — over MCP.**

ThinkUI is a hosted [Model Context Protocol](https://modelcontextprotocol.io) server that answers one
question well: *what did a real product decide here, and what can I learn from it?* Ask about a paywall,
an empty state, a pricing toggle, a dense data table — it returns the screen, the flow it belongs to, the
patterns in play, the app it came from, and the licence that screen carries.

It is evidence for a design decision. It is not a component library to copy out of.

- **Endpoint:** `https://thinkui.xyz/mcp` (streamable HTTP)
- **Auth:** OAuth 2.1 (discovery at `https://thinkui.xyz/.well-known/oauth-protected-resource`) or an API key sent as an `Authorization` header — keys are issued at [thinkui.xyz/account](https://thinkui.xyz/account)
- **Website:** [thinkui.xyz](https://thinkui.xyz)
- **Agent skill:** `npx skills add https://thinkui.xyz --skill thinkui -y`

No local install, no package, no daemon. One URL and one header.

## Tools

| Tool | Answers | Returns |
|---|---|---|
| `find_ui_references` | How a screen, flow or pattern is actually designed | Screens and flows with the app they came from, the patterns used, the components involved |
| `find_ui_elements` | What a component should look like | The component's own markup and stylesheet, ready to paste (MIT) |
| `find_animated_icons` | Which animated icon fits an action | Working animated SVG, no runtime, no icon font (MIT) |
| `find_ui_materials` | Which typeface, icon set or design system to use for a role | Candidates with their maintenance state and source |
| `corpus_stats` | What the server holds | Counts by category, so an empty result is explained rather than silent |

## Connect a client

**Claude Code**

```bash
claude mcp add --transport http thinkui https://thinkui.xyz/mcp \
  --header "Authorization: Bearer your-key"
```

Or project scope, committed without the key (Claude Code expands `${VAR}`):

```json
{
  "mcpServers": {
    "thinkui": {
      "type": "http",
      "url": "https://thinkui.xyz/mcp",
      "headers": { "Authorization": "Bearer ${THINKUI_KEY}" }
    }
  }
}
```

**Cursor** — `.cursor/mcp.json` (project) or `~/.cursor/mcp.json` (global), then restart:

```json
{
  "mcpServers": {
    "thinkui": {
      "url": "https://thinkui.xyz/mcp",
      "headers": { "Authorization": "Bearer your-key" }
    }
  }
}
```

**VS Code / Copilot** — `.vscode/mcp.json`, top-level key is `servers` (not `mcpServers`), and
`"type": "http"` is mandatory. MCP tools exist only in Copilot **agent** mode.

```json
{
  "servers": {
    "thinkui": {
      "type": "http",
      "url": "https://thinkui.xyz/mcp",
      "headers": { "Authorization": "Bearer ${input:thinkui-key}" }
    }
  }
}
```

**Claude Desktop** (`claude_desktop_config.json`), **Windsurf** (`~/.codeium/windsurf/mcp_config.json`,
`serverUrl` key) and **Zed** take the same two values in their own shape.

**ChatGPT and Claude web connectors** sign the user in with OAuth instead of a key. Paste
`https://thinkui.xyz/mcp`; the client discovers the authorization server from
`https://thinkui.xyz/.well-known/oauth-protected-resource`, registers itself, and asks you to
approve read-only access (`mcp:read`). Authorization code with PKCE, no client secret.

### Agent skill

The server holds the references; the skill is the habit of asking for them.

```bash
npx skills add https://thinkui.xyz --skill thinkui -y
```

Installs `.agents/skills/thinkui/SKILL.md`. Also installable from this repo:
`npx skills add Dionisselami/thinkui`.

## What comes back

A result names the app and flow, what the screen demonstrates, the patterns it uses and its licence. Where
the library holds a screenshot it is attached; where the licence is study-only, the result says
*do not ship or republish* and credits the owner. An empty query returns empty — approximating would turn
an honest gap into an invented answer.

## Pricing (USD)

| Plan | Price | Billing | What you get |
|---|---|---|---|
| Starter | $9.99 | monthly, cancel anytime | unlimited tool calls, all five tools, components/icons/materials, email support |
| Pro | $19.99 | monthly, cancel anytime | everything in Starter plus priority support and priority when new sources are added |
| Lifetime | $190 | one-time, never renews | everything in Pro, permanent access |

The library and the tools are identical on every plan. Current prices and terms live at
[thinkui.xyz/pricing](https://thinkui.xyz/pricing).

## Library

Generated at build time from the server's own `corpus_stats` — the numbers are in
[llms.txt](llms.txt) in this repo and on the site, never typed into a marketing paragraph where they can
go stale.

## Licence

This repository is **documentation and distribution metadata** for a commercial hosted service. There is
no server source code here. The MCP server itself is proprietary; the assets it returns are not: UI
elements, animated icons and materials are MIT, and screenshots keep the licence of their original source,
which each result states.

## Links

- Website — https://thinkui.xyz
- Pricing — https://thinkui.xyz/pricing
- Quickstart — https://thinkui.xyz/docs
- What the tools return — https://thinkui.xyz/examples
- Guide: [what a UI-reference MCP server is](https://thinkui.xyz/mcp-server-for-ui-design)
- Guide: [Claude Code setup](https://thinkui.xyz/claude-code-ui-design)
- Guide: [Cursor setup](https://thinkui.xyz/cursor-ui-design)
- Comparison: [vs the Figma MCP server](https://thinkui.xyz/figma-mcp-alternative)
