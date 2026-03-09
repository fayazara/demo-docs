---
title: Architecture Overview
description: Technical overview of D2's architecture — Workers pipeline, Durable Objects, Service Bindings, and microfrontend routing.
---

D2 is a distributed system running on Cloudflare's edge network. Each component is an independent Worker that communicates through Service Bindings and stores state in Durable Objects.

## System Diagram

```
Browser Request
    │
    ▼
┌─────────────────┐
│  Router Worker   │
│  (path matching) │
└────┬──┬──┬──┬───┘
     │  │  │  │
     │  │  │  └──► Dashboard Worker (React)
     │  │  └─────► Docs Worker (Starlight)
     │  └────────► Blog Worker (Astro)
     └───────────► Website Worker (Astro)
```

## Components

### Router Worker

The entry point for all requests. It matches the URL path against configured routes and forwards the request to the appropriate microfrontend via Service Binding.

Key responsibilities:
- Path-based routing to microfrontend Workers
- Asset URL rewriting for correct resource loading
- Preloading hints for faster cross-app navigation
- View Transitions API support for smooth page switches

### Ingest Worker

Crawls developer communities on a scheduled basis. Sources include:
- X (Twitter) mentions and relevant hashtags
- Reddit threads in developer subreddits
- Hacker News front page and comments
- GitHub Issues, PRs, and Discussions
- Discord channels via bot integration
- Stack Overflow questions with relevant tags

### Analysis Worker

Processes raw signals from the Ingest Worker:
- **Sentiment analysis** using Workers AI text classification
- **Topic extraction** and clustering using embeddings
- **Trend detection** based on signal frequency and velocity
- **Priority scoring** to route urgent signals first

### Content Engine

Generates content based on analyzed trends:
- Blog posts for technical topics
- Social media threads for trending discussions
- Video scripts for short-form content
- Documentation updates for common questions

### Feedback Pipeline

Aggregates developer feedback across platforms:
- Groups related signals by topic
- Tracks mention frequency and sentiment over time
- Generates weekly reports for the product team
- Escalates critical issues automatically

## Data Flow

1. **Ingest** Workers run on a cron schedule and push signals to the Analysis Worker
2. **Analysis** Worker classifies signals and routes them to Content Engine or Feedback Pipeline
3. **Content Engine** drafts content, stores it in a Durable Object queue, and publishes on schedule
4. **Feedback Pipeline** aggregates signals in a Durable Object and generates periodic reports
5. **Dashboard** connects via WebSocket to Durable Objects for real-time state updates

## Deployment Model

Each Worker deploys independently. The router's Service Bindings automatically connect to the latest version of each Worker. No router redeployment is needed when updating individual microfrontends.
