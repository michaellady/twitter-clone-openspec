## 1. Project Setup

- [ ] 1.1 Initialize Next.js 14 project with App Router, TypeScript, Tailwind CSS, and ESLint
- [ ] 1.2 Set up Prisma ORM with PostgreSQL connection and initial schema
- [ ] 1.3 Set up Redis client for caching and sessions
- [ ] 1.4 Configure tRPC with Next.js App Router integration
- [ ] 1.5 Set up NextAuth.js with session strategy and database adapter
- [ ] 1.6 Configure S3 client (with MinIO for local dev) for media uploads
- [ ] 1.7 Set up Docker Compose for local PostgreSQL, Redis, and MinIO

## 2. Database Schema

- [ ] 2.1 Create User model (id, email, username, displayName, bio, avatarUrl, bannerUrl, hashedPassword, createdAt)
- [ ] 2.2 Create Tweet model (id, authorId, content, parentId, quoteTweetId, mediaUrls, deleted, createdAt)
- [ ] 2.3 Create Follow model (followerId, followingId, createdAt) with unique constraint
- [ ] 2.4 Create Like model (userId, tweetId, createdAt) with unique constraint
- [ ] 2.5 Create Retweet model (userId, tweetId, createdAt) with unique constraint
- [ ] 2.6 Create Notification model (id, recipientId, type, actorId, tweetId, read, createdAt)
- [ ] 2.7 Create Account and Session models for NextAuth
- [ ] 2.8 Add indexes for feed queries (Tweet.authorId+createdAt, Follow.followerId, Notification.recipientId+createdAt)
- [ ] 2.9 Add PostgreSQL full-text search index on Tweet.content
- [ ] 2.10 Run initial Prisma migration and verify schema

## 3. Authentication

- [ ] 3.1 Implement email/password registration endpoint with validation (username uniqueness, email format, password strength)
- [ ] 3.2 Implement email/password login with bcrypt password verification
- [ ] 3.3 Configure Google OAuth provider in NextAuth
- [ ] 3.4 Configure GitHub OAuth provider in NextAuth
- [ ] 3.5 Implement auto-account creation for first-time OAuth login
- [ ] 3.6 Implement logout endpoint that destroys session
- [ ] 3.7 Implement password reset flow (request link, validate token, update password)
- [ ] 3.8 Build registration page UI
- [ ] 3.9 Build login page UI with OAuth buttons
- [ ] 3.10 Add auth middleware to protect tRPC routes requiring authentication

## 4. User Profiles

- [ ] 4.1 Implement tRPC router for fetching user profile by username
- [ ] 4.2 Implement tRPC mutation for updating profile (displayName, bio)
- [ ] 4.3 Implement avatar upload with client-side resize to 400x400 and S3 pre-signed URL
- [ ] 4.4 Implement banner upload with client-side resize to 1500x500 and S3 pre-signed URL
- [ ] 4.5 Build profile page with header (banner, avatar, stats), tabs (tweets, replies, likes)
- [ ] 4.6 Build edit profile modal with form validation

## 5. Tweet Management

- [ ] 5.1 Implement tRPC mutation for creating tweets (280 char validation, media URLs)
- [ ] 5.2 Implement tRPC mutation for deleting tweets (soft delete, author check)
- [ ] 5.3 Implement tRPC query for fetching single tweet with author info and engagement counts
- [ ] 5.4 Implement reply creation (set parentId, increment reply count)
- [ ] 5.5 Implement @mention detection in tweet text and trigger notifications
- [ ] 5.6 Build tweet composer component with character counter and media attachment UI
- [ ] 5.7 Build tweet card component displaying author, content, media grid, timestamps, engagement buttons
- [ ] 5.8 Build tweet detail page with threaded replies

## 6. Media Upload

- [ ] 6.1 Implement tRPC endpoint to generate S3 pre-signed upload URLs
- [ ] 6.2 Implement client-side upload flow: select files, validate (format, size, count), upload to S3
- [ ] 6.3 Build responsive image grid component (1 full, 2 side-by-side, 3-4 in 2x2)
- [ ] 6.4 Add image lightbox/modal for viewing full-size images

