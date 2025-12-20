# Agent Documentation

## Overview

This repository contains **GitHub Agentic Workflows**, which are AI-powered automation workflows that use agents to perform tasks in your repository. These workflows allow you to use AI agents (like GitHub Copilot) to automate tasks such as issue triage, code analysis, and more.

## What are Agentic Workflows?

Agentic workflows are a markdown-based format that combines natural language instructions with YAML frontmatter to define automated workflows. These workflows are compiled into GitHub Actions and can be triggered by various events (issues, pull requests, schedules, etc.).

For complete documentation on GitHub Agentic Workflows, please refer to:
- **[GitHub Agentic Workflows Documentation](.github/aw/github-agentic-workflows.md)** - Comprehensive guide to creating and managing agentic workflows

## Available Custom Agents

This repository includes the following custom agents in `.github/agents/`:

### 1. **create-agentic-workflow**
- **Purpose**: Design and create new agentic workflows with interactive guidance
- **Use case**: When you need to create a new workflow for automating tasks
- **Features**: 
  - Interactive workflow design
  - Helps determine appropriate triggers (issues, pull requests, schedule, etc.)
  - Configures tools and MCP servers
  - Applies security best practices
  - Generates valid `.md` workflow files

### 2. **debug-agentic-workflow**
- **Purpose**: Debug and refine existing agentic workflows
- **Use case**: When a workflow is failing or needs improvement
- **Features**:
  - Analyzes workflow run logs
  - Audits execution to identify issues
  - Provides improvement recommendations
  - Helps fix missing tool calls or configuration issues

## Important: Compiling Workflows

**⚠️ CRITICAL**: After creating or modifying any agentic workflow file (`.md` files in `.github/workflows/`), you **MUST** compile it to generate the GitHub Actions YAML file.

### Using the GitHub Agentic Workflows MCP Tools

**AGENTS MUST use the GitHub Agentic Workflows MCP tools to compile workflows.** The `agentic-workflows` MCP server provides tools specifically designed for workflow management:

- **`compile`** - Compile markdown workflows to YAML (`.lock.yml` files)
- **`status`** - Show status of workflow files in the repository
- **`logs`** - Download and analyze workflow run logs
- **`audit`** - Investigate workflow run failures and generate reports

To enable these tools in a workflow, add:

```yaml
tools:
  agentic-workflows:
```

### Why Compilation is Required

- Agentic workflows are written in markdown (`.md` files)
- They must be compiled to GitHub Actions YAML (`.lock.yml` files) to execute
- The compilation process:
  - Resolves dependencies and imports
  - Processes tool configurations
  - Generates proper GitHub Actions syntax
  - Validates the workflow structure

### Fix All Errors

After compiling using MCP tools, **fix all errors** before proceeding. Common errors include:
- Invalid YAML frontmatter
- Missing required fields
- Incorrect permissions
- Invalid tool configurations
- Security violations

Recompile after fixing errors to ensure the workflow compiles successfully.

## Getting Started

1. **Create a new workflow**: Use the `create-agentic-workflow` custom agent
2. **Compile the workflow**: Use the `agentic-workflows` MCP tool's `compile` command to generate the `.lock.yml` file
3. **Test the workflow**: Trigger it through the appropriate event (issue, PR, schedule, etc.)
4. **Debug if needed**: Use the `debug-agentic-workflow` custom agent to troubleshoot

## Additional Resources

- [Main README](README.md) - Repository setup and token configuration
- [GitHub Agentic Workflows Documentation](https://githubnext.github.io/gh-aw/) - Official online documentation
