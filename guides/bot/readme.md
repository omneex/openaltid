# Bot
### Repo: [github.com/omneex/openaltid-backend](github.com/omneex/openaltid-backend)
### Docker Image: [omneex/ironicbot:main](https://hub.docker.com/repository/docker/omneex/ironicbot)

#### Please have a host with Docker and NGINX set up already (default installations work just fine), I would also recommend having Portainer as well since that will make orchestration much easier.

## ENV Variables
All you really need to do for the bot is set these variables, everything else should just work TM. 
- The bot does not need to have a direct port allocated but DOES need access to the outside internet.

| ENV Variable | Description |
|---|---|
| APPLICATION_ID | The application ID of the bot Discord app |
| DB_NAME | The name of the database to use for the bot, should leave as `botdb` |
| DISCORD_TOKEN | The Discord Bot token |
| FRONTEND_HOST | The URL to the frontend website, do not include a path, so no `/` at the end |
| MONGO_CONN_STR | The URI connection string for MongoDB, should not go to a specific database so do not include one. (nothing after the `mongodb.net/`) |
| PLATFORM | Either `windows` or `linux`. The host platform the bot is running on (needed since windows has issues with the MongoDB driver) | 
| REDIS_HOST | URL to the Redis host. Either a name or IP, if following the guide this will be `redis`
| REDIS_PORT | The port to use for Redis, should be `6379` unless explicitly set elsewhere |
| RUST_BACKTRACE | Optional. Set to `1` or `full` if you want back tracing for errors.
| RUST_LOG | Should probably be `error,ironic_bot=info` or `error,ironic_bot=debug` depending on how much info you want to print to console. |
