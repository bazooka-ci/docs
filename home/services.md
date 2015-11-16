# Services

Bazooka allows you to use external services (databases, messaging systems...) within your build environment.

Services are simply Docker containers that are linked with the container in which your build runs. This allows us to provide as many services as there are containers on the Docker Hub. It is also possible to use containers from your local Docker registry

## Syntax

To declare Bazooka services for your build, you need to add the *services* attribute to your *.bazooka.yml* file. You can declare as many services as you want. Each service has two attributes:

* **image**: the name of the Docker image that will be used as a service. It can be an image from the docker hub, from a private registry, and optionnaly have a version
* **alias (optional)**: the alias that will be used to link the container (see Docker documentation about container linking](https://docs.docker.com/userguide/dockerlinks/#container-linking) for more information on aliases). If alias is not specified, Bazooka will generate one from the name of the image, escaping unwanted characters such as *:* or *.*

Let's look at an example

```yaml
services:
  - image: mongo
  - image: registry.mycompany.net/myteam/testservice:1.0.0
    alias: test
```


## Internals

When you declare services in your config file, Bazooka will parse the configuration file, start the associated Docker containers, and link them at runtime with your build container. If this was done using the Docker CLI, it would give:

```bash
# Start all services
docker run --name services-mongo -d mongo

# Start the container which is actually building your application
docker run buildcontainer --link services-mongo
```

When linking containers together, several things happen.

* Environment variables are injected into the build container to enable programmatic discovery of the linked container (the bazooka service in our case)
*  A host entry for the linked container (the bazooka service) is add to the /etc/hosts file of the build container

To learn more about container linking and what happens, we recommend to read the [Docker documentation about container linking](https://docs.docker.com/userguide/dockerlinks/#container-linking)
