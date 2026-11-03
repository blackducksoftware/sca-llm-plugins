# BlackDuck SCA Plugin

An MCP server and skills for the Black Duck Software Composition Analysis (SCA) - BDSCA. 
Provides comprehensive project management, vulnerability tracking, policy compliance, 
and component analysis capabilities.

## Installation and Configuration
### Prerequisites
Up and running Black Duck BDSCA server with user account and created access token.

### Installation
```bash
# set environment variables for the session
$ claude
# add the marketplace entry for the server (only needed once)
/plugin marketplace add blackducksoftware/llm-plugins
/plugin install blackduck-bdsca@blackduck-llm-plugins
/reload-plugins
What SCA tools do you have access to?
Show me my latest scans and vulnerabilities.
```

For more examples on the usage of the MCP server, see the **[MCP README.md](https://github.com/blackducksoftware/sca-mcp/blob/main/README.md)**.

### Environment Variables
Available environment variables for configuring the MCP server:

| Variable                                                           | Default             | Description                                                             |
|--------------------------------------------------------------------|---------------------|-------------------------------------------------------------------------|
| `BLACKDUCK_BDSCA_URL`                                              | *(required)*        | Base URL of the Black Duck BDSCA instance                               |
| `BLACKDUCK_BDSCA_TOKEN`                                            | *(none)*            | API bearer token, or use a token file, see Security                     |
| `BLACKDUCK_BDSCA_TOKEN_FILE`                                       | *(none)*            | Path of a file with the token, preferred - used when `BLACKDUCK_BDSCA_TOKEN` is unset |
| `BLACKDUCK_MCP_READ_ONLY`                                          | `false`             | Turn off the tools that change data or write files                      |
| `BLACKDUCK_MCP_DEV_MODE`                                           | `false`             | Send unexpected error details to the AI assistant                       |
| `BLACKDUCK_BDSCA_SSL_VERIFY`                                       | `true`              | Whether to verify SSL/TLS certificates for BDSCA connections             |
| `BLACKDUCK_BDSCA_LOG_LEVEL`                                        | `INFO`              | Logging level: `DEBUG`, `INFO`, `WARNING`, `ERROR`, `CRITICAL`          |
| `BLACKDUCK_BDSCA_LOG_FORMAT`                                       | `colored`           | Log format: `colored`, `json`, `simple`                                 |
| `BLACKDUCK_BDSCA_LOG_FILE`                                         | *(none)*            | Path to a log file (written in addition to stderr)                      |


## Security

### Token storage
Claude Code passes its environment to every MCP server and tool it starts, so a token in
`BLACKDUCK_BDSCA_TOKEN` can be read by all of them. To keep it out of the environment, save
the token in `~/.blackduck/bdsca-token`, make the file readable by your own user only, and
leave `BLACKDUCK_BDSCA_TOKEN` unset. To use another file, set `BLACKDUCK_BDSCA_TOKEN_FILE` to
its path. The `health_check` tool shows where the token came from.

### Token permissions and read-only mode
The MCP server can read and change everything that the token's user can. Use the token of a
dedicated Black Duck user with only the roles you need, see the
[Role and Permission Matrix](https://documentation.blackduck.com/bundle/bd-hub/page/UsersAndGroups/RoleMatrix.html).
Set `BLACKDUCK_MCP_READ_ONLY=true` to turn off the tools that change data or write files:
`update_bom_component`, `comment_bom_component`, `update_vulnerability_remediation`,
`apply_component_mappings`, `match_component_to_new_origin`, `scan` and `create_report`.

### Data sent to your LLM provider
Tool and Resource results, like Vulnerability findings, Project and Component names, Policy
settings and Scan results, go to the LLM that Claude Code uses. The API token never does.
Black Duck does not control how your LLM provider stores or uses this data, or whether it
trains models on it. Before you use the plugin with sensitive or regulated findings, read
your provider's terms and data processing agreement.

## Skills
The plugin includes user-invoked **skills** that orchestrate the MCP
tools into complete security workflows.  
For detailed skill descriptions see **[skills/README.md](skills/README.md)**.

## Support

- [Black Duck Website](https://www.blackduck.com/)
- [Community Portal](https://community.blackduck.com)
- [Email](mailto:support@blackduck.com)
