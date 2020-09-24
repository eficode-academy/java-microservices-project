# Microservice Project

## Setup Rapid

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

Create the Rapid:
```
heroku addons:create cloudamqp:lemur
```

Get the configuration variables for the Rapid:
```
heroku config
```
Copy the CLOUDAMQP_APIKEY and the CLOUDAMQP_URL and paste them somewhere.

## Setup Services

Get the base from GitHub:
```
git clone https://github.com/thedrlambda/java-service-base.git
```

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



