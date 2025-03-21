# Chapter 4: Checkout API Request

On our website, down here, click on a product to open its page and add it to the cart. If we click the "Checkout" button... nothing happens. Let's fix that!

In the previous chapter, we saw how to generate a unique checkout URL for each LemonSqueezy product in the dashboard and share it with the customer. And guess what? We can do the *same* thing with the checkout API endpoint!

## Create a Checkout URL

We *already* have a configured scoped HTTP client, so let’s make our first API request to LemonSqueezy's API. To find the URL, open the API docs, scroll down, and click [Create a checkout](https://docs.lemonsqueezy.com/api/checkouts/create-checkout). This shows us the specific endpoint URL, the *method* we need to use, and it also has a *ton* of config. There's even an example with a JSON response!

If we scroll through the code on the right here... there it is - the `url` key! This is located inside `data`, `attributes`. When our customers hit the "Checkout" button, we want to redirect them to this URL to complete the payment. No problem!

Back in our code, we already have `OrderController.php`, which contains our cart-related methods. Let’s create a new one called `checkout()`...  and make it a route with `#[Route('/checkout', name: 'app_order_checkout')]`. Awesome!

Now, open `cart.html.twig`, scroll down... and set the "Checkout with LemonSqueezy" URL to `app_order_checkout`. Thanks to this, when a customer clicks the "Checkout" button, it will hit this endpoint. Our *next* task is to generate LemonSqueezy's checkout URL via the API and redirect our customers so they can complete their purchase.

To do that, inject `HttpClientInterface $lsClient` and `ShoppingCart $cart`... and let’s move the actual business logic of the API call to a separate method for convenience. Down here, say `$lsCheckoutUrl = $this->createLsCheckoutUrl($lsClient, $cart);` and finally, `return $this->redirect($lsCheckoutUrl);`. Looking good!

To create a new method, hold "Option" + "Enter" to open the menu and choose "Add method" to let PhpStorm do it for us. Convenient! This will return a `string`... and inside, let’s do a sanity check:

`if ($cart->isEmpty())`
`throw new \LogicException('Nothing to checkout!');`

### Make a Request to LS API

Below, say `$lsCheckout = $lsClient->request(Request::METHOD_POST, 'checkouts', []);`... and inside, `'json' => ['data' => ['type' => 'checkouts']]`. We can leave the rest of the options empty for now. LemonSqueezy's API docs don't really clarify which option is ?*required*, so we'll just have to figure it out for ourselves.

Down here, since we have a JSON response, we need to convert it to an array with `$lsCheckout = $response->toArray();`. Then, `return`... and from the example response we saw in the docs, we can read the URL with `$lsCheckout['data']['attributes']['url'];`. Nice!

Okay! Testing time! Back on our site, refresh, click the "Checkout" button, and... an error!

> Invalid URL: scheme is missing in "checkouts". Did you forget to add "http(s)://"?

Hm... looks like it *ignored* our base URL from the scoped client. I *suspect* it injected the default *empty* client instead. Not to worry! We're on the case!

At your terminal, run

```terminal
bin/console debug:autowiring HttpClientInterface
```
to see the related services. Ah ha... to inject LemonSqueezy's client, we need to use *named autowiring* and use the *exact* variable name: `$lemonSqueezyClient`. We shortened it to `$lsClient` in the code. Let's fix that!

Change this to `$lemonSqueezyClient`, and this will solve our problem, but... wait... I kind of like the shorter version we were using. Is there a way we can still use it without breaking our integration? You bet! Instead of just *renaming* this, let's leverage the new `#[Target]` PHP attribute to link it to the correct service. Above the argument, add `#[Target('lemonSqueezyClient')]`.

If we head over and try this whole thing again... ugh... *great*... *another* error. I mean... a *different* error:

> HTTP/2 422 returned for "https://api.lemonsqueezy.com/v1/checkouts".

Hm... a 422 status code tells us that the server was unable to process the request because it contains invalid data, but the exception doesn't give us much info. Let's try dumping the response content instead.

Back over here, before the return, add `dd($response->getContent());`. If we try this again... we get the same error, but now we can click here to see the actual response. Ah... it looks like we need to pass `false` to debug the response without throwing an exception. Add `false` here... refresh one more time, and... okay! In the details here, we can see that `variant.id` is required, as well as `store.id`. It even points us to the specific paths we need: `data/relationships/store/data/id` and `data/relationships/variant/data/id`. We can follow that path in the API docs - `data`... `relationships`... `stores`... `data`... `id`... and so on. Go ahead and add this to our code - `relationships`, `stores`, `data`, `'type' => 'stores'`, and `'id' =>`.

To find the *actual* store ID we need, go to LemonSqueezy's dashboard and click on "Settings", [Stores](https://app.lemonsqueezy.com/settings/stores). Copy the id here and paste it in our code.

The `variant` should follow a similar path to `store`, so we'll say `variant`... `data`... `'type' => 'variants'`... and for `id`, let's just hard-code one from the product we created in the first chapter. Open the dashboard, go to "Store", "Products", and on the right, click the three dots here to open a menu. Down here, click "Copy variant ID", and paste that in our code. Done!

Back on the checkout page, refresh and... *another error*. This one's telling us that the member ID needs to be a string. If we take a look at our code... yep! That's an easy fix! This is currently an integer, but if we do this... tada! Now it's a string! Do the same thing for `variant`, then head back and refresh again. Ah ha! We have a JSON response and, if we expand this and do a quick search... it looks like this has the URL we need! *Sweet*! Go back and comment out the `dd()`, refresh the page again and... *yes*! We're on the LemonSqueezy checkout page, ready to buy some delicious e-lemonade!

Next: Let’s make our hard-coded data *dynamic*.
