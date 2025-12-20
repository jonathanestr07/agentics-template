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

## Using the Debug-Agentic-Workflow Agent

The `debug-agentic-workflow` agent is your expert companion for troubleshooting and improving agentic workflows. It uses the `gh aw` CLI tools to analyze logs, audit runs, and provide actionable recommendations.

### How to Invoke the Debug Agent

You can use the debug agent through GitHub Copilot in your development environment:

1. **Open GitHub Copilot Chat** in your IDE (VS Code, Visual Studio, etc.) or on GitHub.com
2. **Select the agent** from the agents dropdown at the bottom of the chat panel
3. **Choose `debug-agentic-workflow`** from the list of available agents
4. **Provide context** about what you want to debug

**Example prompts after selecting the agent:**

```
Help me debug the daily-team-status workflow

Investigate why this workflow run failed: https://github.com/owner/repo/actions/runs/12345678

The issue-triage workflow is not creating labels properly
```

### Common Debugging Scenarios

#### Scenario 1: Analyzing a Failed Workflow Run

When you have a specific workflow run URL that failed:

```
Investigate the reason there is a missing tool call in this run: 
https://github.com/githubnext/agentics-template/actions/runs/20135841934
```

*(After selecting `debug-agentic-workflow` agent from the agents dropdown)*

**What the agent does:**
1. Extracts the run ID from the URL
2. Downloads the workflow artifacts and logs using `gh aw audit <run-id>`
3. Analyzes missing tools, errors, and execution metrics
4. Provides specific recommendations for fixing the issue

**Expected output:**
- Identification of missing or misconfigured tools
- Specific errors found in the logs
- Actionable fixes (e.g., "Add `web-search` to the tools section")
- Compilation command to validate changes

#### Scenario 2: Debugging a Workflow Without a Specific Run

When you want to analyze a workflow's recent performance:

```
Debug the issue-triage workflow
```

*(After selecting `debug-agentic-workflow` agent from the agents dropdown)*

**What the agent does:**
1. Asks you to choose between:
   - **Option 1**: Analyze existing logs from recent runs
   - **Option 2**: Run the workflow now and audit the results
2. Downloads and analyzes workflow logs
3. Identifies patterns, errors, and inefficiencies
4. Suggests improvements to the workflow configuration

**The agent will guide you through:**
- Verifying the workflow exists
- Choosing a debug mode
- Analyzing logs and metrics
- Making and validating changes

#### Scenario 3: Investigating Missing Tool Errors

When a workflow reports missing tools:

```
Why is my workflow showing "missing tool: web-search" errors?
```

*(After selecting `debug-agentic-workflow` agent from the agents dropdown)*

**What the agent does:**
1. Reviews the workflow's `tools:` configuration
2. Checks audit logs for tool call attempts
3. Identifies if the tool name is incorrect or not configured
4. Provides exact YAML snippets to add the missing tool

**Common fixes:**
- Adding tools to the frontmatter:
  ```yaml
  tools:
    web-search:
  ```
- Correcting tool names (e.g., `safe-outputs-create_pull_request` → `create_pull_request`)
- Enabling safe-outputs configuration

#### Scenario 4: Performance Optimization

When a workflow is slow or uses too many tokens:

```
My workflow is timing out and using too many tokens
```

*(After selecting `debug-agentic-workflow` agent from the agents dropdown)*

**What the agent does:**
1. Analyzes token usage patterns from logs
2. Identifies repetitive or inefficient tool calls
3. Suggests prompt optimizations
4. Recommends timeout adjustments or caching strategies

### Step-by-Step Example: Debugging a Complete Workflow

Let's walk through a complete debugging session:

**1. Start the debug session:**

First, select `debug-agentic-workflow` from the agents dropdown in Copilot Chat, then:

```
I need help debugging the daily-team-status workflow. 
It's not posting the status updates correctly.
```

**2. The agent responds:**
```
🔍 Let's debug your agentic workflow!

I can help you:
- List all workflows with: `gh aw status`
- Analyze the daily-team-status workflow
- Or provide a workflow run URL

How would you like to proceed?
```

**3. You choose an approach:**
```
Analyze existing logs from the last week
```

