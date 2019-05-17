---
id: installation
title: Installation
sidebar_label: Installation
---

Ionic apps are created and developed primarily through the Ionic command-line utility. The Ionic CLI is the preferred method of installation, as it offers a wide range of dev tools and help options along the way. It is also the main tool through which to run the app and connect it to other services, such as Ionic Appflow.

## Install the Ionic CLI

Before proceeding, make sure the latest version of **Node.js** and **npm** are installed. See <https://ionicframework.com/docs/installation/environment> for the details. Install the Ionic CLI globally with npm:

```
$ npm install -g ionic
```

> The -g means it is a global install. For Window’s it's recommended to open an Admin command prompt. For Mac/Linux, run the command with sudo.

## Start an App

Create an Ionic app using one of the pre-made app templates, or a blank one to start fresh. The three most common starters are the `blank` starter, `tabs` starter, and `sidemenu` starter.

Get started with the ionic start command:

```
$ ionic start myApp tabs
```

![Image of Ionic Starter](https://kmramirez3.github.io/ionic-documentation/img/start-app-thumbnails.png)


## Run the App

The majority of Ionic app development can be spent right in the browser using the `ionic serve` command:

```
$ cd myApp
$ ionic serve
```

