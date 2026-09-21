# GitLab plugin for Cursor

Connect [Cursor](https://cursor.com) to your GitLab instance with the
[GitLab MCP server](https://docs.gitlab.com/user/model_context_protocol/mcp_server/).
Plan, track, and manage issues, merge requests, and pipelines — all from within
your editor.

> **Note:** This repository is mirrored on GitHub from
> [GitLab](https://gitlab.com/gitlab-org/developer-relations/cursor-gitlab-plugin).
> Please submit all merge requests and issues on GitLab.
> Pull requests and issues on GitHub will not be processed.

## Requirements

- A GitLab.com account **or** a GitLab Self-Managed instance (18.3+).
- Available on all tiers, including Free. The MCP server moved from Premium to
  Free in GitLab 19.2 and is currently Beta.
- [MCP server access allowed](https://docs.gitlab.com/user/model_context_protocol/mcp_server/#prerequisites):
  on GitLab.com for the top-level group, and on GitLab Self-Managed or GitLab
  Dedicated for the instance.
- For toolset selection, GitLab 19.5 or later. This plugin sends the
  `X-Gitlab-Enabled-Mcp-Server-Toolsets` header, which was
  [added in 19.5](https://docs.gitlab.com/user/model_context_protocol/mcp_server/#select-tool-groups-toolsets)
  behind the `mcp_toolsets` feature flag and is disabled by default. On earlier
  versions, or with the flag off, the header is ignored and you get the default
  toolsets.

## Setup

1. Install the **GitLab** plugin from the Cursor Marketplace.
2. Open **Settings > Cursor Settings > Tools & MCP** and verify the GitLab MCP
   server appears.
3. Save and wait for your browser to open the OAuth authorization page.
   If this does not happen, type `mcp_auth` in the Cursor chat or restart Cursor.
4. Review and approve the authorization request.

> **"Unverified Dynamic Application" warning**: During OAuth authorization, you
> may see a warning that Cursor is an unverified application. This is expected —
> Cursor uses [OAuth Dynamic Client Registration](https://www.rfc-editor.org/rfc/rfc7591),
> and GitLab flags all dynamically registered apps as unverified by design. The
> connection is still secure. Cursor is working with GitLab's Technology Partner
> team to get their OAuth application pre-registered and verified, which will
> remove this warning for all users.
>
> **Enterprise workaround**: GitLab admins can eliminate the warning for their
> instance by pre-registering Cursor as a trusted application. Go to
> **Admin Area → Applications → New Application**, register Cursor, and enable
> **Trusted**. This removes the warning and skips the consent screen for all
> users on that instance.

### Self-Managed instances

The plugin connects to **gitlab.com** by default. To use a self-managed
instance instead, copy `.mcp.json.self-managed.example` to `.mcp.json` and
replace `https://gitlab.example.com` with your instance URL:

```json
{
  "mcpServers": {
    "GitLab": {
      "type": "http",
      "url": "https://your-gitlab-instance.com/api/v4/mcp",
      "headers": {
        "X-Gitlab-Enabled-Mcp-Server-Toolsets": "all"
      }
    }
  }
}
```

## What's included

### MCP server

The core of this plugin. The
[GitLab MCP server](https://docs.gitlab.com/user/model_context_protocol/mcp_server/)
gives Cursor direct access to your GitLab data. Tools are grouped into
[toolsets](https://docs.gitlab.com/user/model_context_protocol/mcp_server/#select-tool-groups-toolsets),
and the plugin's MCP configuration sends
`X-Gitlab-Enabled-Mcp-Server-Toolsets: all`, so every toolset is enabled,
including the three that are otherwise opt-in:

| Toolset | Enabled by default | Covers |
| --------- | ------------------ | ------ |
| `meta` | Always | MCP server metadata |
| `core` | Yes | Projects, groups, users, search, and labels |
| `merge_requests` | Yes | Merge requests, diffs, notes, reviews, and approvals |
| `work_items` | Yes | Issues, epics, tasks, and their comments |
| `repository` | Yes | Branches, commits, files, tags, and releases |
| `ci` | Yes | Pipelines, jobs, job logs, and artifacts |
| `duo_agent_platform` | Opt-in | Starting and tracking Duo Agent Platform sessions |
| `wikis` | Opt-in | Wiki pages |
| `code_security` | Opt-in | Vulnerability triage and scan profiles |

To narrow the list, replace `all` with a comma-separated subset, for example
`core,merge_requests,code_security`. Narrowing is worth doing if you run
several MCP servers at once, because a large combined tool list makes it more
likely the model picks the wrong tool. Toolset names are matched without regard
to case, and an unrecognized name returns a 400 error.

For the current per-tool reference, see the
[MCP server tools documentation](https://docs.gitlab.com/user/model_context_protocol/mcp_server_tools/).

### Rules

- **gitlab-workflow** — GitLab development conventions (conventional commits,
  issue references, small merge requests).

### Skills

- **gitlab-ci-author** — Helps write, debug, and optimize `.gitlab-ci.yml`
  pipeline configuration.

### Agents

- **gitlab-assistant** (GitLab Assistant) — AI agent with product management
  capabilities that helps with Agile planning, prioritization, delivery tracking,
  and stakeholder communication using the MCP server.

### Commands

- **create-issue** — Create an issue with labels, milestone, and assignees.
- **create-merge-request** — Create a merge request from the current branch.
- **review-merge-request** — Review a merge request: summarize changes, check
  pipelines, and flag concerns.
- **pipeline-status** — Check pipeline health and drill into failed jobs.
- **plan-sprint** — Analyze open issues and suggest sprint scope and priorities.
- **backlog-health** — Assess the backlog for staleness, missing labels, and
  unassigned work.

## Example prompts

Try these in Cursor chat. Prefix your prompt with `/gitlab-assistant` to
route it to the GitLab agent:

### Issues and planning

- "/gitlab-assistant Show me all open issues labeled `bug` in `my-group/my-project`"
- "/gitlab-assistant Create an issue for the API rate limiting feature with appropriate labels"
- "/gitlab-assistant What issues are assigned to me in this project?"
- "/gitlab-assistant Summarize the open issues in milestone 3.2"

### Sprint and backlog

- "/gitlab-assistant Help me plan the next sprint for milestone 3.2 in `my-group/my-project`"
- "/gitlab-assistant Analyze the backlog health for `my-group/my-project`"
- "/gitlab-assistant Which open issues have no assignee or labels?"

### Merge requests

- "/gitlab-assistant Create a merge request from my current branch to main"
- "/gitlab-assistant Review merge request !142 in `my-group/my-project`"
- "/gitlab-assistant What files changed in MR !89?"
- "/gitlab-assistant Show me the commits in merge request !56"

### Pipelines and CI/CD

- "/gitlab-assistant What's the pipeline status for `my-group/my-project`?"
- "/gitlab-assistant Which jobs failed in the latest pipeline?"
- "/gitlab-assistant Retry the failed pipeline for this project"

### Search and discovery

- "/gitlab-assistant Search for issues mentioning 'authentication timeout'"
- "/gitlab-assistant Find code related to user permissions in `my-group/my-project`"
- "/gitlab-assistant What labels are available in this project?"

### Comments and collaboration

- "/gitlab-assistant Show me the comments on issue #42 in `my-group/my-project`"
- "/gitlab-assistant Add a comment to issue #42 summarizing the investigation"

## GitLab Duo Agent Platform

This plugin gives you core GitLab workflows in Cursor. For AI-powered agents
that can autonomously plan, analyze, create code, review merge requests, fix
pipelines, and more, explore the
**[GitLab Duo Agent Platform](https://docs.gitlab.com/user/duo_agent_platform/)**
and the **[GitLab Agent Catalog](https://gitlab.com/explore/ai-catalog/agents/)**.

The MCP server in this plugin works on every tier, but the agents and flows
below need GitLab Premium or Ultimate with
[GitLab Duo](https://docs.gitlab.com/user/gitlab_duo/) and
[beta features](https://docs.gitlab.com/user/duo_agent_platform/turn_on_off/#turn-on-beta-and-experimental-features)
enabled. [Compare plans](https://about.gitlab.com/pricing/) or
[start a free trial](https://gitlab.com/-/trial_registrations/new).

### Foundational Agents

- **[Planner Agent](https://docs.gitlab.com/user/duo_agent_platform/agents/foundational_agents/planner/)** — Full work item CRUD, epic/task hierarchy, dependency analysis, estimation, and planning workflows.
- **[Security Analyst Agent](https://docs.gitlab.com/user/duo_agent_platform/agents/foundational_agents/security_analyst_agent/)** — Vulnerability triage, risk assessment, compliance reporting, and remediation planning.
- **[Data Analyst Agent](https://docs.gitlab.com/user/duo_agent_platform/agents/foundational_agents/data_analyst/)** — GLQL queries, volume analysis, team performance metrics, and trend visualization.

### Foundational Flows

- **[Software Development Flow](https://docs.gitlab.com/user/duo_agent_platform/flows/foundational_flows/software_development/)** — AI-generated solutions across the software development lifecycle.
- **[Developer Flow](https://docs.gitlab.com/user/duo_agent_platform/flows/foundational_flows/developer/)** — Convert issues into merge requests.
- **[Fix CI/CD Pipeline Flow](https://docs.gitlab.com/user/duo_agent_platform/flows/foundational_flows/fix_pipeline/)** — Diagnose and fix failing pipelines.
- **[Code Review Flow](https://docs.gitlab.com/user/duo_agent_platform/flows/foundational_flows/code_review/)** — Automate code review with AI-native analysis.
- **[Convert to GitLab CI/CD Flow](https://docs.gitlab.com/user/duo_agent_platform/flows/foundational_flows/convert_to_gitlab_ci/)** — Migrate legacy CI/CD to GitLab.
- **[Agentic SAST Vulnerability Resolution](https://docs.gitlab.com/user/duo_agent_platform/flows/foundational_flows/agentic_sast_vulnerability_resolution/)** — Auto-generate merge requests to fix SAST vulnerabilities.
- **[SAST False Positive Detection](https://docs.gitlab.com/user/duo_agent_platform/flows/foundational_flows/sast_false_positive_detection/)** — Identify and filter false positives in SAST findings.

### Extend further

- **[Custom agents](https://docs.gitlab.com/user/duo_agent_platform/agents/custom/)** — Build team-specific agents for your unique requirements.
- **[External agents](https://docs.gitlab.com/user/duo_agent_platform/agents/external/)** — Connect third-party integrations (Claude Code, OpenAI Codex, Amazon Q, Gemini) to GitLab.

## Links

- [GitLab MCP server documentation](https://docs.gitlab.com/user/model_context_protocol/mcp_server/)
- [MCP server tools reference](https://docs.gitlab.com/user/model_context_protocol/mcp_server_tools/)
- [GitLab Agent Catalog](https://gitlab.com/explore/ai-catalog/agents/)
- [GitLab Duo Agent Platform](https://docs.gitlab.com/user/duo_agent_platform/)

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on how to contribute to
this project.

## License

MIT
