## Mcp

**Model Context Protocol** - connects Ai apps to external data sources, tools and systems.
Consist of:
- **MCP host** - an app that user interacts with like Claude Desktop, Cursor, ChatGpt
- **MCP Client** - maintains connections between them, routes requests/responses, handles discovery of available tools
- **MCP Server** - a lightweight standalone program that connects to a specific external system or a data source.