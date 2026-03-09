---
title: Configuration Reference
description: Reference for D2 router configuration, environment variables, and module settings.
---

## Router Configuration

The router Worker uses a `ROUTES` environment variable to map URL paths to microfrontend Workers.

### Route Format

```json
{
  "routes": [
    {
      "path": "/blog",
      "binding": "BLOG_WORKER",
      "stripPrefix": true
    },
    {
      "path": "/docs",
      "binding": "DOCS_WORKER",
      "stripPrefix": true
    },
    {
      "path": "/dashboard",
      "binding": "DASHBOARD_WORKER",
      "stripPrefix": true
    },
    {
      "path": "/",
      "binding": "WEBSITE_WORKER"
    }
  ]
}
```

### Route Properties

| Property | Type | Required | Description |
| -------- | ---- | -------- | ----------- |
| `path` | `string` | Yes | URL path prefix to match |
| `binding` | `string` | Yes | Name of the Service Binding to the target Worker |
| `stripPrefix` | `boolean` | No | Remove the path prefix before forwarding (default: `false`) |

Routes are matched by specificity — longer paths take precedence over shorter ones.

## Environment Variables

### Router Worker

| Variable | Description |
| -------- | ----------- |
| `ROUTES` | JSON route configuration (see above) |
| `ENABLE_PRELOADING` | Enable preload hints for cross-app navigation (`true`/`false`) |
| `ENABLE_VIEW_TRANSITIONS` | Enable View Transitions API (`true`/`false`) |

### Ingest Worker

| Variable | Description |
| -------- | ----------- |
| `CRAWL_INTERVAL` | Cron schedule for community crawling (default: `*/15 * * * *`) |
| `SOURCES` | Comma-separated list of enabled sources |

### Content Engine

| Variable | Description |
| -------- | ----------- |
| `AI_MODEL` | Workers AI model for content generation |
| `PUBLISH_SCHEDULE` | Cron schedule for content publishing |
| `MAX_DRAFTS_PER_DAY` | Maximum drafts to generate per day |

## Wrangler Configuration

Example `wrangler.toml` for the router Worker:

```toml
name = "d2-router"
main = "src/index.ts"
compatibility_date = "2025-01-01"

[[services]]
binding = "WEBSITE_WORKER"
service = "d2-website"

[[services]]
binding = "BLOG_WORKER"
service = "d2-blog"

[[services]]
binding = "DOCS_WORKER"
service = "d2-docs"

[[services]]
binding = "DASHBOARD_WORKER"
service = "d2-dashboard"
```
