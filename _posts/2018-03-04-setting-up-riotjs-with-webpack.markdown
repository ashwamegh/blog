---
layout: post
title: Setting up RiotJS project with Webpack
date: 2018-03-05 16:27:00 +0530
description: Learn how to setup your Riot JS project with Webpack - a deeper dive # Add post description (optional)
img: riot-plus-webpack.png # Add image post (optional)
tags: [Programming, Learn, RiotJS, Webpack] # add tag
---

Hey guys, I thought of posting this article a long time back but being busy, wasn't able to. So for those who don't know what **RiotJS** is,

> Riot is Web Components for everyone. Think React + Polymer but without the bloat. It’s intuitive to use and it weighs almost nothing. And it works today. No reinventing the wheel, but rather taking the good parts of what’s there and making the simplest tool possible. [[Source](http://riotjs.com/)]

So I think, most of you have got the idea about what **RiotJS** is accompolishing. It is delivering tiny reusable components with minimalistic, fast and a comprehensible API.

Most of us desires to save time by using any **module bundler** and **hot reload** functionality, where comes **Webpack** comes into play. We'll be here implementing, how to setup `RiotJS` projects and `Webpack` together.

> TL;DR : You can view the source code here: [[Source Code](https://github.com/shashank7200/riot-webpack-starter)]

<br/>

## :beginner: Setting up your NPM project

Goto the project directory, open your **terminal**, type commands below:

{% highlight shell %}
# this will create a directory named riot-plus-webpack
mkdir riot-plus-webpack

# this will change your directory to your newly created directory
cd riot-plus-webpack

# this will initialize your NPM project with default rules
npm init -y

{% endhighlight %}

We have now successfully created a NPM project, It's time to add the `webpack` configuration for our project.

<br/>

## :construction: Setting up our Webpack Configuration

Adding Webpack will solve us a lot of manual tasks we need to do everytime we will be changing our code. So what adding webpack to our project solves is:

- Compiling all the riot `tags` to the native `javascript` code.
- Module hot reloading
and much more, but these are the two keypoints which saves developer's time a lot.

<br/>

> Before setting up this configuration, just add some of the `npm` packages, we will need in between:

{% highlight shell %}

# this will add the packages we will need
npm i -D babel-core babel-loader babel-preset-es2015-riot riot-hot-reload riot-tag-loader webpack
webpack-cli webpack-dev-server

npm i -S riot

{% endhighlight %}

The final `package.json` file will be similar to:

{% highlight json %}
{
  "name": "riot-plus-webpack",
  "version": "1.0.0",
  "description": "Starter setup for a riotjs project with webpack",
  "main": "index.js",
  "scripts": {
    "start": "./node_modules/.bin/webpack-dev-server --inline --watch --hot --colors -d --port 3000"
  },
  "keywords": [
    "riotjs",
    "webpack"
  ],
  "author": "Shashank Shekhar [shashank7200.github.io]",
  "license": "MIT",
  "devDependencies": {
    "babel-core": "^6.26.0",
    "babel-loader": "^7.1.3",
    "babel-preset-es2015-riot": "^1.1.0",
    "riot-hot-reload": "^1.0.0",
    "riot-tag-loader": "^2.0.2",
    "webpack": "^4.1.0",
    "webpack-cli": "^2.0.10",
    "webpack-dev-server": "^3.1.0"
  },
  "dependencies": {
    "riot": "^3.9.0"
  }
}


{% endhighlight %}

> To setup the webpack configuration , create a new file:

{% highlight shell %}



# this will create a webpack config file
touch webpack.config.js


{% endhighlight %}


Copy and Paste the code shown below into that file:

{% highlight javascript %}
// webpack.config.js

const path = require('path');
const webpack = require('webpack');

module.exports = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'public'),
    publicPath: '/public/',
    filename: 'main.js'
  },
  devtool: 'inline-source-map',
  module: {
    rules: [
      {
        test: /\.tag$/,
        exclude: /node_modules/,
        loader: 'riot-tag-loader',
        query: {
          type: 'es6', // transpile the riot tags using babel
          hot: true
        }
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel-loader'
      }
    ]
  }
}
{% endhighlight %}

This will initialize the webpack configuration with this project. You have already observed that, we are using `babel-loader` here, so we will need to define a `babel` configuration file so that, webpack can tell babel to use that configuration file to get the presets it'll need to compile down the `tag` files into `javascript` code.

{% highlight shell %}
touch .babelrc
{% endhighlight %}

Copy & paste the code below into that file:
{% highlight json %}
{
  "presets": [ "es2015-riot" ]
}
{% endhighlight %}

## :vertical_traffic_light: Setting up Project Directory

Now we have our npm project ready with a webpack configuration for our riot project, we'll setup our project structure to have some initial files. Our project directory will be looking similar to :
{% highlight shell %}
|____webpack.config.js
|____package-lock.json
|____src
| |____index.js
| |____tags
| | |____hello-world.tag
|____.babelrc
|____public
|____index.html
|____package.json
{% endhighlight %}

As per this project structure, you'll need to create the folders and files, if they don't exist. Don't worry if you are not getting it, once it is done, you'll get it.

First of all, we need to create a `hello-world.tag` file which can hold our riot tag code.

So create a riot tag `src/tags/hello-world.tag`, and put the code shown below, to display programmer's traditional hello world output.

{% highlight html %}
<!--  src/tags/hello-world.tag  -->
<hello-world>
  <h3>Hello from the Hello World Riot Tag</h3>

  <script>
    console.log("Hello from the Hello World Riot Tag");
  </script>
</hello-world>
{% endhighlight %}

In `index.html`, put the source code shown below:

{% highlight html %}
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Riot and Wepack Setup</title>
</head>
<body>
  <!-- Your tags goes here -->
<hello-world></hello-world>
  <!-- This script will contain all the compiled tags -->
  <script src="public/main.js" charset="utf-8"></script>
</body>
</html>
{% endhighlight %}


In `src/index.js` put this source code :

{% highlight javascript %}

import riot from 'riot';
import 'riot-hot-reload';
import './tags/hello-world.tag';

riot.mount('hello-world');

{% endhighlight %}

Your RiotJS and Webpack setup is almost complete , if you have followed this article.
Now its the time to test it out.


## :neckbeard: Preview

[[Source Code](https://github.com/shashank7200/riot-webpack-starter)]

To preview the app, Just open your `terminal` in the project directory and type below command-
{% highlight shell %}
npm Start
{% endhighlight %}

If all goes well, the app will fire up at `localhost:3000` ( your port can be different, for that check your console logs ).

Now you'll be able to see the preview of your `RiotJS` and `Webpack` setup. Which will appear similar to the image below.

<br/>

![Riot and webpack preview]({{"/blog/assets/img/riot-plus-webpack-preview.png"}})


> :bell: &nbsp; If you like this article, Please **Shareit** and if you have any queries regarding RiotJS, Webpack and this setup, please **Comment** below.
