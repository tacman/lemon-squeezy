# Meet LemonSqueezy - Your Merchant of Record.

Hello and *welcome* to another e-commerce tutorial! I don't mean to sell this too hard, 
but I think this tutorial is fantastic! There are *a lot* of payment processors
available, which means selling products online is easy and exciting... until you realize
that handling taxes, and compliance is... well... *not* exciting.

Can we simplify the way we handle all of that information? You bet! To do that, we need a
payment processor that is a Merchant of Record, or "MoR". "What's that" you ask? It's like any
payment processor, but with *superpowers*. "Merchant of Record" means it handles all of the
complicated legal and financial stuff, like taxes, *for* you. No more VAT headaches or compliance
nightmares — just sweet, *sweet* selling.

We'll show you how to set this up using Lemon Squeezy, but what we're covering will 
translate to other services like Paddle or Polar. Which service you choose is up to you: 
we're covering LemonSqueezy, but we don't have a favorite.

Okay, first thing's first! We need to *register* before we can start selling products. Click on
"Get started" and add your name, email address, and a *super* strong password - y'know... because
we're dealing with payment stuff. You can also configure 2FA at a later time for even *more* security.
Remember to use your real email because you'll need to confirm it before you can begin. We'll also send
some test emails that you'll want to see.

## A Quick Tour of the dashboard

I’ve already registered mine, so I'll just to head back and sign in using my credentials. And...
welcome to "Squeeze the Day" - my digital designer lemonade stand! I already have some data on
my dashboard chart I acquired while I was preparing this tutorial. *Yours* should be completely empty.

Upon registering, you'll get a setup page that contains several steps you'll need to complete in
order to get your store up and running, but we can worry about that later. For now, we can start
working on our integration in *test* mode. We can tell that we're in "test mode" by checking this
little switch down here in the sidebar. That means we can use test card numbers to simulate payments
without spending real money. That's gonna be *super* handy because I'm about to spend *a lot* of money
while we're developing. My fake wallet is already crying. As soon as you finish the setup steps and
activate your store, you'll be able to switch between your live (real) store and test (fake) store
using this switch, so keep that in mind.

*Technically* you don't even *need* a website to start selling with LemonSqueezy. Cool, right? You can
just share your LemonSqueezy storefront URL with your customers, and they'll be able to buy your products
directly from there. What are we going to sell? Let’s create our first product and *make that money*!

## Create a Product

The first thing we need to do is go to "Store"... "Products", and click "New Product". Since I have a
lemonade business, let’s name the first product "Classic Lemonade" - *smooth* and simple. For the
description, let's say "A classic citrus lemonade".

*Now* we can set the pricing model, and there are a few of them to choose from: "Single payment" that
charges customers a one-time fee, "Subscription", which we'll see in the next course, "Lead magnet", which
allows access for *free*, and "Pay what you want", where you can set suggested and minimum prices and let
your customers decide how much they want to pay for your product.

Let's go with the simplest option - a "Single payment" - and keep it simple with the "Standard pricing"
model. I'll set the price at... how about $0.99? In the tax category, we need to choose one of few options.
The one that best fits our needs is "Digital goods or services (excluding ebooks)". That's right! Our
lemonade is *digital*. As I mentioned earlier, LemonSqueezy is a "Merchant of Record", so it limits the
products you can sell via their platform to digital-only, so we won’t be able to sell *real* lemonade.
*Bummer*. But hey! We're trailblazers, boldly paving the way for the world’s first digital lemonade!
Let's change the name of our product to "Classic E-Lemonade", just for clarity.

Okay, before we officially list our first product, we need to make sure that it isn’t on the
"Prohibited products" list. We can find that at `docs.lemonsqueezy.com`, where we click on "Resources"...
"Help Docs"... and below, you'll find a "Prohibited products" section:

`https://docs.lemonsqueezy.com/help/getting-started/prohibited-products` .

If you're *still* not sure about your specific product after reviewing this document, just ask their
support team.

All right, let's head back to our product-in-progress! A successful product needs a great image! Upload
a picture of our delicious lemonade... um, *e-lemonade*, that'll make people want to smash that "Buy"
button. And surprise! We already have one! If you download the course code and unzip it, inside you'll
find a `start/` directory with the same code you see here. The `tutorial/` directory has the image we
need. You can just click and drag it to the media section. Ohhhh yeah! That looks so tasty that I already
want to buy it! If you're in the business of selling files, you can attach a file to the product and your
customers will have access to it right after their purchase. The same is true for links! We don't need one
for our e-lemonade, so I'll keep it empty for now.

Next up is "Variants". Variants are useful when you have a product with different options like taste, size,
color, etc. We're definitely going to sell more than one flavor of designer e-lemonade, and our customers
will probably be more likely to buy the other flavors if they can see how delicious each flavor is, right?
Since variants don't have their own image, it makes sense for us to create each new flavor as an individual
product, so we'll leave this blank. More on that later!

If you're selling license keys, you'll want to switch this on. We don't need that, so we can leave it the
way it is. *This* switch is enabled by default and allows us to show this product on the LemonSqueezy
storefront page - helpful if you don't have a web store yet. You can also customize the "Confirmation modal"
and "Email receipt", but the defaults work just fine for now. By the way, LemonSqueezy will handle all
of the emails *for* you. Every time a customer purchases something in your store, LemonSqueezy will send
them an order email, so there's no need to handle those complex emails yourself. What a time saver!

Finally, let’s "Publish" our new product. Click "Publish product" and... that’s it! You just created your
first LemonSqueezy product! Close the Product Details page, and there it is! It has a "Published" status
and it’s already displayed on our storefront! *Sweet*!

Next up, let’s see what our customers see - our shiny new storefront!
