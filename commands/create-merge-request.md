---
name: create-merge-request
description: Create a GitLab merge request from the current branch using the MCP server.
---

# Create merge request

1. Determine the current branch name and the default target branch.
2. Ask the user for a merge request title and description.
3. Use the GitLab MCP `create_merge_request` tool with the project path, source branch, target branch, and title.
4. Report the resulting merge request URL.
