# ER RUSH — multiplayer Netlify project

ER RUSH is a browser-based, cooperative emergency-room triage game for 2–4 players. All patients and treatment choices are fictional game scenarios. This is entertainment software, not medical advice, clinical training, or a real patient-care tool.

## Project contents

- `index.html` — responsive game UI, lobby, treatment beds, timers, scoring, team chat, and shift results.
- `netlify/functions/game.ts` — room creation/joining, player sessions, shared state, and game actions.
- `netlify.toml` — Netlify publish directory and function configuration.
- `package.json` — build and development scripts.
- `scripts/check-frontend.mjs` — dependency-free frontend JavaScript syntax check.

## Local checks

Requirements: Node.js 20 or newer and npm.

```sh
npm install
npm run check:frontend
npm run typecheck
npm run build
```

`npm install` must complete successfully before the TypeScript check and Netlify production build can run. A network-restricted environment may be unable to download the required npm packages.

## Deploy

1. Extract this folder and push its contents to a Git repository.
2. In Netlify, create a site by importing that repository. The repository root should be the `er-rush-netlify` folder.
3. Use the build command `npm run build` and publish directory `.` (already specified in `netlify.toml`).
4. Confirm Netlify Functions are enabled and that the site can use Netlify Blobs. Deploy the site.
5. Test with two separate browsers or devices: create a room, join with the four-digit code, start the shift, and verify that both screens update.

Do not use a plain static-hosting upload as the final deployment method: the serverless function and Blobs storage are required for multiplayer.

## Known limitations that must be tested before calling this production-ready

- **Dependency installation and production build:** not verified in the current environment because npm network access timed out. The generated ZIP intentionally does not include a fake or incomplete lockfile; run `npm install` in a network-enabled environment and commit the generated `package-lock.json` for reproducible installs.
- **Concurrent state writes:** Netlify Blobs is used as shared storage, but it does not provide a transaction/compare-and-swap operation in this implementation. Simultaneous player actions can still race and overwrite each other. For robust production multiplayer, move authoritative room state to a transactional database or add a correctly implemented distributed coordination layer before relying on high-concurrency play.
- **Host disconnect:** the same browser session can rejoin using its session token. Automatic host transfer to another player is not implemented.
- **Live testing:** cross-device synchronization, simultaneous treatment actions, reconnect behaviour, and a complete shift must be tested against a deployed Netlify site. No live deployment or public URL is included with this source package.

## Safety and privacy

Use fictional names only. Do not enter real patient information or other sensitive personal data. The game scenarios are simplified and must not be used to make real medical decisions.
