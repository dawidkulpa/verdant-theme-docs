---
title: "Player list"
nav_order: 4
---

# Player list

The Verdant theme shows who is playing on each game server: a Players tile in the console's resources
column, and an "online/max players" line on the dashboard cards. The counts come from
[GameDig](https://github.com/gamedig/node-gamedig), which speaks the query protocols of 300+ games.

## How it works

```
game servers  <--- GameDig queries ---  agent (a Pterodactyl server)  --- HTTPS --->  panel
                                                                                        |
                         browsers read the panel's cached results  <--------------------+
```

- The **agent** is a small Node.js program that runs as an ordinary Pterodactyl server, created from the
  theme's own egg. Every round it asks the panel which servers to query, queries them, and sends the
  results back. It opens no port; it only connects out to the panel and to the game servers.
- The **panel** keeps the latest result per server in its cache. Browsers read that cache, so opening a
  console or the dashboard never queries a game. Results older than three rounds are shown as stale.
- Only administrators configure it. Server owners see the results, never the settings.
- It can be turned off as a whole in the theme's settings: see [Turning it off](#turning-it-off).

## Requirements

- Pterodactyl panel 2.0 with the Verdant theme installed and active.
- One node with a free allocation for the agent server. The agent needs the allocation only because
  Pterodactyl requires one; nothing listens on it.
- The node must resolve and reach the panel URL (`APP_URL`), and reach the game servers' ports over UDP/TCP.

## Set up

1. Open **Admin > Player list**.
2. Pick the node for the agent and click **Set up**. The panel imports the "Verdant Player List" egg, creates
   the agent server (256 MiB memory, 512 MiB disk, image `ghcr.io/parkervcp/yolks:nodejs_22`), installs it
   and waits for its first report. This takes about a minute; you can leave the page.
3. Tell the panel what each game is:
   - **Admin > Eggs > (egg) > Player list**: the game for every server of that egg.
   - **Admin > Servers > (server) > Player list**: keep the egg's setting, choose another game, or
     "Don't show players" for that server.

Changes apply from the agent's next round. The query interval (10–600 s, default 30 s) is on
Admin > Player list.

### Query port

The agent asks the address the server's primary allocation is bound to; a server bound to every address
(`0.0.0.0`) is asked at its allocation alias, then at its node's address. The port is one of:

| Setting | Port |
| --- | --- |
| Server's primary port | The primary allocation's port |
| Primary port + offset | Primary port plus a fixed offset (-1000 to 1000); picking a game fills in GameDig's offset when it has one |
| From a startup variable | The value of a startup variable, such as `QUERY_PORT` |

Use the port the game answers queries on. It can differ from the port players connect to, and that port
must be an allocation of the server, or Wings does not forward it.

## Managing the agent

| Action | What happens |
| --- | --- |
| **Update** | Shown when the theme ships a newer agent. The agent server is reinstalled and started again. |
| **Regenerate token** | The old token stops working at once; the agent server gets the new one and restarts. |
| **Remove** | Deletes the agent server and its egg (when nothing else uses the egg). Egg and server game settings stay. |
| **Query interval** | Seconds between rounds. Results are kept for six rounds (at least three minutes). |

Deleting the agent server on the Servers page has the same effect as Remove; Admin > Player list then
offers a fresh setup.

## Turning it off

If another extension already shows players, or you don't want the feature, turn it off: **Admin › Themes ›
Verdant › Settings › Player list**. The Players tile, the dashboard counts, **Admin › Player list** and the egg and
server **Player list** tabs all disappear, and the player endpoints answer 404. Remove the agent first (see
[Managing the agent](#managing-the-agent)); an agent left running gets an empty list and queries nothing.
Turning it back on restores everything, including the egg and server game settings.

## Supported games

Every game GameDig 5.3.3 knows: the game picker lists them, searchable by name or GameDig id. What the panel
shows depends on the game:

- Most games answer with the player count, the slot count and the map. Many also send player names, which
  the console lists.
- Counter-Strike 2 sends no player names; the console shows the count only.
- Games whose query needs credentials, such as Palworld (REST API with the admin password), are not
  supported: the agent never sends passwords, so these show "No answer".
- Valve-protocol games (Counter-Strike 2, Project Zomboid, Rust, ARK and others) answer only if their Steam
  query port is reachable. Project Zomboid, for one, answers on its Steam port, which must be an allocation
  of the server.

## Troubleshooting

**"Agent isn't reporting"** on Admin > Player list:

- Start the agent server if it is stopped (its console shows each round: `queried 3 server(s), 2 answered`).
- The agent server's console names the problem: `401` means a wrong token (use Regenerate token), `404`
  means the theme is not installed or not active, `429` means the panel throttled it (raise the interval).
- `cannot reach the panel` or `ENOTFOUND`: the node cannot resolve or reach `APP_URL`. Wings gives containers
  the DNS servers from `docker.dns` in `/etc/pterodactyl/config.yml`, which default to `1.1.1.1` and
  `1.0.0.1`. A panel name that only exists on your LAN needs a LAN resolver there, then a Wings restart.

**"Install failed"**: the node could not download the agent from the panel, or the download did not match
the expected checksum. The agent server's console has the install log. Fix the network, then click Reinstall.

**A game shows "No answer"**: query it from the node with the same game id and port, for example
`npx gamedig --type counterstrike2 192.0.2.10:27015`. If that fails too, the port or the game's query
setting is wrong, not the panel.

## Security

- The agent authenticates with a 48-character random token. The panel stores only its SHA-256 hash; the
  token itself lives in the agent server's `AGENT_TOKEN` variable, hidden from the server's owner.
- The agent can read only what it needs: the list of servers to query (id, game, address, port) and its own
  code. It can write only results for servers on that list.
- The agent endpoints (`/extensions/verdant/players/*`) are rate-limited to 30 requests a minute.
- Users see results only for servers they can access; the dashboard endpoint filters by access, and the
  console endpoint answers 404 for anyone else.

## Licences

GameDig is MIT-licensed; some of its dependencies use other open-source licences, Apache-2.0 among them. The
bundled agent keeps the licence comments of GameDig and its dependencies at the end of `agent.mjs`.
