# GitLab plugin for Cursor

Connect [Cursor](https://cursor.com) to your GitLab instance with the
[GitLab MCP server](https://docs.gitlab.com/user/gitlab_duo/model_context_protocol/mcp_server/).
Manage issues, merge requests, pipelines, and search code — all from within
your editor.

## Requirements

- A GitLab.com account **or** a GitLab Self-Managed instance (18.3+).
- GitLab Premium or Ultimate with
  [GitLab Duo](https://docs.gitlab.com/user/gitlab_duo/) and
  [beta features](https://docs.gitlab.com/user/duo_agent_platform/turn_on_off/#turn-on-beta-and-experimental-features) enabled.

## Setup

1. Install the **GitLab** plugin from the Cursor Marketplace.
2. Open **Settings > Cursor Settings > Tools & MCP** and verify the GitLab MCP
   server appears.
3. Save and wait for your browser to open the OAuth authorization page.
   If this does not happen, restart Cursor.
4. Review and approve the authorization request.

### Self-Managed instances

The plugin connects to **gitlab.com** by default. To use a self-managed
instance instead, copy `.mcp.json.self-managed.example` to `.mcp.json` and
replace `https://gitlab.example.com` with your instance URL:

```json
{
  "mcpServers": {
    "GitLab": {
      "type": "http",
      "url": "https://your-gitlab-instance.com/api/v4/mcp"
    }
  }
}
```

## What's included

### MCP server

The core of this plugin. The
[GitLab MCP server](https://docs.gitlab.com/user/gitlab_duo/model_context_protocol/mcp_server/)
gives Cursor direct access to your GitLab data through these
[tools](https://docs.gitlab.com/user/gitlab_duo/model_context_protocol/mcp_server_tools/):

| Tool | Description |
|------|-------------|
| `create_issue` | Create issues in a project |
| `get_issue` | Retrieve issue details |
| `create_merge_request` | Create merge requests |
| `get_merge_request` | Retrieve merge request details |
| `get_merge_request_diffs` | View merge request file changes |
| `get_merge_request_commits` | List merge request commits |
| `get_merge_request_pipelines` | View merge request pipelines |
| `manage_pipeline` | List, create, retry, cancel, or delete pipelines |
| `get_pipeline_jobs` | List jobs in a pipeline |
| `search` | Search issues, merge requests, and projects |
| `semantic_code_search` | Search code by meaning |
| `create_workitem_note` | Comment on work items |
| `get_workitem_notes` | Retrieve work item comments |
| `search_labels` | Search labels in a project or group |

### Rules

- **gitlab-workflow** — GitLab development conventions (conventional commits,
  issue references, small merge requests).

### Skills

- **gitlab-ci-author** — Helps write, debug, and optimize `.gitlab-ci.yml`
  pipeline configuration.

### Agents

- **gitlab-assistant** — General-purpose assistant that uses the MCP server to
  manage issues, merge requests, and pipelines.

### Commands

- **create-merge-request** — Create a merge request from the current branch.

## GitLab Duo Agent Platform

For AI-powered agents that can autonomously create code, review merge requests,
fix pipelines, and more, explore the
**[GitLab Agent Catalog](https://gitlab.com/explore/ai-catalog/agents/)**.

Available agents and flows include:

- **[Planner Agent](https://docs.gitlab.com/user/duo_agent_platform/agents/foundational_agents/planner/)** — Plan, prioritize, and track work.
- **[Security Analyst Agent](https://docs.gitlab.com/user/duo_agent_platform/agents/foundational_agents/security_analyst_agent/)** — Triage issues, analyze vulnerabilities, and generate fixes.
- **[Custom agents](https://docs.gitlab.com/user/duo_agent_platform/agents/custom/)** — Build team-specific agents for your unique requirements.
- **[External agents](https://docs.gitlab.com/user/duo_agent_platform/agents/external/)** — Connect third-party integrations (Claude Code, OpenAI Codex, Amazon Q, Gemini) to GitLab.
- **[Software Development Flow](https://docs.gitlab.com/user/duo_agent_platform/flows/foundational_flows/software_development/)** — Create a full, multi-step plan before executing it.
- **[Developer Flow](https://docs.gitlab.com/user/duo_agent_platform/flows/foundational_flows/developer/)** — Convert issues into merge requests.
- **[Fix CI/CD Pipeline Flow](https://docs.gitlab.com/user/duo_agent_platform/flows/foundational_flows/fix_pipeline/)** — Diagnose and fix failing pipelines.
- **[Code Review Flow](https://docs.gitlab.com/user/duo_agent_platform/flows/foundational_flows/code_review/)** — Automate code review and enforce standards.
- **[Convert to GitLab CI/CD Flow](https://docs.gitlab.com/user/duo_agent_platform/flows/foundational_flows/convert_to_gitlab_ci/)** — Migrate legacy CI/CD to GitLab.

Learn more about the
[GitLab Duo Agent Platform](https://docs.gitlab.com/user/duo_agent_platform/).

## Links

- [GitLab MCP server documentation](https://docs.gitlab.com/user/gitlab_duo/model_context_protocol/mcp_server/)
- [MCP server tools reference](https://docs.gitlab.com/user/gitlab_duo/model_context_protocol/mcp_server_tools/)
- [GitLab Agent Catalog](https://gitlab.com/explore/ai-catalog/agents/)
- [GitLab Duo Agent Platform](https://docs.gitlab.com/user/duo_agent_platform/)

## License

MIT
