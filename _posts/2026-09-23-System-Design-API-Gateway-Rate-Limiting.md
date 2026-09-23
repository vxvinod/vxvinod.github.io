---
layout: post
title:  "System Design: API Gateways & Rate Limiting"
date:   2026-09-23
desc: "Breaking down what an API Gateway actually solves, why rate limiting belongs there instead of in each service, and the three rate limiting algorithms I need to remember."
---

# System Design: API Gateways & Rate Limiting

I spent today's system design session on API Gateways — a concept I'd heard thrown around a lot but never actually broken down for myself. Here's what I worked out, in my own words.

---

## The problem I realized it solves

Say you've got a backend made up of several services — a User service, an Orders service, a Payments service. Without a gateway, a client app has to know about and talk to *each* service directly. That means every client needs to know every service's address, and every service ends up duplicating the same auth checks, logging, and rate limiting logic on its own.

That's messy. And it gets worse every time you add a new service.

---

## What a gateway actually does

It's a single entry point sitting in front of everything. The client only ever talks to the gateway — the gateway figures out internally where the request should actually go.

Here's what I understood it to be responsible for:

- **Routing** — looking at the request and deciding which backend service should handle it
- **Authentication and authorization** — checking whether the request is even allowed before it reaches any real service
- **Rate limiting** — stopping bulk requests from overwhelming the server
- **Load balancing** — if a service runs on multiple servers, deciding which one gets this particular request
- **Logging and monitoring** — one place to watch the full flow of requests and responses instead of digging through logs scattered across every service
- **Request/response transformation** — converting formats when needed, e.g. an older client sending data in a shape a service doesn't expect anymore

---

## The part that actually clicked for me: why rate limit at the gateway, not in each service?

My first instinct was "doing it in each service separately is just more work." True, but there's a sharper reason underneath that.

If each service rate-limits on its own, none of them know what's happening in the *others*. A client could stay under the limit on the Orders service, and separately stay under the limit on the Payments service — but combined, they could be sending far more total traffic than the system should allow, and no single service would notice, because each one only sees its own slice.

The gateway is the only place that sees the *whole* picture — total requests from a client across every service. That's the real reason it belongs there, not just to avoid duplicating code.

---

## Rate limiting algorithms — the three I need to remember

- **Fixed Window** — count requests in a fixed time block, e.g. 100/minute, resetting on the clock. Simple, but has a burst problem: a client can send 100 requests right at the end of one window and another 100 right at the start of the next, getting 200 requests through in a couple of seconds.
- **Sliding Window** — same idea, but the window moves continuously instead of resetting at a fixed clock boundary. Smooths out that burst problem.
- **Token Bucket** — each client has a bucket that refills with tokens at a steady rate, and each request spends a token. Allows short bursts if the bucket's full, while still holding a long-term average rate. This is apparently what most real systems (Stripe, AWS) actually use.

---

## Why this matters for what I'm building

My LLM Gateway project is structurally the exact same pattern — just fronting AI providers instead of internal microservices. Client talks to my FastAPI gateway, the gateway decides which provider to call.

I've already built the routing piece. Rate limiting is coming later in my build, and now I understand exactly why it belongs at the gateway layer — not inside each provider's call — for the same reason a company-wide gateway centralizes it: one place to see and enforce the real total load, instead of duplicating half-blind limit logic everywhere.

---

## What I'm taking forward

The habit I'm trying to build is explaining things back in my own words before moving on, instead of just nodding along to an explanation. Doing that today is what actually surfaced the gap in my first answer — "extra work" wasn't wrong, but it wasn't the real reason either. The real reason was visibility across the whole system, not just less duplicated code.

---

*Next up: applying this directly when I build rate limiting into my own gateway.*
