# Tenant Breeze (Independent Edition)

Tenant Breeze is a modified version of Laravel Breeze 1.x adapted for multi-tenant applications. It focuses on identifying tenants during the login process using session-based data.

## Features

- 🔐 **Tenant-Aware Login**: Automatically manages a `tenant_path` field in the login form.
- 🏢 **Session-Based Identification**: 
    - If a tenant is already identified in the session, the `tenant_path` is hidden and pre-filled.
    - If no tenant is identified, a visible text input is shown to the user.
- 🚫 **Registration Disabled**: Registration features (routes, controllers, views) have been removed.
- 🧱 **Package Agnostic**: Does not depend on any specific multi-tenancy package, relying only on standard Laravel session mechanisms.

## Requirements

- PHP ^8.1
- Laravel ^10.0 | ^11.0 | ^12.0

## Installation

Install the package via composer:

```bash
composer require rboschin/tenant-breeze --dev
```

After installing the package, run the `breeze:install` command:

```bash
php artisan breeze:install blade
```

## Session Configuration

To interoperate with this package, your tenant identification logic (e.g., middleware) should store the tenant data in the session using the key `simpletenant_tenant`.

### Expected Session Structure

```php
session(['simpletenant_tenant' => [
    'slug' => 'tenant-identifier',
    'name' => 'Tenant Name',
    // ... any other metadata
]]);
```

### Logout Behavior

Upon logout, the standard `$request->session()->invalidate()` call will destroy the entire session, including the `simpletenant_tenant` data, ensuring a clean state for the next request.

## Implementation Details

### Login Form
The login form (`login.blade.php`) uses the following logic:
- It checks for the existence of `session('simpletenant_tenant')`.
- It uses the field name `tenant_path` for the request parameter.
- It attempts to pull the `slug` from the session data to pre-fill the field.

## License

Tenant Breeze is open-sourced software licensed under the [MIT license](LICENSE.md).
