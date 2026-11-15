---
title: "Configuration"
nav_order: 3
---

# Configuration

Verdant works as installed. Four theme settings let you brand it and turn the player list off; the player list
has its own settings in the admin area, and Verdant uses a few of the panel's own settings.

## Theme settings

Open **Admin › Themes** and click **Settings** on the Verdant card. Save, then reload the page; settings apply to
every user on their next page load.

![Verdant settings]({{ '/assets/screenshots/11-theme-settings.png' | relative_url }})

### Accent colour

| | |
| --- | --- |
| Default | Empty: Verdant green |
| Accepts | A hex colour (`#3b82f6`) from the picker or typed, or an `oklch()` colour |
| Affects | Buttons, links, the navigation highlight, focus rings, charts, the terminal cursor, the selection colour and the leaf mark |

Verdant keeps your colour's **hue** and uses its saturation as a ceiling, but sets the lightness of every shade
itself, separately for dark and light. So the colour on screen is a shade of yours, chosen to keep text readable in
both schemes; a very pale or greyish colour gives a muted palette. Clear the field and save to go back to green.
Colours with a meaning do not change: "Running" and other success states stay green, warnings amber, errors red,
and the console's own ANSI colours stay as the game prints them.

### Logo

| | |
| --- | --- |
| Default | None: the leaf mark and the panel name |
| Accepts | SVG, PNG, JPEG, WebP, AVIF or GIF, up to 10 MB |
| Shown | At the top of the sidebar, 28 px high, in place of the leaf and the name |

Upload stores the image at once; **Remove** brings the leaf and name back. A wide logo with transparent background
works best, and should read on both a dark and a light background. The browser tab keeps the panel name.

With Docker, add the volume from [Requirements](installation.md#requirements) first, or the logo is lost when the
container is recreated. Or use **Logo URL** instead.

### Logo URL

| | |
| --- | --- |
| Default | Empty |
| Accepts | An `https://` address of an image, up to 2048 characters |
| Shown | Like an uploaded logo, which it replaces while it is filled |

The image stays where you host it, so nothing is stored on the panel and Docker needs no extra volume. Every
visitor's browser loads it from that address. If it does not load, the sidebar shows the leaf and the panel name.
Addresses that do not start with `https://` are ignored.

### Player list

| | |
| --- | --- |
| Default | On |
| On | The Players tile in the console, player counts on the dashboard cards and in the server switcher, the Player list tile on the admin overview, **Admin › Player list**, and the **Player list** tab of every egg and server. |
| Off | All of the above are gone: no menu entry, no tab, and the pages answer 404. The browser makes no player list requests, and the panel's player endpoints answer 404. |

Turn it off when another extension already shows players, or when you don't want the feature.

If you had set up the agent, remove it first on **Admin › Player list**: the page disappears with the setting.
An agent left running while the setting is off gets an empty list from the panel and queries nothing. Turning the
setting back on restores everything as it was, including the egg and server game settings.

## Player list

Set up and tuned on **Admin › Player list**, and per egg and per server on their **Player list** tabs. See
[Player list](player-list.md).

## Panel settings Verdant uses

| What | Where |
| --- | --- |
| The name in the sidebar (when no logo is set) and the browser tab | The panel's company name: **Admin › Settings › General** |
| The default colour scheme | Dark. Each user switches between dark and light with the sun and moon button in the top bar; the browser remembers the choice. |
| Who sees which servers | The panel's own permissions. Verdant shows only what the stock panel would show the same user. |
| Language | Verdant's own texts are in English for now. More languages will be added if customers need them. |

## What Verdant changes

Verdant replaces these parts of the panel with its own. Everything else is the panel's own page, restyled by the
theme's colours, type and components.

| Part | |
| --- | --- |
| App shell | Sidebar with the panel name, area navigation, server switcher and user; top bar with breadcrumbs and the scheme button |
| Server console | Terminal filling the window, power buttons, resources column (CPU, memory, network, players, disk, uptime) |
| Dashboard | Server cards with state, address, CPU, memory, disk and players; search; "Show others' servers" for admins |
| Server activity log and startup variables | Restyled lists |
| Admin overview (`/panel`) | A tile per admin section with its count |
| Admin › Nodes › a node › About | Status, allocated memory and disk against the node limit and overallocation, Wings and system, delete |

Other admin node tabs (Settings, Configuration, Allocation, Tags, Servers) are the panel's own pages: the panel has
no way for a theme to replace them.

Extensions that add pages, tabs or slots keep working: Verdant renders the panel's slots and extension navigation.
