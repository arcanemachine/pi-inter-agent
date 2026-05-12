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

The extension calls the `inter-agent-pi` and `inter-agent-connect` scripts directly from the project's virtual environment. By default it looks in `/workspace/projects/inter-agent/.venv/bin/`. You can override this path via `settings.json` (see Configuration).

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

## Configuration

You can override the default inter-agent project path in your Pi `settings.json`:

```json
{
  "interAgent": {
    "projectPath": "/workspace/projects/inter-agent"
  }
}
```

Project settings (`.pi/settings.json`) override global settings (`~/.pi/agent/settings.json`).

## Example Workflow

1. Start the server in another terminal:
   ```bash
   cd /workspace/projects/inter-agent
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

## User Acceptance Test

To verify the extension works end-to-end:

1. **Install the extension** (one time):
   ```bash
   pi install /workspace/projects/pi/pi-inter-agent
   ```

2. **Start the inter-agent server** (in a separate terminal):
   ```bash
   cd /workspace/projects/inter-agent
   uv run inter-agent-server
   ```

3. **Start Pi with the extension**:
   ```bash
   pi -e /workspace/projects/pi/pi-inter-agent/src/index.ts
   ```

4. **Run these commands in Pi** and confirm each works:
   - `/inter-agent-status` → should show "State: available"
   - `/inter-agent-connect test-agent` → should show "connected"
   - `/inter-agent-list` → should show "no agents connected" (or your own session)
   - `/inter-agent-send test-agent "hello self"` → should show "sent"
   - `/inter-agent-broadcast "test broadcast"` → should show "sent"
   - `/inter-agent-disconnect` → should show "disconnected"
   - `/inter-agent-shutdown` → should show "server stopped" (and other sessions disconnect)

5. **Verify incoming messages**: In another terminal, connect a second agent and send a message to `test-agent`. You should see a Pi notification.

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
