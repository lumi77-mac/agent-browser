---
name: agent-browser
description: "Headless browser automation CLI. Use this skill to navigate websites, click elements, fill forms, take screenshots, and extract web data using deterministic element references. RUNS IN AN ISOLATED CONTAINER."
---

# Agent Browser Automation (Microservice Mode)

This skill provides headless browser capabilities. The browser engine does NOT run in your local environment. It runs in a dedicated, isolated Docker container named `skill-browser`.

## ⚠️ Execution Context & Shared Volume (CRITICAL)

You are the Master Controller. You **CANNOT** run `agent-browser` directly. You **MUST** route all browser commands to the isolated container using `docker exec skill-browser`. 

Both your container and the `skill-browser` container share the exact same working directory (`/home/node/.openclaw/workspace`). Any screenshots or files saved by the browser will be immediately accessible to you in this directory. You do not need to move files between containers.

## Optimal AI Workflow (Snapshot & Ref Pattern)

Do not guess CSS selectors. Always use the deterministic "snapshot and reference" workflow. Note the `docker exec skill-browser` prefix on EVERY command:

1. **Navigate**: `docker exec skill-browser agent-browser open <url>`
2. **Analyze**: `docker exec skill-browser agent-browser snapshot -i` (Gets interactive elements with refs like `@e1`, `@e2`)
3. **Interact**: Use the refs from the snapshot to perform actions (e.g., `docker exec skill-browser agent-browser click @e1`)
4. **Verify**: Always re-snapshot after navigation or significant DOM changes to get new, updated refs.

## Core Command Reference

### Navigation & Setup
* `docker exec skill-browser agent-browser open <url>`: Navigate to URL
* `docker exec skill-browser agent-browser close`: Close browser instance
* `docker exec skill-browser agent-browser wait --load networkidle`: Wait for page to fully load

### Page Analysis
* `docker exec skill-browser agent-browser snapshot`: Full accessibility tree
* `docker exec skill-browser agent-browser snapshot -i`: Interactive elements only (Recommended)
* `docker exec skill-browser agent-browser screenshot page.png`: Take a screenshot (Saves directly to your shared workspace)

### Interactions (Use @refs)
* `docker exec skill-browser agent-browser click <@ref>`: Click element
* `docker exec skill-browser agent-browser fill <@ref> "<text>"`: Clear and type text into input
* `docker exec skill-browser agent-browser type <@ref> "<text>"`: Type without clearing
* `docker exec skill-browser agent-browser press <key>`: Press key (e.g., Enter, Tab)
* `docker exec skill-browser agent-browser select <@ref> "<value>"`: Select dropdown option
* `docker exec skill-browser agent-browser hover <@ref>`: Hover over element

### Information Extraction
* `docker exec skill-browser agent-browser get text <@ref>`: Get text content
* `docker exec skill-browser agent-browser get attr <@ref> <attr>`: Get specific attribute
* `docker exec skill-browser agent-browser get title`: Get page title
* `docker exec skill-browser agent-browser get url`: Get current URL

## Chaining Commands
For efficiency, chain commands inside a single bash execution string if you do not need intermediate output. 
Example: `docker exec skill-browser bash -c "agent-browser open example.com && agent-browser wait --load networkidle && agent-browser snapshot -i"`