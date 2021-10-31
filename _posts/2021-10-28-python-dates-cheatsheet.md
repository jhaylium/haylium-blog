---
date: 2021-10-28 11:20:40
layout: post
title: Python Dates Cheat Sheet
subtitle: Quick tips and tricks to work with dates in python
description: I used python and a few libraries to build a bot which checks to see if my favorite brand of shoes are on sale in my size.
image:  https://blog-haylium.s3.amazonaws.com/calendar.jpg
# optimized_image: https://res.cloudinary.com/dm7h7e8xj/image/upload/c_scale,w_380/v1506079212/jekflix-capa_vfhuzh.png
optimized_image: https://blog-haylium.s3.amazonaws.com/calendar.jpg
category: code
tags:
  - code
  - python
  - dates
author: jeff
---

The subject of dates comes up pretty often in the projects that I work on. I find myself heading to google pretty often to find 


## Date Offsets using replace
```python
import datetime


today = datetime.datetime.today()
first_of_month = today.replace(day=1)
first_day_of_year = today.replace(day=1, month=1)
previous_year = today.replace(year=(today.year - 1))
same_day_last_year = today.replace(year=(today.year - 1),
                                  day=(today.day + 1))
previous_day_last_year = today.replace(year=(today.year - 1),
                                       day=(today.day -1))

print(f"First of Month: {first_of_month}")
print(f"First day of Year: {first_day_of_year}")
print(f"Previous Year: {previous_year}")
print(f"Same Day Last Year: {same_day_last_year}")
print(f"Previous Day Last Year: {previous_day_last_year}")
```

### Strftime Formats
I use <a href="https://strftime.org/">Strftime.org</a> pretty often when looking up the available formats for strftime. Just in case this awesome resource ever goes away, I've backed up their contents here.

<table>
<tr><th>Code </th><th>Example </th><th>Description</th></tr>
<tr><td>%a </td><td>Sun </td><td>Weekday as locale’s abbreviated name.</td></tr>
<tr><td>%A </td><td>Sunday </td><td>Weekday as locale’s full name.</td></tr>
<tr><td>%w </td><td>0 </td><td>Weekday as a decimal number, where 0 is Sunday and 6 is Saturday.</td></tr>
<tr><td>%d </td><td>08 </td><td>Day of the month as a zero-padded decimal number.</td></tr>
<tr><td>%-d </td><td>8 </td><td>Day of the month as a decimal number. (Platform specific)</td></tr>
<tr><td>%b </td><td>Sep </td><td>Month as locale’s abbreviated name.</td></tr>
<tr><td>%B </td><td>September </td><td>Month as locale’s full name.</td></tr>
<tr><td>%m </td><td>09 </td><td>Month as a zero-padded decimal number.</td></tr>
<tr><td>%-m </td><td>9 </td><td>Month as a decimal number. (Platform specific)</td></tr>
<tr><td>%y </td><td>13 </td><td>Year without century as a zero-padded decimal number.</td></tr>
<tr><td>%Y </td><td>2013 </td><td>Year with century as a decimal number.</td></tr>
<tr><td>%H </td><td>07 </td><td>Hour (24-hour clock) as a zero-padded decimal number.</td></tr>
<tr><td>%-H </td><td>7 </td><td>Hour (24-hour clock) as a decimal number. (Platform specific)</td></tr>
<tr><td>%I </td><td>07 </td><td>Hour (12-hour clock) as a zero-padded decimal number.</td></tr>
<tr><td>%-I </td><td>7 </td><td>Hour (12-hour clock) as a decimal number. (Platform specific)</td></tr>
<tr><td>%p </td><td>AM </td><td>Locale’s equivalent of either AM or PM.</td></tr>
<tr><td>%M </td><td>06 </td><td>Minute as a zero-padded decimal number.</td></tr>
<tr><td>%-M </td><td>6 </td><td>Minute as a decimal number. (Platform specific)</td></tr>
<tr><td>%S </td><td>05 </td><td>Second as a zero-padded decimal number.</td></tr>
<tr><td>%-S </td><td>5 </td><td>Second as a decimal number. (Platform specific)</td></tr>
<tr><td>%f </td><td>000000 </td><td>Microsecond as a decimal number, zero-padded on the left.</td></tr>
<tr><td>%z </td><td>+0000 </td><td>UTC offset in the form ±HHMM[SS[.ffffff]] (empty string if the object is naive).</td></tr>
<tr><td>%Z </td><td>UTC </td><td>Time zone name (empty string if the object is naive).</td></tr>
<tr><td>%j </td><td>251 </td><td>Day of the year as a zero-padded decimal number.</td></tr>
<tr><td>%-j </td><td>251 </td><td>Day of the year as a decimal number. (Platform specific)</td></tr>
<tr><td>%U </td><td>36 </td><td>Week number of the year (Sunday as the first day of the week) as a zero padded decimal number. All days in a new year preceding the first Sunday are considered to be in week 0.</td></tr>
<tr><td>%W </td><td>35 </td><td>Week number of the year (Monday as the first day of the week) as a decimal number. All days in a new year preceding the first Monday are considered to be in week 0.</td></tr>
<tr><td>%c </td><td>Sun Sep 8 07:06:05 2013 </td><td>Locale’s appropriate date and time representation.</td></tr>
<tr><td>%x </td><td>09/08/13 </td><td>Locale’s appropriate date representation.</td></tr>
<tr><td>%X </td><td>07:06:05 </td><td>Locale’s appropriate time representation.</td></tr>
<tr><td>%% </td><td>% </td><td>A literal '%' character.</td></tr>
</table>



