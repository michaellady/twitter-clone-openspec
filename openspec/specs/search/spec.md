## ADDED Requirements

### Requirement: Search tweets
The system SHALL allow users to search tweets by text content using PostgreSQL full-text search. Results SHALL be ordered by relevance, paginated with 20 per page.

#### Scenario: Search with results
- **WHEN** a user searches for "typescript"
- **THEN** the system returns tweets containing "typescript" (or stemmed variants), ranked by relevance, with author info and engagement counts

#### Scenario: Search with no results
- **WHEN** a user searches for a term matching no tweets
- **THEN** the system returns an empty list

### Requirement: Search users
The system SHALL allow searching users by username and displayName. Results SHALL be ordered by follower count (descending), paginated.

#### Scenario: Search users
- **WHEN** a user searches for "mike"
- **THEN** the system returns user profiles where username or displayName contains "mike", ordered by follower count

### Requirement: Search tabs
The system SHALL provide separate tabs for tweet results and user results on the search page. The default tab SHALL be tweets.

#### Scenario: Switch search tabs
- **WHEN** a user performs a search and clicks the "People" tab
- **THEN** the system displays user search results for the same query

### Requirement: Search debouncing
The client SHALL debounce search input by 300ms to avoid excessive API calls while typing.

#### Scenario: Rapid typing
- **WHEN** a user types "hello" quickly (5 keystrokes in under 300ms)
- **THEN** the client sends only one search request after 300ms of no typing
