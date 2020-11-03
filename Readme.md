# Microservice Project

## Setup Rapid

### Locally

Install RabbitMQ locally: https://www.rabbitmq.com/download.html

### On Heroku

Create a directory:
```
mkdir rapid
cd rapid
git init
```

Create an environment on heroku to host the Rapid:
```
heroku create
```

Make sure you have a verified Heroku account. This means you need to input your credit card information on Heroku. You will _not_ be charged for anything in this exercise.

Create the Rapid:
```
heroku addons:create cloudamqp:lemur
```

Get the configuration variables for the Rapid:
```
heroku config
```
Copy the CLOUDAMQP_APIKEY and the CLOUDAMQP_URL and paste them somewhere.

## Setup new service

Get the base from GitHub:
```
git clone https://github.com/thedrlambda/java-service-base.git
```

Rename it to the service name: 
```
mv java-service-base [service name]
```

### Run it locally

To build the project use:
```
mvn clean install
```

Then run the app with either:
```
java -jar target/java-getting-started-1.0.jar
```
Or:
```
heroku local web
```

Use ctrl+c to exit it.

### Deploy it to Heroku:

Create an environment on heroku:
```
heroku create
```

Redirect `git push` to point to heroku:
```
git remote remove origin
git remote rename heroku origin
```

Add the config values for the Rapid:
```
heroku config:set CLOUDAMQP_APIKEY=XXXX
heroku config:set CLOUDAMQP_URL=XXXX
```

First deployment to Heroku:
```
git push -u origin master
```

Subsequent deployments to Heroku:
```
git push
```

To view console output from app running on Heroku use:
```
heroku logs --tail
```

## Exercises

In the beginning just hard code values everywhere and focus on getting the correct message flow. Then focus on deploying everything to Heroku. Then add actual Postgres databases. Then add a front-end. Then add actual authentication and user management. Then make Amazon.

Whenever you make changes see how many of the services you have keep running. 

### Product service
A "fetch-product", or "fetch-product-page" event contains a sessionId, a userId, and a productId, separated by commas.

When the product service receives a "fetch-product", or a "fetch-product-page" event it should send a "display" event containing the sessionId, the string "product", the productId, and a product name.

### Gateway service
When the gateway service receives a "display" event it prints the body.

Initially (for testing) it sends a "fetch-product-page" event. Running it should cause it to display something like:
```
SESSION_ID,product,PRODUCT_ID,Coffee
```

### User service
When the user service receives a "fetch-product-page" event, it sends a "fetch-stock" event with the sessionId, the user's preferred location, and the product id.

### Inventory service
When the inventory service receives a "fetch-stock" event, it should post a "display" event containing the sessionId, the string "stock", the productId, and the stock of the product in the location.

Running the gateway now should cause it to display something like:
```
SESSION_ID,product,PRODUCT_ID,Coffee
SESSION_ID,stock,PRODUCT_ID,5
```

### Recommendation service

When the recommendation service receives a "fetch-product-page" event it should:

1. Store the productId with this users item from a cache table. 
2. Update the cache table with the new productId for this userId. 
3. Lookup the three most commonly linked productIds to this productId. 
4. Send a "display" event with the sessionId, the string "recommendation", and the three product ids. 
5. Send three "fetch-product", one for each productId. 

Running the gateway now should cause it to display something like:
```
SESSION_ID,product,PRODUCT_ID,Coffee
SESSION_ID,stock,PRODUCT_ID,5
SESSION_ID,recommendation,PRODUCT_ID1,PRODUCT_ID2,PRODUCT_ID3
SESSION_ID,product,PRODUCT_ID1,Coffee
SESSION_ID,product,PRODUCT_ID2,Coffee
SESSION_ID,product,PRODUCT_ID3,Coffee
```

