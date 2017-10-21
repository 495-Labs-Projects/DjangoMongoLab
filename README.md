## MongoMart API: Incorporating NoSQL Databases into Django ##
While Postgres, MySQL, and many other SQL databases have been the industry standard for data storage over the years, a new trend in database technology has been emerging: NoSQL databases. SQL systems have been around since the 1970's, meaning they offer advantage in terms of support and community, but the ever-growing world of NoSQL technology is rapidly approaching (in fact, NoSQL database technology encompasses 13% of the entire database market as-is).

Companies like (the now public) MongoDB, Apache (with CouchDB), and others have been pursuing new models for schema design and data storage that adapt more easily to modern web application stacks, offering advantage in scalability and performance.

In this lab, we will explore how to utilize the MongoDB framework within a Django application to create a simple API for an online content management system.

## Part 0: Relevant Documentation
Here are some links to documentation for the relevant frameworks we will be using in this lab. They are great ways to figure out how to best use such frameworks for this lab and beyond!

[MongoDB](https://docs.mongodb.com/) (Mainly the MongoDB Server, Version 3.4)
[mongoengine](http://mongoengine.readthedocs.io/en/latest/tutorial.html)

## Part 1: Setup and Installation
Now we will define a new Django app which is powered by MongoDB in the backend.

1. Create a new Django project named `MongoMart` and navigate to the localhost server to ensure it is working as expected.

2. Create a Django app within `MongoMart` named `products`.

Now we will convert the backend for `MongoMart` to use `mongoengine`. **Please refer to [this guide](https://github.com/495-Labs-Projects/install_django_mongodb) for instructions on installing MongoDB 3.4 and mongoengine.**

Be sure that the development server works as anticipated after converting the backend to `mongoengine` before continuing!

## Part 2: Understanding Schema Design
Naturally, many problems are inherently thought through in a NoSQL-esque manner. As dicsussed in class, the means by which problems are solved in a NoSQL manner are fundamentally different from how they are solved in a traditional Relational SQL system.

Here are the properties of a `product` for our mart that we must include:

```
name
price
inventory_level
description
image_link
*reviews*
*reorders*
```

In a relational system, this model may encompass multiple tables and would like require the use of joins and referential integrity between tables in order to maintain database normalization.

However, in NoSQL thinking, we consider the `product` almost as its own object and think of encapsulating its properties in a JSON object to eliminate joins and improve efficiency (joins are expensive!). Also note that a product can have 0+ reviews and reorders. Finish the schema below (on a piece of paper or something) and if you are not 100% confident, please consult a TA or ask on Piazza before continuing.

```javascript
// The Product Schema (in NoSQL JSON format):
{
  "name": String,
  ...,
  // Note the reviews are represented as a list of objects
  "reviews" = [{
    rating: Number,
    content: Text
  }],
  ...
}

```

## Part 3: Implementation
Now that we have a NoSQL schema design mindset for how we want to structure the products in our MongoMart API, we have to create our model implementation for the `products` app within a models.py file in `products`.

We are going to leave the exact API implementation up to you, except for the following points:
1. `reviews` should be defined as its own Python class.
2. `reorders` should be defined as its own Python class.
3. There should be a means by which an individual product can be selected from a URL endpoint.
4. There should be a means by which all products can be retrieved at once from a URL endpoint.

Note use of GraphQL is not required, but it certainly would add to the usability of your API.

To get you started here, your `models.py` should look something generally like this:

```python
from mongoengine import *

class Review(EmbeddedDocument):
  ...

...

class Product(Document):
  ...
```

*Hint: What is the difference between `Document` and `EmbeddedDocument`?*

Stuck? Consult the [documentation for `mongoengine`](http://mongoengine.readthedocs.io/en/latest/tutorial.html) before asking a TA or on Piazza!

## Part 4: Testing in `mongoengine`
In case you didn't add the `TEST_RUNNER` during the installation step, add the following line to your `settings.py` file within the overall MongoMart project:

```python
TEST_RUNNER = 'mongomart.tests.NoSQLTestRunner'
```

Please now add tests to your `products` app to test the schema design of your `Product` class and such. Verify these tests and comprehensive and pass as expected.
