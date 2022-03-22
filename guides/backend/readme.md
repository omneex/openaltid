# Backend
### Repo: github.com/omneex/openaltid-backend
### Docker Image: omneex/api-openaltid:main 

#### Please have a host with Docker and NGINX set up already (default installations work just fine), I would also recommend having Portainer as well since that will make orchestration much easier.

## API Keys
You will need a lot of API keys for the backend to work, the process for getting some of them can be tedious, but luckily you only ever have to do this once. Feel free to reach out to me about how to obtain these. Below is a list of variables needed to run the backend.
| ENV Variable | Description |
|---|---|
| DISCORD_CLIENT_ID | Discord ID of the application for Oauth, does not need to be the same as the bot ID |
| DISCORD_CLIENT_SECRET | The Oauth secret for Discord |
| FRONTEND_HOST | The URL of the host name pointing at this page. Must start with `https://`. Should NOT have a `/` at the end. |
| GOOGLE_CLIENT_ID | Your Google API client ID. |
| GOOGLE_CLIENT_SECRET | Your Google API Secret |
| HOSTNAME | The URL of the backend API. Must start with `https://`. Should NOT have a `/` at the end. |
| MONGO_DB_URI | The MongoDB URI pointing to your verification_data database should look something like `mongodb+srv://<user>:<pass>@<project>mongodb.net/verification_data?retryWrites=true&w=majority` |
| MONGO_DB_URI_BOT | Another MongoDB URI, but this one pointing to the database used by the bot, similar to the one above except with `/botdb` instead of `verification_data`. |
| REDDIT_CLIENT_ID | Reddit Client ID. |
| REDDIT_CLIENT_SECRET | Reddit Client Secret. |
| REDDIT_REFRESH_TOKEN | Reddit refresh token. Must be generated using the ID and Secret beforehand. See: https://praw.readthedocs.io/en/latest/tutorials/refresh_token.html |
| REDIS_HOST | The host name of the Redis instance. If using docker, this would be the name of the redis container. |
| SECRET | Optional. Use this to sync sessions between multiple instances of the backend.  |
| TWITCH_CLIENT_ID | Twitch API Client ID. |
| TWITCH_CLIENT_SECRET | Twitch API Client Secret. |
| TWITTER_CLIENT_BEARER | The Twitter bearer token from Twitter authentication. |
| TWITTER_CLIENT_ID | Twitter App ID. |
| TWITTER_CLIENT_SECRET | The Twitter app secret. |
| YOUTUBE_API_KEY | The YouTube API key from the Google API console. |
