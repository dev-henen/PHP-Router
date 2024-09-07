# PHP Simple Router

## Overview

This PHP Simple Router is a lightweight routing system for handling HTTP requests. It provides features like middleware, filters, parameterized routes, named routes, and custom error handling. This router helps in managing and organizing your PHP application's routes efficiently.

## Features

- **Middleware Support**: Add middleware that runs before route callbacks.
- **Filters**: Add before and after filters to execute code on specific URL patterns.
- **Parameterized Routes**: Support for dynamic URL parameters.
- **Named Routes**: Easily generate URLs for named routes.
- **Custom Error Handling**: Customizable 404 error page.
- **PSR-7 Compatibility**: Structure ready for PSR-7 compliance.

## Installation

1. Clone or download the repository.
2. Include the `Route` class in your PHP project.

```php
require_once 'Route.php';
```

## Usage

### Initialize the Router

Create an instance of the `Route` class:

```php
$route = new Route();
```

### Functions

#### `addFilter`

Adds a filter that executes code before or after route callbacks based on URL patterns.

**Syntax:**
```php
public function addFilter(string $pattern, callable $filter, string $type = 'before')
```

**Parameters:**
- `pattern`: URL pattern to match.
- `filter`: Callable function to execute.
- `type`: Specifies whether the filter is `before` or `after` the route callback. Default is `before`.

**Example:**
```php
$route->addFilter('*', function() {
    echo "Global before filter<br>";
});
```

#### `for`

Defines a route for an exact URL match.

**Syntax:**
```php
public function for(string $request_url, callable $callback, string $method = 'GET', ?string $name = null)
```

**Parameters:**
- `request_url`: The exact URL to match.
- `callback`: Callable function to execute when the URL matches.
- `method`: HTTP method (e.g., GET, POST). Default is 'GET'.
- `name`: Optional name for the route.

**Example:**
```php
$route->for('/home', function() {
    echo "Home Page<br>";
}, 'GET', 'home');
```

#### `match`

Defines a route with a URL pattern that includes dynamic parameters.

**Syntax:**
```php
public function match(string $url_matching_pattern, callable $callback, string $method = 'GET', ?string $name = null)
```

**Parameters:**
- `url_matching_pattern`: The URL pattern with placeholders for parameters.
- `callback`: Callable function to execute when the URL matches.
- `method`: HTTP method (e.g., GET, POST). Default is 'GET'.
- `name`: Optional name for the route.

**Example:**
```php
$route->match('/user/{id}', function($params) {
    echo "User Page for user with ID: " . $params['id'] . "<br>";
}, 'GET', 'user_profile');
```

#### `error_page`

Defines a custom error page for handling 404 errors.

**Syntax:**
```php
public function error_page(callable $callback)
```

**Parameters:**
- `callback`: Callable function to execute when no routes match the request URL.

**Example:**
```php
$route->error_page(function() {
    echo "Custom 404 Page Not Found<br>";
});
```

#### `useMiddleware`

Adds middleware that runs before route callbacks.

**Syntax:**
```php
public function useMiddleware(callable $middleware)
```

**Parameters:**
- `middleware`: Callable function to execute as middleware.

**Example:**
```php
$route->useMiddleware(function() {
    echo "Running middleware<br>";
});
```

#### `run`

Processes the incoming request and executes the corresponding route callback, middleware, and filters.

**Syntax:**
```php
public function run()
```

**Example:**
```php
$route->run();
```

#### `generateUrl`

Generates a URL for a named route.

**Syntax:**
```php
public function generateUrl(string $name, array $params = []): string
```

**Parameters:**
- `name`: Name of the route.
- `params`: Array of parameters to replace in the URL pattern.

**Returns:**
- Generated URL as a string.

**Example:**
```php
echo $route->generateUrl('user_profile', ['id' => 42]);
```

## Complete Example

Here's a complete example demonstrating all the features:

```php
<?php

require_once 'Route.php';

$route = new Route();

$route->addFilter('*', function() {
    echo "Global before filter<br>";
});

$route->addFilter('/admin/*', function() {
    echo "Admin before filter<br>";
});

$route->addFilter('/admin/*', function() {
    echo "Admin after filter<br>";
}, 'after');

$route->for('/home', function() {
    echo "Home Page<br>";
}, 'GET', 'home');

$route->match('/user/{id}', function($params) {
    echo "User Page for user with ID: " . $params['id'] . "<br>";
}, 'GET', 'user_profile');

$route->error_page(function() {
    echo "Custom 404 Page Not Found<br>";
});

$route->useMiddleware(function() {
    echo "Running middleware<br>";
});

$route->run();

echo $route->generateUrl('user_profile', ['id' => 42]);
```

## Notes

- Ensure your server environment correctly passes the `REQUEST_URI` and `REQUEST_METHOD` server variables.
- Make sure that the URL patterns in your `for` and `match` methods correctly match the incoming requests.

## License

This project is licensed under the MIT License.

## Contributing

Feel free to fork this repository and contribute by submitting a pull request. For major changes, please open an issue first to discuss what you would like to change.

## Contact

For any questions or feedback, please open an issue on the GitHub repository.