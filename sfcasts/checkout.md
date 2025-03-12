# Chapter 2: Checkout products

In previous chapter we created a new product, and we also make it shown on the so-called storefront. What is that? That’s your personal online store where customers can see the list of your products and buy them directly even if you don’t have a website! Let's open the storefront from the left sidebar: below "Squeeze the Day" store name, and click on "My Store". Here it is: https://squeeze-the-day.lemonsqueezy.com/ - So you can see the name of your store: squeeze-the-day as a subdomain on lemonsqueezy.com… and it seems it’s not activated yet. LemonSqueezy want you to activate your store first to be able to use it. For this we will need to finish more steps on the Setup page. I will not do it right now, first of all because it’s a training project, and second - we will focus on the API integration in this course.

Before I was able to see the storefront in the test mode, so it seems LemonSqueezy recently changed something, probably they will return access to the storefront later even without completing the setup steps.

But for now, we can still watch the LemonSqueezy checkout page - go to the product list again, click on the dots on the product line, and next click on the Preview, and you’re on the Checkout page ready to buy this product

Also, each product has a "Share" button where you can generate a link to the checkout page for the specific product and share it with your customers - it will lead them directly to the LS checkout page where they can buy that product. And you can see you can configure a lot of params here including the quantity of the project. When you generate the unique link - you can copy and it, share or open in a new tab. If you scroll down - you will see the quantity now is set to 5.

Of course, that’s not that flexible as your own website, but you can configure some styles like color of buttons or backgrounds, etc. For this go to the LS dashboard - I will close the share product modal. And in Design section you will find several tabs. In General tab you  for example will be able to upload a logo, even upload a favicon here - tiny but mighty. Or maybe want some preconfigured theme? Or go wild and set up your own colours! I will leave it up to you.

There's even more flexibility on the other tab: Design > Store: ( https://app.lemonsqueezy.com/design/storefront ) - that’s btw how your storefront will look like for the customers, we even have our product here. So definitely go wild and create our own unique style to stand out your store among others!

The same about the Checkout tab - you can preview how it will look like in some styles, and it also allows you to override some styles if needed.

OK, back to the checkout page, let’s try to buy the product. We have 2 options here: pay by card or pay with PayPal. I will go with pay y card, we have the email already pre-filled because we authenticated in the LemonSqueezy dashboard. And for card numbers we can use testing cards from LemonSqueezy which you can find in the LemonSqueezy docs. I will open in a new tab, go to Resources > Help docs > and in the left side bar we need t choose test mode and the test card numbers in the right sidebar. ( https://docs.lemonsqueezy.com/help/getting-started/test-mode#test-card-numbers )- there’re several of them but we need a successful card so I will go with the 1st one. I will copy 4242424242424242, paste, and  we can use any future expiration date valid for now, for example 12/25, and any 3 numbers of security code. And it requires us to insert some billing address - when I click Buy we see some validation errors. I will write "Broadway 1", then choose from the list to autocomplete everything. Tax ID is optional.

Now press "Pay", and... Boom! "Thanks for your order!"! Did it work? Press this "View Order" button - here’s where you can generate a PDF invoice for your order if you need it.

Here LemonSqueezy requires us to insert the address again, not sure why, probably for the security reasons, or maybe pre-fill does not work completely. We can even add some custom notes if you need something to add to your invoice and choose the language in which this invoice should be generated. Next press Generate invoice and it downloads a pdf - let’s open it to see how it looks like.

### Invoice Emails

Now let’s check the inbox of the email you use during the checkout to see if we have any emails. Yes, we do! Open this "your classic e-lemonade receipt" to see how it looks like. We can view the order from here and also generate the invoice that we just did. You actually might receive 2 emails: 1 as a customer and 1 as a store owner because we registered the store with the same email. I turned off that 2nd type of emails to have less spam in my inbox. You can do it in Settings > General > Account and switch the sales notifications off ( https://app.lemonsqueezy.com/settings/account ). And down below you will find this switch: "Receive sale notifications by email". Don't forget to Save changes.

Btw you can configure it in the LS dashboard > Design > Email ( https://app.lemonsqueezy.com/design/email ). We can also control the email when create a checkout, we will see later in the course.

## LemonSqueezy Dashboard

This all is from the customer's perspective, but what about the store owner? Back to dashboard - here we go, our charts already go up! Open Store > Orders to see the orders list - the last one should be our order for $4.95. And there’s also a customers page: go to Store > Customers where you can see some info about the customer and the orders they bought.

Please, ignore that "archived" label on my customer - it just mean the customer will no longer receive marketing emails. Once I tried to archive it and now I can’t change it back, probably something that LemonSqueezy will add later :)

Anyway, as we can see LS handles all the emails for us, so you don't need to worry about it. You will find the invoice email in your inbox shortly. Sour (🍋)! Um, I mean, sweet! And that's really great! You can start selling your products without a website, just create more products, publish them on the storefront, and share the link to your storefront to your friends!

But since we're developers, and since we already have a super popular lemonade stand (our on-site sales clearly confirm this 😎), *and* we already have an awesome website about it that I will show you in a second - how handy! - we would like to have an integration of LemonSqueezy directly on *our* website using their rich API. Let's do it next!
