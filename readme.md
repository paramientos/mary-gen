# MaryGen Documentation

## Table of Contents
1. [Introduction](#introduction)
2. [Features](#features)
3. [Requirements](#requirements)
4. [Installation](#installation)
5. [Configuration](#configuration)
6. [Usage](#usage)
7. [Command Structure](#command-structure)
8. [Generated Components](#generated-components)
9. [Customization](#customization)
10. [Translation Feature](#translation-feature)
11. [Troubleshooting](#troubleshooting)
12. [Contributing](#contributing)
13. [License](#license)
14. [Support](#support)

## Introduction

MaryGen is a powerful Laravel package designed to streamline the process of generating MaryUI components and Livewire pages for your Laravel models. It automates the creation of CRUD (Create, Read, Update, Delete) interfaces, saving developers significant time and effort in setting up admin panels or data management systems.

## Features

- Automatic generation of MaryUI components for Laravel models
- Creation of Livewire pages with full CRUD functionality
- Intelligent form field generation based on database schema
- Automatic table column generation
- Built-in sorting, pagination, and search capabilities
- Easy customization options
- Automatic route generation
- Translation support for generated content (new in version 0.35.0)

## Requirements

- Laravel 8.x or higher
- PHP 7.4 or higher
- [MaryUI](https://github.com/robsontenorio/mary) package
- [Livewire Volt](https://livewire.laravel.com/docs/volt) package

## Installation

1. Ensure you have a Laravel project set up.

2. Install the MaryUI:

   https://mary-ui.com/docs/installation


3. Install the Livewire, Livewire Volt packages (if not already installed with MaryUI):
   ```bash
   composer require livewire/livewire livewire/volt && php artisan volt:install
   ```

4. Install the MaryGen package:
   ```bash
   composer require soysaltan/marygen --dev
   ```

5. (Optional) Publish the configuration file:
   ```bash
   php artisan vendor:publish --provider="SoysalTan\MaryGen\MaryGenServiceProvider" --tag="config"
   ```

## Configuration

After publishing the configuration file, you can modify `config/marygen.php` to customize the package behavior:

```php
return [
    'model_namespace' => 'App\Models',
    'use_mg_like_eloquent_directive' => true,
];
```

- `model_namespace`: Define the namespace for your models. Default is `App\Models`.
- `use_mg_like_eloquent_directive`: Determine whether to use the MgLike Eloquent directive for search functionality. For example:

```php
$q->mgLike(['id', 'username', 'email', 'password', 'name', 'lastname', 'title', 'phone', 'avatar', 'time_zone', 'last_login_at', 'status', 'created_at', 'updated_at'], $this->search))
```

## Usage

To generate a MaryUI component and Livewire page for a model, use the following command:

```bash
php artisan marygen:make {--m|model=} {--w|view=} {--d|dest_lang=} {--s|source_lang=}
```

- `--m|model`: The name of the model for which you want to generate the components.
- `--w|view`: (Optional) The name of the view file. If not provided, it will use the lowercase model name.
- `--d|dest_lang`: (Required if source_lang presents) The destination language code for translation. 
- `--s|source_lang`: (Optional) The source language code for translation.If not present, it detects the source language automatically.

Example:
```bash
php artisan marygen:make --model=User --view=admin-users --dest_lang=es
```

This command will generate a Livewire page for the User model with CRUD functionality, name the view file `admin-users.blade.php`, and translate the content from English to Spanish.

## Command Structure

The `MaryGenCommand` class is the core of MaryGen. Here's an overview of its main methods:

- `handle()`: Orchestrates the entire generation process.
- `checkPackageInComposerJson()`: Verifies the presence of required packages.
- `generateFormFields()`: Creates form fields based on the model's table structure.
- `getIconForColumn()`: Assigns appropriate icons to form fields.
- `getMaryUIComponent()`: Maps database column types to MaryUI components.
- `generateTableColumns()`: Builds table columns for displaying model data.
- `generateLivewirePage()`: Produces the Livewire component with CRUD functionality.
- `updateRoute()`: Adds a route for the newly created page.

## Generated Components

MaryGen generates the following components:

1. A Livewire component class (`app/Livewire/{ModelName}.php`) with:
   - CRUD operations (create, read, update, delete)
   - Sorting functionality
   - Pagination
   - Search capability
   - Data validation

2. A Blade view (`resources/views/livewire/{model-name}.blade.php`) with:
   - A responsive table displaying the model data
   - Modals for creating and editing records
   - A search input field
   - Pagination controls

## Customization

You can customize the generated components by modifying the following methods in the `MaryGenCommand` class:

- `getMaryUIComponent()`: Adjust the mapping between database column types and MaryUI components.
- `getIconForColumn()`: Modify the icon selection for form fields.
- `generateLivewirePage()`: Customize the structure of the generated Livewire component and Blade view.

Additionally, you can edit the generated files directly to further tailor them to your specific needs.

## Translation Feature

MaryGen includes a translation feature that allows you to generate content in different languages. This feature uses the Google Translate API to translate field names, labels, and other text elements in the generated components.
It uses `stichoza/google-translate-php` package. (https://github.com/Stichoza/google-translate-php)
To use the translation feature:

1. Specify the destination language using the `--dest_lang` option when running the `marygen:make` command.
2. Optionally, specify the source language using the `--source_lang` option. If not provided, Google Translate will attempt to auto-detect the source language.

Example:
```bash
php artisan marygen:make --model=Product --view=product-management --dest_lang=fr --source_lang=en
```

This command will generate the components for the Product model and translate the content from English to French.

Note: The translation feature requires an active internet connection to communicate with the Google Translate API.

## Troubleshooting

Common issues and their solutions:

1. **MaryUI package not found**:
   - Error: `MaryUI package not found! Please install using: 'composer req robsontenorio/mary'`
   - Solution: Run `composer require robsontenorio/mary` to install the MaryUI package.

2. **Livewire Volt package not found**:
   - Error: `Livewire Volt package not found! Please see doc: 'https://livewire.laravel.com/docs/volt#installation'`
   - Solution: Install Livewire Volt using `composer require livewire/livewire livewire/volt && php artisan volt:install`.

3. **Model not found**:
   - Error: `Model {modelName} does not exist!`
   - Solution: Ensure the specified model exists in your model namespace (default: `App\Models`).

4. **View file already exists**:
   - Error: `File {viewName}.blade.php already exists!`
   - Solution: Choose a different name for your view or manually delete the existing file if you want to overwrite it.

5. **Translation errors**:
   - Error: Various Google Translate API errors
   - Solution: Ensure you have an active internet connection and that the language codes you're using are valid. Check the Google Translate documentation for supported language codes.

## Contributing

Contributions to MaryGen are welcome! Here's how you can contribute:

1. Fork the repository
2. Create a new branch for your feature or bug fix
3. Write your code and tests
4. Submit a pull request with a clear description of your changes

Please ensure your code adheres to the existing style conventions and includes appropriate tests.

## License

MaryGen is open-source software licensed under the MIT license.

## Support

For more information or support:
- Open an issue on the [GitHub repository](https://github.com/soysaltan/mary-gen)
- Contact the package maintainer through the repository's contact information

---

For the latest updates and more detailed information about MaryUI, please visit the [official MaryUI documentation](https://mary-ui.com/docs/installation).
