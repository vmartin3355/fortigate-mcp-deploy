# FortiGate MCP Server Docker Deployment

Deploy the FortiGate MCP Server using Docker. Manages FortiGate firewalls and
FortiManager instances through the Model Context Protocol (MCP).

## Prerequisites

- Docker CE 29+ installed
- Docker Compose v2+

## Quick Start

### 1. Clone this repository

```bash
git clone https://github.com/vmartin3355/fortigate-mcp-deploy.git
cd fortigate-mcp-deploy
```

### 2. Generate an API key

```bash
openssl rand -hex 32
```

### 3. Edit config/config.json

- Set `require_auth` to `true`
- Paste your generated key into the `api_tokens` array

### 4. Edit devices.yaml

Uncomment the section you need (FortiGate devices, FortiManager, or both)
and fill in your real IPs, API tokens, and credentials.

### 5. Start the server

```bash
docker compose up -d
```

### 6. Verify it is running

```bash
docker compose logs -f
```

The server runs on HTTPS port 8814 with a self-signed certificate
generated automatically on first startup.

## Connect from Claude Desktop (Windows)

```json
{
  "mcpServers": {
    "fortigate": {
      "command": "C:\\PROGRA~1\\nodejs\\npx.cmd",
      "args": [
        "-y",
        "mcp-remote",
        "https://YOUR-SERVER-IP:8814/mcp",
        "--header",
        "Authorization:${AUTH_HEADER}"
      ],
      "env": {
        "NODE_TLS_REJECT_UNAUTHORIZED": "0",
        "AUTH_HEADER": "Bearer YOUR-API-KEY-HERE"
      }
    }
  }
}
```

## Management Commands

```bash
docker compose up -d          # Start in background
docker compose down            # Stop
docker compose logs -f         # View live logs
docker compose restart         # Restart after config changes
```

## File Layout

```
fortigate-mcp-deploy/
â”œâ”€â”€ docker-compose.yml         # How to run the container
â”œâ”€â”€ config/
â”‚   â””â”€â”€ config.json            # Auth settings, API key, logging
â””â”€â”€ devices.yaml               # FortiGate/FortiManager credentials
```

## Security

- API key authentication is enforced when `require_auth` is `true`
- TLS certificate is auto-generated on first startup
- Credentials in `devices.yaml` are mounted read-only into the container
- No credentials are baked into the Docker image
