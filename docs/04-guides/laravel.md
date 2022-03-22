---
sidebar_label: Laravel
title: Deploy a Laravel app
description: How to deploy a Laravel app on your server.
---

This guide outlines the steps you need to follow to deploy a Laravel app on Easypanel.

## Create Procfile

To specify how the app should start, you need to create a `Procfile` file at the root of your project with the following contents.

For Apache server use:

```
web: php artisan migrate -f && heroku-php-apache2 public/
```

For Nginx server use:

```
web: php artisan migrate -f && heroku-php-nginx public/
```

For built-in server use:

```
web: php artisan migrate -f && php artisan serve
```

## Configure Trusted Proxies

Your app will sit behind a reverse proxy server. That's why you need to configure Laravel to trust that proxy.

```php
<?php

namespace App\Http\Middleware;

use Illuminate\Http\Middleware\TrustProxies as Middleware;
use Illuminate\Http\Request;

class TrustProxies extends Middleware
{
    /**
     * The trusted proxies for this application.
     *
     * @var string|array
     */
    protected $proxies = '*';

    // ...
}
```

## Setup Database Services

Before deploying your Laravel app, you should create a database service like **MySQL**, **Postgres**, and **Redis**. Create the ones you need and keep their credentials handy because you will need them later.

## Setup App Service

1. Create a new **App** service

   The **name** of the service is arbitrary. The **domain name** is where your app will live.

2. Connect Github repository

   You can do this in the **Source** panel from the **General** tab.

3. Setup environment variables

   You can use your `.env.example` as a starting point. To connect your app to the database services, use the credentials found on each service.

   A quick way to configure a database is to use the **Connection URL** in the `DATABASE_URL` variable.

4. Configure persistent storage

   In the advanced tab, you can find the volumes section. Add the following

   | Type   | Source    | Target     |
   | ------ | --------- | ---------- |
   | Volume | `storage` | `/storage` |

5. Click the **Deploy** button

## FAQ

### How to specify the PHP version?

TODO: Link to the version section in Languages -> PHP.

### How to change the PHP settings?

TODO: Link to the version section in Languages -> Runtime Settings.