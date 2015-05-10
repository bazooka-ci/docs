# Bazooka Architecture

## Components

Except for the CLI, all of Bazooka components are run as Docker containers.

The command `bzk service start` of the CLI will start Bazooka.

This is simply done by starting three Docker containers, linking them togethers when needed, and exposing two ports.

The three containers are:

* **mongodb**: Persistant data in bazooka is stored in Mongo. This container does not expose any port on the host machine.

* **bazooka/server**: server exposes the Bazooka API, which is used by the Web interface, Bazooka CLI, and any other client that may want to interact with Bazooka
    * Exposes port 3000
    * is linked with mongodb

* **bazooka/web**: web exposes the Web Interface of Bazooka
    * Exposes port 8000
    * is linked with bazooka/server

![Bazooka architecture](https://raw.githubusercontent.com/bazooka-ci/docs/master/assets/img/bzk_archi.png)

Here's an example of a typical running bazooka instance:

```sh
> docker ps
CONTAINER ID        IMAGE                   PORTS                           NAMES
6c3be3d47ee4        bazooka/web:latest      443/tcp, 0.0.0.0:8000->80/tcp   bzk_web
ac3201746425        bazooka/server:latest   0.0.0.0:3000->3000/tcp          bzk_server
d207368ae0f3        mongo:latest            27017/tcp                       bzk_mongodb
```

## Build Pipeline

At its heart, Bazooka is a tool to build projects.
Building a project is done by starting a job.

Starting a job will trigger a build pipeline, which consists of running a couple of non-persistant Docker containers in an orchestrated fashion, each performing a specific step in the build pipeline.

The following diagram is a graphical representation of the what happens when a Bazooka job is triggered:

![Bazooka architecture](https://raw.githubusercontent.com/bazooka-ci/docs/master/assets/img/bzk_build_pipeline.png)

* **Step 0**: The server will start a Docker container for the orchestration of the job, *bazooka/orchestration*.
This is the brain of the build, and as its name suggests, is responsible for orchestrating the different steps involved in building a project.

* **Step 1**: The "first" concrete step of a build pipeline is to fetch the source code from an SCM repository (git, mercurial...).

* **Step 2**: After the project's source code was fetched, Bazooka will then parse the *.bazooka.yml* file at the root of the project to determine your build configuration.
If the `language` attribute is set in your *.bazooka.yml*, the parser will invoke a dedicated language parser.

* **Step2b**: The language parser will parse the language-specific attributes (for example the `jdk` attribute of a java project).
The parser/language parser pair will output a set of Dockerfiles, generated according to the *.bazooka.yml* configuration.
Each Dockerfile describes a **variant** of the build.
For example, you can have one variant per jdk version present in your *.bazooka.yml*.

* **Step3**: The orchestration container will build a docker container for every Dockerfile generated in the previous step.

* **Step4**: The orchestration container will then run the build containers.
The result of running these containers will determine the state of the individual variant builds and the global job (one of `SUCCESS`, `FAILED` and `ERRORED`).
