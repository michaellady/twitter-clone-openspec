## ADDED Requirements

### Requirement: View user profile
The system SHALL allow any visitor to view a user's public profile, including displayName, username, bio, avatarUrl, bannerUrl, follower count, following count, tweet count, and join date.

#### Scenario: View existing profile
- **WHEN** a visitor navigates to /@username
- **THEN** the system displays the user's profile with all public fields and their tweet timeline

#### Scenario: View non-existent profile
- **WHEN** a visitor navigates to a username that does not exist
- **THEN** the system returns 404 with "This account doesn't exist"

### Requirement: Edit profile
The system SHALL allow authenticated users to edit their own profile fields: displayName (max 50 chars), bio (max 160 chars), avatarUrl, and bannerUrl.

#### Scenario: Update display name and bio
- **WHEN** an authenticated user submits updated displayName and bio within character limits
- **THEN** the system updates the profile and returns the updated profile

#### Scenario: Bio exceeds limit
- **WHEN** a user submits a bio longer than 160 characters
- **THEN** the system rejects with error "Bio must be 160 characters or less"

### Requirement: Upload avatar
The system SHALL allow users to upload a profile avatar image. The avatar SHALL be resized client-side to 400x400 pixels before upload.

#### Scenario: Upload avatar
- **WHEN** a user uploads an image as their avatar
- **THEN** the system stores the image via media-upload, updates the user's avatarUrl, and returns the new URL

### Requirement: Upload banner
The system SHALL allow users to upload a profile banner image. The banner SHALL be resized client-side to 1500x500 pixels before upload.

#### Scenario: Upload banner
- **WHEN** a user uploads an image as their banner
- **THEN** the system stores the image via media-upload, updates the user's bannerUrl, and returns the new URL
