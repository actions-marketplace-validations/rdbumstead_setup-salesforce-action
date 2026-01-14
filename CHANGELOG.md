# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.1] - 2026-01-14

### Documentation

- Updated author and support links to point to consulting resources
- Refined "Related Projects" section
- General README formatting improvements

## [1.1.0] - 2026-01-14

### Added

- **Custom Alias Support**: New `alias` input parameter allows users to specify custom org aliases
  - Enables multi-org workflows (authenticate to multiple orgs in same job)
  - Defaults to `TargetOrg` for backward compatibility
  - Useful for data migration, multi-environment validation, and Dev Hub + scratch org workflows

### Changed

- Authentication step now uses custom alias instead of hardcoded values
- Improved authentication command structure with explicit flag passing (fixes potential quoting issues)

### Fixed

- Resolved authentication issue with FLAGS variable expansion
- Improved error handling with proper flag quoting

## [1.0.0] - 2026-01-13

### Added

- Initial release of Setup Salesforce CLI Action
- JWT-based authentication for Salesforce orgs
- Automatic Salesforce CLI installation with Node.js version control
- Intelligent caching for CLI and plugins (3-5x speedup)
- Optional sfdx-git-delta plugin installation (default: off)
- Optional @salesforce/plugin-code-analyzer installation (default: off)
- Optional Prettier installation with Salesforce plugins (default: off)
- Optional ESLint installation with Salesforce plugins (default: off)
- Optional sfdx-lwc-jest installation for LWC testing (default: off)
- Dev Hub configuration support
- Skip authentication option for CLI-only setups
- Automatic org type detection and environment variables
- Composite action pattern for easy reuse

### Features

- Zero-configuration minimal setup (CLI + auth only)
- All plugins and tools optional (install only what you need)
- Secure JWT authentication with automatic key cleanup
- Smart caching with configuration-aware cache keys
- Plugin version checking to avoid redundant installs
- Comprehensive error handling and validation
- Support for both standard orgs and Dev Hubs

### Performance

- Minimal setup: 20 seconds (cached)
- Recommended setup (delta + scanner): 35 seconds (cached)
- Full stack setup (all tools): 45 seconds (cached)
- ~95% cache hit rate in typical CI/CD workflows

### Documentation

- Comprehensive README with multiple use case examples
- Step-by-step Connected App setup guide
- Troubleshooting guide for common issues
- Performance comparison tables
- Complete input reference

[1.1.1]: https://github.com/rdbumstead/setup-salesforce-action/releases/tag/v1.1.1
[1.1.0]: https://github.com/rdbumstead/setup-salesforce-action/releases/tag/v1.1.0
[1.0.0]: https://github.com/rdbumstead/setup-salesforce-action/releases/tag/v1.0.0
