---
name: agent-browser
description: "Headless browser automation via Docker Microservice. Navigate, interact, and extract web data using deterministic references."
---

# Agent Browser Automation (Microservice Mode)

This skill provides headless browser capabilities via a dedicated, isolated Docker container named `skill-browser`.

## ⚠️ Execution Context (CRITICAL)

- **Route All Commands**: You MUST use `docker exec skill-browser`.
- **Absolute Path**: Use the full path `/usr/local/bin/agent-browser` for reliability.
- **Shared Workspace**: Files (screenshots, downloads) are shared at `/home/node/.openclaw/workspace`. You can access them directly in your current directory.

## 🚀 First-Time Initialization (MANDATORY)

To start the browser on this ARM64 environment, your **FIRST** command in a session MUST include the system path and sandbox flags:

```bash
docker exec skill-browser /usr/local/bin/agent-browser open [https://example.com](https://example.com) --executable-path /usr/bin/chromium --no-sandbox --disable-setuid-sandbox

```

## 🛠️ Standard Execution (After Initialization)

Once the browser is running, you can use cleaner commands:

1. **Navigate**: `docker exec skill-browser /usr/local/bin/agent-browser open <url>`
2. **Snapshot**: `docker exec skill-browser /usr/local/bin/agent-browser snapshot -i` (Get @e1, @e2...)
3. **Interact**: `docker exec skill-browser /usr/local/bin/agent-browser click @e1`
4. **Screenshot**: `docker exec skill-browser /usr/local/bin/agent-browser screenshot page.png`

## Core Command Reference

### Interaction (Always use @refs from Snapshot)

* `... agent-browser fill <@ref> "<text>"`: Clear and type.
* `... agent-browser press Enter`: Submit form or trigger action.
* `... agent-browser wait --load networkidle`: Ensure page is ready.

### Chaining (Recommended for Efficiency)

```bash
docker exec skill-browser bash -c "/usr/local/bin/agent-browser open <url> && /usr/local/bin/agent-browser wait && /usr/local/bin/agent-browser snapshot -i"

```