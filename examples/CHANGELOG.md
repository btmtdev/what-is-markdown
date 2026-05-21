# Changelog

All notable changes to this project will be documented in this file.

Format based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- Dark mode support

## [1.2.0] - 2026-05-20

### Added
- User authentication with OAuth2
- Rate limiting on API endpoints

### Changed
- Upgraded database driver to v5

### Fixed
- Memory leak in WebSocket handler

## [1.1.0] - 2026-04-15

### Added
- Search functionality
- Export to CSV

### Deprecated
- Legacy `/api/v1/users` endpoint (use `/api/v2/users`)

## [1.0.0] - 2026-03-01

### Added
- Initial release
- User CRUD operations
- REST API
- Basic UI dashboard

[Unreleased]: https://github.com/username/my-project/compare/v1.2.0...HEAD
[1.2.0]: https://github.com/username/my-project/compare/v1.1.0...v1.2.0
[1.1.0]: https://github.com/username/my-project/compare/v1.0.0...v1.1.0
[1.0.0]: https://github.com/username/my-project/releases/tag/v1.0.0
