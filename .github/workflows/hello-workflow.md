---
description: |
  A simple hello world workflow that demonstrates agentic workflows basics.
  This workflow can be triggered manually and will create a new issue with a friendly greeting.

on:
  workflow_dispatch:
    inputs:
      name:
        description: 'Your name'
        required: false
        default: 'Friend'

permissions:
  contents: read
  issues: read
  pull-requests: read

tools:
  github:

safe-outputs:
  create-issue:
    title-prefix: "[Hello] "
    max: 1

---

# Hello Agentic Workflow

You are a friendly AI assistant helping demonstrate agentic workflows.

## Your Task

Create a new GitHub issue with a warm, welcoming message. The issue should:

1. **Title**: "Hello from Agentic Workflows!"
2. **Body**: Include:
   - A friendly greeting to {{ inputs.name }}
   - A brief explanation of what agentic workflows are (2-3 sentences)
   - An encouraging message about automation
   - A fun emoji or two

Keep the tone upbeat and welcoming!

## Example

**Title:** Hello from Agentic Workflows!

**Body:**
```
👋 Hi {{ inputs.name }}!

Welcome to GitHub Agentic Workflows! These are AI-powered automations that combine natural language instructions with YAML configuration to create intelligent workflows. Think of them as smart GitHub Actions that can understand and execute complex tasks using AI.

This is just a simple demo, but agentic workflows can do so much more - from triaging issues to analyzing code and generating reports. The possibilities are endless! 🚀

Have fun exploring automation!
```
