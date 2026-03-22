## ADDED Requirements

### Requirement: Like a tweet
The system SHALL allow authenticated users to like tweets. Each user SHALL be able to like a tweet at most once.

#### Scenario: Successful like
- **WHEN** an authenticated user likes a tweet they haven't liked
- **THEN** the system records the like, increments the tweet's like count, and sends a notification to the tweet author

#### Scenario: Like already-liked tweet
- **WHEN** a user likes a tweet they already liked
- **THEN** the system returns success idempotently without incrementing the count

### Requirement: Unlike a tweet
The system SHALL allow users to remove their like from a tweet.

#### Scenario: Successful unlike
- **WHEN** a user unlikes a tweet they previously liked
- **THEN** the system removes the like and decrements the tweet's like count

### Requirement: Retweet
The system SHALL allow authenticated users to retweet (share) another user's tweet to their followers. Each user SHALL be able to retweet a tweet at most once.

#### Scenario: Successful retweet
- **WHEN** an authenticated user retweets a tweet
- **THEN** the system records the retweet, increments the retweet count, adds the tweet to the user's profile timeline with retweet attribution, and notifies the original author

#### Scenario: Undo retweet
- **WHEN** a user undoes a retweet
- **THEN** the system removes the retweet, decrements the count, and removes it from the user's timeline

#### Scenario: Retweet own tweet
- **WHEN** a user attempts to retweet their own tweet
- **THEN** the system rejects with error "Cannot retweet your own tweet"

### Requirement: Quote tweet
The system SHALL allow users to quote-tweet by creating a new tweet that embeds a reference to another tweet.

#### Scenario: Successful quote tweet
- **WHEN** a user creates a tweet with a quoteTweetId referencing an existing tweet
- **THEN** the system creates the tweet with the quoted tweet embedded, and notifies the quoted tweet's author

### Requirement: View likers
The system SHALL allow viewing the list of users who liked a specific tweet, paginated.

#### Scenario: View who liked a tweet
- **WHEN** a user requests the likers of a tweet
- **THEN** the system returns a paginated list of user profiles who liked that tweet
