# Installation instructions

## Pre-requisites

### Install Docker

Docker is an essential part of Bazooka. To install it, follow [these instructions](https://docs.docker.com/installation/)

## Download

Everything in Bazooka can be done through the CLI (Command Line Interface), including installing bazooka itself

Download Bazooka CLI with the following links and add it to your `PATH`

* [darwin_386](https://bintray.com/artifact/download/bazooka/bazooka/bzk_darwin_386/bzk)
* [darwin_amd64](https://bintray.com/artifact/download/bazooka/bazooka/bzk_darwin_amd64/bzk)
* [linux_386](https://bintray.com/artifact/download/bazooka/bazooka/bzk_linux_386/bzk)
* [linux_amd64 ](https://bintray.com/artifact/download/bazooka/bazooka/bzk_linux_amd64/bzk)
* [linux_arm](https://bintray.com/artifact/download/bazooka/bazooka/bzk_linux_arm/bzk)

## Installation

You'll need to export the following evironement variables:

* `BZK_HOME`: Bazooka Home Folder, the path of a directory on your host where bazooka will work. It will contain workspaces of your build, artefacts...
* `BZK_SCM_KEYFILE`: (Optional) Bazooka Default SCM private key: The path to the private key bazooka will try to use by default when fetching SCM data, for instance with git

### Docker Compose

Bazooka comes with a docker compose file in `hack/dockercompose`.

You'll first need to export the followinf environement variable:

* `BZK_HOST`: The docker0 bridge ip in linux, or the docker-machine or boot2docker virtual machine ip address

Move to the `hack/dockercompose` directory and execute the following to start bazooka:

```bash
$ cd hack/dockercompose
$ docker-compose --x-networking up
```

### Capitan

Bazooka comes with a `capitan.cfg.sh` file suitable for use with the tiny but powerful [capitan](https://github.com/byrnedo/capitan) tool for managing docker containers:

```bash
$ cd hack/capitan
$ capitan up
```

### Crowdr (Linux only)

Bazooka comes with a `crowdr.cfg.sh` file suitable for use with the [crowdr](https://github.com/polonskiy/crowdr) tool for managing docker containers:

```bash
$ cd hack/crowdr
$ crowdr run
```
