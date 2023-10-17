---
title:  "How I Outsmarted a Retail Giant's Queue System And Got International Media Exposure"
date: 2023-10-16 22:57:00 +0200
img_path: /assets/img/bypass-queue/
categories: [misconfigurations]
tags: [queue,techniques,server,bypass]
image:
  path: press.png
---

## Introduction

In the fast-paced world of online retail, gaining access to limited products can feel like a race against time. So, **what happens when you discover a hidden shortcut to the finish line?** Brace yourselves for a thrilling tale of ingenuity as I reveal how reading documentation and trying different things out, allowed me to bypass the queue on a major retail website, enable others to grab an extraordinary number of limited items, and **make a splash in international media.**

## Hype for PS5's

It was early 2021, the new PlayStation 5 had just released and it was **selling for a 200-300€ markup in secondary marketplaces**. Of course, these aren't selling for a high markup for no reason: they are very limited and extremely hard to buy even when they restock. That's exactly the reason why I developed CarnageBot from the beginning, to provide software which would automate everything, so our customers had it easier to buy limited items such as the PS5.

## Bypassing the Queue

I coded a module inside the bot for Game UK, a popular retail website, which would actively monitor any product selected by the user, add it to the cart as soon as it received stock, and do a full checkout. At the beginning it was working great, Game UK would announce on Twitter (now X) that they would be restocking PS5s on their website, we would tell our users to run CarnageBot and they would buy them as soon as they released.

![Game UK announcing PS5 Launch](launch-tweet.png){: w="500" h="300"}

### First Challenge

Fast forward to some months later, **my heart dropped**: they had implemented a virtual queue on all PlayStation 5 products, thus forcing our software to wait in line until it was our turn. As you are probably thinking: most times, **by the time you had completed the queue, all products were already sold out**. Sure, it was easier for our software to enable purchase as you could enter queue with thousands of tasks, since all requests were being done via code. Nevertheless **I had to** find a solution.

![Virtual Queue](virtual-queue.jpg){: w="330" h="500"}

### Broken Queue-It Trigger

Using my *think outside the box* skills, led me to a thought: how was it possible for the queue to be implemented **exclusively** on PS5 product pages? As is expected, Game UK wouldn't want to limit access to other non PS5 related products to their real customers: it would be completely unnecessary, as those had no stock limitations. 

### Queue-It Implementation

Queue-It, a very popular queuing system was being used by Game UK to limit users into accessing PS5 product pages. This is the same queue technology behind Ticketmaster, Liverpool tickets, and many, many more websites. However, I was sure there had to be a way to bypass the queue and so I read their [website](https://queue-it.com/developers/how-queue-it-works/):

"The virtual waiting room is implemented using an actions and triggers framework. When a trigger happens (e.g. a user visits a specific web page), you can trigger the waiting room logic (e.g. the user is redirected to your customized waiting room)".

**Available triggers include**:

- Request URL
- Request Body
- User Agent
- Cookie
- HTTP Header
- JavaScript

Noticing this behaviour, I made the assumption, they were enabling the queue to **Request URLs** which matched PS5 product pages, such as: `https://www.game.co.uk/en/playstation-5-console-ea-sports-fc-free-charging-dock-white-2922772`. 

So I thought of something, what if I made the bot continually try to add to basket the product, instead of first monitoring the product page? Could it be possible the Queue was only active on product pages, and not on other requests?

**Sure enough, that actually worked!**

I hard-coded the add-to-cart URLs for all PS5 products into the bot:
```c#
 if (pid == "2836790")
{
  atcUrl = "https://www.game.co.uk/webapp/wcs/stores/servlet/OrderChangeServiceItemAdd?updateable=0&langId=44&catEntryId_1=2836790&quantity=1&contractId=&URL=AjaxMiniShoppingBagView&page=pdpPage&componentsPartNumber=800821%2C800845&catalogId=10201&errorViewName=AjaxActionErrorResponse&calculationUsage=-1%2C-2%2C-3%2C-4%2C-5%2C-6%2C-7&partNumber=151640-kit&storeId=10151&productId_1=2836790&mediaId=";
}
```
I had found a bypass to the annoying queue system, and enabled our customers to purchase PS5s before than anyone else. And so they lived happily ever after, right? Well, unfortunately, a few weeks later **Game UK had caught my bypass out** and implemented a seemingly more robust queue system...

### Second Challenge

Unfortunately, they realised our bot was making add to cart requests directly to their PS5 products, and this time, they also implemented the queue in the add to cart endpoints! But I wasn't going to give up.

### Analysing Game UK's E-Commerce

Promptly, I realised they were using [WebSphere](https://help.hcltechsw.com/commerce/8.0.0/database/concepts/cdb_schema_overview.html) to handle the e-commerce of the website: product launches, order handling, carts, etc. To better understand my target, I read the publicly available documentation and stumbled upon something interesting:

![Alternative Endpoints for Carting a Product](order-item-urls.png){: w="700" h="900"}

WebSphere commerce websites had exposed specific endpoints for product handling, and one caught my interest [**OrderItemUpdate**](https://help.hcltechsw.com/commerce/8.0.0/developer/refs/rosorderitemupdate.html).

The description of the endpoint says: *OrderItemUpdate URL updates order items in an existing order.* Moreover it had a very similar structure to the regular add to cart url all product pages presented:

![Order Item Update](order-item-update.png)

I knew the queue was being triggered upon **two actions**: Request URLs to PS5 product pages, and Request URLs to add to cart queries with PS5 product pages on them.
However, replacing the `/servlet/OrderChangeServiceItemAdd` endpoint by `/servlet/OrderItemUpdate`, didn't work straight away. This new endpoint I had found, only worked on existing orders, so I added a random product onto the basket, removed it, and then I could in effect update the basket with any new product I wanted, since it would've counted as *creating an order*.

Sure enough, I implemented this behaviour onto the bot, and waited for the next release to see if it would work.

## International Media Exposure

Mid January 2021, Game UK was ready for a large restock of PS5s, and many people were ready to wait in line. Game UK tweeted about it: the **restock was live**! Our customers ran the bot, skipped completely the queue and they started checking out as many consoles as they wanted!

![Game UK Success](game-uk-success.png)

Among our userbase, they had collectively **bought thousands of consoles** once again, while most people were stuck in line waiting for their position to come. By the time it happened, they were all **out of stock**, as our userbase had bought most of them. 

Press started picking this up promptly, first small media were writing articles about the success obtained by our software. Soon after, [Forbes](https://www.forbes.com/uk/advisor/personal-finance/2021/01/21/playstation-5-calls-for-action-on-scalper-bots/), [BBC](https://www.bbc.co.uk/news/technology-55740291), [The Guardian](https://www.theguardian.com/money/2021/jan/22/scalper-bots-uk-xbox-series-x-playstation-5) and many other international media picked up the news and wrote articles mentioning the bot I had developed and how it was able to enable our users to buy thousands of consoles, which by then still had a net profit of 200-300€ per unit.

This was an incredible achievement for me, despite all the hate we received, we had proven ourselves. Leveraging my critical thought process, we dominated the web software automation market for purchasing other limited items for much more time to come, finding other bypasses and interesting behaviour in websites which I may share some day.

![Collage Screen Of Press](press.png)

## Conclusions

There are some conclusions I extract from this experience:
- Never give up. Try to **understand your target from the inside**, how things are operating and connected between each other. This could lead to interesting findings.
- Sometimes, bad press can be good press. Thanks to all the press we received, our brand was even more established and grew even larger.
- Nothing lasts forever. After Game UK noticed once again our bypass, they patched it correctly and I wasn't able to find any other bypass on their website.