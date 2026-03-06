# SoulFire AFK Bot

A highly configurable GitHub Actions workflow that runs a Minecraft AFK bot using SoulFire to stay connected to a server continuously.

## Features

- **Fully Configurable**: 40+ customizable options via workflow_dispatch form
- **Auto-registration & Auto-login**: Works with cracked/offline servers (like Falix)
- **Anti-AFK**: Prevents the server from kicking the bot due to inactivity
- **Auto-reconnect**: Automatically reconnects if the connection is lost
- **Multi-bot support**: Run multiple bot instances simultaneously
- **Headless mode**: Runs in CI environment without display
- **Continuous operation**: Triggers new workflow runs to stay online indefinitely
- **Web UI access**: Optional ngrok tunnel to monitor and control the bot remotely
- **Email notifications**: Get notified when bot runs complete or fail
- **Plugin system**: Auto-jump, auto-eat, auto-armor, auto-respawn, and more

## Requirements

### GitHub Token Setup (Optional but Recommended)

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
   - Name: `GITHUBTOKEN`
   - Value: Paste your PAT

> **Note**: The workflow will still work without a GitHub token, but it won't automatically trigger the next run. You'll need to manually start each run.

### Ngrok Setup (Optional)

To access the SoulFire web UI remotely:

1. **Create a free ngrok account**:
   - Sign up at https://ngrok.com/
   - Get your authtoken from the dashboard

2. **Add ngrok authtoken to your repository**:
   - Go to your repository → Settings → Secrets and variables → Actions
   - Click "New repository secret"
   - Name: `NGROK_AUTH_TOKEN`
   - Value: Paste your ngrok authtoken

> **Note**: Without an authtoken, ngrok will work on the free tier but with limitations (random URLs, connection limits).

## Usage

### Starting the Bot

1. Go to the **Actions** tab in your repository
2. Select the **SoulFire AFK Bot** workflow
3. Click **Run workflow**
4. Configure options in the form (see Configuration Options below)
5. Click **Run workflow**

### How It Works

1. The workflow downloads the latest SoulFireDedicated.jar from GitHub releases
2. Creates a comprehensive command with all your settings
3. Runs the bot in headless mode for the specified duration (default: 340 minutes = 5h40m)
4. Optionally starts an ngrok tunnel for web UI access
5. After the duration completes, it triggers a new workflow run via GitHub API
6. The new run starts fresh, and the cycle continues

### Duration Settings

- **Maximum recommended**: 340 minutes (5 hours 40 minutes)
- **GitHub Actions limit**: 6 hours (360 minutes)
- Leave a 20-minute buffer for cleanup and API calls

## Configuration Options

The workflow provides comprehensive configuration options organized by category:

### Server Connection

| Option | Description | Default |
|--------|-------------|---------|
| `server_address` | Minecraft server address (e.g., play.example.com) | `ahsmpw.falixsrv.me` |
| `server_port` | Server port | `25565` |
| `protocol_version` | Minecraft protocol version (empty for auto-detect) | `` |

### Bot Configuration

| Option | Description | Default |
|--------|-------------|---------|
| `bot_amount` | Number of bot instances | `1` |
| `username` | Bot username (for cracked/offline servers) | `AFKBot` |
| `username_format` | Username format for multiple bots (e.g., Bot%d) | `` |

### Account Settings

| Option | Description | Default |
|--------|-------------|---------|
| `account_type` | Account authentication type (OFFLINE/MICROSOFT/MOJANG) | `OFFLINE` |
| `microsoft_email` | Microsoft account email (for Microsoft auth) | `` |
| `microsoft_password` | Microsoft account password (use secrets for security!) | `` |

### Join Settings

| Option | Description | Default |
|--------|-------------|---------|
| `join_delay_min` | Minimum join delay in seconds (for multiple bots) | `0` |
| `join_delay_max` | Maximum join delay in seconds (for multiple bots) | `5` |

### Anti-AFK Settings

