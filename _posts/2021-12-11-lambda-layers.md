---
date: 2021-12-11 10:46:40
layout: post
title: Using Lambda Layers with AWS Lambda
subtitle: Getting third party python libraries to run in AWS Lambda
description: Setting up lambda layers to run pypi libraries in AWS Lambda.
image: https://blog-haylium.s3.amazonaws.com/lambda-layers-2.jpg
optimized_image: https://blog-haylium.s3.amazonaws.com/lambda-layers-2.jpg
category: code
tags:
  - aws
  - aws lambda
  - serverless
  - python
author: jeff
---
## Setting Up Lambda Layers
### What is AWS Lambda?
Lambda is AWS' serverless offering - it allows you to run code without having to worry about
setting up a server of your own and deploying code to it. This effectively lets us run 
lightweight programs with a variety of different triggers and in different runtimes.

My languages of choice are python and C++ and luckily for me AWS lambda is compatible with the following
languages:

- Python
- Java
- Go
- PowerShell
- Node.js
- C#
- Ruby

### Using Python Libraries with Lambda
Lambda runs in its own containerized environment, which you don't have access to as a result any libraries or modules that your python package uses must be included in your package. You have two options here, you could either zip your
dependencies along with your code and upload that, or you can create a lambda layer.

I'm a fan of the layer approach because it then allows you to use those dependencies with other lambda functions that 
you create.

### Creating a Lambda Layer
The first thing to do is to create the directory that you'll be installing your lambda layer packages into. You can make a layer that contains multiple python libraries, which is what I did with my <a href="#">AirTable Project</a> for <a href="https://www.givingclosetproject.org">The Giving Closet Project</a>. Make sure to name the directory python as 
that is what AWS is expecting.
```
mkdir python
cd python
```
Once that is created and you are inside the directory, you can install your libraries. Just to reiterate you can install more than one library.
```
# Single library 
pip install pyairtable -t .

# Multiple libraries
pip install pandas -t .
```
The -t that you see there is the flag for target and it is used to specify where pip should install the libraries.
In this case I've used "." so pip is going to install it in the current directory python/.

After the installation is complete it is time to zip the folder. If you are on a unix based system - MacOS or Linux - you can use the following command to zip the directory.
`zip -r <layer-name>.zip .` 
If you're running Windows zip the python directory that created in whatever way you normally would.

Once that's done you can upload the layer to lambda.

### Upload Using the Console
Navigate to Lambda and then Layers
![image-title-here](https://blog-haylium.s3.amazonaws.com/lambda-layer-start.png){:class="img-responsive"}


After clicking on the create layer button,  you'll enter the name and upload the zip that you created earlier. 
![image-title-here](https://blog-haylium.s3.amazonaws.com/lambda-layer-create.png){:class="img-responsive"}

You'll also choose the architecture that you want lambda to use and 
pick your runtimes -language and version. Once complete you can hit select and start using your layers!

![image-title-here](https://blog-haylium.s3.amazonaws.com/lambda-layer-create-2.png){:class="img-responsive"}

Now you'll be able to use that outside library whenever you need it!

