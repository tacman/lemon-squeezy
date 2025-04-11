# Start the Course Project App

I *totally* get it. The words "payment system integration" sound super
intimidating, but *trust me*, by the end of this course, you'll be saying:

> With LemonSqueezy, it's *easy peasy*!

Let's check out our project! We downloaded the course code in the previous
chapter, but if you *haven’t* yet, you can download it from this page now. Unzip
it, go to the `start/` directory, and don’t forget to check the `README.md` file
for all the setup instructions. I’ve already completed these steps, so I'll go
ahead and start the web server with

```terminal
symfony serve -d
```

and click on this URL.

Welcome to "Squeeze the Day" - our digital lemonade stand! We have some products
here, we can add them to our cart, and we can even specify the quantity. The
*actual* cart page, where we can check out, is where we'll need LemonSqueezy's
integration.

We *also* provide weekly lemonade delivery for subscribers! How convenient!
We'll talk about subscriptions in the next course. We can also register, log in,
and find some basic account info. Let's do that!

In `AppFixtures.php`, you'll find some default user credentials. Copy this
email, go back to the login page, and paste that into the email field. The
*password* is "lemonpass". Hit "Sign In"... and check out the "Account" page.
Pretty basic stuff at the moment.

## Install HTTP Client

Okay, now we can work on our API. For help getting started, let's take a look at
the docs. Go to `lemonsqueezy.com`... click on "Resources", "Developer Docs",
and "Guides". Then find a chapter called "Getting Started with the API". It
looks like we need to create an API *key* (we'll do that later), and then *make*
all of our API requests using the specific domain and headers mentioned here.
LemonSqueezy also requires some authentication, so we'll also need to pass an
authorization token. To send API requests from our Symfony application, we can
leverage the Symfony HttpClient component, which is *perfect* for executing HTTP
requests.

To do that, at your terminal, run:

```terminal
composer require symfony/http-client
```

Looks like it was already installed as an *indirect* dependency, so Composer
just added it to `composer.json` as *direct* dependency. *Sweet*. Now we need to
configure it! In `config/packages/`, create a new file called
`http_client.yaml`.

## Create a Scoped HTTP Client

In our new file, create a *scoped client* that will help us send requests to
LemonSqueezy's API - `framework:`, `http_client:`, `scoped_clients:` - and call
it `lemon_squeezy.client`. *Then*, for `base_uri:`, over in the docs under
"Making requests", copy this URL - `'https://api.lemonsqueezy.com/v1/'` - and
paste it here. Next, under `headers:`, set `Accept:` to
`'application/vnd.api+json'`, and do the same for `Content-Type:`.

For authorization, we need to add a *Bearer token*, but first, let's set up the
API key, so we can make API requests. Open the LemonSqueezy dashboard and go
to "Settings", "API", and "Add API key". Let's call it "API", click "Create API
key", and copy the key we generated. This is *top secret*, so... pretend you
never saw this. I'll generate a new, *secret-er* one later.

Since we want to keep this a secret, we *don't* want to save this in our `.env`
file with everything else, because that's committed to the repository. To keep
this safe and secure, create a new `.env.local` file and save it there instead.
This file isn't committed to the repository, so it will be safe here.
Write `LEMON_SQUEEZY_API_KEY=` and paste our API key. *Now*, we can copy *this*
line, *excluding* the key, and in `.env`... down here... paste. This really
just lets us know that we need this environment variable for our app to work.
On production, you can set this as a real environment variable with your
cloud hosting. Or, to make this even *more* secure, take a look at
Symfony's secrets management system. You can find more information about that in
the docs on `symfony.com`.

Finally, back in `http_client.yaml`, under `base_uri`, add
`auth_bearer: '%env(LEMON_SQUEEZY_API_KEY)%'`. *Done*!

Now we’re ready to send an HTTP request to LemonSqueezy's API! Let's do that
*next*.
