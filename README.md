# flask-restful-wsgi
Sample flask_restful app with Apache mod_wsgi configs

## Purpose
This repo illustrates a simplified approach for deploying a flask_restful (Python Flask) app on Apache 2.4 using mod_wsgi. In general, this is a more specific elaboration to [this guidance](http://flask.pocoo.org/docs/0.12/deploying/mod_wsgi/). Say you have a front-end that can be deployed as a static site (think React app) with the flask_restful application serving as the API layer for that front-end, this repo will help you with a basic Apache config for that: static site is at '/' and the api is at '/api'.

## Pre-reqs
I started with a Red Hat 7 instance (on AWS EC2) and did the following setup:
* `sudo easy install pip`
* `sudo pip install flask flask_restful`
* `sudo yum install httpd`
* `sudo yum install mod_wsgi`

Then, I setup standard app directories for apache conf and the app itself:
* /var/www/apps
* /etc/httpd/conf/apps

## [Pre-reqs] *if you don't have python system privileges*
Create a virtualenv where you can install python directories, put that in your deployment folder:
* `cd /var/www/apps/spam`
* `virtualenv spam-venv`
* `source spam-venv/bin/activate`
* `pip install flask flask_restful`
* In your apache conf, point to the spam-api-venv.wsgi file

## Configure
* Adjust httpd.conf
  * Set your listen port [optional]
  * Add this line to include your app configurations: `Include /etc/httpd/conf/apps/*.conf`
* Take the spam.conf from this project and place in /etc/httpd/conf/apps/
* Edit spam.conf to match your port and servername (lines 1 and 3)
* Edit spam.conf to use a virtualenv if necessary (comment line 14, uncomment line 17)
* Export this repo into a spam directory in /var/www/apps, so now you'll have the app ready like:
  * /var/www/apps/spam
    * /spam-fe/*.html
    * /spam-api/spam-api.wsgi
    * /spam-api/controllers/*.py
    * /spam-api/resources/*.py

## Deploy
When you restart apache, you'll have anything in /var/www/apps/spam/spam-fe served as simple static files (html, js, css, etc) at the root, '/', for your servername. You'll have your flask_restful app, rooted in /var/www/apps/spam/spam-api deployed at '/api' for your servername.
