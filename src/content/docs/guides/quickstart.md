---
title: Quick Start
description: Set up D2 locally and deploy your first module to Cloudflare Workers.
---

This guide walks you through setting up D2's development environment and deploying the router Worker with microfrontend routing.

## Prerequisites

- [Node.js](https://nodejs.org/) v18 or later
- [pnpm](https://pnpm.io/) package manager
- A [Cloudflare account](https://dash.cloudflare.com/sign-up) with Workers enabled
- [Wrangler CLI](https://developers.cloudflare.com/workers/wrangler/) installed globally

```bash
npm install -g wrangler
wrangler login
```

## Project Structure

D2 is organized as a monorepo with each microfrontend as a separate directory:

```
d2/
├── website/        # Marketing landing page (Astro)
├── blog/           # Engineering blog (Astro)
├── docs/           # Documentation (Astro Starlight)
├── dashboard/      # Monitoring dashboard (React + shadcn/ui)
└── router/         # Router Worker (microfrontend routing)
```

## Install Dependencies

```bash
# Install dependencies for all projects
pnpm install --recursive
```

## Local Development

Each microfrontend can be developed independently:

```bash
# Start the marketing site
cd website && pnpm dev

# Start the blog
cd blog && pnpm dev

# Start the docs
cd docs && pnpm dev

# Start the dashboard
cd dashboard && pnpm dev
```

## Deploy

Deploy each Worker independently:

```bash
# Deploy the marketing site
cd website && npx wrangler deploy

# Deploy the blog
cd blog && npx wrangler deploy

# Deploy the router (connects all microfrontends)
cd router && npx wrangler deploy
```

The router Worker uses Service Bindings to connect to each microfrontend Worker. Once deployed, all apps are accessible from a single domain:

- `/` — Marketing site
- `/blog` — Engineering blog
- `/docs` — Documentation
- `/dashboard` — Monitoring dashboard

## Next Steps

- Read the [Architecture Overview](/architecture/overview/) to understand how the modules connect
- Check the [Configuration Reference](/reference/configuration/) for router and module settings
