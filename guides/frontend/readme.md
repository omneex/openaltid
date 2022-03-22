# Frontend website
### Repo: [github.com/omneex/openaltid-frontend](https://github.com/omneex/openaltid-frontend)
For more information about how to host a VueJS project I recommend reading through this guide: li.vuejs.org/guide/deployment.html

## General Recommendation
### Netlify
#### https://cli.vuejs.org/guide/deployment.html#netlify
This the recommendation and is the one that is used by default. 

In general I do not recommend self hosting for this, the site is static so you would get little benefit from using self hosting and Netlify massively simplifies deployment.

#### Configuration
The only part of configuration you will need to do that is specific to this project is that you need an enviroment variable with a url pointing to your backend API.
`VUE_APP_API_HOST=some.url.to.api` (Do NOT include a path, so no `/` after the URL). Note: This will be public inside the website so ensure that this is behind a proper URL.
#### SSL
The backend WILL require you to connect through SSL, if the link does not start with `https://` things will not work properly, as well as being very insecure since users will be loging in using their Discord account.

#### Branding Config
`TODO`
