# sentry-selfhosted-mcp

A Model Context Protocol (MCP) server designed specifically for interacting with self-hosted Sentry instances.

## Overview

This server provides tools to retrieve information and perform actions on issues within a self-hosted Sentry setup. It reads the Sentry instance URL, authentication token, and organization slug from environment variables.

## Available Tools

The following tools are available:

1.  **`get_sentry_issue`**
    *   Description: Retrieve details for a specific Sentry issue by ID or URL.
    *   Input: `{ "issue_id_or_url": "string" }` (e.g., "12345" or "https://sentry.example.com/organizations/org/issues/12345/")

2.  **`list_sentry_projects`**
    *   Description: List all projects within the configured Sentry organization.
    *   Input: `{}` (No arguments needed)

3.  **`list_sentry_issues`**
    *   Description: List issues for a specific project, optionally filtering by query or status.
    *   Input: `{ "project_slug": "string", "query": "string" (optional), "status": "resolved" | "unresolved" | "ignored" (optional) }`

4.  **`get_sentry_event_details`**
    *   Description: Retrieve details for a specific event ID within a project.
    *   Input: `{ "project_slug": "string", "event_id": "string" }`

5.  **`update_sentry_issue_status`**
    *   Description: Update the status of a Sentry issue.
    *   Input: `{ "issue_id": "string", "status": "resolved" | "ignored" | "unresolved" }`

6.  **`create_sentry_issue_comment`**
    *   Description: Add a comment to a Sentry issue.
    *   Input: `{ "issue_id": "string", "comment_text": "string" }`

## Installation & Setup

1.  **Clone/Place Project:** Clone this repository or place the project files in your desired location.
2.  **Install Dependencies:** Navigate into the project directory (`sentry-selfhosted-mcp`) and run:
    ```bash
    cd <path/to/sentry-selfhosted-mcp>
    npm install
    ```
3.  **Build Server:** Compile the TypeScript code from the project directory:
    ```bash
    npm run build
    ```
    This creates the executable JavaScript file in the `build/` directory.

## Configuration

This server requires the following environment variables to be set:

*   `SENTRY_URL`: The base URL of your self-hosted Sentry instance (e.g., `https://sentry.beoflow.app`).
*   `SENTRY_AUTH_TOKEN`: Your Sentry API authentication token (ensure it has necessary scopes like `issue:read`, `project:read`, `event:read`, `issue:write`, `comment:write`).
*   `SENTRY_ORG_SLUG`: The slug of your Sentry organization (e.g., `beoflow`).

**Example MCP Client Configuration:**

Add the following entry to your MCP client's configuration file (e.g., `cline_mcp_settings.json` for the VS Code extension, `claude_desktop_config.json` for Claude.app):

```json
    "sentry-selfhosted-mcp": {
      "command": "node",
      "args": [
        "<full/path/to/sentry-selfhosted-mcp/build/index.js>"
      ],
      "env": {
        "SENTRY_URL": "YOUR_SENTRY_URL",
        "SENTRY_AUTH_TOKEN": "YOUR_SENTRY_AUTH_TOKEN",
        "SENTRY_ORG_SLUG": "YOUR_SENTRY_ORG_SLUG"
      },
      "disabled": false,
      "autoApprove": [],
      "transportType": "stdio"
    }
```

Replace `YOUR_SENTRY_URL`, `YOUR_SENTRY_AUTH_TOKEN`, and `YOUR_SENTRY_ORG_SLUG` with your actual values.

After updating the settings file, the MCP client should automatically connect to the server.
