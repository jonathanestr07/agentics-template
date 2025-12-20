# agentics-template

A template to get started with [GitHub Agentic Workflows](https://githubnext.github.io/gh-aw/).

## Quick Setup

To use Copilot in your workflows, configure a GitHub token:

1. Create a [Personal Access Token](https://github.com/settings/personal-access-tokens/new) with **Copilot Requests** permission
2. Add it as a repository secret named `COPILOT_GITHUB_TOKEN`:
   - Go to **Settings** > **Secrets and variables** > **Actions**
   - Click **New repository secret**
   - Name: `COPILOT_GITHUB_TOKEN`
   - Paste your token and click **Add secret**

## Debugging Workflows

When your agentic workflows need debugging or optimization:

- **Quick automated fixes**: Comment `/q` in any issue or PR to automatically optimize workflows
- **Interactive debugging**: In GitHub Copilot Chat, select `debug-agentic-workflow` from the agents dropdown for guided troubleshooting

See [AGENTS.md](AGENTS.md) for comprehensive debugging guides and examples.

## Learn More

- [GitHub Agentic Workflows Documentation](https://githubnext.github.io/gh-aw/)
- [Token Configuration Reference](https://github.com/githubnext/gh-aw/blob/main/docs/src/content/docs/reference/tokens.md)
