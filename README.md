# Magento 2 SEO Robots Pack

Complete SEO robots meta tag and X-Robots-Tag HTTP header management for Magento 2 with flexible directive management and comprehensive Google robots directive support.

## Overview

This metapackage provides a complete solution for managing robots meta tags and X-Robots-Tag HTTP headers in Magento 2. Version 2.0 introduces flexible directive management, supporting all Google robots directives with independent configuration for meta tags and HTTP headers.

## Included Packages

### Core Modules
- **hryvinskyi/magento2-seo-robots-api** (v2.0.0) - API interfaces and contracts
- **hryvinskyi/magento2-seo-robots** (v2.0.0) - Core robots functionality with validation and migration
- **hryvinskyi/magento2-seo-robots-admin-ui** (v2.0.0) - Enhanced admin UI with multiselect directives
- **hryvinskyi/magento2-seo-robots-frontend** (v2.0.0) - Frontend application with independent X-Robots-Tag support

## Key Features

### Flexible Directive Management
- **No limitations** - Combine any robots directives freely (not limited to 8 predefined options like v1.x)
- **Multiselect UI** - Hold Ctrl/Cmd to select multiple directives in admin
- **Comprehensive validation** - Automatically detects conflicting directives (e.g., index + noindex)
- **Pattern-based rules** - Configure directives per URL pattern with priority system

## Demo
![configuration_settings.gif](docs/images/configuration_settings.gif)

### Supported Robots Directives

#### Basic Directives (Select Multiple)
- `index` / `noindex` - Control page indexing
- `follow` / `nofollow` - Control link following
- `noarchive` - Prevent cached copy
- `nosnippet` - Prevent snippet display in search results
- `notranslate` - Prevent translation offers
- `noimageindex` - Prevent image indexing
- `none` - Equivalent to noindex,nofollow
- `all` - Equivalent to index,follow (default)
- `max-snippet:[number]` - Maximum snippet length (-1 for no limit)
- `max-image-preview:[none|standard|large]` - Maximum image preview size
- `max-video-preview:[seconds]` - Maximum video preview duration (-1 for no limit)
- `unavailable_after:[date]` - Content removal date (RFC 850 format)

### X-Robots-Tag Configuration
- Configure X-Robots-Tag HTTP header separately from meta robots tag
- Set different directives for HTTP header vs HTML meta tag
- Automatic fallback to meta robots if no independent configuration

### URL Pattern-Based Control
- Define robots directives per URL pattern (e.g., `*/product/*`, `catalog_product_view`)
- Priority-based matching (highest priority wins)
- Supports wildcards and full action names

### HTTPS & Pagination Support
- Separate directive configuration for HTTPS pages
- Automatic robots directives for paginated content (?p= parameter)
- Special handling for 404 pages (automatic NOINDEX,NOFOLLOW)

## Installation

```bash
composer require hryvinskyi/magento2-seo-robots-pack:^2.0
php bin/magento module:enable Hryvinskyi_SeoRobotsApi Hryvinskyi_SeoRobots Hryvinskyi_SeoRobotsAdminUi Hryvinskyi_SeoRobotsFrontend
php bin/magento setup:upgrade
php bin/magento setup:di:compile
php bin/magento cache:flush
```

## Configuration

Navigate to **Stores > Configuration > Hryvinskyi SEO > Meta Robots**

### Basic Configuration
1. **Enabled**: Set to "Yes" to enable robots management
2. **Robots Meta Header Rules**: Configure pattern-based rules
   - **Priority**: Higher priority rules match first
   - **URL Pattern**: Use wildcards (e.g., `*/customer/*`) or action names (e.g., `catalog_category_view`)
   - **Meta Robots Directives**: Select multiple directives (hold Ctrl/Cmd)
   - **X-Robots-Tag Directives**: Optionally configure independent directives for HTTP header
3. **X-Robots-Tag Rules (Independent)**: Separate rules for X-Robots-Tag header only
4. **Robots Directives for HTTPS Pages**: Multiselect directives for HTTPS
5. **Set NOINDEX,NOFOLLOW for 404 page**: Automatically apply to error pages
6. **Paginated Content**: Configure robots for pages with ?p= parameter
7. **Enable X-Robots-Tag Header**: Enable/disable HTTP header

### Configuration Examples

#### Example 1: Noindex Product Pages
- Pattern: `catalog_product_view`
- Meta Directives: `noindex`, `follow`
- Result: `<meta name="robots" content="NOINDEX,FOLLOW">`

#### Example 2: X-Robots-Tag
- Pattern: `cms_page_view`
- X-Robots Directives: `index`, `follow`
- Result:
  - HTTP Header: `X-Robots-Tag: INDEX,FOLLOW`

