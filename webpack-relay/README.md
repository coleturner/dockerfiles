# Webpack + Relay
Runs Webpack Dev Server binding to 0.0.0.0 with Watchman for rebuilding GraphQL Schema when source files change. Configured by default to work with `graphql-ruby`.

## Motivation
When working with GraphQL, this framework will automatically compile the GraphQL schema used with Relay Compiler. By adding the recommended Webpack plugin, there is no need to run additional services to keep the backend schema and frontend schema in sync.

This Dockerfile requires the following:
- package.json
- webpack.config.js

Recommended:
- [Relay Compiler Webpack Plugin](https://github.com/thusfresh/relay-compiler-webpack-plugin)

Environmental Variables:
```
SCHEMA_PATH /mnt/app/graphql
SCHEMA_EXT '*.rb'
BACKEND_HOSTNAME web
NODE_ENV development
WATCHMAN_VERSION v4.7.0
```