| Option | Description | Default |
|--------|-------------|---------|
| `anti_afk_enabled` | Enable anti-AFK to prevent being kicked | `true` |
| `anti_afk_delay_min` | Minimum anti-AFK action delay in seconds | `30` |
| `anti_afk_delay_max` | Maximum anti-AFK action delay in seconds | `60` |
| `anti_afk_distance_min` | Minimum movement distance for anti-AFK | `1` |
| `anti_afk_distance_max` | Maximum movement distance for anti-AFK | `3` |

### Auto-Reconnect Settings

| Option | Description | Default |
|--------|-------------|---------|
| `auto_reconnect_enabled` | Enable automatic reconnection on disconnect | `true` |
| `auto_reconnect_delay_min` | Minimum reconnect delay in seconds | `5` |
| `auto_reconnect_delay_max` | Maximum reconnect delay in seconds | `10` |

### Auto-Register/Login Settings

| Option | Description | Default |
|--------|-------------|---------|
| `auto_register_enabled` | Enable auto-registration on cracked servers | `true` |
| `auto_login_enabled` | Enable auto-login on cracked servers | `true` |
| `register_password` | Password for auto-register and auto-login | `afkbot123` |
| `register_command` | Register command format (use %password% for password) | `/register %password% %password%` |
| `login_command` | Login command format (use %password% for password) | `/login %password%` |
| `register_timeout` | Register command timeout in milliseconds | `5000` |
| `login_timeout` | Login command timeout in milliseconds | `5000` |

### Client Settings

| Option | Description | Default |
|--------|-------------|---------|
| `client_locale` | Client locale (e.g., en_us) | `en_us` |
| `render_distance` | Client render distance (2-32) | `2` |
| `chat_colors_enabled` | Enable chat colors | `true` |
| `chat_enabled` | Enable chat messages | `true` |
| `listed` | List bot in server player list | `true` |

### Plugin Settings

| Option | Description | Default |
|--------|-------------|---------|
| `auto_jump_enabled` | Enable auto-jump plugin | `true` |
| `auto_eat_enabled` | Enable auto-eat plugin | `true` |
| `auto_eat_hunger_threshold` | Hunger threshold for auto-eat (0-20) | `18` |
| `auto_armor_enabled` | Enable auto-armor equip plugin | `true` |
| `auto_respawn_enabled` | Enable auto-respawn on death | `true` |

### Advanced Settings

| Option | Description | Default |
|--------|-------------|---------|
| `max_tick_rate` | Maximum tick rate | `20` |
| `attack_cooldown` | Attack cooldown in ticks | `0` |
| `follow_path_enabled` | Enable path following | `false` |

### Web UI & Tunneling

| Option | Description | Default |
|--------|-------------|---------|
| `enable_web_ui` | Enable SoulFire web UI | `true` |
| `web_ui_port` | Web UI local port | `8080` |
| `enable_ngrok` | Enable ngrok tunnel for remote access | `false` |
| `ngrok_region` | Ngrok server region (us/eu/ap/au/sa/jp/in) | `us` |

### Email Notifications

| Option | Description | Default |
|--------|-------------|---------|
| `email_enabled` | Enable email notifications | `false` |
| `email_to` | Recipient email address | `` |
| `smtp_server` | SMTP server address (leave empty for system sendmail) | `` |
| `smtp_port` | SMTP server port | `587` |
| `smtp_user` | SMTP username (for authenticated SMTP) | `` |
| `smtp_password` | SMTP password (use secrets for security!) | `` |

### Root Email

| Option | Description | Default |
|--------|-------------|---------|
| `root_email` | Email for SoulFire root user | `root@localhost` |

### Duration

| Option | Description | Default |
|--------|-------------|---------|
| `duration` | Run duration in minutes | `340` |

## Accessing the Web UI

SoulFire provides a web UI to monitor and control your bot. Here's how to access it:

### Local Access (GitHub Actions Runner)
The web UI runs on port 8080 by default inside the GitHub Actions runner. You cannot directly access this from outside.

### Remote Access via Ngrok
To access the web UI remotely:

1. Set `enable_ngrok` to `true` in the workflow inputs
2. Optionally configure `ngrok_region` for better performance
3. Run the workflow
4. The ngrok URL will be displayed in the workflow logs:
   ```
   Web UI URL: https://xxxxxxxx.ngrok-free.app
   ```

