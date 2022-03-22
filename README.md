# Open/Alt.ID
The meta repo for descriptions and guides of use and installation of Open/Alt.ID

# Installation
## Introduction
### There are 5 components that make up the system:
- #### _A static frontend page, written in VueJS_ - This is the website that the users will use to login to and verify with.
- #### _A backend API, written in Typescript with ExpressJS_ - This is the API that the frontend uses, it sort of acts as a mediator between the website and the bot. This is where the actual 'verification' occurs.
- #### _A Discord bot, written in Rust with SerenityRS_ - This is the bot that both users and mod will use to initate verification. This bot is now entirely interactions based through slash commands.
- #### _A Redis messaging system_ - This is the messaging system for passing information between the backend API and the Discord bot. The backend API also uses this as a session store ***IMPORTANT: This must be kept secure and is not to be accessable from outside of your host***
- #### _A MongoDB database_ - This is where verification data is stored as well as where guild settings are stored.

### Use Docker
Everything but the frontend website and MongoDB is designed to be run from Docker (though you could serve the frontend from there as well if you wanted to). The easiest way to do this is to use Docker Compose to deploy a stack containing all the parts needed. 

### Configuration
The project is designed to be configured using enviroment variables, either stored in a file that is then loaded by Docker, or added as part of the run commands. This means that it is especially important to keep your host properly secured.

### Routing
You have a few options when setting up the routing, first off you can simply put everything on seperate VPS instances and use a mixture of private network and a public facing instance, or you can put everything onto a single docker host and use NGINX as a reverse proxy to route traffic to where it needs to go.

### Database
It is recommended to use a hosting service for hosting MongoDB, it both simplifies configuration and ensures that the database is properly secured. Plus the project really doesn't use that much IO so you can easily run the server on a free tier instance from MongoDB Atlas

## Individual install guides
