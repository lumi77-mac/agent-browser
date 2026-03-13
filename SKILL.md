---
name: agent-browser
description: "Headless browser automation CLI. Use this skill to navigate websites, click elements, fill forms, take screenshots, and extract web data using deterministic element references."
---

# Agent Browser Automation

This skill provides headless browser capabilities. It allows the agent to navigate the web, analyze the DOM, and interact with web elements.

## ⚠️ Installation (CRITICAL: ARM64 Docker Environment)

You are running inside an ARM64 Node.js Docker container. You **MUST NOT** build this tool from source (do not run `pnpm build` or `pnpm build:native`). 

To install or update this skill, you MUST use the global NPM method followed by the browser binary download:

```bash
npm install -g agent-browser
agent-browser install

```

## Optimal AI Workflow (Snapshot & Ref Pattern)

Do not guess CSS selectors. Always use the deterministic "snapshot and reference" workflow:

1. **Navigate**: `agent-browser open <url>`
2. **Analyze**: `agent-browser snapshot -i` (Gets interactive elements with refs like `@e1`, `@e2`)
3. **Interact**: Use the refs from the snapshot to perform actions (e.g., `agent-browser click @e1`, `agent-browser fill @e2 "text"`)
4. **Verify**: Always re-snapshot after navigation or significant DOM changes to get new, updated refs.

## Core Command Reference

### Navigation & Setup

* `agent-browser open <url>`: Navigate to URL
* `agent-browser close`: Close browser instance
* `agent-browser wait --load networkidle`: Wait for page to fully load

### Page Analysis

* `agent-browser snapshot`: Full accessibility tree
* `agent-browser snapshot -i`: Interactive elements only (Recommended)
* `agent-browser screenshot page.png`: Take a screenshot

### Interactions (Use @refs)

* `agent-browser click <@ref>`: Click element
* `agent-browser fill <@ref> "<text>"`: Clear and type text into input
* `agent-browser type <@ref> "<text>"`: Type without clearing
* `agent-browser press <key>`: Press key (e.g., Enter, Tab)
* `agent-browser select <@ref> "<value>"`: Select dropdown option
* `agent-browser hover <@ref>`: Hover over element

### Information Extraction

* `agent-browser get text <@ref>`: Get text content
* `agent-browser get attr <@ref> <attr>`: Get specific attribute
* `agent-browser get title`: Get page title
* `agent-browser get url`: Get current URL

## Chaining Commands

For efficiency, chain commands using `&&` if you do not need intermediate output.
Example: `agent-browser open example.com && agent-browser wait --load networkidle && agent-browser snapshot -i`