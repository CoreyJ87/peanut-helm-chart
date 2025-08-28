# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.1.1] - 2025-01-27

### Fixed
- Update chart-releaser action configuration to properly detect charts
- Fix charts directory path for automated releases

## [0.1.0] - 2025-01-27

### Added
- Initial Helm chart for PeaNUT
- Support for PeaNUT environment variables configuration
- Persistent volume support for configuration storage
- Ingress configuration with external-dns annotations
- Resource limits and requests
- Service account creation
- Comprehensive README with usage examples
- Apache 2.0 license
- Chart linting and validation
- Support for customizable storage classes

### Features
- Deploy PeaNUT UPS monitoring dashboard on Kubernetes
- Configurable web authentication
- Persistent configuration storage
- Ingress support for external access
- Resource management
- Health checks (liveness and readiness probes)

[Unreleased]: https://github.com/CoreyJ87/peanut-helm-chart/compare/v0.1.1...HEAD
[0.1.1]: https://github.com/CoreyJ87/peanut-helm-chart/compare/v0.1.0...v0.1.1
[0.1.0]: https://github.com/CoreyJ87/peanut-helm-chart/releases/tag/v0.1.0
