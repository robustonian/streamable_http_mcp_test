# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

- **Start the server**: `npx tsx src/index.ts` or `./serve.sh`
- **Install dependencies**: `pnpm install` (uses pnpm as the package manager)
- **Environment setup**: Copy `.env.example` to `.env` and modify PORT if needed

## Architecture Overview

This is an MCP (Model Context Protocol) server implementation using the Streamable HTTP transport, based on [this article](https://azukiazusa.dev/blog/mcp-server-streamable-http-transport/). The server:

- Built with TypeScript and Express.js
- Uses `@modelcontextprotocol/sdk` for MCP protocol implementation
- Implements StreamableHTTPServerTransport for HTTP-based MCP communication
- Currently stateless (sessionIdGenerator: undefined)
- Runs on configurable port (default: 3000) at `/mcp` endpoint
- Supports PORT environment variable configuration

### Key Components

- **MCP Server**: Configured as "my-server" v0.0.1, handles MCP protocol communication
- **Express App**: HTTP server handling POST requests for MCP communication
- **Transport Layer**: StreamableHTTPServerTransport manages the MCP message flow
- **Tools**: Currently implements a "dice" tool that generates random numbers with configurable sides
- **Environment Configuration**: PORT can be set via environment variable or .env file

### Request Handling

- POST `/mcp`: Main MCP request endpoint
- GET `/mcp`: Returns 405 Method Not Allowed (SSE compatibility placeholder)
- DELETE `/mcp`: Returns 405 Method Not Allowed (stateful server placeholder)

### Error Handling

Follows JSON-RPC 2.0 error specification with proper error codes and graceful shutdown handling.

### Adding New Tools

Tools are defined using `mcpServer.tool()` with Zod schema validation. The dice tool demonstrates the pattern:
- Tool name and description
- Zod schema for input validation
- Async handler function returning MCP-formatted response