#### Example 3: Customer Account Protection
- Pattern: `*/customer/account/*`
- Meta Directives: `noindex`, `nofollow`, `noarchive`
- Result: Full protection from search engines

## Migration from v1.x

### Automatic Migration

Version 2.0 includes automatic migration via Setup Patch:

```php
\Hryvinskyi\SeoRobots\Setup\Patch\Data\MigrateRobotsConfiguration
```

**What Gets Migrated:**
- Old code-based configurations (1-8) automatically convert to directive arrays:
  - Code 1 (NOINDEX_NOFOLLOW) → `['noindex', 'nofollow']`
  - Code 4 (INDEX_FOLLOW) → `['index', 'follow']`
  - etc.
- `https_meta_robots` setting
- `paginated_robots_type` setting
- All URL patterns and priorities preserved

**No Manual Steps Required** - Simply run `setup:upgrade`

### Breaking Changes

1. **API Changes**: `RobotsListInterface::getMetaRobotsByCode()` deprecated (still works)
2. **Return Types**: `getHttpsMetaRobots()` and `getPaginatedMetaRobots()` now return arrays instead of integers
3. **Configuration Format**: Stored as directive arrays instead of integer codes

## API Usage

### Building Robots String from Directives

```php
use Hryvinskyi\SeoRobotsApi\Api\RobotsListInterface;

class YourClass
{
    private $robotsList;

    public function __construct(RobotsListInterface $robotsList)
    {
        $this->robotsList = $robotsList;
    }

    public function example()
    {
        // Build robots string from directive array
        $directives = ['noindex', 'follow', 'noarchive'];
        $robotsString = $this->robotsList->buildMetaRobotsFromDirectives($directives);
        // Result: "NOINDEX,FOLLOW,NOARCHIVE"
    }
}
```

### Validating Directives

```php
// Validate directive array for conflicts
$directives = ['index', 'noindex']; // Conflicting!
$validation = $this->robotsList->validateDirectives($directives);

if (!$validation['valid']) {
    foreach ($validation['errors'] as $error) {
        // Handle error: "Conflicting directives: index and noindex cannot be used together"
    }
}
```

### Getting Available Directives

```php
// Get all basic directives
$basicDirectives = $this->robotsList->getBasicDirectives();
// Returns: ['index', 'noindex', 'follow', 'nofollow', 'noarchive', ...]

// Get advanced directives
$advancedDirectives = $this->robotsList->getAdvancedDirectives();
// Returns: ['max-snippet', 'max-image-preview', ...]
```

## Extension Points

### Custom Robots Providers

Implement `RobotsProviderInterface` to provide custom robots logic:

```php
namespace Vendor\Module\Model;

use Hryvinskyi\SeoRobotsFrontend\Model\RobotsProviderInterface;
use Magento\Framework\App\RequestInterface as HttpRequestInterface;

class CustomRobotsProvider implements RobotsProviderInterface
{
    public function getRobots(HttpRequestInterface $request): ?string
    {
        // Your custom logic
        if ($request->getParam('custom_param')) {
            return 'NOINDEX,FOLLOW';
        }
        return null;
    }

    public function getSortOrder(): int
    {
        return 100; // Execution order
    }
}
```

Register in `di.xml`:

```xml
<type name="Hryvinskyi\SeoRobotsFrontend\Model\ApplyRobots">
    <arguments>
        <argument name="robotsProviders" xsi:type="array">
            <item name="custom" xsi:type="object">Vendor\Module\Model\CustomRobotsProvider</item>
        </argument>
    </arguments>
</type>
```

## Requirements

- PHP 8.1, 8.2, or 8.3
- Magento 2.4.x
- Composer

## Troubleshooting

### Configuration Not Applying

1. Clear Magento cache: `php bin/magento cache:flush`
2. Verify module is enabled: `php bin/magento module:status`
3. Check pattern matching - use action names instead of URL paths
4. Verify priority - higher priority rules override lower ones

### Migration Issues

If automatic migration fails:
1. Backup your configuration: `app/etc/config.php`
2. Manually convert using the code-to-directive mapping in CHANGELOG.md
3. Report issue at: https://github.com/hryvinskyi/magento2-seo-robots-pack/issues

### Directive Conflicts

Valid combinations:
- ✅ noindex, follow
- ✅ index, nofollow, noarchive
- ❌ index, noindex (conflicting)
- ❌ follow, nofollow (conflicting)

## Support

- Report issues: https://github.com/hryvinskyi/magento2-seo-robots-pack/issues
- Email: volodymyr@hryvinskyi.com

## Author

**Volodymyr Hryvinskyi**
- Email: volodymyr@hryvinskyi.com
- Website: https://hryvinskyi.com

## License

MIT License - see LICENSE file for details

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for detailed version history.
