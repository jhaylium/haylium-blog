---
date: 2021-12-10 11:20:40
layout: post
title: Digging into Gravity Forms Wordpress API
subtitle: Exploring the API of this popular WordPress form builder
description: 
image: https://blog-haylium.s3.amazonaws.com/Gravity_Forms.png
optimized_image: https://blog-haylium.s3.amazonaws.com/Gravity_Forms.png
category: code
tags:
  - api
  - gravity forms
  - wordpress
author: jeff
---

# Gravity Forms
<a href="https://www.gravityforms.com/">GravityForms</a> is a WordPress plugin that allows people to set up forms on their sites.
You can use this for everything from surveying people to using them as order forms. I'm 
on the Board of a non-profit called <a href="https://givingclosetproject.org">The Giving Closet</a>.
Local social workers and teachers can order clothing for disadvantaged children and pick them 
up from different facilities. In order to get a better accounting of how many children we're
serving I sat down and dug into the API.

### Gravity Forms API Access
The first thing that you'll need to do is get your gravity secret and key from WordPress.
Click on the Gravity Forms Icon in the left Navbar and then click on the REST API Button. 

![gravity-form-api](https://blog-haylium.s3.amazonaws.com/gf-api.PNG){:class="img-responsive"}

Once that is done you can click on Add Key and create your secret and key that you'll use to interact with the API.


### Connecting to the API
I decided to create a Gravity Form class to connect to the API so that I could use it across different forms or
even clients if I needed to. In order to switch between clients you can turn `url` into a parameter. This is all
you need to connect to the API.

```
import requests
import csv
import pandas

class GravityForms:

    def __init__(self, secret, key, date=None):
        self.url = "https://<wordpress url>/wp-json/gf/v2"
        self.consumer_secret = secret
        self.consumer_key = key
        self.headers = {"Content-Type: application/json"}
        if date:
            self.report_date = date
        else:
            self.report_date =begin_last_week.strftime('%Y-%m-%d')
```

### Grabbing the list of forms
In order to get any data from a particular form you need the form id. This id's weren't
readily apparent, but you can get them by grabbing the list of all available forms. As such
my first method pulls back the list of available forms so that you can identify the Form ID 
and other metadata.

```
    def get_forms(self):
        r = requests.get(f"{self.url}/forms", auth=(self.consumer_key, self.consumer_secret))
        print(r.status_code)
        if r.status_code == 200:
            return r.json()
        else:
            return -1
```

# Getting Form Information
Once you have the form id you can start to get the data from the actual forms. I noticed something
interesting when pulling the actual data. If you use the endpoint that returns the actual responses.
The keys in the JSON payload aren't the actual names of the questions asked in the forms. They're id's
this isn't too odd as you'll see it with a number of different providers like this. 

```
{
    "28": "Redacted",
    "65": "Redacted",
    "66": "Test",
    "6": "Test",
    "7": "Redacted",
    "76": "Email",
    "74": "Redacted",
    "112": "Redacted",
    "87.4": "Redacted",
    "87.5": "Redacted",
    "87.6": "Redacted"
}
```
The only problem is that I had no idea where to get the actual question names. Luckily for me, it didn't
take much research to find the endpoint that contained the questions themselves. The code below will 
give you the links between those answers and their questions.

```
    def get_form_info(self, form_id):
        r = requests.get(f"{self.url}/forms/{form_id}", auth=(self.consumer_key, self.consumer_secret))
        if r.status_code == 200:
            return r.json()
        else:
            return -1
```

### The Data
Now that I had the ids and the links between the answer names and their keys it was time to pull the 
data. This was pretty straight forward.

```
    def get_form_entries(self, form_id, date=None):
        if date is None:
            date = self.report_date
        # Due to this being in markup the following line is incorrect each { should be two curly braces
        date_search = f'{"start_date": "{date}"}'
        r = requests.get(f"{self.url}/forms/{form_id}/entries?search={date_search}&paging[page_size]=1000",
                         auth=(self.consumer_key, self.consumer_secret))
        if r.status_code == 200:
            return r.json()
        else:
            print(r.content)
            return -1
```

#### Date Search
The search keyword allows you to input values that can be used to filter the data. I chose to filter based on 
the date and there are two ways to accomplish this:

The start date and end date keywords allow you to pull a specific range of dates 
`search={"start_date":"2021-04-01", "end_date": "2021-04-02"}'`

The `start_date` keyword on it own will return all data after the date you choose and the `end_date` keyword will do the opposite.


### The Final Touches
I used pandas to combine the headers from the info call and the data from the entries call. I wrote the data 
to a dataframe and saved that to a CSV to send over to the CEO of the Giving Closet Project. Later on I ended 
up writing a package that pushes the data to Airtable which you can read about in this <a href="#">post</a>.

