# Miguel Analytics agent plugins

Installable MCP + skills for the private [Miguel Analytics](https://miguelanalytics.dev) client portal.

The package is generic. It does not include client data. After install, the agent signs in with Google through the MCP; the allowlist selects the client at runtime.

Plugin: **miguel-analytics-portal**

- MCP: `https://netzun.miguelanalytics.dev/api/mcp` (Streamable HTTP). Future client hosts use the same pattern (`https://<client>.miguelanalytics.dev/api/mcp`) and are chosen by authentication, not by a secret in this repo.
- Skills: how to read the audit (findings, tracking plan, per-platform pages), which tools to call, and how to brief the client's dev team.

## Install

### One line (Claude Code, Codex, Cursor, and other compatible agents)

```bash
npx plugins add mepumapillo/miguel-analytics-agent-plugins
```

### Claude Code

In the Claude Code chat:

```text
/plugin marketplace add mepumapillo/miguel-analytics-agent-plugins
/plugin install miguel-analytics-portal@miguel-analytics
```

Or in a terminal:

```bash
claude plugin marketplace add mepumapillo/miguel-analytics-agent-plugins
claude plugin install miguel-analytics-portal@miguel-analytics
```

### Codex

```bash
codex plugin marketplace add mepumapillo/miguel-analytics-agent-plugins
codex plugin add miguel-analytics-portal@miguel-analytics
```

### Cursor

Install the MCP with this Cursor deeplink (or the `https://cursor.com/install-mcp` equivalent, which GitHub will not strip):

- [Add MCP in Cursor](https://cursor.com/install-mcp?name=miguel-analytics-portal&config=eyJ1cmwiOiJodHRwczovL25ldHp1bi5taWd1ZWxhbmFseXRpY3MuZGV2L2FwaS9tY3AifQ%3D%3D)

```text
cursor://anysphere.cursor-deeplink/mcp/install?name=miguel-analytics-portal&config=eyJ1cmwiOiJodHRwczovL25ldHp1bi5taWd1ZWxhbmFseXRpY3MuZGV2L2FwaS9tY3AifQ%3D%3D
```

Skills (Agent Skills format):

```bash
npx skills add mepumapillo/miguel-analytics-agent-plugins
```

### Manual MCP only

If you just need the server, add this HTTP MCP and complete Google sign-in when prompted:

```json
{
  "mcpServers": {
    "miguel-analytics-portal": {
      "url": "https://netzun.miguelanalytics.dev/api/mcp"
    }
  }
}
```

## After install

1. Sign in with an allowlisted Google account when the MCP requests it.
2. Start with `get_index` or the resource `portal://llms.txt`.
3. Follow the skill: findings, then tracking plan and platform pages, then `get_page` / `search` / `get_pdf_link` as needed.

The index and full text are **not** public files. Agents must read them through the authenticated MCP.

## Repo layout

```text
.claude-plugin/marketplace.json
marketplace.json
plugins/miguel-analytics-portal/   # plugin (MCP + skill)
skills/miguel-analytics-portal/    # same skill, for `npx skills add`
```

No client findings, emails, or secrets belong in this repository.
