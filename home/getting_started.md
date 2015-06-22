# Getting Started

Once you have installed Bazooka, you can start using it to test and deploy your projects

## Step 1: Get a project to test

The first thing you need is a project to test, that is hosted on any Version Control System (VCS) that Bazooka can talk to.

## Step 2: Commit a .bazooka.yml file to your repository

As described in our philosophy, the configuration of your bazooka build is versioned alongside your code, in a `.bazooka.yml` file at the root of your project

Create this `.bazooka.yml` file and commit it in your repository.

The format of this file is described in detail in the section [Configure your build](../home/build_configuration.html) and you can also refer to the [language guides](/home/languages) for specific details on each language.

## Step 3: Register your project in Bazooka

### With the CLI

```
bzk project create NAME SCM_TYPE SCM_URI [SCM_KEY]
```

For instance, if you want to build a project name *my_awesome_project* hosted on Github with bazooka:

```
bzk project create bazooka git \
  git@github.com:awesome_orga/my_awesome_project.git ~/.ssh/id_github
```

The private SCM Key is optional. If none is provided, Bazooka will try to use the default SCM Key provided during [Bazooka installation](../home/installation.html)

### With the web Interface

* Open the home page of bazooka in your browser.
* Click on *Manage projects* in the top left corner of the bazooka interface.

![Manage Projects](https://raw.githubusercontent.com/bazooka-ci/docs/master/assets/img/getting_started1.png)

* Fill the fields like this

![Add project](https://raw.githubusercontent.com/bazooka-ci/docs/master/assets/img/getting_started2.png)

* And then click *Create new project* to confirm

## Step 4: Setup SCM Hook

To setup a SCM hook, you will need the ID of your project, as well as the hook key, a secret key that will be used for the authentication.

You can get all the information you need with bazooka cli

First get the ID of your project with the `bzk project list` command
```
> bzk project list
PROJECT ID     NAME                SCM TYPE       SCM URI
f0cd487a       my_project          git            https://github.com/me/project.git
983db807       my_other_project    git            https://github.com/me/other.git
```

Then get the hook key with the `bzk project show` command

```
> bzk project show f0cd487a
Project information:
NAME           my_project
ID             f0cd487a0c258a5e89acd12ec214659e
SCM TYPE       git
SCM URI        https://github.com/me/project.git
HOOK KEY       74ef04633587e289d7507dae295dc9c8
```

### Setup Github Webhook

Go to the settings of your github project, in the tab *Webhooks & Services*

In the *Webhooks* space, choose *Add Webhook*

Fill the fields like this

![Github Webhook](https://raw.githubusercontent.com/bazooka-ci/docs/master/assets/img/github_webhook.png)

The Payload URL should look like this

*https://url_of_your_bazooka_server/project/id_of_your_project/github*

And the secret field is the bazooka hook key of your project

## Step 5: Trigger your first build

A build can be triggered either manually or by a SCM hook.

To manually trigger a build, use the Bazooka cli:

```
bzk job start PROJECT_NAME [SCM_REFERENCE]
```

The default value for SCM_REFERENCE, if not explicitely set, is *master*

When a SCM hook is set up, a build is automatically triggered every time you push a new commit to the remote repository.
