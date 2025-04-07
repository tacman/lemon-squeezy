# Chapter 2: Checkout Products

In the previous chapter, we created a new product and displayed it on our
storefront. Now our customers can see the product and purchase it directly, even
though we don't have a website. *Nice*! 😎

We can open our storefront on the left sidebar. Below our store name - "Squeeze
the Day" - click on "My Store". Here it is:

`https://squeeze-the-day.lemonsqueezy.com/`

The name of our store appears as a subdomain, but it hasn't been activated yet.
LemonSqueezy wants us to *activate* our store before we can use it. To do that,
we need to finish a few more steps on the Setup page. Since the *main* focus of
this course is API integration, we won't activate it yet.

If you said "Hey! Weren't we able to see the store front in test mode in the
last chapter?", you're correct! It looks like LemonSqueezy recently made some
changes, so we should have access to the storefront again later, even without
completing the setup steps. For now, we can still see the LemonSqueezy checkout
page by going to the product list, clicking on these dots on the product line,
and then "Preview". Voila! We're on the checkout page, ready to purchase this
product! 🎉

Each product also has a "Share" button where you can generate a link to the
checkout page for that specific product and *share* it with your customers. That
will lead them *directly* to the LemonSqueezy checkout page where they can buy
that product. We can also configure a bunch of params here, including the
*quantity*. When you generate the unique link, you can copy and share it or open
it in a new tab. If you scroll down, you can see that the "Quantity" now is set
to "5". The storefront isn't as flexible as your *own* website, but you *can*
configure some style options like the color of buttons, backgrounds, etc.

Let's take a look at that. Head back to the dashboard, close the "Share Product"
modal, and in the "Design" section on the left, you'll find several tabs. In
the "General" tab, you can upload a logo or favicon. Tiny, but mighty! 💪 You can
also use a preconfigured theme, or choose your own colors. Let your imagination
run wild!

There's even more flexibility in the "Store" tab! This is what our storefront
will look like to our customers, and you can even see our new product here. This
is a great place to create your own style and make your store stand out from the
competition.

The "Checkout" tab allows you to preview what your storefront will look like in
some styles versus others, and it also allows you to *override* some styles if
needed.

Okay, back on our checkout page, let’s actually try to *buy* our e-lemonade. We
have two options here: "Pay by Card" *or* "Pay with PayPal". Let's go with "Pay
by Card". The email field is pre-filled because we authenticated in the
LemonSqueezy dashboard. For the card number, we can use LemonSqueezy's test
cards, which you can find in the docs. If we open their site in new tab, we can
go to "Resources", "Help Docs", and in the left sidebar, click "Test Mode". The
test card numbers we need are on the right here.

There’re several to choose from, but I'll just use the first one. Copy "
4242424242424242"... *paste*... and we can use any future expiration date for
now. I'll say "12/25", and use any three numbers for the security code. This
also requires us to enter a billing address. If we try to click "Buy" without
one, we get a validation error. I'll say "Broadway 1", then choose from the list
of suggestions to autocomplete all the fields. Adding a "Tax ID number" is
optional, so we can leave it blank.

If we click "Pay"... boom! "Thanks for your order!" If we click "View Order"...
here’s where we can generate a PDF invoice for our order if we happen to need
it.

To do that, LemonSqueezy requires us to enter an address again - either for
security reasons *or* because the pre-fill isn't working. We can even add some
custom notes and choose the language our invoice will be generated in. If we
click "Generate Invoice", we can see that an invoice file was downloaded. If we
open that... nice! There's our info!

### Invoice Emails

Now that you've purchased your first product, check the inbox of the email you
used during the checkout process to see if you received an email. Yep! We *do*!
Open "Your Classic E-Lemonade receipt". We can view the order the placed and we
can *also* generate the same invoice we saw earlier. As a store owner, you may
receive an additional email because our store is connected to the same email
address. I disabled those emails to avoid spamming my inbox, and you can do the
same by going to "Settings", "General", "Account", and switching the sales
notifications off.

Don't forget to save your changes! You can also configure this in the
LemonSqueezy dashboard by going to "Design", "Email".

We can *also* control the email created a checkout. More on that later!

## LemonSqueezy Dashboard

So... all of this is from the *customer's* perspective, but what about the store
owner? If we head back to the dashboard... here we go! Our revenue chart already
reflects the purchase we made! Open "Store"... "Orders" to see the orders list.
The last one should be the order we placed for $4.95. Nice! There’s also a "
Customers" page here, where we can see some info about each customer and the
products they ordered. You can ignore the "Archived" labels on the orders on my
screen. That just means the customer will no longer receive marketing emails.
That... was an *accident*, actually. I tried to archive this once, and now I
can't change it back. Hopefully that's a feature LemonSqueezy will add later. 😅

Anyways, as you can see, LemonSqueezy handles all of the transactional emails
for us, so we don't need to worry about it. *Sour*... I mean... *sweet*! You can
start selling your products without a website; Just create more products,
publish them on the storefront, and share the *link* to your storefront with
your friends! 🎉

But since we're developers, and since we *already* have a super popular
e-lemonade stand (our sales stats *clearly* confirms it), *and* we already have
an awesome website for it that we'll see soon, *now* we want to integrate
LemonSqueezy directly on our site using their rich API. Let's do that *next*!
