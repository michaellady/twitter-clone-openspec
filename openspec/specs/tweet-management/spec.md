## ADDED Requirements

### Requirement: Create a tweet
The system SHALL allow authenticated users to create tweets with text content up to 280 characters. Tweets MAY include media attachments (handled by media-upload capability).

#### Scenario: Successful tweet creation
- **WHEN** an authenticated user submits tweet text within 280 characters
- **THEN** the system creates the tweet, associates it with the user, timestamps it, and returns the created tweet

#### Scenario: Tweet exceeds character limit
- **WHEN** a user submits tweet text longer than 280 characters
- **THEN** the system rejects the tweet with error "Tweet exceeds 280 character limit"

#### Scenario: Empty tweet without media
- **WHEN** a user submits a tweet with no text and no media
- **THEN** the system rejects the tweet with error "Tweet must have text or media"

### Requirement: Delete a tweet
The system SHALL allow users to delete their own tweets. Deleting a tweet SHALL remove it from all timelines and feeds.

#### Scenario: Author deletes own tweet
- **WHEN** a user requests deletion of a tweet they authored
- **THEN** the system soft-deletes the tweet, removing it from feeds, and returns success

#### Scenario: Non-author attempts deletion
- **WHEN** a user requests deletion of a tweet authored by another user
- **THEN** the system rejects with 403 Forbidden

### Requirement: Reply to a tweet
The system SHALL allow users to reply to existing tweets. Replies SHALL reference the parent tweet via parentId and appear in the parent tweet's reply thread.

#### Scenario: Successful reply
- **WHEN** an authenticated user submits a reply to an existing tweet
- **THEN** the system creates the reply tweet with parentId set, increments the parent's reply count, and returns the reply

#### Scenario: Reply to deleted tweet
- **WHEN** a user attempts to reply to a deleted tweet
- **THEN** the system rejects with error "Cannot reply to a deleted tweet"

### Requirement: View a tweet
The system SHALL allow any user (authenticated or not) to view a tweet by its ID, including its author info, engagement counts, and reply thread.

#### Scenario: View existing tweet
- **WHEN** a user requests a tweet by valid ID
- **THEN** the system returns the tweet with author details, like count, retweet count, reply count, and whether the current user has liked/retweeted it

#### Scenario: View deleted tweet
- **WHEN** a user requests a deleted tweet
- **THEN** the system returns 404 Not Found

### Requirement: Tweet with mentions
The system SHALL detect @username mentions in tweet text and notify mentioned users.

#### Scenario: Tweet mentions a user
- **WHEN** a user creates a tweet containing "@alice"
- **THEN** the system creates a mention notification for user "alice" linking to the tweet
