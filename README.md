# Open/Alt.ID
The meta repo for descriptions and guides of use and installation of Open/Alt.ID

# What does is it?
### Open
#### This system is completely open source and there is no guess work about what is being done with the data takes. It is the opinion of the developers that security by obscurity is inherently flawed and that openness is the first step to trust. Both between admins and developers and admins to users.
### Verification
#### OpenAltID leverages social media connections of a user's account, along with other data from the user's account, to which goes through an algorithm to generate a confidence score, all this really is a score card, more evidence of an established account, the higher the score. The actual score needed is configurable in the database and has many parameters to adjust it how you want it.
### Identification
#### Information about the users account, namely the social media accounts they connect are stored and will flag the user as an alt if those same social media accounts are seen again from another user.

# Is it impervious? 
## Short Answer 
### ***NO***, but its generally good enough.

## Long Answer
### No security system is perfect, while this is a best effort shot at detecting alternate accounts, if a user has enough patience and enough dedication they can get through it. However, this does make the process of getting an alt account in to a server much, much harder with as little input from moderation as possible. And at least on the major servers it is used on, there is little effect on user retention, 90%+ users will pass through without any additional input vs a simple "read the rules and run this command"-type user join flows. 

# Installation
## Introduction
### There are 5 components that make up the system:
- #### _[A static frontend page, written in VueJS](https://github.com/omneex/openaltid/tree/main/guides/frontend)_ - This is the website that the users will use to login to and verify with.
- #### _[A backend API, written in Typescript with ExpressJS](https://github.com/omneex/openaltid/tree/main/guides/backend)_ - This is the API that the frontend uses, it sort of acts as a mediator between the website and the bot. This is where the actual 'verification' occurs.
- #### _[A Discord bot, written in Rust with SerenityRS](https://github.com/omneex/openaltid/tree/main/guides/bot)_ - This is the bot that both users and mod will use to initate verification. This bot is now entirely interactions based through slash commands.
- #### _[A Redis messaging system](https://redis.io/topics/introduction)_ - This is the messaging system for passing information between the backend API and the Discord bot. The backend API also uses this as a session store ***IMPORTANT: This must be kept secure and is not to be accessable from outside of your host***
- #### _[A MongoDB database](https://www.mongodb.com/what-is-mongodb)_ - This is where verification data is stored as well as where guild settings are stored.

### Use Docker
Everything but the frontend website and MongoDB is designed to be run from Docker (though you could serve the frontend from there as well if you wanted to). The easiest way to do this is to use Docker Compose to deploy a stack containing all the parts needed. 

### Configuration
The project is designed to be configured using enviroment variables, either stored in a file that is then loaded by Docker, or added as part of the run commands. This means that it is especially important to keep your host properly secured.

### Routing
You have a few options when setting up the routing, first off you can simply put everything on seperate VPS instances and use a mixture of private network and a public facing instance, or you can put everything onto a single docker host and use NGINX as a reverse proxy to route traffic to where it needs to go.

### Database
It is recommended to use a hosting service for hosting MongoDB, it both simplifies configuration and ensures that the database is properly secured. Plus the project really doesn't use that much IO so you can easily run the server on a free tier instance from MongoDB Atlas

## Guide
### Step 1 - Set up frontend
- Deploy the frontend to your URL, it is recommended to use Netlify for this as it simplifies the deployment.
- See: [guides/frontend](https://github.com/omneex/openaltid/tree/main/guides/frontend) for more details
### Step 2 - Provision a MongoDB Database
- If you are going to deploy your own database make sure to set up the database and allow connections from the host/s with the backend and bot.
### Step 3 - Set up docker images
- The easiest way I recommend to deploy is to use Docker Compose with the `docker-compose.yml` below. With that all you need to do is `docker compose up` in the directory containing the file, or use Portainer Stacks.
- At this time I do NOT recommend using watchtower, please wait for a new release before updating. 
- See: [guides/backend](https://github.com/omneex/openaltid/tree/main/guides/backend), [guides/bot](https://github.com/omneex/openaltid/tree/main/guides/bot) for more details about the set up and variables needed for these.

```docker-compose.yml
# docker-compose.yml
version: '2'

services:
  bot:
    restart: unless-stopped 
    image: omneex/ironicbot:main
    links:
      - redis
    environment:
      - DISCORD_TOKEN=
      - APPLICATION_ID=
      - MONGO_CONN_STR=
      - DB_NAME=
      - GPG_KEY=
      - REDIS_HOST=redis
      - REDIS_PORT=6379
      - RUST_LOG=error,ironic_bot=debug
      - PLATFORM=linux
  # Optionally you can use env file
  # env_file:
  #   - .env     
    ports:
      - "8082"

  api:
    restart: unless-stopped 
    image: omneex/api-openaltid:main
    links:
      - redis
    ports:
      - "8080:8080"
    environment:
      - DISCORD_CLIENT_ID=
      - DISCORD_CLIENT_SECRET=
      - FRONTEND_HOST=
      - GOOGLE_CLIENT_ID=
      - GOOGLE_CLIENT_SECRET=
      - HOSTNAME=
      - MONGO_DB_URI=
      - MONGO_DB_URI_BOT=
      - REDIS_HOST=redis
      - REDIS_PORT=6379 
      - REDDIT_CLIENT_ID=
      - REDDIT_CLIENT_SECRET=
      - REDDIT_REFRESH_TOKEN=
      - SECRET=
      - TWITCH_CLIENT_ID=
      - TWITCH_CLIENT_SECRET=
      - TWITTER_CLIENT_BEARER=
      - TWITTER_CLIENT_ID=
      - TWITTER_CLIENT_SECRET=
      - YOUTUBE_API_KEY=
  #   Optionally you can use env file
  # env_file:
  #   - .env     

  redis:
    image: redis
    ports:
      - "6379"
```
### Step 4 - Set up NGINX Reverse Proxy
- At this point you should be ready to using NGINX to point your URL to port `8080` using reverse proxy. You can use the conf below as reference. ***PLEASE NOTE: YOU MUST SET UP SSL FOR THIS. USE LETS ENCRYPT AFTER CREATING YOUR CONF***
- When setting up SSL please ensure to have it redirect to https when it asks.
```.conf
# example.conf
server {
    server_name someurl.com www.someurl.com;
    location / {
        proxy_pass         http://0.0.0.0:8080;
        proxy_redirect     off;
        proxy_set_header   Host $host;
        proxy_set_header   X-Real-IP $remote_addr;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Host $server_name;
    }

    listen 80;
    listen [::]:80;
}
```
### Step 5 - Invite the bot
- You MUST ensure to invite the bot with the `bot` and `applications.commands` scopes.
- It will need the usual permissions a bot needs, including the ability to manage user's roles.
- Example invite link: `https://discord.com/oauth2/authorize?client_id=CLIENT_ID_HERE&scope=bot%20applications.commands&permissions=294474083392`
