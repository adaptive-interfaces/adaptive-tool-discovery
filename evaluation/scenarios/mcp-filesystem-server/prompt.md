# Scenario: MCP Filesystem Server

Apply the Adaptive Tool Discovery (ATD) skill to map the capabilities
of the MCP reference filesystem server.

Source:
<https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem>

The server must be installed and running locally before this scenario.
See the README section "Set Up MCP Filesystem Server" for setup instructions.

Discover available tools by reading the server manifest or schema.
Do not rely on prior knowledge of MCP tool names or schemas.

Produce a tool registry at `tools/mcp-filesystem.toml` covering all
exposed tools.

After completing the registry:

1. Create or update `tools/manifest.toml` to include `tools/mcp-filesystem.toml`
2. Copy `tools/mcp-filesystem.toml` to `evaluation/scenarios/mcp-filesystem-server/tools/`
3. Write a summary of the ATD run to `evaluation/scenarios/mcp-filesystem-server/response.txt`
4. Score the output against `evaluation/rubric.md` and include the score table
   in `response.txt`
