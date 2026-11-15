---
title: "Overview"
nav_order: 1
permalink: "/"
---

# Verdant

A theme for Pterodactyl Panel 2.0: a sidebar layout that uses the whole window, a full-height console with live
resources beside it, dark and light schemes in Verdant green or your own colour and logo, and quiet motion
throughout. A built-in player list shows who is playing on each game server, for 300+ games through
[GameDig](https://github.com/gamedig/node-gamedig).

- [Installation](installation.md): install, activate, update, remove.
- [Configuration](configuration.md): logo, accent colour, the player list switch, the panel settings it uses.
- [Player list](player-list.md): set up the agent, choose the game per egg or server, troubleshooting.
- [Changelog](changelog.md)

## A tour

**Console.** The terminal fills the window. The resources column beside it shows CPU, memory and network with
20-second charts, the players online with their names, disk and uptime. Each tile folds away. On a phone the column
becomes a strip of three values that opens the tiles in a panel.

![Console]({{ '/assets/screenshots/01-console.png' | relative_url }})

**Dark and light.** Every screen in both schemes; each user switches with the button in the top bar.

![Dashboard, light]({{ '/assets/screenshots/04-dashboard-light.png' | relative_url }})

**Dashboard.** A card per server with its state, address (click to copy), CPU, memory, disk and players. Cards
reflow to the width of the window. Admins can show other users' servers.

![Dashboard]({{ '/assets/screenshots/03-dashboard.png' | relative_url }})

**Server switcher.** From any server page, jump to another server; each shows its state or players.

![Server switcher]({{ '/assets/screenshots/05-switcher.png' | relative_url }})

**Search.** The panel's server search, opening and closing with the same motion as every other dialog.

![Search]({{ '/assets/screenshots/06-search.png' | relative_url }})

**Admin overview.** A tile per admin section with its count, the panel version and whether an update is out.

![Admin overview]({{ '/assets/screenshots/07-admin-overview.png' | relative_url }})

**Node details.** A node's About tab shows its status, allocated memory and disk against the node limit and its
overallocation ceiling, the Wings version and system, and when the node can be deleted.

![Node details]({{ '/assets/screenshots/08-node.png' | relative_url }})

**Player list.** One small agent, set up from **Admin › Player list** in a click, asks every game server who is
online. Pick the game once per egg; override it per server.

![Player list]({{ '/assets/screenshots/09-player-list-admin.png' | relative_url }})

![Egg player list]({{ '/assets/screenshots/10-egg-player-list.png' | relative_url }})

**Your brand.** Set your logo and accent colour in **Admin › Themes › Verdant › Settings**; every screen follows,
in dark and light.

![Your logo and colour]({{ '/assets/screenshots/13-brand.png' | relative_url }})

**Phones.** Every screen adapts to a phone (tested at 390 px wide).

![Phone]({{ '/assets/screenshots/12-phone.png' | relative_url }})

## Support

Support channels, and what to include in a report, are on the product page.

## Credits

- [GameDig](https://github.com/gamedig/node-gamedig) (MIT) answers the player queries; the agent bundles it with
  its licence notices.
- [Geist and Geist Mono](https://vercel.com/font) (SIL Open Font License 1.1) are the theme's typefaces; the licence
  ships in the package.
- Icons: [Lucide](https://lucide.dev) (ISC), through the panel.
