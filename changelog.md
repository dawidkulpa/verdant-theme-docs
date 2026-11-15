---
title: "Changelog"
nav_order: 6
---

# Changelog

## 0.5.0 (2026-11-15)

First public release.

- **Accent colour.** New theme setting. The whole palette (buttons, links, navigation, charts, terminal accents,
  the leaf mark) takes the colour's hue in both schemes, with lightness tuned per scheme for contrast. Empty means
  Verdant green, exactly as before. States keep their meaning: "Running", "Online" and "Connected" stay green,
  warnings amber, errors red.
- **Logo.** New theme setting: an uploaded image replaces the leaf and the panel name in the sidebar.

## 0.4.0 (2026-11-15)

- **Player list on/off.** New theme setting, **Admin › Themes › Verdant › Settings › Player list** (on by default).
  Off removes the Players tile, the dashboard and switcher counts, the admin overview tile, **Admin › Player list**
  and the egg and server **Player list** tabs; the player endpoints answer 404 and an agent left running queries
  nothing.
- **Console.** The Inbound and Outbound rates show their unit small beside the number, so values such as
  `253.68 KiB/s` are no longer cut off. On phones the resource strip shows used memory without the limit, which
  did not fit.
- **Dashboard.** On narrow cards (phones) a resource's limit moves to its own line instead of being cut off.

## 0.3.0 (2026-11-14)

- Admin › Nodes › a node's **About** tab: status, allocated memory and disk against the node limit and its
  overallocation ceiling, Wings version and system, and delete.

## 0.1.0 (2026-10-24)

- Sidebar shell, full-window console with the resources column, dashboard cards, server switcher, admin overview,
  dark and light schemes, and the GameDig player list.

Versions before 0.5.0 were not distributed.
