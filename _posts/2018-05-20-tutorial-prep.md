---
layout: post
title:  "Dutch PHP Tutorial Preparation"
---
For anybody who will be attending my tutorial at Dutch PHP next month 
here is what you need to do to get ready. 

## Install Node and NPM

This may be a PHP conference but Chrome loves JavaScript so that is what we need to use.

So go ahead at follow the instructions at the [Node.js download page](https://nodejs.org/en/download/).
For Ubuntu users you may want to take a look at some [alternate instructions](https://nodesource.com/blog/installing-node-js-tutorial-using-nvm-on-mac-os-x-and-ubuntu/)
if you need help getting it running.

Now you can try running the following command at a command line.
```
npm -v
```
If this works you are set.

## Project Setup
Next we need to set up a basic project. The best way to do this is to clone or download 
my [GitHub repository](https://github.com/derek-b/headless-tutorial). Once you have that 
downloaded get back to a command line in the project directory and run:
```
npm install
```

This will install Puppeteer, Mocha, and Chai for use with our tests.

## Run the Site
The next part is not required but could be useful. You should have the ability to run 
php at your command line. There are lots of ways to install PHP that are detailed on the 
[PHP website](http://php.net/manual/en/install.php). Once you have that install run the 
following from our project root.
```
php -S localhost:8000 -t src/
```

## High Five!
We're ready to learn.