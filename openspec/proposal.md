## Why

We need a full-stack Twitter clone to demonstrate modern web application architecture — real-time feeds, social graphs, authentication, and interactive UI patterns. This serves as a reference implementation covering the most common patterns in social media applications.

## What Changes

- Add user registration and authentication (email/password, OAuth)
- Add tweet creation with 280-character limit, media attachments
- Add social graph: follow/unfollow users
- Add engagement: likes, retweets, replies (threaded)
- Add real-time home feed assembled from followed users' tweets
- Add user profiles with bio, avatar, banner
- Add notifications for mentions, likes, follows, retweets
- Add search across tweets and users
- Add responsive SPA frontend with infinite scroll

## Capabilities

### New Capabilities
- `user-auth`: Registration, login, sessions, OAuth, password reset
- `tweet-management`: Create, delete, edit tweets with media and 280-char limit
- `social-graph`: Follow/unfollow, follower/following lists, suggestions
- `feed-assembly`: Real-time home timeline from followed users, algorithmic ranking
- `engagement`: Likes, retweets, quote tweets, threaded replies
- `user-profiles`: Profile pages, bio, avatar, banner, edit profile
- `notifications`: Real-time notifications for social interactions
- `search`: Full-text search across tweets and user profiles
- `media-upload`: Image/video upload, storage, and CDN delivery

### Modified Capabilities

_(none — this is a greenfield project)_

## Impact

- **New codebase**: Separate frontend (React/Next.js) and backend (Node.js API)
- **Database**: PostgreSQL for relational data, Redis for caching/sessions
- **Real-time**: WebSocket connections for live feed updates and notifications
- **Storage**: Object storage for media uploads
- **Dependencies**: React, Next.js, Tailwind CSS, Prisma, Socket.io
