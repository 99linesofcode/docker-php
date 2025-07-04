# docker-php

Docker PHP is a collection of services and containers intended to be pulled into a project as a submodule or subtree. It contains all the services necessary to serve a modern Laravel application for development purposes. It's very similar to what Laravel Sail provides but is further configured to suit my personal needs and preferences and comes with a dedicated Nginx container to run your application in.

Additionally, this repository contains a production ready FrankenPHP container that is preconfigured to automatically request SSL certificates using a Cloudflare DNS challenge. To be found in the `frankenphp/` subdirectory.

## Configuration

1. Run `git submodule add https://github.com/99linesofcode/docker-php.git docker` from your application root;
1. Copy the `docker-compose.yml.dist` to the application root;
1. Configure the environment variables defined in the `.env.example` file in your `.env` in the application root;
1. Run `docker compose up -d` to spin up your development environment.

Almost all the relevant configuration is done in/from the `docker-compose.yml` file. Optional services are disabled by default. You can enable these by uncommenting their respective service blocks.

## Production

There are several ways to run Docker PHP in production. Using `docker compose -d` similarly to how you run it in development or by manually building and packaging the application up into a docker image and pushing it to and pulling it from a container registry.

The FrankenPHP container uses the multi-stage build process and can be further optimized for production. If you intend to use `docker compose` you will have to change the `services.frankenphp.build.target` to `production`.

If you choose to package your application and serve it differently, make sure to pass the `--target production` flag to your `docker build` command like so: `docker build --target production -t frankenphp:production -f ./frankenphp/Dockerfile .`.

## Contributing to docker-php

Thank you for considering contributing to docker-php. Please review our [Contribution Guidelines](https://github.com/99linesofcode/.github/blob/main/.github/CONTRIBUTING.md).

## Code of Conduct

In order to ensure that the community is welcoming to all, please review and abide by the [Code of Conduct](https://github.com/99linesofcode/.github?tab=coc-ov-file).

## Security Vulnerabilities

Please review [our security policy](https://github.com/99linesofcode/.github?tab=security-ov-file) on how to report security vulnerabilities.

## License

docker-php is open-sourced software licensed under the [MIT license](https://github.com/99linesofcode/docker-php?tab=MIT-1-ov-file)
