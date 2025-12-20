# agentics-template
A template to get started with GitHub Agentic Workflows

## Getting Started

This repository is a template for creating GitHub Agentic Workflows. Agentic workflows allow you to use AI agents (like GitHub Copilot) to automate tasks in your repository.

## Configuring Tokens for Agentic Workflows

GitHub Agentic Workflows require specific tokens to authenticate with various services. GitHub Actions always provides `GITHUB_TOKEN` automatically, but for advanced features like Copilot workflows, you'll need to configure additional tokens.

### Quick Start: Tokens You Need to Configure

Create these repository secrets based on what features you need:

| When you need this... | Secret to create | Notes |
|----------------------|------------------|-------|
| Cross-repo operations / remote GitHub tools | `GH_AW_GITHUB_TOKEN` | PAT or app token with cross-repo access |
| Copilot workflows (CLI, engine, agent tasks, etc.) | `COPILOT_GITHUB_TOKEN` | Needs Copilot Requests permission and repo access |
| Assigning agents/bots to issues or pull requests | `GH_AW_AGENT_TOKEN` | Used by `assign-to-agent` and Copilot assignee/reviewer flows |
| Any GitHub Projects v2 operations | `GH_AW_PROJECT_GITHUB_TOKEN` | **Required** for `update-project`. Default `GITHUB_TOKEN` cannot access Projects v2 API |
| Isolating MCP server permissions (advanced optional) | `GH_AW_GITHUB_MCP_SERVER_TOKEN` | Only if you want MCP to use a different token than other jobs |

### Step-by-Step: Configuring Tokens for Copilot

Follow these steps to set up GitHub Copilot in your agentic workflows:

#### 1. Create a Personal Access Token for Copilot

1. Go to [GitHub Personal Access Tokens settings](https://github.com/settings/personal-access-tokens/new)
2. Configure the token:
   - **Resource owner**: Your user account (not organization)
   - **Repository access**: "Public repositories" or select specific repos
   - **Permissions**: Select "Copilot Requests" (required)
3. Generate the token and copy it

#### 2. Set the Copilot Token as a Repository Secret

Add the token as a repository secret using the GitHub website:

1. Go to your repository on GitHub
2. Click **Settings** > **Secrets and variables** > **Actions**
3. Click **New repository secret**
4. Set the name to `COPILOT_GITHUB_TOKEN`
5. Paste your Personal Access Token in the **Secret** field
6. Click **Add secret**

#### 3. (Optional) Configure Additional Tokens

If you need additional capabilities, add these tokens as repository secrets following the same steps above:

- **For cross-repository operations**: Create a secret named `GH_AW_GITHUB_TOKEN`
- **For agent assignment operations**: Create a secret named `GH_AW_AGENT_TOKEN`
- **For GitHub Projects v2 operations**: Create a secret named `GH_AW_PROJECT_GITHUB_TOKEN`

## Creating Workflows

### Using the Create Agentic Workflow Agent

The easiest way to create a new agentic workflow is to use the built-in custom agent:

1. Go to your repository on GitHub
2. Click on the **Actions** tab
3. Click **Create new agentic task**
4. In the agent selection, choose **create-agentic-workflow**
5. Describe what you want your workflow to do (e.g., "Create a workflow that triages new issues and adds labels")
6. The agent will generate the workflow file for you

### Debugging and Improving Workflows

If you encounter issues with a workflow or want to improve it:

1. Go to your repository on GitHub
2. Click on the **Actions** tab
3. Click **Create new agentic task**
4. In the agent selection, choose **debug-agentic-workflow**
5. Provide the GitHub URL to your failed workflow run (e.g., `https://github.com/owner/repo/actions/runs/12345`)
6. The agent will analyze the logs and suggest fixes or improvements

## Additional Resources

- [GitHub Agentic Workflows Documentation](https://githubnext.github.io/gh-aw/)
- [Token Reference](https://github.com/githubnext/gh-aw/blob/main/docs/src/content/docs/reference/tokens.md)
- [CLI Documentation](https://githubnext.github.io/gh-aw/setup/cli/)
