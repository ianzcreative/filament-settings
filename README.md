# Filament Settings

[![Latest Version on Packagist](https://img.shields.io/packagist/v/ianstudios/filament-settings.svg?style=flat-square)](https://packagist.org/packages/ianstudios/filament-settings)
[![Total Downloads](https://img.shields.io/packagist/dt/ianstudios/filament-settings.svg?style=flat-square)](https://packagist.org/packages/ianstudios/filament-settings)

This package adds a way to interact with key:value in Filament.

## Installation

You can install the package via composer:

```bash
composer require ianstudios/filament-settings
```

Configure the Ianstudios/Settings package as described in the [Settings documentation](https://github.com/ianzcreative/settings).

Add the plugin to your desired Filament panel:

```php
use Ianstudios\FilamentSettings\Filament\Plugins\FilamentSettingsPlugin;

class FilamentPanelProvider extends PanelProvider
{
    public function panel(Panel $panel): Panel
    {
        return $panel
            // ...
            ->plugins([
                FilamentSettingsPlugin::make()
                    ->pages([
                        // Add your own setting pages here
                    ])
            ]);
    }
}
```

## Usage

Create a settings page at 'app/Filament/Pages/Settings/Settings.php':

```php
namespace App\Filament\Pages\Settings;

use Closure;
use Filament\Forms\Components\Tabs;
use Filament\Forms\Components\TextInput;
use Ianstudios\FilamentSettings\Filament\Pages\Settings as BaseSettings;

class Settings extends BaseSettings
{
    public function schema(): array|Closure
    {
        return [
            Tabs::make('Settings')
                ->schema([
                    Tabs\Tab::make('General')
                        ->schema([
                            TextInput::make('general.brand_name')
                                ->required(),
                        ]),
                    Tabs\Tab::make('Seo')
                        ->schema([
                            TextInput::make('seo.title')
                                ->required(),
                            TextInput::make('seo.description')
                                ->required(),
                        ]),
                ]),
        ];
    }
}
```

Register the setting page in the FilamentServiceProvider:

```php
use Ianstudios\FilamentSettings\Filament\Plugins\FilamentSettingsPlugin;

class FilamentPanelProvider extends PanelProvider
{
    public function panel(Panel $panel): Panel
    {
        return $panel
            // ...
            ->plugins([
                FilamentSettingsPlugin::make()
                    ->pages([
                        App\Filament\Pages\Settings\Settings::class,
                    ])
            ]);
    }
}
```

You can add as many setting pages as you want. But when you do, make sure to override the `public static function getNavigationLabel() : string` method on your settings page. This is because multiple pages with the same navigation label will override each other in the Filament navigation.

### Changing the navigation label

You can change the navigation label by overriding the `getNavigationLabel` method:

```php
namespace App\Filament\Pages\Settings;

use Ianstudios\FilamentSettings\Filament\Pages\Settings as BaseSettings;

class Settings extends BaseSettings
{
    public static function getNavigationLabel(): string
    {
        return 'Custom label';
    }
}
```

### Changing the page title

You can change the page title by overriding the `getTitle` method:

```php
namespace App\Filament\Pages\Settings;

use Ianstudios\FilamentSettings\Filament\Pages\Settings as BaseSettings;

class Settings extends BaseSettings
{
    public function getTitle(): string
    {
        return 'Custom title';
    }
}
```

## Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information on what has changed recently.

## Credits

- [All Contributors](../../contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
