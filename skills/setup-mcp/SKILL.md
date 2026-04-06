---
name: setup-mcp
description: Configure .mcp.json from mcp.sample.json by asking the user for required values and verifying each server — trigger phrases: "setup mcp", "configure mcp", "setup mcp servers"
user-invocable: true
---

# Setup MCP

Guide the user through configuring `.mcp.json` from the `mcp.sample.json` template, asking for any missing values, writing the final file, and verifying each server is reachable.

## Steps

### 1. Read the sample

Read `mcp.sample.json` from the project root. If it does not exist, stop and tell the user: "No `mcp.sample.json` found in the project root. Please create one first."

### 2. Check for an existing `.mcp.json`

If `.mcp.json` already exists:
- Read it and compare it to `mcp.sample.json`.
- List any servers from the sample that are missing or still have unfilled placeholders (values matching `<...>`).
- Ask the user: "`.mcp.json` already exists. Do you want to (a) reconfigure it from scratch, or (b) only fill in missing/placeholder values?"
- Proceed accordingly.

### 3. Identify placeholders

Scan `mcp.sample.json` for any values that match the pattern `<SOMETHING>` — these are required inputs. For each placeholder found, note:
- Which server it belongs to.
- What the placeholder name suggests (e.g., `<GITHUB_TOKEN>` → a GitHub personal access token or GitHub Copilot token).

### 4. Ask the user for each placeholder

For each placeholder, explain:
- What the value is for.
- Where to obtain it (use the guidance in the **Placeholder Reference** section below).
- Ask the user to provide it.

Ask about all placeholders in a single message when possible, grouped by server.

If the user cannot provide a value, do not block — mark that server as "manual setup required" and continue with the rest.

### 5. Write `.mcp.json`

Replace all placeholders with the values provided by the user and write the result to `.mcp.json`. Omit any servers the user skipped.

### 6. Verify each server

For each server written to `.mcp.json`, attempt to verify it is reachable:

**HTTP/SSE servers** (`"type": "http"` or `"type": "sse"`):
- Extract the `url`.
- Run: `curl -s -o /dev/null -w "%{http_code}" --max-time 5 "<url>"`
- A 2xx or 4xx response means the endpoint is reachable (auth errors are expected without full credentials — that is fine).
- A connection error or timeout means the server is not reachable.

**stdio servers** (`"command"` field present):
- Check that the command exists: `which <command>` (e.g., `which npx`).
- If the command is `npx`, verify Node.js is installed: `node --version`.
- Do not actually run the MCP server process — just confirm the runtime is available.

### 7. Report results

Print a summary table:

```
Server          Type    Status     Notes
-----------     ----    ------     -----
atlassian       http    OK         Reachable (200)
github          http    OK         Reachable (401 — add token to authenticate)
chrome-devtools stdio   OK         npx available (node v20.x)
```

For any server that failed verification or was skipped, print step-by-step manual instructions (see **Manual Setup Instructions** below).

---

## Placeholder Reference

Use this to explain each placeholder to the user:

| Placeholder | Server | How to obtain |
|---|---|---|
| `<GITHUB_TOKEN>` | github | Create a token at https://github.com/settings/tokens — needs `repo` and `copilot` scopes. For GitHub Copilot MCP, use a Copilot-enabled token or GitHub App token. |

If you encounter a placeholder not listed here, describe it based on its name and ask the user to provide the appropriate credential or value.

---

## Manual Setup Instructions

Print these for any server the user could not configure or that failed verification:

### Atlassian
The Atlassian MCP server uses OAuth. No token is needed in `.mcp.json`, but you must authenticate on first use:
1. Start Claude Code in this project.
2. When prompted by the Atlassian MCP server, complete the OAuth browser flow.
3. Your credentials will be stored securely by the MCP server.

### GitHub
1. Go to https://github.com/settings/tokens and create a fine-grained token with `repo` access.
2. Open `.mcp.json` and replace `<GITHUB_TOKEN>` with the token value.
3. Keep this file out of version control — add `.mcp.json` to `.gitignore` if it is not already there.

### chrome-devtools
1. Install Node.js from https://nodejs.org (LTS version recommended).
2. Verify with: `node --version` and `npx --version`
3. The server will be downloaded automatically by `npx` on first use — no manual install needed.

---

## Rules

- Never print token values back to the user in plain text after they are provided.
- Never commit `.mcp.json` — warn the user if it is not in `.gitignore`.
- Do not modify `mcp.sample.json`.
- If verification fails for a server, do not remove it from `.mcp.json` — leave it in place and give manual instructions.
