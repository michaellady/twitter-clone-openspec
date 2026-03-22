## Context

We are building a greenfield Twitter clone — a full-stack social media application. There is no existing codebase for this app. The goal is a modern, real-time social platform demonstrating production-grade patterns.

## Goals / Non-Goals

**Goals:**
- Full tweet lifecycle: create, edit, delete with 280-char limit
- User authentication with email/password and OAuth (Google, GitHub)
- Social graph with follow/unfollow and follower recommendations
- Real-time home feed assembled from followed users
- Engagement features: likes, retweets, quote tweets, threaded replies
- Notifications for social interactions
- Full-text search across tweets and users
- Responsive SPA with infinite scroll
- Media upload for images

**Non-Goals:**
- Direct messages / chat (future phase)
- Video upload and transcoding (future phase)
- Algorithmic/ML-based feed ranking (chronological only for v1)
- Ads or monetization features
- Mobile native apps (web-only for v1)
- Content moderation / trust & safety tooling (future phase)
- Analytics dashboard

## Decisions

### Frontend: Next.js 14 + Tailwind CSS
**Choice:** Next.js with App Router, React Server Components, Tailwind CSS
**Alternatives considered:**
- Vite + React SPA — simpler but loses SSR benefits for SEO and initial load
- Remix — strong but smaller ecosystem, less community support
- **Rationale:** Next.js gives us SSR for public profiles/tweets (SEO), client components for interactive feed, and excellent DX. Tailwind for rapid, consistent styling.

### Backend: Next.js API Routes + tRPC
**Choice:** Co-located API routes using tRPC for type-safe client-server communication
**Alternatives considered:**
- Separate Express/Fastify API — more flexibility but adds deployment complexity and loses type sharing
- GraphQL — powerful but over-engineered for this use case
- **Rationale:** tRPC gives end-to-end type safety with zero code generation. Co-locating with Next.js simplifies deployment to a single service.

### Database: PostgreSQL + Prisma ORM
**Choice:** PostgreSQL with Prisma for schema management and queries
**Alternatives considered:**
- MongoDB — flexible schema but poor for relational social graph data
- Drizzle ORM — lighter but less mature migration tooling
- **Rationale:** Social data is inherently relational (users → tweets → likes → follows). Prisma provides excellent TypeScript integration and migration management.

### Real-time: Server-Sent Events (SSE)
**Choice:** SSE for real-time feed updates and notifications
**Alternatives considered:**
- WebSockets (Socket.io) — bidirectional but we only need server→client push
- Polling — simpler but wastes resources and adds latency
- **Rationale:** SSE is simpler than WebSockets, works through proxies/CDNs, and auto-reconnects. We only need server-to-client push for feed updates and notifications.

### Auth: NextAuth.js (Auth.js)
**Choice:** NextAuth.js for session management and OAuth
**Alternatives considered:**
- Custom JWT implementation — more control but more security surface area
- Clerk/Auth0 — vendor lock-in and cost at scale
- **Rationale:** Battle-tested, supports multiple OAuth providers, integrates natively with Next.js, handles session management securely.

### Caching: Redis
**Choice:** Redis for session store, feed caching, and rate limiting
**Rationale:** Industry standard for caching hot data. Feed timelines are cached per-user in Redis sorted sets for O(1) retrieval.

### Media Storage: S3-compatible object storage
**Choice:** AWS S3 (or MinIO for local dev) for image uploads
**Rationale:** Scalable, cheap, CDN-friendly. Pre-signed URLs for direct client uploads avoid proxying through the API server.

## Architecture

```
┌─────────────────────────────────────────┐
│           Next.js Application           │
│  ┌───────────┐  ┌────────────────────┐  │
│  │   Pages    │  │   API Routes       │  │
│  │  (RSC +    │  │   (tRPC router)    │  │
│  │   Client)  │  │                    │  │
│  └─────┬─────┘  └────────┬───────────┘  │
│        │                  │              │
│        └──────┬───────────┘              │
│               │                          │
│        ┌──────┴──────┐                   │
│        │  NextAuth   │                   │
│        └──────┬──────┘                   │
└───────────────┼──────────────────────────┘
                │
    ┌───────────┼───────────┐
    │           │           │
┌───┴───┐  ┌───┴───┐  ┌───┴───┐
│ Postgres│  │ Redis │  │  S3   │
│  (data) │  │(cache)│  │(media)│
└─────────┘  └───────┘  └───────┘
```

## Data Model (key entities)

- **User**: id, email, username, displayName, bio, avatarUrl, bannerUrl, createdAt
- **Tweet**: id, authorId, content (280 chars), parentId (for replies), quoteTweetId, mediaUrls[], createdAt
- **Follow**: followerId, followingId, createdAt
- **Like**: userId, tweetId, createdAt
- **Retweet**: userId, tweetId, createdAt
- **Notification**: id, recipientId, type (like|retweet|follow|reply|mention), actorId, tweetId?, read, createdAt

## Risks / Trade-offs

- **[Fan-out on write vs read for feed]** → We use fan-out on read (query at read time) for v1. Simpler but slower for users following many accounts. Mitigation: Redis caching of assembled timelines, paginated queries.
- **[SSE connection limits]** → Browsers limit ~6 SSE connections per domain. Mitigation: Single multiplexed SSE connection for all real-time events.
- **[N+1 queries on feed]** → Tweet list pages could trigger many queries. Mitigation: Prisma eager loading, DataLoader pattern for batching.
- **[Image processing]** → No server-side resizing in v1. Mitigation: Client-side resize before upload, accept minor quality trade-off.
- **[Search quality]** → PostgreSQL full-text search is adequate for v1 but won't scale to millions of tweets. Mitigation: Can migrate to Elasticsearch/Meilisearch later without API changes.

## Migration Plan

Not applicable — greenfield project. Deployment strategy:
1. Set up PostgreSQL + Redis infrastructure
2. Deploy Next.js app (Vercel or Docker)
3. Configure S3 bucket + CDN for media
4. Set up OAuth credentials (Google, GitHub)
5. Run Prisma migrations to create schema
