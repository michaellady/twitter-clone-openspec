## ADDED Requirements

### Requirement: User registration with email and password
The system SHALL allow users to register with an email address, username, and password. The username SHALL be unique, 3-15 characters, alphanumeric plus underscores. The password SHALL be at least 8 characters. The email SHALL be validated for format and uniqueness.

#### Scenario: Successful registration
- **WHEN** a user submits a valid email, unique username, and password meeting requirements
- **THEN** the system creates the account, hashes the password, and logs the user in with an active session

#### Scenario: Duplicate email
- **WHEN** a user submits an email already associated with an account
- **THEN** the system rejects registration with error "Email already in use"

#### Scenario: Duplicate username
- **WHEN** a user submits a username already taken
- **THEN** the system rejects registration with error "Username already taken"

#### Scenario: Weak password
- **WHEN** a user submits a password shorter than 8 characters
- **THEN** the system rejects registration with error "Password must be at least 8 characters"

### Requirement: User login with email and password
The system SHALL allow registered users to log in with their email and password.

#### Scenario: Successful login
- **WHEN** a user submits correct email and password
- **THEN** the system creates a session and returns a session token

#### Scenario: Invalid credentials
- **WHEN** a user submits incorrect email or password
- **THEN** the system rejects login with error "Invalid email or password" without revealing which field is wrong

### Requirement: OAuth login
The system SHALL support OAuth login via Google and GitHub providers. If no account exists for the OAuth email, the system SHALL create one automatically using the provider's profile data.

#### Scenario: First-time OAuth login
- **WHEN** a user authenticates via Google or GitHub for the first time
- **THEN** the system creates an account using the OAuth profile (email, name, avatar) and logs the user in

#### Scenario: Returning OAuth login
- **WHEN** a user authenticates via OAuth with an email matching an existing account
- **THEN** the system links the OAuth provider and logs the user in

### Requirement: Session management
The system SHALL maintain user sessions using secure HTTP-only cookies. Sessions SHALL expire after 30 days of inactivity.

#### Scenario: Session persistence
- **WHEN** a user makes a request with a valid session cookie
- **THEN** the system authenticates the request and refreshes the session expiry

#### Scenario: Session expiry
- **WHEN** a user makes a request with an expired session cookie
- **THEN** the system rejects the request with 401 and redirects to login

### Requirement: Logout
The system SHALL allow users to log out, invalidating their current session.

#### Scenario: Successful logout
- **WHEN** a logged-in user requests logout
- **THEN** the system destroys the session and clears the session cookie

### Requirement: Password reset
The system SHALL allow users to reset their password via email. The reset link SHALL expire after 1 hour.

#### Scenario: Request password reset
- **WHEN** a user submits a registered email for password reset
- **THEN** the system sends an email with a one-time reset link valid for 1 hour

#### Scenario: Complete password reset
- **WHEN** a user follows a valid, non-expired reset link and submits a new password
- **THEN** the system updates the password, invalidates the reset link, and invalidates all existing sessions
