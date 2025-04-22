# Listening to Webhooks

## Assigning a LemonSqueezy Customer to a User

So far, we have users in our application… and customers in LemonSqueezy. But right now they’re total strangers to each other.

To build any meaningful integration - like fetching a user’s orders, debugging issues, or just having a smoother support experience when customers contact us about their orders - we need to connect them. Specifically: we want to store the LemonSqueezy customer ID on our User entity.

And here’s the twist: when someone completes a checkout, LemonSqueezy creates the customer *for us*, behind the scenes. You can’t pre-create a customer or pass an existing customer ID during checkout. Nope! LemonSqueezy decides that based on the email address and whether the user is logged in to their LemonSqueezy account.

## Webhooks

Sooo... how can we grab that customer ID and link it to our user? Manually? Yes… but not something we want, we need to automate it. And the answer is *webhooks*!

We’ll send some metadata when creating the Checkout - like the user’s ID from *our* system. Then, when we receive the webhook (more on that in a second), we will read that metadata, look up the proper user in our database, and save the customer ID on it.

Now, you might be wondering: “Wait! Can I also get the Customer ID right after the checkout?”

Yep! LemonSqueezy also dispatches a `checkout:complete` JavaScript event that includes the Customer ID. And it indeed may be *awesome* for local development when you don’t want to configure webhook tunnels.

So let’s do both!

## Symfony Webhook Component

Since webhooks are the real deal for production because they are flexible and robust solution - let’s go with them first.

There's some docs about it. Open LemonSqueezy homepage > "Resources" > choose "Help docs", and then in the "Webhooks" section click on "Event types". There are only few events about Orders, and to sync customer with app user I think we will need to listen to this `order_created` event that should send us an Order object data… click on it to see it gives us the customer ID we need.

OK, now we *could* create a `WebhookController` controller to handle this ourselves. But Symfony’s got our back and recently released a new Webhook Component that can help us to handle this in a smoother way! Let's leverage it in our application. Open your terminal and install it with: `composer require webhook` - it will brings us some important dependencies. And MakerBundle now has a command that helps us to start, so handy!

Run `bin/console make:webhook`. Name it `lemon-squeezy`. For the matcher, let's choose: `PathRequestMatcher`, then `MethodRequestMatcher`, and since LS sends us data in JSON format - add also `IsJsonRequestMatcher`. Hit enter one more time - great, it generated for us a parser and a consumer - we will see what they do soon. It also created a new endpoint, you can run `bin/console debug:router | grep webhook` - there it is - our new `/webhook/{type}` URL where type should be our lemon-squeezy webhook name we set earlier. It’s a specific URL that will handle LemonSqueezy webhooks. Open it in the browser to see it works. Yeah, it throws a RejectWebhookException saying that:

> Request does not match.

That’s something we will configure in a minute. I will copy the URL - we will need it in a second.

Now go to "Settings" > [Webhooks](https://app.lemonsqueezy.com/settings/webhooks) and click on "+". For callback, I will paste the URL we copied: https://127.0.0.1:8000/webhook/lemon-squeezy . Hmmmm, but you may already see the problem - it will not work this way as it's not a public URL, LemonSqueezy will not be able to hit our local host.

In the next chapter, I’ll show you how to set up the webhooks properly using a tool like Ngrok, so LemonSqueezy can actually talk to your local machine.
