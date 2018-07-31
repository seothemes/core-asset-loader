# Asset Loader

Load theme scripts and stylesheets through configuration.

## Installation

This component should be installed using Composer, with the command `composer require d2/core-asset-loader`.

## Usage

Within your config file (typically found at `config/defaults.php`), use the `AssetLoader::SCRIPTS` and `AssetLoader::STYLES` class constants as array keys to add theme scripts and stylesheets.

Set `AssetLoader::ENQUEUE` to true to immediately enqueue the asset, or set it to false (or omit it) to register the asset to be enqueued later (within a template or partial) using `wp_enqueue_*( 'handle' )`.

When loading JS scripts you can use `AssetLoader::LOCALIZE` to pass data to the individual script using `wp_localize_script`.

For example:

```php
use D2\Core\AssetLoader;

$d2_assets = [
     AssetLoader::SCRIPTS => [
        [
           AssetLoader::HANDLE   => 'generico',
           AssetLoader::URL      => AssetLoader::path( '/assets/js/generico.js' ),
           AssetLoader::DEPS     => [ 'jquery' ],
           AssetLoader::VERSION  => CHILD_THEME_VERSION,
           AssetLoader::FOOTER   => true,
           AssetLoader::ENQUEUE  => true,
           AssetLoader::LOCALIZE => [
               AssetLoader::LOCALIZEVAR  => 'generico_menu_params',
               AssetLoader::LOCALIZEDATA => [
                   'mainMenu'    => __( 'Toggle Menu', 'generico' ),
                   'subMenu'     => __( 'Toggle Submenu', 'generico' ),
                   'menuClasses' => [
                       'combine' => [
                           '.nav-primary',
                       ],
                       'others'  => [],
                   ],
               ]
           ],
        ],
     ],
     AssetLoader::STYLES => [
        [
           AssetLoader::HANDLE   => 'fontawesome',
           AssetLoader::URL      => 'https://stackpath.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css',
           AssetLoader::VERSION  => '4.7.0',
      ],
   ],
];

return [
    AssetLoader::class => $d2_assets,
];
 ```

### Automatic loading of minified files. 

A static function `AssetLoader::path()` is available to optionally generate the full path to your asset. Pass in the path to your asset relative to the theme directory, such as `AssetLoader::path( '/assets/js/generico.js' )`. If a minified version of the file exists in the same directory (and `SCRIPT_DEBUG` is not set to `true`), then the minified file will be used automatically.

## Load the component

Components should be loaded in your theme `functions.php` file, using the `Theme::setup` static method. Code should run on the `after_setup_theme` hook (or `genesis_setup` if you use Genesis Framework).

```php
add_action( 'after_setup_theme', function() {
    $config = include_once __DIR__ . '/config/defaults.php';
    D2\Core\Theme::setup( $config );
} );
```
