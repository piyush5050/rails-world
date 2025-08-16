# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Rails World Conference App - A Rails 8.0 application for managing conferences, sessions, speakers, and attendees with features like scheduling, notifications, and progressive web app capabilities.

## Development Commands

### Setup and Running
```bash
bin/setup          # Initial setup (run once)
bin/dev            # Start development server (localhost:3000)
```

### Testing
```bash
bundle exec rspec                      # Run all tests
bundle exec rspec spec/path/to/spec   # Run specific test file
HEADLESS=false bundle exec rspec      # Run tests with visible browser
```

### Linting and Code Quality
```bash
# Ruby
bundle exec rubocop              # Check Ruby files
bundle exec rubocop -a           # Auto-fix Ruby issues

# ERB Templates
bundle exec erblint --lint-all   # Check ERB files
bundle exec erblint --lint-all -a # Auto-fix ERB (use with caution)

# JavaScript
standard                         # Check JS files (requires global install)
standard --fix                   # Auto-fix JS issues

# Spelling
typos                           # Check spelling
typos -w                        # Auto-fix spelling

# Code Quality
bundle exec rubycritic          # Generate code quality report
bundle exec database_consistency # Check DB/model consistency
```

### Database Tasks
```bash
bundle exec rails db:migrate     # Run migrations
bundle exec rails db:seed        # Seed database
bundle exec rails db:reset       # Reset database
bundle exec rails annotate_models # Update model annotations
```

### Admin Interface
- Access at `http://localhost:3000/avo`
- Default credentials in `config/seeds.rb`

## Architecture Overview

### Stack
- **Framework**: Rails 8.0 with Ruby 3.3.3
- **Database**: SQLite3 with enhanced adapter
- **Job Processing**: SolidQueue with Mission Control UI
- **Frontend**: Hotwire (Turbo + Stimulus), Import Maps, Tailwind CSS
- **Admin**: Avo admin panel
- **Deployment**: Kamal 2

### Key Directories
- `app/controllers/` - Request handling with authentication/authorization concerns
- `app/models/` - Core models: Conference, Session, Speaker, User, Profile
- `app/views/` - ERB templates with Tailwind styling
- `app/javascript/` - Stimulus controllers and custom JS
- `app/avo/` - Admin panel resources
- `app/notifiers/` - Notification system (web push support)
- `app/policies/` - Authorization policies using Action Policy

### Core Models
- **Conference**: Main conference entity
- **Session**: Individual conference sessions with slug-based URLs
- **Speaker**: Conference speakers with profile links
- **User**: Authentication with bcrypt
- **Profile**: User profiles with UUID-based URLs
- **Tag**: Session categorization
- **Location**: Venue information

### Authentication & Authorization
- Custom authentication using bcrypt (no Devise)
- Session-based auth with cookie store
- Authorization via Action Policy
- Constraints for authenticated/unauthenticated routes

### Features
- **Progressive Web App**: Service worker, offline support, manifest
- **Notifications**: Web push notifications via Noticed gem
- **Real-time Updates**: Hotwire for dynamic UI updates
- **Admin Panel**: Full CRUD operations via Avo
- **Feature Flags**: Environment variable based (see `app/models/feature.rb`)

### Testing Approach
- RSpec for unit and integration tests
- Capybara with Cuprite for system tests
- Use `data-test-id` attributes for element selection
- Factory Bot for test data
- Test helpers in `spec/support/`

### Deployment
- Kamal 2 for containerized deployment
- Staging and production environments configured
- Required services: AppSignal, AWS S3, MailPace
- Environment-specific deploy files: `config/deploy.{environment}.yml`

## Important Conventions

### Code Style
- Ruby: Standard Ruby style guide via rubocop
- JavaScript: StandardJS conventions
- Use existing patterns and libraries in the codebase
- Avoid adding comments unless necessary

### View Helpers
- `data-test-id` for test element selection
- `find_dti()` helper method for tests
- Stimulus controllers for JavaScript behavior
- Turbo frames/streams for dynamic updates

### Database
- Migrations in `db/migrate/`
- Use `friendly_id` for slug generation
- UUID-based URLs for profiles
- SQLite with production enhancements (Litestream backups)

## Environment Variables
Feature flags follow `{FEATURE_NAME}_ENABLED` convention. Common features:
- `LITESTREAM_BACKUP_ENABLED` - Database backups
- Check `app/models/feature.rb` for full list