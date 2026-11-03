# Black Duck BDBA Plugin

An MCP server and skills for the Black Duck Binary Analysis (BDBA). Identifies open-source
components and vulnerabilities in binaries, firmware, and container images, and provides
scanning, vulnerability triage, version override and reporting capabilities.

## Installation and Configuration
### Prerequisites
Up and running Black Duck BDBA server with user account and created access token.

### Installation
```bash
# set environment variables for the session
$ claude
# add the marketplace entry for the server (only needed once)
/plugin marketplace add blackducksoftware/llm-plugins
/plugin install blackduck-bdba@blackduck-llm-plugins
/reload-plugins
What BDBA tools do you have access to?
Show me my latest binary scans and their vulnerabilities.
```

For the full list of tools and more examples on the usage of the MCP server, see the
**[BDBA MCP guide](https://github.com/blackducksoftware/sca-mcp/blob/main/docs/bdba.md)**.

### Environment Variables
Available environment variables for configuring the MCP server:

| Variable                     | Default      | Description                                                                          |
|------------------------------|--------------|--------------------------------------------------------------------------------------|
| `BLACKDUCK_BDBA_URL`         | *(required)* | Base URL of the Black Duck BDBA instance                                             |
| `BLACKDUCK_BDBA_TOKEN`       | *(none)*     | API bearer token, or use a token file, see Security                                  |
| `BLACKDUCK_BDBA_TOKEN_FILE`  | *(none)*     | Path of a file with the token, preferred - used when `BLACKDUCK_BDBA_TOKEN` is unset |
| `BLACKDUCK_MCP_READ_ONLY`    | `false`      | Turn off the tools that change data or write files                                   |
| `BLACKDUCK_MCP_DEV_MODE`     | `false`      | Send unexpected error details to the AI assistant                                    |
| `BLACKDUCK_BDBA_SSL_VERIFY`  | `true`       | Whether to verify SSL/TLS certificates for BDBA connections                          |
| `BLACKDUCK_BDBA_LOG_LEVEL`   | `INFO`       | Logging level: `DEBUG`, `INFO`, `WARNING`, `ERROR`, `CRITICAL`                       |
| `BLACKDUCK_BDBA_LOG_FORMAT`  | `colored`    | Log format: `colored`, `json`, `simple`                                              |
| `BLACKDUCK_BDBA_LOG_FILE`    | *(none)*     | Path to a log file (written in addition to stderr)                                   |


## Security

### Token storage
Claude Code passes its environment to every MCP server and tool it starts, so a token in
`BLACKDUCK_BDBA_TOKEN` can be read by all of them. To keep it out of the environment, save
the token in `~/.blackduck/bdba-token`, make the file readable by your own user only, and
leave `BLACKDUCK_BDBA_TOKEN` unset. To use another file, set `BLACKDUCK_BDBA_TOKEN_FILE` to
its path. The `health_check` tool shows where the token came from.

### Token permissions and read-only mode
The MCP server can read and change everything that the token's user can. Use the token of a
dedicated Black Duck user with only the permissions you need.
Set `BLACKDUCK_MCP_READ_ONLY=true` to turn off the tools that change data or write files:
`scan_file`, `rescan_scan`, `download_scan_binary`, `create_triage`, `update_triage`,
`delete_triage`, `create_version_override`, `delete_version_override`, `create_report` and
`get_report`.

### Data sent to your LLM provider
Tool and Resource results, like Components, Vulnerability findings, Triages and Scan
results, go to the LLM that Claude Code uses. The API token never does. Black Duck does not
control how your LLM provider stores or uses this data, or whether it trains models on it.
Before you use the plugin with sensitive or regulated findings, read your provider's terms
and data processing agreement.

## Skills
The plugin includes user-invoked **skills** that orchestrate the MCP
tools into complete security workflows:

- **[binary-analysis](skills/binary-analysis/SKILL.md)** — find an existing scan, review the
  components found by binary fingerprinting, and present the vulnerabilities by severity

## Support

- [Black Duck Website](https://www.blackduck.com/)
- [Community Portal](https://community.blackduck.com)
- [Email](mailto:support@blackduck.com)
