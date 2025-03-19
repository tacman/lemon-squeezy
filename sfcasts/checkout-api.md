# Chapter 4: Checkout API Request

Let’s open a product page and add it to the cart. If I press Checkout button - nothing happens. That’s what we’re going to implement in this course. But how can we charge customer this specific amount using LemonSqueezy API? In the previous chapter we saw that we can generate a unique checkout URL for each LemonSqueezy product in the dashboard and share it to the customer. And guess what? The same checkout URL we can generate via LemonSqueezy the checkout API endpoint.

## Create a Checkout URL

Now when we have a configured scoped HTTP client - let’s make our first API request to the LemonSqueezy API. To know the URL, let’s open the API docs and find the [Create a checkout](https://docs.lemonsqueezy.com/api/checkouts/create-checkout) endpoint. It show the specific endpoint URL and the method that we need to use, and also has a lot of configuration options even with an example of the JSON response. Here it is, the `url` key inside of the `attributes` which inside of `data`. When a customer hit this checkout button - we will redirect our customers to this URL to complete the payment.

Let's do it! We already have OrderController with a cart-related methods inside. Let’s create a new one called `checkout()`. Make it route: `#[Route('/checkout', name: 'app_order_checkout')]`.

Open the `cart.html.twig` template and set "Checkout with LemonSqueezy" link URL to `app_order_checkout`. Now when customer click the link - it hit this endpoint and our task is to generate the LemonSqueezy checkout URL via API and redirect the customer to it to let them finish the order.

For this, inject `HttpClientInterface $lsClient` and `ShoppingCart $cart`. Let’s move the actual business logic of the API call in a separate method for convenience. Inside, `$lsCheckoutUrl = $this->createLsCheckoutUrl($lsClient, $cart);`. I will create this method later. And finish it with `return $this->redirect($lsCheckoutUrl);`.

- Now let's implement that `createLsCheckoutUrl()`. I will press Option + Enter to open the menu and choose "Add method" to let PhpStorm create it for us. And it will return a `string`. Inside, first of all, let’s do a sanity check`if ($cart->isEmpty())` - `throw new \LogicException('Nothing to checkout!');`

### Make a Request to LS API

Next `$lsCheckout = $lsClient->request(Request::METHOD_POST, 'checkouts', []);`. Inside options - let's just it as : `'json' => ['data' => ['type' => 'checkouts']]`. will I leave rest of the options empty for now, it’s not clear enough which option is required from their API docs, so we will have to figure it out ourselves.  Below, since we have a JSON response, we need to convert it to array with `$lsCheckout = $response->toArray();`. Then `return`.. And from the example response we saw in the docs we can read the URL as `$lsCheckout['data']['attributes']['url'];`.

It’s time to execute this request and see if there will be any errors. An error!
> Invalid URL: scheme is missing in "checkouts". Did you forget to add "http(s)://"?

Hm, it ignored our base URL from the scoped client, it feels like it inject the default empty client.

Run `bin/console debug:autowiring HttpClientInterface` to see the related services. Aha, to inject LS client we need to use *named autowiring* and use the exact variable name: `$lemonSqueezyClient` while we have shortened it to: `$lsClient` in the code.

We can rename it to `$lemonSqueezyClient` here and it will work, but I would prefer a shorter name that we have. Can we achieve it? You bet! Instead of renaming, let's leverage the new `#[Target]` PHP attribute to link it to the correct service. Add `#[Target('lemonSqueezyClient')]` - above the argument.

Update again - great, an error! I mean, a *different* error now! As you can see:
> HTTP/2 422 returned for "https://api.lemonsqueezy.com/v1/checkouts".

Hm, 422 status code indicates that the server was unable to process the request because it contains invalid data but not much information we see from the exception - let's dump the response content instead.

Before the return, add: `dd($response->getContent());`. Update the page again to see the same error… nothing changed? Ah, yes, we should to pass `false` to debug the response without throwing the exception: `dd($response->getContent(false));`.

Refresh again to see the actual response. In the detail we see the variant.id is required and also store.id field is required too. It even point us to the specific path: data.relationships.store.data.id. Let’s check the API docs - data.relationships.store.data.type -> "stores" and "id" - the ID. And data.relationships.variant.data.type -> "variants" and "id" - the ID.

To find the actual store ID - go to the Dashboard > Settings > [Stores](https://app.lemonsqueezy.com/settings/stores). Copy it from there and paste it in the code.

For the variant - It should be similar path like we had with the store. Let's hardcode one from the product we created earlier in the previous chapters. Open the dashboard, go to Store > Products > ... on the product > Copy variant ID and paste to the code.

Let’s go to the checkout page, update it again to see another error that a member should be a string. Make the store ID string - we have an integer here. And the same should be with the variant.

Update again - we have a JSON response that should contain the URL we need. Go comment out the debug statement, save and update the page - yes, finally we're on the LS checkout page and buying the product.

What next? Let’s make the hardcoded data dynamic in the next chapter!
