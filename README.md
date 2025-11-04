# Augment Agent Rules

Custom rule files for Augment AI coding assistant to enhance its capabilities and ensure consistent, high-quality assistance.

## Overview

This repository contains a comprehensive set of rules that guide the Augment Agent's behavior, decision-making, and interaction patterns. These rules are designed to work together as a cohesive system while remaining independently useful.

## Rule Files

Rules have been consolidated into two streamlined documents that remove verbose examples and templates, retaining only essential, actionable guidance. All references use "Chevy (user)" in third person.

- **[core-protocol.md](rules/core-protocol.md)** - Core cognitive protocol, context engineering essentials, prohibitions, and "NEVER Create Excessive Documentation" policy. Defines the mandatory initiation sequence (THINK → SEARCH → INVENTORY → PLAN → VALIDATE), memory management, tool usage, and strict prohibitions.

- **[gitflow-mcp.md](rules/gitflow-mcp.md)** - GitFlow branching practices merged with MCP tools priority and usage. Covers branch structure, workflow sequences, PR requirements, and prioritization of MCP tools (mcpdocker, serena, openmemory, exa) over standard tools.

## Configuration

- **[settings.json](settings.json)** - Augment configuration including model selection, indexing directories, and MCP server configurations.

## MCP Servers

This configuration includes the following MCP servers:

- **mcpdocker** - Docker MCP Gateway for documentation retrieval, code search, and web search
- **serena** - Advanced code intelligence for symbol manipulation and codebase analysis
- **openmemory** - Persistent memory management across conversations
- **exa** - Advanced web search and code context retrieval

## Usage

These rules are automatically loaded by the Augment Agent when working in this directory. The agent follows a structured approach:

1. **THINK** - Deep analysis of request intent and complexity
2. **SEARCH** - Memory retrieval for context
3. **INVENTORY** - Tool/MCP availability verification
4. **PLAN** - Structured approach with success criteria
5. **VALIDATE** - Confirm understanding before execution

## Contributing

When adding new rules:

- Use the standard frontmatter format with `type: "agent_requested"` and clear description
- Keep files under 500 lines
- Use clear headers, lists, code blocks, and examples
- Make content actionable and easy to scan
- Follow formatting standards from existing rule files
- Do NOT add REFERENCES sections or version/last-updated metadata
- Make rules package-manager agnostic where applicable

## License

MIT License - Feel free to use and adapt these rules for your own projects.

