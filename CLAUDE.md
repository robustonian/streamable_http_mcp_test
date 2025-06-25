# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

- **Start the server**: `npx tsx src/index.ts` or `./serve.sh`
- **Install dependencies**: `pnpm install` (uses pnpm as the package manager)

## Architecture Overview

This is an MCP (Model Context Protocol) server implementation using the Streamable HTTP transport. The server:

- Built with TypeScript and Express.js
- Uses `@modelcontextprotocol/sdk` for MCP protocol implementation
- Implements StreamableHTTPServerTransport for HTTP-based MCP communication
- Currently stateless (sessionIdGenerator: undefined)
- Runs on port 2987 at `/mcp` endpoint

### Key Components

- **MCP Server**: Configured as "my-server" v0.0.1, handles MCP protocol communication
- **Express App**: HTTP server handling POST requests for MCP communication
- **Transport Layer**: StreamableHTTPServerTransport manages the MCP message flow
- **Tools**: Currently implements a "dice" tool that generates random numbers

### Request Handling

- POST `/mcp`: Main MCP request endpoint
- GET `/mcp`: Returns 405 Method Not Allowed (SSE compatibility placeholder)
- DELETE `/mcp`: Returns 405 Method Not Allowed (stateful server placeholder)

### Error Handling

Follows JSON-RPC 2.0 error specification with proper error codes and graceful shutdown handling.