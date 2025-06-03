# docker-php

Docker PHP is a collection of services and containers intended to be pulled into a project as a submodule or subtree. It contains all the services necessary to serve a modern Laravel application for development purposes. It's very similar to what Laravel Sail provides but is further configured to suit my personal needs and preferences and comes with a dedicated Nginx container to run your application in.

Additionally, this repository contains a production ready FrankenPHP container that is preconfigured to automatically request SSL certificates using a Cloudflare DNS challenge. See the `frankenphp/` subdirectory.

## Configuration

1. Run `git submodule add https://github.com/99linesofcode/docker-php.git docker` from your application root;
1. Copy the `docker-compose.yml` to the application root;
1. Configure the environment variables defined in the `.env.example` file in your `.env` in the application root;
1. Run `docker compose up -d` to spin up your development environment.

Almost all the relevant configuration is done in/from the `docker-compose.yml` file. By default the devcontainer is disabled.

## Contributing to docker-base

Thank you for considering contributing to docker-base. Please review our [Contribution Guidelines](https://github.com/99linesofcode/.github/blob/main/.github/CONTRIBUTING.md).

## Code of Conduct

In order to ensure that the community is welcoming to all, please review and abide by the [Code of Conduct](https://github.com/99linesofcode/.github?tab=coc-ov-file).

## Security Vulnerabilities

Please review [our security policy](https://github.com/99linesofcode/.github?tab=security-ov-file) on how to report security vulnerabilities.

## License

docker-base is open-sourced software licensed under the [MIT license](https://github.com/99linesofcode/docker-base?tab=MIT-1-ov-file)
