---
date: 2021-09-26 11:20:40
layout: post
title: The Quest for Shoes
subtitle: Using Python, Selenium, and Beautiful Soup to get the best price.
description: I used python and a few libraries to build a bot which checks to see if my favorite brand of shoes are on sale in my size.
image: https://blog-haylium.s3.amazonaws.com/vibram.jpg
# optimized_image: https://res.cloudinary.com/dm7h7e8xj/image/upload/c_scale,w_380/v1506079212/jekflix-capa_vfhuzh.png
optimized_image: https://blog-haylium.s3.amazonaws.com/vibram.jpg
category: code
tags:
  - code
  - python
  - web automation
  - beautiful soup
author: jeff
---

One thing to know about me is that I love <a href="https://us.vibram.com/">Vibram’s </a> Five Fingers line of shoes. I've been wearing these guys since 2010 when I stumbled across a shoe store that carried the brand on Las Olas in Fort Lauderdale. I've abandoned the shod lifestyle and I'm never turning back.


I'm as loyal as they come but the shoes can be pricy at $130 a pair. Mine fall apart pretty quickly because of how much I use them, so I always buy two pairs when they're half off. I've searched for 3 months to no avail. And like I said before Vibrams, while great, tend to fall apart with overuse so I refuse to pay full price.

This whole thing got a lot worse because I spent 30 days in Chicago, and developed a blister where the hole in my five fingers is. Imagine just scraping the asphalt with the tip of your second toe while you walk about 5 to 10 mi every day. It got old - quick. 

I looked at the site for what felt like the millionth time and thought to myself, "Man there's got to be a better way to do this. You know how to program how about you make something?" And that is exactly what I did.

I've been using Python for the better part of 6 years now and the solution was pretty straightforward. 


## Step 1: Automate the Site -  Selenium
I used good ole selenium for this. Selenium is a test automation program and there is a pretty well documented and well-used Python library for it. I  used it pretty extensively when I built <a href="https://pmibroker.io">pmibroker.io</a> so I had some old code that I used as a reference.

This part of the program goes to the website closes the 10% off modal that appears and then selects my size to filter the list to just shoes that'll fit me. Be warned Vibrams use European sizes and I'm thankful I bought my first pair in a store so that they could size me. 

### Webdriver advice
If you are using Chrome make sure that your webdriver is compatible with 

## Step 2: Scrape The Data - Beautiful Soup
Ah yes, data scraping. Again this is a pretty well-traveled field and you'll find plenty of tutorials out there telling you how to do it so I won't. For this part of the program, I chose to use beautiful soup which allowed me to grab the data from the driver object that I created in step one. It takes all of the HTML from the site that I've navigated to. Now all I had to do was parse it. Having read plenty of markup languages in my journey as a dev this was pretty straightforward too. I created variables to store the URL, product name, and price of all the shoes in my size. This was my first time using BS4 and to be honest it wasn't that different from parsing XML even though I chose to grab the soup as HTML. 

```python
page_source = browser.page_source
soup = BeautifulSoup(page_source, 'html.parser')
product_selector = soup.find_all('div', class_='product-tile')
browser.quit()
for product in product_selector:
    price = product.find('span', class_='product-sales-price').get_text()
    name = product.find('a', class_='name-link')
    price_var = float(price[1:])
    name_var = name.get_text().strip().replace("\n", "").replace("\t", "")
    url_var = name['href']

    body = f"\n Product Name: {name_var} \n\n Price: ${price_var} \n\n Url: {url_var}"
```

## Step 3: Decions, Decisions - The Algo
Okay, I might be overblowing this part. I just wrote an if statement to identify any shoes that were under $90.
```python
if price_var < 90.0:
```

## Step 4: Notification - Twilio
So I really didn't feel like running this thing myself. Automation was going to be key if I was going to be able to buy shoes in my size before anyone else. And if it's going to be automated the process has to let me know that there are shoes available in my size. So I did the only wise thing and signed up for Twilio account. That's right If my program finds shoes that meet the parameters of my complex algorithm It shoots me a text message. I'd never used Twilio before this but the documentation is out of this world it's so easier to read and implement. This part was probably about 4 to 5 lines of code I don't remember and as I'm laying in bed dictating this to my phone I'm not going to bother being accurate. 

#### Update:
Ok, I felt bad about leaving you hanging so I checked the number of lines. The entire implementation came out to 7 lines of code if you include the import statement. 

```python
from twilio.rest import Client
client = Client(account_sid, auth_token)
if price_var < 90.0:
        message = client.messages.create(
            body=body,
            from_=f'{twilio_phone_number}',
            to=f'+{my_phone_number}'
        )
```

All in all The program works very well but it still has an identified any shoes in my size. I haven't yet decided on a place for it to live so it just runs locally off of my laptop for now. I will eventually either move it to one of four locations: AWS, digital ocean, the Dell PC that I got for free and turned into a Linux machine, or one of the raspberry pis that I've laying around. 

The working version of the code took me about an hour and a half to write while I was watching a fantastic documentary on HBO Max about Q. It’s pretty solid, think about doing the same if you're looking for a simple project to complete.

