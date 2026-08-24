# umbrel-app

Umbrel packaging for [p2poolv2](https://github.com/p2poolv2/p2poolv2) — a peer-to-peer Bitcoin mining pool that lets miners coordinate directly and verify their own rewards, with no centralized pool operator.

Adding this store to your Umbrel lets you install p2poolv2 the same way you'd install any other app — no manual Docker setup, no editing files by hand. It connects automatically to your existing Bitcoin Node app on Umbrel.

## What's in this repo

- `umbrel-app-store.yml` — this store's identity
- `p2poolv2/` — the actual app package: `docker-compose.yml`, `umbrel-app.yml`, and icon assets

The Docker image this points to is built from the [p2poolv2 repo](https://github.com/p2poolv2/p2poolv2)'s own Dockerfile and published publicly on Docker Hub — no special access needed to pull it, Umbrel does this automatically on install.

## Installing on a real Umbrel

If you already have a real Umbrel device or umbrelOS running somewhere:

1. Open your Umbrel dashboard
2. Go to **Settings → Community App Stores**
3. Add this URL: `https://github.com/p2poolv2/umbrel-app`
4. Install **P2Poolv2** like any other app

That's it — no cloning, no commands.

## Testing this without a real Umbrel device

You don't need a VM or physical Umbrel hardware. `getumbrel/umbrel` has a built-in dev environment that runs a real, working umbrelOS instance inside a single Docker container, directly on your machine.

**Requirements:** Docker, Node.js 22+

\`\`\`bash
git clone https://github.com/getumbrel/umbrel.git
cd umbrel
npm install
npm run dev start
\`\`\`

First run builds the full umbrelOS image from scratch — this takes a while (several hours is normal). Subsequent runs reuse Docker's build cache and are much faster. Keep this running in a terminal you won't accidentally close — closing it kills the build.

Once it's running, get the container's IP:

\`\`\`bash
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' umbrel-dev
\`\`\`

Open that IP in your browser, complete the first-time setup, then follow the same steps as above (Settings → Community App Stores → add the URL → install).

From there, installing P2Poolv2 will offer to install **Bitcoin Node** as a dependency if you don't already have one — Umbrel automatically wires up RPC credentials and ZMQ between the two apps, no manual config needed.

## Current status

- Installs cleanly, dashboard loads, connects to Bitcoin Node automatically (RPC + ZMQ) with zero manual configuration
- Handles the Bitcoin node still syncing (initial block download) gracefully once the corresponding fix in p2poolv2 core is merged and the image is rebuilt
- Payout address (`solo_address`) is not yet configurable per-install — still an open design question, tracked in discussion with the p2poolv2 team

## Questions / issues

Open an issue on this repo, or reach out in the [p2poolv2 Matrix room](https://github.com/p2poolv2/p2poolv2#readme).
