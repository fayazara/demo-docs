---
title: Introduction
description: What D2 is and how it works as an autonomous DevRel agent on Cloudflare Workers.
---

D2 is an autonomous AI agent designed for Developer Advocacy. It runs 24/7 on Cloudflare's global network, crawling developer communities, generating content, collecting product feedback, and reporting results — all without human intervention.

## What D2 Does

- **Crawls** developer communities across X, Reddit, Hacker News, Discord, GitHub, and Stack Overflow
- **Analyzes** signals using Workers AI for sentiment analysis and trend detection
- **Generates** blog posts, social media threads, and video scripts based on trending topics
- **Collects** product feedback and synthesizes it into structured reports
- **Reports** weekly metrics including content published, community interactions, and top signals

## Built on Cloudflare

D2 is built entirely on Cloudflare's developer platform:

| Service | Role in D2 |
| ------- | ---------- |
| **Workers** | Core compute for each pipeline module |
| **Durable Objects** | Persistent state for tracking, aggregation, and planning |
| **Workers AI** | On-edge inference for analysis and content generation |
| **Service Bindings** | Zero-latency communication between modules |
| **Static Assets** | Frontend serving for the dashboard and marketing site |

## Architecture at a Glance

D2 follows a five-stage pipeline:

1. **Ingest** — Crawl community sources for relevant signals
2. **Parse** — Extract sentiment, topics, and urgency
3. **Classify** — Route to the appropriate handler
4. **Execute** — Generate content, draft responses, or compile reports
5. **Report** — Aggregate metrics and surface top signals

Each stage is a separate Worker. Durable Objects maintain state between executions. The entire system deploys as a set of independent microfrontends connected through a router Worker.

## Next Steps

- Follow the [Quick Start](/guides/quickstart/) to set up a local development environment
- Read the [Architecture Overview](/architecture/overview/) for a deeper technical dive
