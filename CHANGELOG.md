# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-01-12

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

### Fixed

- **Cross-platform compatibility**: Removed `sudo apt-get` dependence to support Windows and macOS runners.
- **Security improvement**: Implemented `trap` for guaranteed JWT key cleanup on success or failure.

### Features

- Zero-configuration minimal setup (CLI + auth only)
- All plugins and tools optional (install only what you need)
- Secure JWT authentication with automatic key cleanup
- Smart caching with configuration-aware cache keys
- Plugin version checking to avoid redundant installs
- Comprehensive error handling and validation
- Support for both standard orgs and Dev Hubs
- Skip auth capability for custom authentication flows

### Performance

- Minimal setup: 20 seconds (cached)
- Recommended setup (delta + scanner): 35 seconds (cached)
- Full stack setup (all tools): 45 seconds (cached)
- ~95% cache hit rate in typical CI/CD workflows
- Conditional plugin installation to skip unnecessary steps

### Documentation

- Comprehensive README with multiple use case examples
- Step-by-step Connected App setup guide
- **Documentation**: Replaced troubleshooting and report bug emojis for better visual clarity.
- Performance comparison tables
- Complete input reference

[1.0.0]: https://github.com/rdbumstead/setup-salesforce-action/releases/tag/v1.0.0
