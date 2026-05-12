# pi-inter-agent

Pi extension for connecting to the [inter-agent](https://github.com/arcanemachine/inter-agent) message bus.

## Features

- **Background listener** — Stay connected to the bus and receive messages as Pi notifications
- **Commands** — Connect, disconnect, send, broadcast, list, status, shutdown
- **Tools** — LLM-callable tools for send, broadcast, list, and status
- **State persistence** — Connection state survives Pi session reloads
- **Safe truncation** — Long messages are truncated to 1000 characters in notifications

## Installation

### From Local Path

```bash
pi install /workspace/projects/pi/pi-inter-agent
```

### Direct Load (Development)

```bash
pi -e /workspace/projects/pi/pi-inter-agent/src/index.ts
```

## Prerequisites

The inter-agent server must be running in your workspace:

```bash
cd /workspace/projects/inter-agent
uv run inter-agent-server
```

The extension uses `uv run inter-agent-pi ...` and `uv run inter-agent-connect ...` internally, so the inter-agent package must be installed in the active virtual environment.

## Commands

| Command | Usage | Description |
|---------|-------|-------------|
| `/inter-agent-connect` | `/inter-agent-connect <name> [label]` | Connect to the bus as `name` |
| `/inter-agent-disconnect` | `/inter-agent-disconnect` | Disconnect from the bus |
| `/inter-agent-send` | `/inter-agent-send <to> <text>` | Send a direct message |
| `/inter-agent-broadcast` | `/inter-agent-broadcast <text>` | Broadcast to all agents |
| `/inter-agent-list` | `/inter-agent-list` | List connected sessions |
| `/inter-agent-status` | `/inter-agent-status` | Check server status |
| `/inter-agent-shutdown` | `/inter-agent-shutdown` | Stop the server |

## Tools

| Tool | Description |
|------|-------------|
| `inter_agent_send` | Send a direct message to a routing name |
| `inter_agent_broadcast` | Broadcast a message to all agents |
| `inter_agent_list` | List connected agent sessions |
| `inter_agent_status` | Check server availability and identity |

## Example Workflow

1. Start the server in another terminal:
   ```bash
   uv run inter-agent-server
   ```

2. In Pi, connect to the bus:
   ```
   /inter-agent-connect my-pi-session --label "Pi Agent"
   ```

3. Send a message to another agent:
   ```
   /inter-agent-send agent-b "run tests"
   ```

4. Or broadcast to everyone:
   ```
   /inter-agent-broadcast "build is green"
   ```

5. Check who's connected:
   ```
   /inter-agent-list
   ```

## Development

```bash
cd /workspace/projects/pi/pi-inter-agent
npm install
npm run typecheck
npm run build
npm run format
```

## License

MIT
