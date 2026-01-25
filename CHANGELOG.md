# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.0.2] - 2026-01-25

### Changed
- Updated `hryvinskyi/magento2-seo-robots` to 2.0.2
- Updated `hryvinskyi/magento2-seo-robots-admin-ui` to 2.0.2
- Updated `hryvinskyi/magento2-seo-robots-frontend` to 2.0.2

### Fixed
- Fixed pagination check to apply robots tag for all paginated pages (including ?p=1)
- Fixed disabled state handling in DirectiveMultiselect admin UI
- Fixed robots directive formatting to use comma without space separator

## [2.0.0] - 2026-01-11

### BREAKING CHANGES
- Complete rewrite of robots directive system from hardcoded 8-option combinations to flexible directive arrays
- Changed API interfaces (RobotsListInterface) - added new methods, deprecated old ones
- Configuration storage format changed (automatic migration via setup patch included)
- Updated all module versions to 2.0.0
- Requires PHP 7.4+ and Magento 2.4.x

### Added
- Support for ALL Google robots directives including:
  - Basic directives: index, noindex, follow, nofollow, noarchive, nosnippet, notranslate, noimageindex, none, all
  - Advanced directives with values: max-snippet:[N], max-image-preview:[none|standard|large], max-video-preview:[N], unavailable_after:[date]
- Multiselect UI for directive selection
- Independent X-Robots-Tag HTTP header configuration (separate from meta robots)
- Flexible directive combination - no longer limited to 8 predefined options
- Comprehensive validation for directive conflicts and formats
- Automatic migration from v1.x configuration (no manual reconfiguration needed)
- New API methods in RobotsListInterface:
  - buildMetaRobotsFromDirectives() - converts directive array to robots string
  - getBasicDirectives() - returns all available basic directives
  - getAdvancedDirectives() - returns advanced directives
  - validateDirectives() - validates directive array for conflicts
- New configuration methods in ConfigInterface:
  - getXRobotsRules() - independent X-Robots-Tag rules
  - getHttpsXRobotsDirectives() - HTTPS-specific X-Robots directives
  - getPaginatedMetaRobots() now returns array instead of integer

### Changed
- Replaced dropdown directive selector with multiselect interface
- Meta robots and X-Robots-Tag now independently configurable
- Improved admin UI organization and usability
- Better error messages and validation feedback
- Enhanced documentation with migration guide
- Updated dependencies: all modules now require ^2.0.0 versions

### Deprecated
- RobotsListInterface::getMetaRobotsByCode() - still works for backward compatibility but use buildMetaRobotsFromDirectives() instead
- Old numbered constants (NOINDEX_NOFOLLOW = 1, etc.) - marked as deprecated, use directive arrays instead

### Removed
- Hardcoded 8-option limitation
- Old MetaRobots source model (replaced with DirectiveList)
- Old admin UI blocks (replaced with RobotsRules)

### Fixed
- Return type inconsistencies in Config model methods
- Directive validation edge cases
- Migration data loss scenarios

### Migration
- Automatic configuration migration via Setup Patch (MigrateRobotsConfiguration)
- Old code-based configurations automatically converted to new directive array format
- No manual intervention required for existing installations
- Backward compatibility maintained where possible

## [1.0.7] - 2026-01-11 (Previous)

### Added
- X-Robots-Tag HTTP header support (optional, configurable)
- New configuration option to enable/disable X-Robots-Tag header

### Changed
- Updated `hryvinskyi/magento2-seo-robots-api` to 1.0.2
- Updated `hryvinskyi/magento2-seo-robots` to 1.0.5
- Updated `hryvinskyi/magento2-seo-robots-admin-ui` to 1.0.4
- Updated `hryvinskyi/magento2-seo-robots-frontend` to 1.0.5

### Fixed
- Corrected configuration path in documentation

## [1.0.6] - 2025-10-08

### Added
- Added `hryvinskyi/magento2-seo-robots-category-pack` suggestion in composer.json

### Features in Category Pack
- Category page robots meta tag control
- Apply robots settings to products in a category
- Product-specific robots directives via categories

## [1.0.5] - Previous Release

### Added
- Base SEO Robots functionality
- Admin UI components
- Frontend robots meta tag application
- API interfaces and contracts
