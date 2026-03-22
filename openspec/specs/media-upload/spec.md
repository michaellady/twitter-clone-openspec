## ADDED Requirements

### Requirement: Upload images to tweets
The system SHALL allow users to attach up to 4 images per tweet. Supported formats SHALL be JPEG, PNG, GIF, and WebP. Maximum file size SHALL be 5MB per image.

#### Scenario: Attach images to tweet
- **WHEN** a user creates a tweet with 1-4 valid images
- **THEN** the system uploads each image to S3, stores the URLs in the tweet's mediaUrls array, and returns the tweet with media

#### Scenario: Too many images
- **WHEN** a user attempts to attach more than 4 images
- **THEN** the system rejects with error "Maximum 4 images per tweet"

#### Scenario: File too large
- **WHEN** a user attempts to upload an image larger than 5MB
- **THEN** the system rejects with error "Image must be under 5MB"

#### Scenario: Invalid format
- **WHEN** a user attempts to upload a non-supported file format
- **THEN** the system rejects with error "Supported formats: JPEG, PNG, GIF, WebP"

### Requirement: Pre-signed upload URLs
The system SHALL generate pre-signed S3 URLs for direct client-to-S3 uploads, avoiding proxying file data through the API server.

#### Scenario: Request upload URL
- **WHEN** an authenticated user requests an upload URL with filename and content type
- **THEN** the system returns a pre-signed PUT URL and the final public URL for the uploaded file

### Requirement: Image display
The system SHALL display tweet images in a responsive grid layout: 1 image full-width, 2 images side-by-side, 3-4 images in a 2x2 grid.

#### Scenario: Display 2 images
- **WHEN** a tweet with 2 images is rendered
- **THEN** the images display side-by-side in equal-width columns

#### Scenario: Display 4 images
- **WHEN** a tweet with 4 images is rendered
- **THEN** the images display in a 2x2 grid with equal dimensions