**4. The agent investigates:**
```
📊 Downloading logs for daily-team-status...

Analyzing the results:
- Token usage: 25,000 tokens average
- Errors found: 3 runs with "permission denied" errors
- Missing tools: "add-comment" tool attempted but not configured
```

**5. The agent provides recommendations:**
```
✅ Recommendations:

1. Add missing safe-output configuration:
   safe-outputs:
     add-comment:
       max: 5

2. Fix permission error - add to frontmatter:
   permissions:
     issues: write

3. Compile to validate: gh aw compile daily-team-status
```

**6. You make the changes and validate:**
```
I've made those changes. Can you help validate them?
```

**7. The agent validates:**
```
🎯 Running validation...

gh aw compile daily-team-status

✅ Compilation successful!
No errors or warnings.

Would you like to run the workflow again to verify the improvements?
```

### Available Debug Commands

The debug agent uses these underlying commands (you don't need to run them manually):

- `gh aw status` - List all workflows in the repository
- `gh aw logs <workflow-name> --json` - Download and analyze workflow logs
- `gh aw audit <run-id> --json` - Investigate a specific workflow run
- `gh aw compile <workflow-name>` - Validate workflow syntax and configuration
- `gh aw run <workflow-name>` - Trigger a workflow run (requires `workflow_dispatch`)

### Tips for Effective Debugging

1. **Provide specific information**: Include workflow names, run URLs, or error messages
2. **Start with recent runs**: Focus on the most recent failures for relevant context
3. **One issue at a time**: Debug individual problems before moving to the next
4. **Validate after changes**: Always compile workflows after making modifications
5. **Test incrementally**: Make small changes and test between iterations

## Using the /q Command for Workflow Optimization

The `/q` command is an intelligent assistant that automatically analyzes, optimizes, and fixes agentic workflows in your repository. Think of it as your workflow "quartermaster" - it investigates performance issues, identifies missing tools, and creates pull requests with improvements.

### What is /q?

`/q` is a special workflow triggered by a slash command that:
- **Investigates** workflow performance using live logs and audits
- **Identifies** missing tools and permission issues
- **Detects** inefficiencies through repetitive tool calls analysis
- **Extracts** common patterns for reusable workflow steps
- **Creates** pull requests with optimized workflow configurations

### How to Use /q

#### Basic Usage

Simply comment `/q` on any issue or pull request where you have write access:

**In an issue:**
```
/q
```

**In a pull request:**
```
/q
```

**With specific instructions:**
```
/q Optimize the issue-triage workflow - it's using too many tokens
```

```
/q Check why the daily-team-status workflow keeps failing
```

### What Happens When You Use /q

When you invoke `/q`, the workflow automatically:

1. **🔍 Analyzes the context** - Reads the issue/PR to understand what needs improvement
2. **📊 Gathers live data** - Downloads recent workflow logs and audit reports
3. **🔎 Investigates issues** - Identifies missing tools, permission errors, performance problems
4. **✏️ Makes improvements** - Modifies workflow files to fix identified issues
5. **✅ Validates changes** - Compiles all modified workflows to ensure correctness
6. **🚀 Creates a PR** - Submits a pull request with optimizations (if changes are needed)

### Common Use Cases

#### Use Case 1: General Workflow Optimization

**Scenario:** You want to improve all workflows in your repository.

**Command:**
```
/q Analyze all workflows and optimize them
```

**What /q does:**
- Reviews logs from all workflows in the last 7 days
- Identifies common issues across workflows
- Creates shared configurations for repeated patterns
- Submits a PR with optimizations for multiple workflows

**Example PR created:**
```
Title: [q] Optimize workflows - Add missing tools and fix permissions

Changes:
- Added web-search tool to 3 workflows
- Fixed permission errors in issue-triage workflow
- Extracted common reporting pattern to shared/reporting.md
- Optimized token usage in daily-team-status workflow
```

#### Use Case 2: Debugging a Specific Workflow

**Scenario:** Your `issue-triage` workflow keeps failing with missing tool errors.

**Command:**
```
/q Fix the issue-triage workflow - missing tool errors
```

**What /q does:**
- Focuses analysis on the `issue-triage` workflow
- Downloads recent logs to identify which tools are missing
- Adds missing tools to the workflow configuration
- Validates the changes and creates a PR

**Example PR created:**
```
Title: [q] Fix issue-triage workflow - Add missing tools

Issues Found:
- Missing tool: github-mcp-server (found in runs #12345, #12346)
- Permission error: issues.write required (error in run #12347)

Changes Made:
- Added github-mcp-server to tools configuration
- Added issues: write permission
- Compiled successfully ✅
```

#### Use Case 3: Performance Improvement

**Scenario:** A workflow is using too many tokens and timing out.

**Command:**
```
/q The daily-team-status workflow is using too many tokens and timing out
```

**What /q does:**
- Analyzes token usage patterns from logs
- Identifies repetitive tool calls
- Optimizes the workflow prompt
- Adds appropriate timeout settings
- Creates a PR with performance improvements

**Example PR created:**
```
Title: [q] Optimize daily-team-status performance

Issues Found:
- High token usage: avg 45,000 tokens per run
- Repetitive tool calls: file reading repeated 20+ times
- Timeout in 2 of 10 runs

Changes Made:
- Extracted file reading logic to shared step
- Added cache-memory configuration
- Increased timeout-minutes from 15 to 30
- Expected 40% reduction in token usage
```

#### Use Case 4: Permission Issues

**Scenario:** Workflows are failing due to permission errors.

**Command:**
```
/q Workflows are getting permission denied errors
```

**What /q does:**
- Scans logs for permission-related errors
- Identifies which permissions are missing
- Adds minimal necessary permissions
- Creates a PR with permission fixes

### Understanding /q Output

When `/q` finishes, you'll see:

1. **A comment on your issue/PR** (if no changes were needed):
   ```
   🔍 Q Analysis Complete
   
   I've analyzed your workflows and found no issues requiring changes.
   All workflows are properly configured and running smoothly.
   ```

2. **A new pull request** (if changes were made):
   - **Title**: Prefixed with `[q]` and describes the optimization
   - **Labels**: Automatically tagged with `automation` and `workflow-optimization`
   - **Description**: Detailed report of issues found, changes made, and validation results

### /q Pull Request Structure

Every PR created by `/q` includes:

```markdown
# Q Workflow Optimization Report

## Issues Found (from live data)

### issue-triage
- Log Analysis: Analyzed 15 runs from Dec 13-20
- Run IDs Analyzed: #12345, #12346, #12347
- Issues Identified:
  - Missing tools: github-mcp-server (attempted in 8/15 runs)
  - Permission errors: issues.write required (3 failures)

## Changes Made

### issue-triage (workflows/issue-triage.md)
- Added missing tool: github-mcp-server (found in run #12345)
- Fixed permission: Added issues: write (error in run #12347)

## Expected Improvements

- Reduced missing tool errors by adding 1 tool
- Fixed 3 permission issues
- Optimized 1 workflow for better performance

## Validation

All modified workflows compiled successfully:
- ✅ issue-triage

## References

- Run IDs investigated: #12345, #12346, #12347
```

### Permissions Required

To use `/q`, you need one of these repository roles:
- `admin` - Full repository access
- `maintainer` - Can manage repository settings
- `write` - Can push to the repository

### Best Practices for Using /q

1. **Be specific in your request**: Provide context about what you want optimized
2. **Use regularly**: Run `/q` after making workflow changes or when noticing issues
3. **Review the PR**: Always review the changes before merging
4. **Provide feedback**: If `/q` misses something, add details to your comment
5. **Combine with debug agent**: Select `debug-agentic-workflow` agent in Copilot Chat for interactive debugging, use `/q` for automated fixes

### /q vs debug-agentic-workflow Agent

| Feature | /q Command | debug-agentic-workflow Agent |
|---------|-----------|------------------------------|
| **Invocation** | Comment `/q` in issue/PR | Select agent from dropdown in Copilot Chat |
| **Interaction** | Automated, no interaction | Interactive, guided debugging |
| **Output** | Creates PR automatically | Provides recommendations for you to implement |
| **Use Case** | Quick automated fixes | Deep investigation and learning |
| **Speed** | Fast, runs in background | Slower, collaborative process |
| **Control** | Less control, automatic | Full control, step-by-step |

**When to use /q:**
- You want quick automated optimizations
- You trust the system to make changes
- You want a PR ready for review
- You're investigating multiple workflows

**When to use debug-agentic-workflow agent:**
- You want to understand what's wrong
- You want to learn debugging techniques
- You need interactive guidance
- You want full control over changes

### Examples of /q in Action

#### Example 1: After Deploying New Workflows

**Scenario:** You've just created several new workflows and want to ensure they're optimized.

```
/q Review all new workflows and ensure they're properly configured
```

**Result:** `/q` analyzes all workflows, identifies missing tools in 2 workflows, adds them, and creates a PR.

#### Example 2: Investigating Repeated Failures

**Scenario:** You notice the same workflow failing multiple times.

```
/q The issue-triage workflow has failed 5 times today - please investigate and fix
```

**Result:** `/q` analyzes the failure logs, identifies a permission issue, fixes it, and creates a PR with the solution.

#### Example 3: Token Budget Concerns

**Scenario:** Workflows are using too many tokens and increasing costs.

```
/q Optimize all workflows for token usage - costs are too high
```

**Result:** `/q` analyzes token patterns, identifies inefficiencies, extracts common patterns, and creates a PR with optimizations that reduce token usage by 30%.

### Advanced /q Usage

#### Targeting Specific Workflows

Mention the workflow name in your command:

```
/q Optimize the weekly-research workflow for better performance
```

#### Multiple Concerns

List multiple issues:

```
/q Check for missing tools, permission errors, and high token usage across all workflows
```

#### From a Workflow Run URL

Reference a specific failed run:

```
/q Investigate this failed run and fix the issues: 
https://github.com/owner/repo/actions/runs/12345678
```

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

## Quick Reference: Debugging Workflows

### Decision Tree: Which Tool to Use?

```
Do you need to fix workflows?
│
├─ Yes, I want automated fixes
│  └─ Use /q command in an issue/PR
│     Example: /q Optimize all workflows
│
└─ Yes, but I want to understand and control the process
   └─ Select debug-agentic-workflow agent in Copilot Chat
      Example: Select agent, then type: Help me debug issue-triage workflow
```

### Quick Command Reference

| Task | Command | Where |
|------|---------|-------|
| Auto-optimize all workflows | `/q` | Issue/PR comment |
| Auto-fix specific workflow | `/q Fix [workflow-name]` | Issue/PR comment |
| Interactive debugging | Select `debug-agentic-workflow` agent, then describe issue | Copilot Chat |
| Debug from URL | Select `debug-agentic-workflow` agent, then provide run URL | Copilot Chat |
| Create new workflow | Select `create-agentic-workflow` agent | Copilot Chat |

### Common Debugging Scenarios - Quick Solutions

| Problem | Solution | Tool |
|---------|----------|------|
| Workflow failing with missing tool | Add tool to `tools:` section | /q or debug agent |
| Permission denied errors | Add permission to `permissions:` | /q or debug agent |
| High token usage | Optimize prompts, add caching | /q or debug agent |
| Workflow timeout | Increase `timeout-minutes` | /q or debug agent |
| Unknown issue | Start with interactive debugging | debug agent |
| Multiple workflows broken | Automated fix for all | /q |

### Workflow Debugging Checklist

When debugging a workflow, check these in order:

- [ ] **Compilation**: Does `gh aw compile [workflow]` succeed?
- [ ] **Tools**: Are all required tools listed in `tools:` section?
- [ ] **Permissions**: Are necessary permissions in `permissions:` section?
- [ ] **Safe-outputs**: Are write operations configured in `safe-outputs:`?
- [ ] **Network**: Does workflow need network access in `network:` allowlist?
- [ ] **Triggers**: Is the workflow triggered by the expected events?
- [ ] **Recent logs**: Do logs show specific errors or patterns?

## Additional Resources

- [Main README](README.md) - Repository setup and token configuration
- [GitHub Agentic Workflows Documentation](https://githubnext.github.io/gh-aw/) - Official online documentation
- [Debug-Agentic-Workflow Agent](.github/agents/debug-agentic-workflow.agent.md) - Full agent instructions
- [Q Workflow](.github/workflows/q.md) - /q command implementation
