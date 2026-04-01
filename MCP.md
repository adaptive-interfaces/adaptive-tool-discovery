# MCP

MCP stands for Model Context Protocol,
an open standard Anthropic published in late 2024
for connecting AI agents to external tools and data sources
in a standardized way.

## Core Idea

Before MCP, every tool integration was bespoke.
If you wanted Claude to read your filesystem,
query a database, or call an API,
you had to write custom code for each one.
MCP standardizes the interface so any MCP-compatible agent
can talk to any MCP-compatible server
without custom integration code.
Think of it like USB. Before USB,
every peripheral had its own connector.
MCP is the USB for AI tool connections.

## How It Works

An MCP server exposes a set of tools: functions the agent can call.
The server declares:

- what tools exist
- what inputs each tool takes
- what each tool returns

The agent discovers these tools at runtime by reading the server's manifest,
then invokes them by name with structured inputs.
The server handles execution and returns structured outputs.

The three things MCP servers can expose

1. Tools: functions the agent can invoke (read a file, query a database, send a message)
2. Resources: data the agent can read (file contents, database records)
3. Prompts: pre-built prompt templates

ATD is doing MCP discovery manually for any tool surface.
For MCP servers specifically,
the discovery step is formalized.
The server manifest tells exactly what tools exist and what their schemas are,
rather than requiring docs-fetching or help-output parsing.

## Example

The MCP filesystem server exposes tools like:

```text
read_file(path)
write_file(path, contents)
list_directory(path)
create_directory(path)
move_file(source, destination)
search_files(path, pattern)
get_file_info(path)
```

Claude Code connects to the server,
reads the manifest, and can now call any of these tools directly.
No custom filesystem code needed.

## How Claude Code uses MCP

Claude Code has built-in MCP support.
You configure servers in .claude/mcp_settings.json and
Claude Code connects to them at session start.
The agent can then invoke their tools as if they were native capabilities.

## The Ecosystem

There are already hundreds of MCP servers:
for GitHub, Slack, Google Drive, databases, browsers, and more.
The adaptive-interfaces org's ATD skill is particularly relevant
here because mapping MCP server capabilities
is exactly the kind of tool discovery ATD is designed to do.
