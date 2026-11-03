# Black Duck SCA — LLM Plugins

[![Website](https://img.shields.io/badge/website-https%3A%2F%2Fwww.blackduck.com-blue)](https://www.blackduck.com/)
[![Email](https://img.shields.io/badge/email-support%40blackduck.com-green)](mailto:support@blackduck.com)

Plugins that connect Claude Code to the Black Duck Software Composition Analysis (SCA)
products. Each plugin bundles a Black Duck [MCP server](https://github.com/blackducksoftware/sca-mcp)
with skills that turn its tools into complete security workflows, such as: investigate your
software's security posture and vulnerabilities, triage findings, scan code and binaries, and
generate vulnerability reports and SBOMs.

## Table of Contents

1. [Products](#products)
2. [Latest changes](#latest-changes)
3. [Getting Started](#getting-started)
   - [Prerequisites](#prerequisites)
   - [Installation](#installation)
   - [Update](#update)
4. [Security](#security)
5. [Support](#support)

## Products

This repository ships two plugins, one per product. Install the one you have an instance
for, or both. Each has its own guide with the configuration, the environment variables, the
skills and the security notes.

| Plugin Guide | Product | Use it to |
|--------------|---------|-----------|
| [BDSCA](https://github.com/blackducksoftware/sca-llm-plugins/blob/main/claude-plugin-bdsca/README.md) | [**Black Duck SCA**](https://www.blackduck.com/software-composition-analysis-tools/black-duck-sca.html) | Explore Projects and BOMs, triage Vulnerabilities, check Policy and License compliance, run scans, generate SBOM, VEX/CSAF and Notices reports |
| [BDBA](https://github.com/blackducksoftware/sca-llm-plugins/blob/main/claude-plugin-bdba/README.md) | [**Black Duck Binary Analysis**](https://www.blackduck.com/software-composition-analysis-tools/binary-analysis.html) | Scan binaries and firmware, review the components found in them, triage Vulnerabilities, override component versions, generate reports |

The full list of MCP tools for each product is in the
[MCP server guides](https://github.com/blackducksoftware/sca-mcp/blob/main/README.md#products).

## Latest changes

See the [changelog](https://github.com/blackducksoftware/sca-llm-plugins/blob/main/CHANGELOG.md)
for the latest changes.

## Getting Started

### Prerequisites

- **[Claude Code](https://docs.claude.com/en/docs/claude-code/overview)**
- **A Black Duck instance** (SCA, Binary Analysis or both) with a user account and API token
- **[uv](https://docs.astral.sh/uv/getting-started/installation/)** — runs the bundled MCP
  server with Python 3.13
- **Java 11+** _(optional)_ — required only for BDSCA source scanning via Detect

### Installation

Set the URL of your instance, e.g. `BLACKDUCK_BDSCA_URL` or `BLACKDUCK_BDBA_URL`, and store
the API token as described in [Token storage](#token-storage). Then, in Claude Code:

```bash
# add the marketplace (only needed once)
/plugin marketplace add blackducksoftware/llm-plugins
# install the plugin for your product, or both
/plugin install blackduck-bdsca@blackduck-llm-plugins
/plugin install blackduck-bdba@blackduck-llm-plugins
/reload-plugins
```

Once installed, ask "Am I connected to the Black Duck instance?" to check the setup. The
plugin guides list all the environment variables:

- [Configuring the BDSCA plugin](https://github.com/blackducksoftware/sca-llm-plugins/blob/main/claude-plugin-bdsca/README.md#environment-variables)
- [Configuring the BDBA plugin](https://github.com/blackducksoftware/sca-llm-plugins/blob/main/claude-plugin-bdba/README.md#environment-variables)

### Update

```bash
/plugin marketplace update blackduck-llm-plugins
/reload-plugins
```

## Security

### Token permissions and read-only mode

The MCP servers can read and change everything that the token's user can, including
vulnerability remediation status, triages and version overrides. Use the token of a dedicated
Black Duck user with only the permissions you need. Set `BLACKDUCK_MCP_READ_ONLY=true` to turn
off the tools that change data or write files. The plugin guides list these tools.

### Token storage

Claude Code passes its environment to every MCP server and tool it starts, so a token in an
env var can be read by all of them. To keep tokens out of the environment, save each one in a
file that only your own user can read, and configure only the URLs:

```bash
mkdir -p ~/.blackduck && chmod 700 ~/.blackduck
printf '%s' "<bdsca-api-token>" > ~/.blackduck/bdsca-token
printf '%s' "<bdba-api-token>" > ~/.blackduck/bdba-token
chmod 600 ~/.blackduck/bdsca-token ~/.blackduck/bdba-token
```

To use another file, set `BLACKDUCK_BDSCA_TOKEN_FILE` or `BLACKDUCK_BDBA_TOKEN_FILE` to its
path. The `health_check` tool shows where the token came from.

### Data sent to your LLM provider

Tool and Resource results go to the LLM that Claude Code uses. These may hold Vulnerability
findings, Project, Version and Component names, Policy settings, and Scan results. The API
tokens never go to the LLM.

Black Duck does not control how your LLM provider stores or uses this data, or whether the
provider trains models on it. Before you use the plugins with sensitive or regulated
findings, read your provider's terms and data processing agreement, and use its zero data
retention or enterprise options where you need them.

## Support

- [Black Duck Website](https://www.blackduck.com/)
- [Community Portal](https://community.blackduck.com)
- [Email](mailto:support@blackduck.com)
