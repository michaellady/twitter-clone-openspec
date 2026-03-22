## ADDED Requirements

### Requirement: Generate notifications
The system SHALL create notifications for the following events: like on user's tweet, retweet of user's tweet, new follower, reply to user's tweet, @mention in a tweet.

#### Scenario: Like notification
- **WHEN** user B likes user A's tweet
- **THEN** the system creates a notification for user A of type "like" with actorId=B and tweetId

#### Scenario: Follow notification
- **WHEN** user B follows user A
- **THEN** the system creates a notification for user A of type "follow" with actorId=B

#### Scenario: Reply notification
- **WHEN** user B replies to user A's tweet
- **THEN** the system creates a notification for user A of type "reply" with actorId=B and tweetId of the reply

#### Scenario: Mention notification
- **WHEN** user B posts a tweet mentioning @A
- **THEN** the system creates a notification for user A of type "mention" with actorId=B and tweetId

### Requirement: List notifications
The system SHALL provide an authenticated user's notifications in reverse-chronological order, paginated with 20 per page.

#### Scenario: View notifications
- **WHEN** an authenticated user requests their notifications
- **THEN** the system returns the 20 most recent notifications with actor profile info and tweet preview (if applicable)

### Requirement: Mark notifications as read
The system SHALL allow users to mark individual notifications or all notifications as read.

#### Scenario: Mark single notification read
- **WHEN** a user marks a specific notification as read
- **THEN** the system sets read=true for that notification

#### Scenario: Mark all notifications read
- **WHEN** a user requests to mark all notifications as read
- **THEN** the system sets read=true for all of the user's unread notifications

### Requirement: Real-time notification delivery
The system SHALL push new notifications to connected clients via SSE. The client SHALL display an unread count badge on the notifications icon.

#### Scenario: Real-time notification
- **WHEN** a new notification is created for a user who has an active SSE connection
- **THEN** the system sends the notification via SSE and the client increments the unread badge count

### Requirement: No self-notifications
The system SHALL NOT generate notifications for a user's own actions (e.g., liking own tweet).

#### Scenario: Self-action suppressed
- **WHEN** a user likes their own tweet
- **THEN** no notification is created
