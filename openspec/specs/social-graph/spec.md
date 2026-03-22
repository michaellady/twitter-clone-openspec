## ADDED Requirements

### Requirement: Follow a user
The system SHALL allow authenticated users to follow other users. A user SHALL NOT be able to follow themselves.

#### Scenario: Successful follow
- **WHEN** an authenticated user requests to follow another user
- **THEN** the system creates the follow relationship, increments the follower's following count and the target's follower count, and sends a notification to the target

#### Scenario: Follow self
- **WHEN** a user attempts to follow themselves
- **THEN** the system rejects with error "Cannot follow yourself"

#### Scenario: Follow already-followed user
- **WHEN** a user attempts to follow a user they already follow
- **THEN** the system returns success idempotently (no duplicate relationship)

### Requirement: Unfollow a user
The system SHALL allow users to unfollow users they currently follow.

#### Scenario: Successful unfollow
- **WHEN** a user requests to unfollow a user they follow
- **THEN** the system removes the follow relationship and decrements both counts

#### Scenario: Unfollow user not followed
- **WHEN** a user attempts to unfollow a user they do not follow
- **THEN** the system returns success idempotently

### Requirement: View followers list
The system SHALL allow viewing the list of users who follow a given user, paginated with cursor-based pagination.

#### Scenario: View followers
- **WHEN** a user requests the followers list for a profile
- **THEN** the system returns a paginated list of follower profiles (id, username, displayName, avatarUrl, bio) with a cursor for the next page

### Requirement: View following list
The system SHALL allow viewing the list of users a given user follows, paginated with cursor-based pagination.

#### Scenario: View following
- **WHEN** a user requests the following list for a profile
- **THEN** the system returns a paginated list of followed profiles with a cursor for the next page

### Requirement: Follow suggestions
The system SHALL suggest users to follow based on mutual connections (users followed by people you follow).

#### Scenario: Suggestions for active user
- **WHEN** an authenticated user requests follow suggestions
- **THEN** the system returns up to 10 suggested profiles, excluding users already followed, ordered by number of mutual connections
