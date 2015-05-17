# Getting Started

## Bazooka Overview

Bazooka is a highly modular Continuous Integration and Continuous Delivery Server.
Out of the box, Bazooka supports many SCMs (Git, Mercurial, ...) to fetch the source code of your application, and can easily be extended to support other SCMs.
Bazooka also comes with built-in support for many languages (Java, Go, Python, Node...), with the possiblity of supporting others by creating custom docker images, or easier still, by using any container available on [docker Hub](https://hub.docker.com/) as a build environment for your build process.

## Step 1: Register a new project

Once Bazooka is up and running, you can register your project in Bazooka

### Register a new project with CLI
```
bzk project create NAME SCM_TYPE SCM_URI [SCM_KEY]
```

For instance, if you want to build bazooka on bazooka itself:
```
bzk project create bazooka git \
  git@github.com:bazooka-ci/bazooka.git ~/.ssh/id_github
```

The private SCM Key is optional. If none is provided, Bazooka will try to use the default SCM Key provided during [Bazooka installation](../home/installation.html)

### Register a new project with the Web Interface

(Coming Soon)

## Step 2: Setup SCM Hook

To setup a SCM hook, you will need the ID of your project, as well as the hook key, a secret key that will be used for the authentication.

You can get all the information you need with bazooka cli

```
> bzk project list
PROJECT ID     NAME                SCM TYPE       SCM URI                               HOOK KEY
f0cd487a       my_project          git            https://github.com/me/project.git     9a2ec0e2284761449d1d718c3dbe373a
983db807       my_other_project    git            https://github.com/me/other.git       03556bc99e790a0093e21fcf81d8ea27
```

### Setup Github Webhook

Go to the settings of your github project, in the tab *Webhooks & Services*

In the *Webhooks* space, choose *Add Webhook*

Fill the fields like this

![Github Webhook](https://raw.githubusercontent.com/bazooka-ci/docs/master/assets/img/github_webhook.png)

The Payload URL should look like this

*https://<url of your bazooka server>/project/<id of your project>/github*

## Step 3: Add the .bazooka.yml file to your repository

As described in our philosophy, the configuration of your bazooka build is versioned alongside your code.

Create a `.bazooka.yml` file and commit it in your repository.
The format of this file is described in detail in the section [Configure your build](../home/build_configuration.html)

## Step 4: Trigger your first build

A build can be triggered either manually or by an SCM hook.

To manually trigger a build, use the Bazooka cli:

```
bzk job start NAME master
```

When an SCM hook is set up, a build is automatically triggered whenver you push a new commit to the remote repository.
