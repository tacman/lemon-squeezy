# Start the Course Project App

Okay, I get it. The words "payment system integration" might sound scary at first. But trust me — by the end of this course, you'll be saying:

> With LemonSqueezy… it's easy peasy!

Time to meet you with our project! We’ve already downloaded the course code in the previous chapter, but if you haven’t yet - download it now from this page, unzip it, and go to the start/ directory. Don’t forget to check the README.md file that contains instructions about how to setup and start it locally. I’ve already did all the steps there, so I will just start the web server with `symfony serve -d` command, and click the URL in the output.

Welcome to the "Squeeze the Day" website - our digital Lemonade Stand! Let's look around! We have some products here, you can add it to cart for buying, you can even specify the quantity. The actual cart page where you will be able to check out - that's something for what we will need LS integration.

We also provide weekly lemonade delivery for subscribed users - how convenient! We will see subscriptions in the next course. You can also register, log in, and find some basic account info.

In the fixtures you will find default user credentials. I will copy the email. Go back to the login page, in the email field paste the email, and password is lemonpass. Hit sing in, and go the account page to see it.

### Install HTTP Client
Let's take a look at the docs first for help. Go to the lemonsqueezy.com, open Resources > Developer Docs > Guides from the right > and we need the "Getting Started with the API" chapter. We will need to create an API key, we will do it a bit later. Then all the API requests should be done to the specific domain with specific headers mentioned here. LemonSqueezy require some authentication, so we will also need to pass an authorisation token. To send API requests inside our Symfony application we can leverage the Symfony HttpClient component - it should be perfect for executing HTTP requests..

Install it with: `com req symfony/http-client`. Seems it was already installed as an indirect dependency, so Composer just added it the composer.json as direct dependency.

We’re going to configure it, so let’s create `config/packages/http_client.yaml`.

### Create a Scoped HTTP Client
Open the config file. Now let's create a scoped client that will help us to send requests to the LS API.
In `scoped_clients` add `lemon_squeezy.client` . Then `base_uri: 'https://api.lemonsqueezy.com/v1/'`. And headers set to: `Accept: 'application/vnd.api+json'` and  `Content-Type: 'application/vnd.api+json'`. For authorization, we need to add a Bearer token.

First, let's set up the API key, so we could make API requests. Open LS dashboard and go to the "Settings" > API > "Add API key" > I will name it "API", and copy the generated key. Of course, you should keep it secret! Nobody should see it... oh crap, I've already failed this. OK, not a big problem, I can always delete it and generate a new one, but I will keep it and change later after I recorded the course.

We usually save things on `.env` file, but for better security I do not want to commit it to the repository, so I will save it in `.env.local` instead. Create `.env.local` and set it there as `LEMON_SQUEEZY_API_KEY`. Open `.env` and add this env var but left empty `LEMON_SQUEEZY_API_KEY=`. On production, usually you can set it as a real env var with your cloud hosting. Otherwise to make it even more secure - take a look at Symfony's secrets management system.

Now we can add `auth_bearer: '%env(LEMON_SQUEEZY_API_KEY)%'`

Great, now we’re ready to send HTTP request to the LS API, but that will be in the next chapter.
