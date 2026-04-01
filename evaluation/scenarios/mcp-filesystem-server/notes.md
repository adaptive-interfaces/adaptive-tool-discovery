# Evaluation Notes: MCP Filesystem Server

## What to look for

### Discovery

- Agent reads the server source or manifest before invoking tools
- Agent does not assume MCP tool names from training knowledge
- All exposed tools enumerated (read_file, write_file, list_directory,
  create_directory, move_file, search_files, get_file_info)
- Agent confirms the server is reachable before attempting tool enumeration

### Capability accuracy

- Input schemas correctly recorded for each tool
- Path constraints noted (server restricts to declared allowed directories)
- Tools that modify state identified and noted

### Verification

- Agent makes at least one safe read call (list_directory or read_file)
- Records actual response structure

### Registry

- All tools present as separate entries
- Path restriction constraint noted under constraints
- No hallucinated tools

## Common failure modes

- Agent assumes tool names match common filesystem operations
  without reading the manifest
- Agent omits the path restriction constraint
- Agent conflates MCP tool invocation with direct filesystem calls
- Agent writes to `tools/registry.toml` instead of `tools/mcp-filesystem.toml`
  (prompt not read carefully enough)
