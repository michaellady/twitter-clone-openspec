## ADDED Requirements

### Requirement: Home timeline
The system SHALL assemble a home timeline for authenticated users containing tweets from users they follow, ordered reverse-chronologically. The feed SHALL use cursor-based pagination returning 20 tweets per page.

#### Scenario: View home feed
- **WHEN** an authenticated user requests their home timeline
- **THEN** the system returns the 20 most recent tweets from followed users, including retweets, with a cursor for the next page

#### Scenario: Empty feed
- **WHEN** a user who follows nobody requests their home timeline
- **THEN** the system returns an empty list with suggested users to follow

### Requirement: Feed includes retweets
The system SHALL include retweets in the home timeline. A retweet SHALL appear as the original tweet with "retweeted by @username" attribution.

#### Scenario: Retweet in feed
- **WHEN** a followed user retweets a tweet
- **THEN** the retweet appears in the follower's home timeline with the retweeter's attribution and the original tweet content

### Requirement: Feed deduplication
The system SHALL deduplicate tweets in the feed. If a tweet appears via multiple paths (e.g., followed by author AND retweeted by another followed user), it SHALL appear only once, prioritizing the earliest appearance.

#### Scenario: Duplicate tweet in feed
- **WHEN** a user follows both the author and someone who retweeted the same tweet
- **THEN** the tweet appears only once in the timeline

### Requirement: Real-time feed updates
The system SHALL push new tweets to connected clients via SSE when a followed user posts a new tweet. The client SHALL display a "New tweets" indicator rather than injecting tweets directly into the scroll position.

#### Scenario: New tweet from followed user
- **WHEN** a followed user posts a new tweet while the follower has the feed open
- **THEN** the system sends an SSE event to the follower's client, which displays "N new tweets" at the top of the feed

### Requirement: User timeline
The system SHALL provide a timeline of a specific user's own tweets (including replies and retweets), accessible to any visitor, paginated.

#### Scenario: View user timeline
- **WHEN** a visitor requests a user's profile timeline
- **THEN** the system returns that user's tweets in reverse-chronological order, paginated with 20 per page
