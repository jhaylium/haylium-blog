---
date: 2021-11-29 00:56:40
layout: post
title: 8 Things to Know About Bonds
subtitle: Breaking down bond calculations
description: 
image: https://blog-haylium.s3.amazonaws.com/bonds.jpeg
optimized_image: https://blog-haylium.s3.amazonaws.com/bonds.jpeg
category: finance
tags:
  - finance
  - series 65
  - finra
  - bonds
author: jeff
---
# Bonds - The Finer Points
I was having some issues remembering how to calculate bond yields, so I decided to make a blog
post about it.

### Yield to Maturity
The yield to maturity on a bond is the yield that you can expect if the bond is held to maturity and not called.

##### Trading at Premium
A bond trades at a premium when it is priced above par. This can happen in an environment where rates are low as bond prices increase as interest fall in order to deliver on the coupon

The calculation takes into account the bonds current price, par and years to maturity.
```
Annual Income - (Bond Price -  Par)/Years to Maturity
______________________________________________________
                (Bond Price + Par )/ 2
```

##### Trading at Discount
A bond trades at a discount when it is priced lower than par.
```
Annual Income - (Par - Bond Price )/ Years to Maturity
_______________________________________________________
                (Bond Price + Par )/2
```

### Yield to Call
This is the yield that can be expected if a bond is called. Companies will often call bonds that have higher interest rates as a way to reduce their debt service.
```
Annual Income - (Bond Price - Redemption Price)/ Years to Call
_______________________________________________________________
        (Bond Price + Redemption Price)/2
```

### Current Yield

```
Annual Interest
_______________
Bond Market Price
```

### Bond Conversion 
Convertible bonds can be redeemed for common stock, there are two key points here - Conversion Ratio & Parity Price.

##### Conversion Ratio
The conversion Ratio is how many shares of common stock you can redeem your bond for.

```
    Par
________________
conversion Price
```

If the bond is trading at a premium or a discount substitute Par for the purchase price.

##### Parity Price
The price where the convertible bond and the stock have the same value.
```
Stock Price * Conversion Ratio
```


