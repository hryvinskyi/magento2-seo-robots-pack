# Magento 2 SEO Robots Pack

SEO robots meta tags management solution for Magento 2.

## Overview

This metapackage installs all components for comprehensive robots meta tag management in Magento 2, including base functionality and advanced category-based control.

## Included Packages

### Base SEO Robots Functionality
- **SeoRobotsApi** - API interfaces and contracts
- **SeoRobots** - Core robots functionality
- **SeoRobotsAdminUi** - Admin panel UI components
- **SeoRobotsFrontend** - Frontend robots meta tag application with extension points

### Category-Based Robots Management (Suggested)
- **SeoRobotsCategoryPack** - Complete category robots solution including:
  - SeoRobotsCategoryApi - Category-specific API interfaces
  - SeoRobotsCategory - Category robots core functionality
  - SeoRobotsCategoryAdminUi - Category admin UI
  - SeoRobotsCategoryFrontend - Category frontend integration

## Features

### Base Features
- Global robots meta tag configuration
- X-Robots-Tag HTTP header support
- URL pattern-based robots control
- Action-based robots control
- Extension point for custom robots providers

## Installation

```bash
composer require hryvinskyi/magento2-seo-robots-pack
php bin/magento module:enable Hryvinskyi_SeoRobotsApi Hryvinskyi_SeoRobots Hryvinskyi_SeoRobotsAdminUi Hryvinskyi_SeoRobotsFrontend Hryvinskyi_SeoRobotsCategoryApi Hryvinskyi_SeoRobotsCategory Hryvinskyi_SeoRobotsCategoryAdminUi Hryvinskyi_SeoRobotsCategoryFrontend
php bin/magento setup:upgrade
php bin/magento cache:flush
```

## Configuration

### Global Configuration
Navigate to **Stores > Configuration > Hryvinskyi SEO > Robots**

## Available Robots Directives

- INDEX, FOLLOW
- NOINDEX, FOLLOW
- INDEX, NOFOLLOW
- NOINDEX, NOFOLLOW
- INDEX, FOLLOW, NOARCHIVE
- NOINDEX, FOLLOW, NOARCHIVE
- INDEX, NOFOLLOW, NOARCHIVE
- NOINDEX, NOFOLLOW, NOARCHIVE

## Extension Points

The package provides extension points for custom robots providers. See https://github.com/hryvinskyi/magento2-seo-robots-frontend?tab=readme-ov-file#how-to-extend for implementation details.

## Requirements

- PHP ^7.4|^8.0|^8.1|^8.2
- Magento 2.4.x

## Author

**Volodymyr Hryvinskyi**
- Email: volodymyr@hryvinskyi.com

## License

MIT
