# Docker: How It Works

> A practical beginner-friendly guide to Docker, images, containers,
> Dockerfiles, registries, volumes, networking, and Docker Compose.

## Table of contents

1. [What is Docker?](#1-what-is-docker)
2. [Why use Docker?](#2-why-use-docker)
3. [Docker architecture](#3-docker-architecture)
4. [Images vs. containers](#4-images-vs-containers)
5. [Docker vs. virtual machines](#5-docker-vs-virtual-machines)
6. [Installing and checking Docker](#6-installing-and-checking-docker)
7. [Registries and image tags](#7-registries-and-image-tags)
8. [Running containers](#8-running-containers)
9. [Ports, networks, and volumes](#9-ports-networks-and-volumes)
10. [Creating an image with a
    Dockerfile](#10-creating-an-image-with-a-dockerfile)
11. [Docker Compose](#11-docker-compose)
12. [Useful Docker commands](#12-useful-docker-commands)
13. [A practical MySQL example](#13-a-practical-mysql-example)
14. [A practical PostgreSQL example](#14-a-practical-postgresql-example)
15. [A typical Docker workflow](#15-a-typical-docker-workflow)
16. [Good practices and common
    mistakes](#16-good-practices-and-common-mistakes)
17. [Key concepts at a glance](#17-key-concepts-at-a-glance)

------------------------------------------------------------------------

## 1. What is Docker?

**Docker is a platform for building, packaging, distributing, and
running applications in containers.**

A container runs an application in an isolated environment with the
files, runtime, libraries, and dependencies it needs. This makes it
easier to run the same application in development, testing, and
production without manually installing every dependency on each machine.

For example, a project may need:

- Java to run a Spring Boot application.
- Node.js to build or run a JavaScript application.
- MySQL or PostgreSQL to store data.
- Redis for caching or queues.

Instead of installing and configuring every service directly on your
computer, you can run many of them as containers.

**Important:** Docker does not magically make every container run on
every operating system or CPU architecture. The host must support the
image's platform. Docker Desktop uses a lightweight Linux virtual
machine to run Linux containers on macOS and Windows; Linux can run
Linux containers using the host's Linux kernel.

## 2. Why use Docker?

### The problem Docker helps solve

Without containerization, a developer might install a particular version
of Java, Node.js, MySQL, or another dependency on their computer. A
teammate or production server might have a different version or
configuration. The application can then behave differently or fail to
start.

Docker helps by making the application's runtime environment
reproducible.

### Main advantages

- **Consistency:** use a defined environment across development,
  testing, and deployment.
- **Isolation:** services can run separately without installing each
  one directly on the host.
- **Portability:** share images and run them on compatible Docker
  environments.
- **Faster setup:** a teammate can start required services with
  documented commands or a Compose file.
- **Version control:** choose specific image tags, such as
  `postgres:16`, rather than relying on whichever version happens to
  be installed locally.
- **Resource efficiency:** containers usually require fewer resources
  than full virtual machines because they share the host kernel.
- **Scalability and automation:** container workflows integrate well
  with CI/CD pipelines and deployment platforms.

Docker does not automatically solve every deployment problem. You still
need to configure networking, persistent storage, secrets, backups,
monitoring, security, and the target environment.

## 3. Docker architecture

Docker commonly involves these components:

### Docker CLI (client)

The command-line interface you use in a terminal. For example:

```bash
docker ps
docker pull nginx
docker run nginx
```

The CLI sends requests to the Docker Engine.

### Docker Engine

The Docker Engine is the service that builds images and manages
containers, networks, and volumes. Its background service is commonly
called `dockerd`.

### Docker Desktop

Docker Desktop is a desktop application that provides a graphical
interface and a convenient Docker environment. It includes Docker Engine
components and command-line tools. On macOS and Windows, it uses a
lightweight VM to run Linux containers.

### Registry

A registry stores and distributes Docker images. Docker Hub is a widely
used public registry. Organizations can also use private registries.

### How a command works

For example, when you run `docker run nginx`:

1. The Docker CLI sends the request to the Docker Engine.
2. The Engine looks for the requested image locally.
3. If the image is not available locally, Docker tries to pull it from
   a registry.
4. The Engine creates a container from the image.
5. The container's main process starts.

## 4. Images vs. containers

### Docker image

A **Docker image is a read-only template** used to create containers. It
contains the application files and the runtime, libraries, and other
dependencies packaged into the image.

An image is built from layers. Each layer represents a set of filesystem
changes. Layers can be reused between images, which can reduce storage
and download time.

Examples:

- A Java/Spring Boot image may contain a JRE and an application JAR.
- A Node.js image may contain Node.js, application files, and
  production dependencies.
- A PostgreSQL image contains PostgreSQL and the files needed to run
  it.

### Docker container

A **container is an isolated, runnable instance of an image**. It has a
writable container layer and its own runtime configuration. A container
may be running, stopped, or in another lifecycle state.

You can create multiple containers from the same image, just as you can
create multiple objects from a class in object-oriented programming.

### The recipe analogy

- **Dockerfile:** the instructions or recipe.
- **Image:** the prepared package created from the recipe.
- **Container:** an instance created from that package and started as
  a process.

The analogy is useful, but remember that a container is not necessarily
running all the time. A stopped container still exists until it is
removed.

### Image and container lifecycle

```text
Dockerfile
   |
   | docker build
   v
Docker image
   |
   | docker run
   v
Container
   |
   +-- docker stop --> stopped container
   |
   +-- docker start -> existing container starts again
   |
   +-- docker rm ----> container is removed
```

Removing a container does not remove a separately stored named volume.

## 5. Docker vs. virtual machines

  -----------------------------------------------------------------------

  Feature                 Docker container        Virtual machine

  ----------------------- ----------------------- -----------------------

  Operating system        Shares the host kernel  Runs a guest operating
                          (or runs through a VM   system with its own
                          layer on Docker         kernel
                          Desktop)                

  Startup                 Usually fast            Usually slower because
                                                  a full guest OS must
                                                  start

  Size                    Often smaller           Often larger

  Isolation               Process and resource    A separate virtualized
                          isolation               machine boundary

  OS flexibility          Container OS must be    Can run a different
                          compatible with the     guest OS supported by
                          host kernel/platform    the hypervisor

  Typical use             Packaging and running   Running separate
                          services and            operating systems or
                          applications            stronger machine-level
                                                  boundaries

  -----------------------------------------------------------------------

Docker containers are not simply virtual machines. On Linux, containers
use operating-system features such as namespaces and control groups.
Docker Desktop runs Linux containers in a lightweight VM on macOS and
Windows.

Choose based on the requirement: containers are excellent for packaging
applications and services; VMs are useful when you need a separate guest
operating system.

## 6. Installing and checking Docker

Follow the official installation instructions for your operating system:

- [Docker Engine installation
  documentation](https://docs.docker.com/engine/install/)
- [Docker Desktop documentation](https://docs.docker.com/desktop/)

On Ubuntu, follow the current official instructions for your Ubuntu
release rather than assuming that a single installation command is
suitable for every machine.

Check whether Docker is installed:

```bash
docker --version
docker version
docker info
```

Run the test image:

```bash
docker run hello-world
```

This downloads the `hello-world` image if necessary, creates a
container, runs it, and prints a confirmation message.

On Linux, Docker commands may require `sudo` depending on how Docker is
installed and configured. Adding a user to the `docker` group can grant
root-level privileges; only do this for trusted users and understand the
security implications. See the [post-installation
guide](https://docs.docker.com/engine/install/linux-postinstall/).

## 7. Registries and image tags

A **container registry** stores Docker images. Docker downloads images
from a registry and can upload images to one when you have the
appropriate access.

Examples include:

- Docker Hub
- Private registries hosted by an organization
- Cloud registries such as Amazon ECR
- Self-hosted registry solutions

Pull an image:

```bash
docker pull nginx:1.27
```

The general syntax is:

```bash
docker pull IMAGE:TAG
```

A **tag** identifies a variant or version of an image. For example,
`postgres:16` selects the PostgreSQL 16 image line. The `latest` tag is
a tag name, not a guarantee that an image is the newest, safest, or most
appropriate version for your project.

For reproducible deployments, choose a specific supported version and
update it deliberately. For stricter supply-chain control, teams can pin
images by digest as well.

Useful commands:

```bash
docker images
docker image ls
docker pull postgres:16
docker image inspect postgres:16
```

## 8. Running containers

Start an Nginx container in the foreground:

```bash
docker run nginx:1.27
```

Run it in the background with `-d` (detached mode):

```bash
docker run -d --name web-server nginx:1.27
```

Common options:

- `--name web-server`: assigns a readable container name.
- `-d`: runs the container in the background.
- `-p HOST_PORT:CONTAINER_PORT`: publishes a container port on the
  host.
- `-e NAME=value`: sets an environment variable.
- `-v VOLUME_NAME:CONTAINER_PATH`: mounts a named volume.
- `--network NETWORK_NAME`: attaches the container to a Docker
  network.

Inspect running containers:

```bash
docker ps
```

Show all containers, including stopped ones:

```bash
docker ps -a
```

View logs:

```bash
docker logs web-server
docker logs -f web-server
```

Stop, start, and remove a container:

```bash
docker stop web-server
docker start web-server
docker rm web-server
```

A container's lifecycle and its image's lifecycle are separate. Stopping
a container does not delete it, and deleting a container does not delete
the image it was created from.

## 9. Ports, networks, and volumes

### 9.1 Port mapping

Containers are isolated by default. To access a service from your host,
publish a port when starting the container.

```bash
docker run -d --name web-server -p 8080:80 nginx:1.27
```

The format is:

```text
HOST_PORT:CONTAINER_PORT
```

Here, host port `8080` forwards traffic to port `80` inside the
container. Open `http://localhost:8080` in your browser.

Publishing a port can make a service accessible beyond your own computer
depending on the host's network and firewall configuration. For
local-only access, you can bind it to loopback:

```bash
docker run -d --name web-local -p 127.0.0.1:8080:80 nginx:1.27
```

### 9.2 Docker networks

A Docker network lets containers communicate with each other. On a
user-defined bridge network, containers can normally resolve each other
by container name.

Create a network:

```bash
docker network create dev-network
```

Run MySQL on that network:

```bash
docker run -d \
  --name mysql-db \
  --network dev-network \
  -e MYSQL_ROOT_PASSWORD=change-this-local-password \
  mysql:8.4
```

Run another service on the same network and configure it to connect to
the database host `mysql-db` on port `3306`.

Inside another container, `localhost` means that same container, not the
MySQL container. Use the database container's name (or a Compose service
name) when connecting between containers on the same network.

Inspect and remove networks:

```bash
docker network ls
docker network inspect dev-network
docker network rm dev-network
```

### 9.3 Volumes and persistent data

A container's writable layer is not the right place for important data
that must survive container removal. Use a Docker volume or an
appropriate bind mount for persistent files.

Create a named volume:

```bash
docker volume create mysql-data
```

Mount it when running MySQL:

```bash
docker run -d \
  --name mysql-db \
  -e MYSQL_ROOT_PASSWORD=change-this-local-password \
  -v mysql-data:/var/lib/mysql \
  mysql:8.4
```

In this example, `mysql-data` is the volume name and `/var/lib/mysql` is
the data directory inside the container.

If you stop and remove the container, the named volume normally remains:

```bash
docker stop mysql-db
docker rm mysql-db
docker volume ls
```

When you create a replacement container, mounting the same volume lets
it reuse the persisted data. **Do not mount the same database data
directory into two database containers at the same time** unless the
database system explicitly supports that arrangement.

Inspect a volume:

```bash
docker volume inspect mysql-data
```

Remove a volume only when you are sure its data is no longer needed:

```bash
docker volume rm mysql-data
```

Removing a volume can permanently delete the data it contains. Volumes
are not a substitute for tested backups.

## 10. Creating an image with a Dockerfile

A **Dockerfile** is a text file containing instructions for building a
Docker image. It describes the base image, working directory, files to
copy, dependencies to install, and default command.

### Example: simple Node.js application

Project structure:

```text
node-app/
├── Dockerfile
├── package.json
├── package-lock.json
└── server.js
```

Example `Dockerfile`:

```dockerfile
FROM node:22-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .

EXPOSE 3000

CMD ["node", "server.js"]
```

What each instruction does:

- `FROM node:22-alpine`: starts from an image that includes Node.js.
- `WORKDIR /app`: sets the working directory for subsequent
  instructions.
- `COPY package*.json ./`: copies package manifests into the image.
- `RUN npm ci`: installs dependencies during the image build.
- `COPY . .`: copies the remaining project files.
- `EXPOSE 3000`: documents the port the application is expected to
  use; it does not publish the port by itself.
- `CMD ["node", "server.js"]`: defines the default command to run when
  the container starts.

This example assumes the project has a valid `package-lock.json` and
that `server.js` starts the application and listens on port `3000`,
ideally on `0.0.0.0` inside the container.

### Add a `.dockerignore` file

Create `.dockerignore` in the project root to avoid sending unnecessary
or sensitive files into the build context.

```text
node_modules
.git
.env
npm-debug.log
```

Adjust the file for your project. Do not copy secrets into an image.

### Build and run the image

Run these commands from the directory containing the Dockerfile:

```bash
docker build -t node-app:1.0 .
docker run -d --name node-app-container -p 3000:3000 node-app:1.0
```

The final `.` tells Docker to use the current directory as the build
context.

Open `http://localhost:3000` if the app serves HTTP on port 3000.

View output and clean up:

```bash
docker logs node-app-container
docker stop node-app-container
docker rm node-app-container
```

### Common Dockerfile instructions

  Instruction    Purpose

  -------------- ----------------------------------------------------

  `FROM`         Selects the base image
  `WORKDIR`      Sets the working directory
  `COPY`         Copies files from the build context into the image
  `RUN`          Executes a command during image building
  `ENV`          Defines environment variables in the image
  `EXPOSE`       Documents a container port
  `CMD`          Sets the default command or arguments
  `ENTRYPOINT`   Defines the main executable for the container
  `ARG`          Defines a build-time argument

**The basic workflow:** write a Dockerfile → build an image → run a
container from that image.

## 11. Docker Compose

A project often needs multiple services at once: an application, a
database, a cache, and perhaps an administration interface.

**Docker Compose** lets you describe and manage these services together
in a `compose.yaml` file. Compose can create the required networks and
volumes, build or pull images, and start the services with one command.

### Example: MySQL and phpMyAdmin

Create `compose.yaml`:

```yaml
services:
  db:
    image: mysql:8.4
    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
      MYSQL_DATABASE: appdb
      MYSQL_USER: appuser
      MYSQL_PASSWORD: ${MYSQL_PASSWORD}
    volumes:
      - mysql-data:/var/lib/mysql
    ports:
      - "127.0.0.1:3306:3306"
    healthcheck:
      test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
      interval: 10s
      timeout: 5s
      retries: 10

  phpmyadmin:
    image: phpmyadmin:5
    environment:
      PMA_HOST: db
    ports:
      - "127.0.0.1:8080:80"
    depends_on:
      db:
        condition: service_healthy

volumes:
  mysql-data:
```

Create a `.env` file beside `compose.yaml`:

```dotenv
MYSQL_ROOT_PASSWORD=replace-with-a-strong-local-password
MYSQL_PASSWORD=replace-with-another-strong-local-password
```

Do not commit a real `.env` file containing secrets to Git. Add `.env`
to `.gitignore`, and use an appropriate secrets-management solution for
shared or production environments.

Start the services:

```bash
docker compose up -d
```

Compose downloads missing images, creates the network and volume, and
starts the services.

Open phpMyAdmin at `http://localhost:8080`. The database host inside the
Compose network is `db`, not `localhost`. Use the configured database
username and password to sign in.

Useful Compose commands:

```bash
docker compose ps
docker compose logs
docker compose logs -f db
docker compose stop
docker compose start
docker compose down
```

`docker compose down` removes the Compose containers and network but
**normally preserves named volumes**. To delete the volumes too, use
`docker compose down -v` only when you intentionally want to remove
their persisted data.

### `image` vs. `build` in Compose

- `image: mysql:8.4` tells Compose which existing image to use or
  pull.
- `build: .` tells Compose to build an image from a Dockerfile in the
  specified build context.

A Compose file can use both patterns across different services.

## 12. Useful Docker commands

### Images

```bash
docker images
docker pull nginx:1.27
docker image inspect nginx:1.27
docker image rm nginx:1.27
```

### Containers

```bash
docker ps
docker ps -a
docker run --name web -d -p 8080:80 nginx:1.27
docker logs web
docker exec -it web sh
docker stop web
docker start web
docker rm web
```

`docker exec -it web sh` opens a shell inside a running container if the
image includes `sh`. Some minimal images may not include a shell.

### Build an image

```bash
docker build -t my-app:1.0 .
```

### Volumes and networks

```bash
docker volume ls
docker volume inspect mysql-data
docker network ls
docker network inspect dev-network
```

### Clean up carefully

```bash
docker container prune
docker image prune
docker system df
```

Prune commands remove unused resources. Review what will be removed and
avoid deleting data or images needed by other projects.

## 13. A practical MySQL example

Pull the image:

```bash
docker pull mysql:8.4
```

Create and start a container with a named volume:

```bash
docker run -d \
  --name mysql-db \
  -p 127.0.0.1:3306:3306 \
  -e MYSQL_ROOT_PASSWORD=change-this-local-password \
  -v mysql-data:/var/lib/mysql \
  mysql:8.4
```

Connect with a MySQL client:

```bash
docker exec -it mysql-db mysql -uroot -p
```

Docker prompts for the password set in `MYSQL_ROOT_PASSWORD`.

Stop and remove the container:

```bash
docker stop mysql-db
docker rm mysql-db
```

The `mysql-data` volume remains unless you remove it explicitly. If you
create a new container with the same volume, MySQL can reuse the
existing database files.

**Notes:** - The password shown is an example; use a strong secret for
real environments. - If port 3306 is already in use, choose a different
host port, such as `3307:3306`. - Do not share one database data volume
between multiple MySQL containers at the same time.

## 14. A practical PostgreSQL example

Start PostgreSQL with a named volume:

```bash
docker run -d \
  --name my-postgres \
  -e POSTGRES_USER=myuser \
  -e POSTGRES_PASSWORD=change-this-local-password \
  -e POSTGRES_DB=mydatabase \
  -p 127.0.0.1:5432:5432 \
  -v pgdata:/var/lib/postgresql/data \
  postgres:16
```

The example uses the PostgreSQL 16 image. For an existing project,
choose and maintain the version that matches your requirements.

Connect to PostgreSQL from inside the container:

```bash
docker exec -it my-postgres psql -U myuser -d mydatabase
```

### Optional: pgAdmin

Run pgAdmin:

```bash
docker run -d \
  --name pgadmin \
  -p 127.0.0.1:5050:80 \
  -e PGADMIN_DEFAULT_EMAIL=admin@example.com \
  -e PGADMIN_DEFAULT_PASSWORD=change-this-local-password \
  dpage/pgadmin4
```

Open `http://localhost:5050` and sign in with the configured pgAdmin
credentials.

To connect pgAdmin to PostgreSQL:

- If both containers are on the same user-defined Docker network, use
  the PostgreSQL container name as the host, for example
  `my-postgres`, and port `5432`.
- If pgAdmin runs outside that Docker network, configure a reachable
  host and the published PostgreSQL port.
- `localhost` inside the pgAdmin container refers to pgAdmin itself,
  not the PostgreSQL container.

For a reusable multi-service setup, Docker Compose is usually more
convenient than manually managing these containers.

## 15. A typical Docker workflow

For an application you develop yourself:

1. **Define the environment:** identify the runtime, libraries, and
   services the application needs.
2. **Choose base images:** select suitable images and versions.
3. **Write a Dockerfile:** describe how to package your application.
4. **Build an image:** use `docker build`.
5. **Run a container:** use `docker run`, or describe multiple services
   in Compose.
6. **Configure access:** set up ports, networks, environment variables,
   and persistent volumes.
7. **Test and inspect:** check logs, health, and application behavior.
8. **Version and distribute:** tag and push the image to an appropriate
   registry if needed.
9. **Deploy:** pull or build the intended image in the target
   environment and configure production secrets, storage, networking,
   and monitoring.

For a multi-service application, a common development setup might
include:

```text
Your computer
└── Docker Engine
    ├── Application container
    ├── PostgreSQL container
    └── Redis container
```

Each service can use its own image and container. Compose can manage the
services and their shared network.

## 16. Good practices and common mistakes

- **Use specific image versions.** Avoid relying on `latest` when you
  need predictable deployments.
- **Persist important data.** Databases should use volumes or another
  appropriate persistent storage system.
- **Back up databases.** A volume protects data from routine container
  removal, but it is not a backup.
- **Protect secrets.** Do not commit passwords or production `.env`
  files to source control or bake secrets into images.
- **Be deliberate about published ports.** Publish only the services
  and ports that need to be reachable.
- **Use container DNS names.** Containers on the same user-defined
  network should connect using service or container names, not each
  other's `localhost`.
- **Use `.dockerignore`.** Avoid sending build artifacts, local
  dependencies, Git history, and secrets into the build context.
- **Keep images lean.** Use suitable base images and avoid unnecessary
  tools and files.
- **Check logs and status.** `docker ps`, `docker logs`, and
  `docker inspect` are useful starting points when a service fails.
- **Do not assume `docker stop` deletes a container.** It stops it;
  `docker rm` removes it.
- **Do not assume deleting a container deletes its named volume.**
  Volumes are separate resources.
- **Treat Docker access as privileged.** Access to the Docker daemon
  can confer significant control over the host.
- **Plan production separately.** Add appropriate secrets management,
  backups, health checks, TLS, access controls, monitoring, and update
  procedures.

## 17. Key concepts at a glance

  -----------------------------------------------------------------------

  Concept                             Simple definition

  ----------------------------------- -----------------------------------

  Docker                              Platform for building and running
                                      containerized applications

  Docker Engine                       Service that manages images,
                                      containers, networks, and volumes

  Docker CLI                          Terminal tool for sending commands
                                      to Docker

  Docker Desktop                      Desktop application that provides
                                      Docker tooling and a graphical
                                      interface

  Dockerfile                          Instructions for building an image

  Image                               Read-only package/template used to
                                      create containers

  Container                           Isolated runnable instance of an
                                      image

  Registry                            Storage and distribution service
                                      for images

  Tag                                 Name identifying an image version
                                      or variant

  Port mapping                        Makes a container port reachable
                                      through a host port

  Network                             Allows containers to communicate
                                      with each other

  Volume                              Persistent storage managed
                                      separately from a container

  Docker Compose                      Tool for defining and running
                                      multi-container applications

  -----------------------------------------------------------------------

### The most important idea

**Dockerfile → Image → Container**

- Write a `Dockerfile` to describe how to package an application.
- Build an image from it.
- Run one or more containers from that image.
- Use volumes for persistent data, networks for container-to-container
  communication, and Compose to manage related services together.

### Official documentation

- [Docker
  overview](https://docs.docker.com/get-started/docker-overview/)
- [Docker Engine
  installation](https://docs.docker.com/engine/install/)
- [Dockerfile
  reference](https://docs.docker.com/reference/dockerfile/)
- [Docker run
  reference](https://docs.docker.com/reference/cli/docker/container/run/)
- [Docker Compose documentation](https://docs.docker.com/compose/)
- [Docker volumes](https://docs.docker.com/engine/storage/volumes/)
- [Docker networking](https://docs.docker.com/engine/network/)
