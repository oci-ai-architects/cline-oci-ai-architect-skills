# MCP Architecture for Oracle

You are an expert in Model Context Protocol (MCP) integration with Oracle Cloud Infrastructure.

## When to Use
- Integrating MCP servers with OCI services
- Building Oracle Database MCP connections
- Creating OCI infrastructure MCP tools
- Enterprise MCP architecture patterns

## Oracle MCP Servers

### Oracle Database MCP
```json
{
  "mcpServers": {
    "oracle-database": {
      "command": "npx",
      "args": ["-y", "@anthropic/oracle-database-mcp"],
      "env": {
        "ORACLE_USER": "your_user",
        "ORACLE_PASSWORD": "your_password",
        "ORACLE_CONNECT_STRING": "host:port/service"
      }
    }
  }
}
```

**Tools:** query, describe_table, list_tables, get_ddl

### Oracle AI Database 26ai + MCP
26ai introduces **Select AI Agent** — in-database AI agents that can be called via MCP:
- Natural language to SQL translation
- In-database vector search via MCP
- Agent-to-agent communication
- MCP as the protocol layer between agents and database

## Architecture Patterns

### Pattern 1: Direct Database Access
```
AI Assistant → Oracle DB MCP → AI Database 26ai
```

### Pattern 2: OCI Resource Management
```
AI Assistant → OCI MCP → OCI APIs → Resources
```

### Pattern 3: Agent-Orchestrated
```
                        ┌→ Oracle DB MCP
AI Assistant → ADK Agent ┼→ OCI MCP
                        └→ Custom Tools
```

### Pattern 4: Enterprise (Production)
```
┌──────────────────────────────────────┐
│              Enterprise Zone         │
│  ┌─────────────┐    ┌────────────┐  │
│  │ AI Assistant│───▶│ MCP Gateway│  │
│  └─────────────┘    └──────┬─────┘  │
│                             │        │
│      ┌──────────────────────┼────────│
│      ▼                      ▼        │
│  ┌───────────┐        ┌──────────┐  │
│  │ DB MCP    │        │ OCI MCP  │  │
│  └─────┬─────┘        └────┬─────┘  │
│        ▼                    ▼        │
│  ┌───────────┐        ┌──────────┐  │
│  │ Oracle    │        │ OCI      │  │
│  │ AI DB 26ai│        │ Services │  │
│  └───────────┘        └──────────┘  │
└──────────────────────────────────────┘
```

## Security
- Use instance principals (no API keys on instances)
- Private endpoints for MCP servers
- Audit all MCP tool invocations
- Least privilege IAM policies

## Resources
- [Oracle MCP Servers](https://github.com/oracle/mcp-servers)
- [MCP Specification](https://modelcontextprotocol.io/)
- [Select AI Agent (26ai)](https://docs.oracle.com/en/database/oracle/oracle-database/26/)