⚠️ **Security Warning**: Anyone with the ngrok URL can control your bot! Keep the URL private and monitor access. Consider using short-duration runs or only enabling ngrok temporarily.

### What You Can Do in the Web UI
- View bot status and logs
- Monitor connection health
- Control bot behavior
- View server chat
- Manage plugins
- And more!

## Server Compatibility

### Cracked/Offline Servers
- Set `account_type` to `OFFLINE`
- Enable `auto_register_enabled` and `auto_login_enabled`
- Configure `register_password` for authentication
- Customize `register_command` and `login_command` if needed

### Premium/Online Servers
- Set `account_type` to `MICROSOFT` or `MOJANG`
- Provide Microsoft/Mojang credentials
- Disable `auto_register_enabled` and `auto_login_enabled`
- The bot will authenticate using your account

### Multiple Bots
- Set `bot_amount` to the desired number (e.g., 5)
- Use `username_format` to generate unique usernames (e.g., `Bot%d`)
- Adjust `join_delay_min` and `join_delay_max` to stagger connections
- Note: Premium servers require separate accounts for each bot

## Troubleshooting

### Bot not connecting
- Check the server address and port are correct
- Verify the server is online
- Check protocol version matches server
- Review workflow logs for error messages

### Authentication issues
- For cracked servers: verify auto-register commands match server requirements
- For premium servers: verify Microsoft credentials are correct
- Check account type is set correctly

### Workflow not triggering next run
- Verify `GITHUBTOKEN` secret is set
- Check that the token has `repo` scope
- Look at the Actions tab for any API errors

### Getting kicked for AFK
- Ensure `anti_afk_enabled` is `true`
- Adjust anti-AFK delays to match server timeout settings
- Check SoulFire logs for anti-AFK activity

### Ngrok not working
- Verify `NGROK_AUTH_TOKEN` secret is set
- Check ngrok region is correct
- Review ngrok logs in the workflow output
- Note: ngrok free tier has connection limits

### Email notifications not working
- Verify email address is correct
- For SMTP: check server, port, username, and password
- For sendmail: verify the runner has sendmail installed
- Check workflow logs for email errors

### Web UI not accessible
- Ensure `enable_web_ui` is `true`
- Check `web_ui_port` is not conflicting
- For remote access: ensure `enable_ngrok` is `true`
- Verify ngrok URL is displayed in logs

## Security Considerations

1. **Passwords and Secrets**: Never hardcode sensitive data in the workflow. Use GitHub Secrets:
   - `GITHUBTOKEN` for GitHub API access
   - `NGROK_AUTH_TOKEN` for ngrok authentication
   - SMTP credentials in workflow inputs (use secrets reference syntax)

2. **Web UI Access**: The ngrok URL gives anyone full control of your bot. Only enable when needed and monitor access.

3. **Microsoft Credentials**: When using Microsoft auth, consider using a dedicated Minecraft account rather than your main Microsoft account.

4. **Server Rules**: Ensure your bot complies with the target server's rules and terms of service.

## Advanced Configuration Examples

### Example 1: Single AFK Bot on Cracked Server
```
server_address: myserver.com
username: AFKBot
account_type: OFFLINE
auto_register_enabled: true
auto_login_enabled: true
register_password: mypassword123
anti_afk_enabled: true
duration: 340
```

### Example 2: Farming Bots on Premium Server
```
server_address: farming.hypixel.net
bot_amount: 3
username_format: Farmer%d
account_type: MICROSOFT
microsoft_email: mybot@email.com
microsoft_password: ${{ secrets.MC_PASSWORD }}
auto_jump_enabled: true
auto_eat_enabled: true
auto_respawn_enabled: true
duration: 300
```

### Example 3: Monitoring Bot with Web UI
```
server_address: monitoring.mc-server.net
username: MonitorBot
account_type: OFFLINE
enable_web_ui: true
enable_ngrok: true
ngrok_region: us
auto_reconnect_enabled: true
duration: 340
```

## License

This project is provided as-is for educational and personal use.
