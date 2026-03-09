---
title: Service Bindings
description: How D2 uses Cloudflare Service Bindings for zero-latency communication between Workers.
---

Service Bindings are the backbone of D2's inter-module communication. They allow one Worker to call another directly, without going through the public internet — resulting in zero additional latency.

## How Service Bindings Work

A Service Binding is a reference from one Worker to another, configured in `wrangler.toml`:

```toml
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

In your Worker code, you call the bound service like a standard `fetch`:

```typescript
export default {
  async fetch(request: Request, env: Env) {
    const url = new URL(request.url);

    if (url.pathname.startsWith('/blog')) {
      return env.BLOG_WORKER.fetch(request);
    }

    if (url.pathname.startsWith('/docs')) {
      return env.DOCS_WORKER.fetch(request);
    }

    if (url.pathname.startsWith('/dashboard')) {
      return env.DASHBOARD_WORKER.fetch(request);
    }

    return env.WEBSITE_WORKER.fetch(request);
  }
};
```

## Benefits for D2

### Zero Network Overhead

Service Bindings invoke the target Worker directly within Cloudflare's infrastructure. There's no DNS resolution, no TLS handshake, no network round trip. The call behaves like a function invocation.

### Independent Deployments

Each Worker behind a Service Binding can be deployed independently. When you update the blog Worker, the router automatically calls the latest version. No configuration change or router redeployment needed.

### Type Safety

You can define TypeScript interfaces for your Service Bindings:

```typescript
interface Env {
  BLOG_WORKER: Fetcher;
  DOCS_WORKER: Fetcher;
  DASHBOARD_WORKER: Fetcher;
  WEBSITE_WORKER: Fetcher;
}
```

## D2's Binding Topology

The router Worker binds to all four microfrontend Workers. Additionally, the pipeline Workers bind to each other for the data processing flow:

| Source Worker | Binding | Target Worker |
| ------------- | ------- | ------------- |
| Router | `WEBSITE_WORKER` | Website |
| Router | `BLOG_WORKER` | Blog |
| Router | `DOCS_WORKER` | Docs |
| Router | `DASHBOARD_WORKER` | Dashboard |
| Ingest | `ANALYSIS_WORKER` | Analysis |
| Analysis | `CONTENT_ENGINE` | Content Engine |
| Analysis | `FEEDBACK_PIPELINE` | Feedback Pipeline |