## 7. Social Graph

- [ ] 7.1 Implement tRPC mutation for follow (with self-follow prevention, idempotency)
- [ ] 7.2 Implement tRPC mutation for unfollow (with idempotency)
- [ ] 7.3 Implement tRPC query for followers list (cursor-based pagination)
- [ ] 7.4 Implement tRPC query for following list (cursor-based pagination)
- [ ] 7.5 Implement follow suggestions query (mutual connections, exclude already-followed)
- [ ] 7.6 Build follow/unfollow button component
- [ ] 7.7 Build followers/following list pages
- [ ] 7.8 Build "Who to follow" sidebar widget

## 8. Engagement

- [ ] 8.1 Implement tRPC mutation for like/unlike (idempotent, update counts)
- [ ] 8.2 Implement tRPC mutation for retweet/undo retweet (prevent self-retweet, update counts)
- [ ] 8.3 Implement quote tweet creation (set quoteTweetId, render embedded tweet)
- [ ] 8.4 Implement tRPC query for tweet likers list (paginated)
- [ ] 8.5 Build like button with optimistic UI update and animation
- [ ] 8.6 Build retweet button with dropdown (retweet / quote tweet)
- [ ] 8.7 Build embedded quote tweet component

## 9. Feed Assembly

- [ ] 9.1 Implement home timeline tRPC query (tweets from followed users, cursor pagination, 20 per page)
- [ ] 9.2 Implement feed deduplication logic (retweet + original from different followed users)
- [ ] 9.3 Add Redis caching for assembled timeline pages
- [ ] 9.4 Implement user timeline tRPC query (single user's tweets, paginated)
- [ ] 9.5 Build home feed page with infinite scroll
- [ ] 9.6 Build empty state with follow suggestions for new users

## 10. Real-Time (SSE)

- [ ] 10.1 Set up SSE endpoint for authenticated users (multiplexed connection)
- [ ] 10.2 Implement server-side event publishing for new tweets from followed users
- [ ] 10.3 Implement server-side event publishing for new notifications
- [ ] 10.4 Build client-side SSE hook with auto-reconnect
- [ ] 10.5 Build "N new tweets" indicator at top of home feed
- [ ] 10.6 Build real-time unread notification badge

## 11. Notifications

- [ ] 11.1 Implement notification creation service (like, retweet, follow, reply, mention events)
- [ ] 11.2 Add self-notification suppression (no notification for own actions)
- [ ] 11.3 Implement tRPC query for notifications list (paginated, with actor info and tweet preview)
- [ ] 11.4 Implement tRPC mutation for mark-as-read (single and bulk)
- [ ] 11.5 Build notifications page with notification cards grouped by type
- [ ] 11.6 Build notification bell icon with unread count badge

## 12. Search

- [ ] 12.1 Implement tRPC query for tweet full-text search (PostgreSQL ts_vector, ranked by relevance)
- [ ] 12.2 Implement tRPC query for user search (username + displayName, ordered by follower count)
- [ ] 12.3 Build search page with input, debounced (300ms), and tabs (Tweets / People)
- [ ] 12.4 Build search result cards for tweets and users

## 13. Layout and Navigation

- [ ] 13.1 Build app shell with sidebar navigation (Home, Search, Notifications, Profile)
- [ ] 13.2 Build responsive layout (sidebar collapses to bottom nav on mobile)
- [ ] 13.3 Build right sidebar with search bar, trending, and who-to-follow widgets
- [ ] 13.4 Add loading skeletons for all major content areas

## 14. Testing and Cross-Platform

- [ ] 14.1 Write unit tests for tweet validation, mention parsing, feed deduplication
- [ ] 14.2 Write integration tests for auth flows (register, login, OAuth)
- [ ] 14.3 Write integration tests for core social actions (tweet, like, follow, retweet)
- [ ] 14.4 Verify all file path handling uses path.join() for cross-platform compatibility
- [ ] 14.5 Add Windows CI verification step
