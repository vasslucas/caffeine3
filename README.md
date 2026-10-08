# Caffeine

A self-hosted browser workspace with local encrypted profiles, a customizable
home canvas, and Scramjet-powered browsing. Built with React, TypeScript, Vite,
and Tailwind CSS.

## Make it yours

- The home screen greets you by time of day, with a compact quick-travel row.
- **Customize caffeine** is docked in the bottom-right. In edit mode, drag
  elements or use arrow keys (Shift for larger steps), then adjust their type,
  color, size, and position in the editor.
- Choose **Add widget** in the top-left to add notes, tasks, a focus timer,
  breathing guide, calendar, world clocks, countdown, calculator, hydration
  tracker, mood check-in, dice, quick link, text card, daily thought, gentle
  reminder, or day-progress meter. None are added by default.
- Notes, tasks, and widget values stay on this device, scoped to your profile.
  Remove a widget from the editor when you no longer want it on your canvas.
- The **femboy-ify!** slider switches to an alternate pink, heart-accented layout.
  Turn it off to restore the normal layout. Your choice is saved with your
  profile; its small disclaimer appears once per browser installation.
- Typeface choices preview their actual fonts. Google Fonts need a network
  connection; system fonts depend on what is installed on your device.

## Local profiles and privacy

First-run onboarding walks through your profile, wallpaper, and preferences
with a live preview. Profile PINs/passwords protect the locally encrypted login
vault, not proxy access. Profiles are not isolated browser accounts: proxied
site cookies share this origin. Do not use the proxy origin for unrelated apps.
No profile recovery or cloud sync is provided. Clearing site data removes local
profiles, widget contents, and saved customization.

## Run the browser

```sh
pnpm install
pnpm build:host
pnpm start
```

The production server defaults to port 8080 and honors `PORT`. Use a dedicated
HTTPS origin (or localhost) so the proxy service worker can control browser tabs.
`pnpm dev` serves the interface only; it does not run the Wisp WebSocket backend.

On Railway or another public host, set `HOST=0.0.0.0` and `NODE_ENV=production`.
No proxy password is required. Leave `PROXY_PASSWORD` unset to allow browsing
without signing in. Anyone who can reach the host can then use its proxy and
consume its bandwidth. For optional password protection, set `PROXY_PASSWORD`
and unlock server access in Settings → Connection. Your reverse proxy must
forward WebSocket upgrades for `/wisp/`.

## Proxy routing

Scramjet v2 and its separate controller intercept frame-specific `/p/` requests
and rewrite website HTML, scripts, and assets. Epoxy v3 sends network requests
through the bundled `/wisp/` server, avoiding browser CORS restrictions.
Scramjet v2 is currently an alpha release, pinned to `2.0.67-alpha.2` with matching
controller `0.0.14`. Reopen old tabs after upgrading; v1 proxy URLs are not compatible.
Install and build scripts synchronize the browser assets from the installed packages.
If the service worker is absent, the server returns an explicit 503
instead of serving the Caffeine application inside its own tab.

After updating, reload the top-level Caffeine page to activate the new worker.
If an older worker persists, unregister it in your browser's Application →
Service Workers tools and reload. Static hosting can show the interface but
cannot provide the bundled Wisp backend.
