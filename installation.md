---
title: "Installation"
nav_order: 2
---

# Installation

Verdant is one package, `verdant-<version>.pteroext`. In Pterodactyl 2.0 a theme is an extension of type `theme`,
so the panel lists Verdant in two places:

- **Admin › Extensions**: every installed package. Here you enable, disable and remove it.
- **Admin › Themes**: the themes among them. Here you make Verdant the look of the panel, try it on yourself first,
  and change its [settings](configuration.md).

The player list is part of the same package, not a separate extension. It can be turned off in the theme's settings.

## Requirements

- Pterodactyl Panel **2.0** (tested on 2.0.0-beta.2). Panel 1.x is not supported.
- Shell access to the panel, to run `php artisan`. With the official Docker image, run the commands inside the panel
  container, for example `docker compose exec panel php artisan …`.
- The panel's `extensions` directory must be writable by the user that runs the commands.
- With Docker, only if you upload a [logo](configuration.md#logo): the official compose file does not keep uploaded
  files when the container is recreated. Add this volume to the panel service, then `docker compose up -d`:

  ```yaml
  - "/srv/pterodactyl/extension-files:/app/storage/app/private/extension-files"
  ```
- For the player list only: a node with one free allocation for the agent server, and Wings able to pull
  `ghcr.io/parkervcp/yolks:nodejs_22`. See [Player list](player-list.md#requirements).

Nothing changes on Wings or on your game servers. Verdant works with any egg.

## Install

1. Get the package. Copy the install command from the extension's page or **Your purchases** on
   [pterodactyl.io](https://pterodactyl.io/marketplace) (its link works for 15 minutes), or download the
   `.pteroext` file from BuiltByBit and copy it to the panel host.
2. In the panel directory:

   ```sh
   php artisan p:extension:install "<link or path to the .pteroext>" --enable
   ```

   It prints `Installed Verdant (verdant) v<version> - enabled.`
3. Open **Admin › Themes**. Click **Preview for me** on the Verdant card: only you see Verdant, everyone else keeps
   the current theme. Look around, then click **Activate** to switch everyone. From the command line:

   ```sh
   php artisan p:theme:activate verdant
   ```

4. Optional: add your [logo and accent colour](configuration.md#theme-settings) in **Admin › Themes › Verdant ›
   Settings**.
5. Optional: set up the [player list](player-list.md#set-up), or turn it off in the
   [theme settings](configuration.md#player-list) if you use another extension for it.

`php artisan p:extension:list` shows `verdant` as enabled, and `php artisan p:theme:list` shows it as active.

## Update

Install the new package over the old one:

```sh
php artisan p:extension:install "<link or path to the .pteroext>" --enable --replace
```

The theme stays active. Theme settings, the player list agent, its token and every egg and server game setting are
kept. When a release ships a newer agent, **Admin › Player list** offers **Update**.

## Something looks wrong

- **Safe mode** shows the built-in Classic theme to you only, without changing anything for others: open
  `https://<your panel>/?safe_mode=1`, or click **Safe mode** on **Admin › Themes**. Leave it with **Leave safe mode**.
- To put everyone back on Classic: **Activate** on the Classic card in **Admin › Themes**, or
  `php artisan p:theme:reset`.
- After an update, reload the page once if the browser still shows the old version.

## Remove

1. If you set up the player list, open **Admin › Player list** and click **Remove**. It deletes the agent server
   and, when nothing else uses it, its egg.
2. Switch the panel back to Classic: `php artisan p:theme:reset`.
3. Remove the package:

   ```sh
   php artisan p:extension:remove verdant
   ```

The panel keeps the theme's saved settings in its `extension_settings` table (rows with `extension = 'verdant'`),
so a later reinstall picks them up. Delete those rows if you want nothing left behind. Removing Verdant never
changes a game server.
