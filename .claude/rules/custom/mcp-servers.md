# External MCP Servers

**Last Updated:** 2026-02-01

This project has custom MCP servers configured in `mcp_servers.json`. Access them via `mcp-cli`.

## Quick Reference

| Command                            | Description                  |
| ---------------------------------- | ---------------------------- |
| `mcp-cli`                          | List all servers and tools   |
| `mcp-cli <server>`                 | Show tools with parameters   |
| `mcp-cli <server> -d`              | Show tools with descriptions |
| `mcp-cli <server>/<tool>`          | Get tool JSON schema         |
| `mcp-cli <server>/<tool> '<json>'` | Execute tool                 |

---

## github

**Description:** GitHub platform integration via GitHub Copilot MCP.

**Transport:** HTTP (requires `GH_TOKEN` environment variable)

**Tool Categories:**

| Category | Tools |
|----------|-------|
| Repository | `create_repository`, `fork_repository`, `create_branch`, `list_branches` |
| Files | `get_file_contents`, `create_or_update_file`, `delete_file` |
| Issues | `issue_read`, `issue_write`, `list_issues`, `search_issues`, `add_issue_comment` |
| Pull Requests | `list_pull_requests`, `create_pull_request`, `merge_pull_request` |
| Reviews | `pull_request_review_write`, `add_comment_to_pending_review` |
| Releases | `list_releases`, `get_latest_release`, `get_release_by_tag` |
| Search | `search_code`, `search_issues`, `search_pull_requests`, `search_users` |
| Copilot | `assign_copilot_to_issue` |

**Key Tools:**

| Tool | Description |
|------|-------------|
| `get_me` | Get current user info and permissions (call first) |
| `issue_read` | Read issue details |
| `issue_write` | Create or update issues |
| `create_pull_request` | Create a new PR |
| `search_code` | Search code across repositories |
| `assign_copilot_to_issue` | Assign Copilot to work on an issue |

**Examples:**

```bash
# Get current user
mcp-cli github/get_me '{}'

# List issues
mcp-cli github/list_issues '{"owner": "BusinessPlus-Community", "repo": ".github"}'

# Create a branch
mcp-cli github/create_branch '{"owner": "BusinessPlus-Community", "repo": ".github", "branch": "feature/new-feature"}'

# Get file contents
mcp-cli github/get_file_contents '{"owner": "BusinessPlus-Community", "repo": ".github", "path": "README.md"}'

# Search code
mcp-cli github/search_code '{"query": "def process org:BusinessPlus-Community"}'
```

**Best Practices:**

1. **Call `get_me` first** - Understand current user permissions
2. **Use pagination** - Request 5-10 items at a time
3. **Use `minimal_output: true`** - When full details aren't needed
4. **Use `search_*` for targeted queries** - Complex filters, specific criteria
5. **Use `list_*` for broad retrieval** - All items with basic filtering
6. **Check for duplicates** - Search before creating issues/PRs

**PR Review Workflow:**
1. Create pending review with `pull_request_review_write` (method: `create`)
2. Add comments with `add_comment_to_pending_review`
3. Submit review with `pull_request_review_write` (method: `submit_pending`)

---

## Usage Notes

1. **GH_TOKEN required:** Set `GH_TOKEN` environment variable with GitHub token
2. **Prefer gh CLI for simple ops:** Use `gh` command for basic GitHub operations
3. **Use MCP for complex workflows:** Multi-step operations, Copilot integration
4. **JSON arguments:** Always use valid JSON for tool arguments
