
## What does the Base Image provide

In this case there are several docker images provided by the base image. These serve two purposes:

First, they ship upstream utilities and scripts that may be required for building and running your Laravel installation. These are typically placed in the `/lagoon` directory inside the container.

Second, they ship the packages (as a convenience) that are required by the metapackage (Laravel's core files, for example, and any extra packages required by the upstream base image).

## Using the metapackage

The base image exposes the packages it provides via a metapackage (essentially a composer package that wraps several other components).
These include the Laravel core packages and standard utility packages.
This means that you do _not_ have to include Laravel as a direct dependency in your project.

```
"require": {
    "amazeelabs/algm_laravel_baseimage": "*"
},
```

As in this example project's composer.json, you will see that we only require the metapackage above (TODO: add repo information).


Furthermore, in order to ensure that the metapackage is always up to date, you will note that there is a call to the `./lagoon/pre_composer` script in the composer.json file.

```
"pre-install-cmd": [
    "./lagoon/pre_composer"
],
"pre-update-cmd": [
    "./lagoon/pre_composer"
],
```

When working inside the container, this script will run before any install or update composer command and will update the base image to the latest version (along with any of its dependencies).
Forcing an update like this is essential to keep your project's direct dependencies compatible with the metapackage, given that at any point that the base image is updated and redeployed it will attempt to update the metapackage.



## Configuration

The base images provided have provide the default values for the environment variables used by Laravel.

These are values for:

* DB_CONNECTION
* DB_HOST
* DB_PORT
* DB_DATABASE
* DB_USERNAME
* DB_PASSWORD
* REDIS_HOST
* REDIS_PASSWORD
* REDIS_PORT

Ensure that your config files (typically located in `/config`) make use of these by default.

## Queues

If your project makes use of queues, you can make use of the "artisan-worker" service.
This is disabled by default - look at the comments in the docker-compose.yml