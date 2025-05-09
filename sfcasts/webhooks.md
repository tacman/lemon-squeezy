# Listening to Webhooks

## Assigning a LemonSqueezy Customer to a User

*So*... our site has *users*, and LemonSqueezy has *customers*... *but* as far as our code knows, they’re *total* strangers to each other. To build any meaningful integration - like fetching a user’s orders, debugging issues, or just having a smoother support experience when customers contact us about their orders - we need to *connect* them. More specifically, we want to *store* the LemonSqueezy customer ID on the corresponding `User` entity.

But here's a twist: When someone finishes checking out, LemonSqueezy creates a "customer" *for us* behind the scenes. You can’t *pre-create* a customer ID or pass an existing one during checkout. Nope! LemonSqueezy *assigns* one based on the email address they entered and whether they're logged into their LemonSqueezy account.

## Webhooks

So... how can we grab *that* customer ID and link it to our user? Manually? Well, *yes*… but who wants to do that? Not me. Let's *automate* it instead, using some handy dandy *webhooks*.

We can send some metadata when the Checkout's created - like the user’s ID from *our* system - and when we receive the *webhook* (more on that in a second), we'll *read* that metadata, look up the proper user in our database, and stash that customer ID with it.

If you said "Wait... Can't we get the Customer ID right *after* they check out too?” that's correct! LemonSqueezy *also* dispatches a `checkout:complete` JavaScript event upon checkout that *includes* the Customer ID, which could be *awesome* for local development when you don’t want to configure webhook tunnels.

So... let’s do *both*!

## Symfony Webhook Component

Since webhooks are a flexible, robust solution for production, let's start there. Over on the LemonSqueezy homepage, go to "Resources", choose "Help docs", and *way* down here in the "Webhooks" section, click on "Event types".

If we scroll a bit... I only see a few events dealing with orders. For our purposes, it looks like we'll need this `order_created` event - that should give us the order data we're looking for. If we click on it, and scroll down... yep! This returns `customer_id`! *Nice*! 

Now, we *could* create a `WebhookController` to handle this ourselves, but Symfony’s got our back and recently released a new Webhook component that can help us handle this *even better*! Let's try it out! At your terminal, install it with:

```terminal
composer require webhook
```

This gives us some much-needed dependencies. MakerBundle *also* has a command to help us get started. Run:

```terminal
bin/console make:webhook
```

We'll call this `lemon-squeezy`. For the matcher, let's choose: `PathRequestMatcher`, which is option 6, and *then* `MethodRequestMatcher` - option 5. And since LemonSqueezy sends us data in JSON format, *also* select 4 - `IsJsonRequestMatcher`. Hit enter one more time, and... *nice*! This generated a parser *and* a consumer - more on those in a moment - and it also created a new endpoint. If we run

```terminal
bin/console debug:router | grep webhook
```

... there it is - our new `/webhook/{type}` URL! The `{type}` should be the `lemon-squeezy` webhook name we set earlier. This is a specific URL that'll handle LemonSqueezy webhooks. If we open that in the browser... yep! It throws a `RejectWebhookException`:

> Request does not match.

Let's configure that! Copy this URL and, back on the LemonSqueezy dashboard, in "Settings", "Webhooks", click on the plus sign to edit the webhook. In the "Callback URL" field, paste the URL we copied a moment ago. But... hm... you may have already spotted the problem here. This URL *isn't* public, so LemonSqueezy's unable to hit our localhost.

We need to set up our webhooks *properly* using a tool like Ngrok, so LemonSqueezy can actually talk to our local machine. That's *next*.
