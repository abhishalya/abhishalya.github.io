---
layout: post
title:  "Deploying Julia Web Applications using Heroku"
date:   2019-12-10 00:00:00
categories: blog-post
---

I will be explaining the process to deploy your
[Dashboards](https://github.com/waralex/Dashboards.jl) or any Julia web
applications using a free service like [Heroku](https://heroku.com). This is
also a part of my Google Code-in task. So, let's get started :)

## Step 1: Prepare

You'll need two things before moving ahead.
1. Your Dashboards application files.
2. Heroku account.

You can create a Heroku account on their site.

Once you've both of these things move to Step 2.

## Step 2: Install Heroku CLI.

You need to install Heroku CLI. You can refer to their
[official documentation](https://devcenter.heroku.com/articles/heroku-cli#download-and-install)
for the detailed instructions.

You should then login to your account using the following command:

```
$ heroku login
```
(Don't type `$`!)

It will open up your browser to enter your credentials or just with a single
button to Log in. Once you've done that, you can close that window and you'll
find yourselves logged in on the CLI.

## Step 3: Create Heroku application

You can create either by CLI or move to your dashboard and click on `New`.
This will ask you the application name and location. Just select any meaningful
name which is available and move on. Let's assume here your app name is
`sample-app-heroku`.

## Step 4: Set up new project

I'm assuming you already have a working Dashboards application. So, create a
new folder anywhere to manage your heroku deployment. To demonstrate I'm using
`sample-app` as directory for my app.

```
mkdir sample-app
cd sample-app
```

Copy all the files of your Dashboards application to `sample-app` directory.

Once you've done that, you can do following operations to link it with your
heroku app.

```
git init
git heroku git:remote -a sample-app-heroku
```

Remember to replace `sample-app-heroku` to whichever name you've used while
creating your app on Heroku.

## Step 5: Add Project.toml, Manifest.toml and Procfile to the project

You need two files to manage your dependencies. You can simply copy your
existing Project.toml file from `$HOME/.julia/environments/v1.3/Project.toml`
and Manifest.toml file from `~/.julia/environments/v1.3/Manifest.toml`.

Your Julia version might be different hence, make sure to change the `v1.3` to
your corresponding Julia version.

Also, add a `Procfile` to the directory, containing following:

```
web: julia --project app.jl $PORT
```

where `app.jl` represents your entry point for your application. `$PORT`
represents the PORT number heroku will be using for your application.
You must parse this PORT number on your script before starting the server.

## Step 6: Add Buildpack

Heroku won't be able to detect Julia application automatically, hence we'll need
to add custom buildpack to the app. You can do this by moving to your app settings
page on heroku site and on the buildpack row, add this:

```
https://github.com/Optomatica/heroku-buildpack-julia.git
```

This will take care of installing all dependencies required.

## Step 7: Commit and Push!

Now since all files are ready, just commit those files and push to heroku.

Follow the below commands:

```
git add -A
git commit -m "Initial push"
git push heroku master
```

You'll see the application building dependencies and once the process completes
the application will be live!

I hope this will make your process of deploying apps easier. You can deploy
any web apps this way (not just limited to Dashboards).
