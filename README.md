# SoulFire AFK Bot

A GitHub Actions workflow that runs a Minecraft AFK bot using SoulFire to stay connected to a server continuously.

## Features

- **Auto-registration & Auto-login**: Works with cracked/offline servers (like Falix)
- **Anti-AFK**: Prevents the server from kicking the bot due to inactivity
- **Auto-reconnect**: Automatically reconnects if the connection is lost
- **Web UI access**: LocalTunnel integration to access SoulFire's web interface remotely
- **Continuous operation**: Triggers new workflow runs to stay online indefinitely
- **Configurable duration**: Run for up to 5h40m (340 minutes) before re-triggering

## Requirements

### GitHub Token Setup

To enable the automatic re-triggering feature (the "infinite loop"), you need to configure a GitHub token:

1. **Create a Personal Access Token (PAT)**:
   - Go to GitHub Settings → Developer settings → Personal access tokens → Tokens (classic)
   - Click "Generate new token (classic)"
   - Give it a name (e.g., "SoulFire AFK Bot")
   - Select scopes: `repo` (full control of private repositories)
   - Click "Generate token" and copy the token

2. **Add the token to your repository**:
   - Go to your repository → Settings → Secrets and variables → Actions
   - Click "New repository secret"
   - Name: `GITHUB_TOKEN`
   - Value: Paste your PAT

> **Note**: The workflow will still work without a GitHub token, but it won't automatically trigger the next run. You'll need to manually start each run.

## Usage

### Starting the Bot

1. Go to the **Actions** tab in your repository
2. Select the **SoulFire AFK Bot** workflow
3. Click **Run workflow**
4. Optionally customize:
   - **Username**: The bot's in-game name (default: `AFKBot`)
   - **Server**: The server address (default: `ahsmpw.falixsrv.me`)
   - **Password**: Password for auto-register/login on cracked servers (default: `afkbot123`)
   - **Duration**: How long to run in minutes (default: `340`)
   - **Enable Tunnel**: Enable LocalTunnel to access SoulFire's web UI (default: `true`)

5. Click **Run workflow**

6. After the workflow starts, look for the tunnel URL in the logs (e.g., `https://abc123.loca.lt`)
7. Click the URL to access the SoulFire web interface

### How It Works

1. The workflow downloads the latest SoulFireDedicated.jar from GitHub releases
2. Installs LocalTunnel to expose the SoulFire web UI (if enabled)
3. Creates a configuration file with your settings
4. Starts LocalTunnel pointing to port 38765 (SoulFire's web UI port)
5. Runs the bot for the specified duration (default: 340 minutes = 5h40m)
6. After the duration completes, it triggers a new workflow run via GitHub API
7. The new run starts fresh, and the cycle continues

### Duration Settings

- **Maximum recommended**: 340 minutes (5 hours 40 minutes)
- **GitHub Actions limit**: 6 hours (360 minutes)
- Leave a 20-minute buffer for cleanup and API calls

## Configuration Options

| Option | Description | Default |
|--------|-------------|---------|
| `username` | Bot's username (cracked server) | `AFKBot` |
| `server` | Server address | `ahsmpw.falixsrv.me` |
| `password` | Password for auto-register and auto-login | `afkbot123` |
| `duration` | Run time in minutes | `340` |
| `enable_tunnel` | Enable LocalTunnel for web UI access | `true` |

## LocalTunnel Web UI Access

When `enable_tunnel` is set to `true`, the workflow will:

1. Install LocalTunnel via npm (no auth token required)
2. Start LocalTunnel in the background, pointing to port 38765
3. Display a public URL (e.g., `https://abc123.loca.lt`) in the workflow logs
4. Automatically clean up the tunnel when the workflow ends

The SoulFire web UI allows you to:
- View real-time bot status
- Monitor server connection
- Check bot position and inventory
- Access bot logs
- Manually control the bot (if enabled)

> **Note**: LocalTunnel provides a free service without requiring authentication, making it easier to set up than ngrok. The tunnel URL changes on each workflow run.

## Server Compatibility

This bot is configured for:
- **Cracked/Offline servers**: Auto-register and auto-login enabled
- **Premium servers**: Set `autoRegister` and `autoLogin` to `false` in the config

## Troubleshooting

### Bot not connecting
- Check the server address is correct
- Verify the server is online
- Make sure the port (25565) is accessible

### Workflow not triggering next run
- Verify `GITHUB_TOKEN` secret is set
- Check that the token has `repo` scope
- Look at the Actions tab for any API errors

### Getting kicked for AFK
- The anti-AFK feature should prevent this
- Check SoulFire logs for any issues

### LocalTunnel not working
- Check that `enable_tunnel` is set to `true` in the workflow inputs
- Look for the tunnel URL in the "Run SoulFire bot" step logs
- If the URL is not displayed, check the `tunnel.log` output in the workflow run
- LocalTunnel may be temporarily unavailable - wait a moment and try again
- The tunnel URL changes on each workflow run, so you'll need to get the new URL each time

## License

This project is provided as-is for educational and personal use.
