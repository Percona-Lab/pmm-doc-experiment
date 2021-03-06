# Running PMM Server via Docker

Docker images of PMM Server are stored at the [percona/pmm-server](https://hub.docker.com/r/percona/pmm-server/tags/) public
repository. The host must be able to run Docker 1.12.6 or later, and have
network access.

PMM needs roughly 1GB of storage for each monitored database node with data
retention set to one week. Minimum memory is 2 GB for one monitored database
node, but it is not linear when you add more nodes.  For example, data from 20
nodes should be easily handled with 16 GB.

Make sure that the firewall and routing rules of the host do not constrain the
Docker container. For more information, see How do I troubleshoot communication issues between PMM Client and PMM Server?.

For more information about using Docker, see the [Docker documentation](https://docs.docker.com).

**NOTE**: By default, retention is set to 30 days for
Metrics Monitor.  Also consider
disabling table statistics, which can greatly
decrease Prometheus database size.

## Setting Up a Docker Container for PMM Server

A Docker image is a collection of preinstalled software which enables running
a selected version of PMM Server on your computer. A Docker image is not
run directly. You use it to create a Docker container for your PMM Server.
When launched, the Docker container gives access to the whole functionality
of PMM.

The setup begins with pulling the required Docker image. Then, you proceed by
creating a special container for persistent PMM data. The last step is
creating and launching the PMM Server container.

### Pulling the PMM Server Docker Image

To pull the latest version from Docker Hub:

```
docker pull percona/pmm-server:2
```

This step is not required if you are running PMM Server for the first time.
However, it ensures that if there is an older version of the image tagged with
`2.9.0` available locally, it will be replaced by the actual latest
version.

### Creating the `pmm-data` Container

To create a container for persistent PMM data, run the following command:

```
docker create -v /srv --name pmm-data \
percona/pmm-server:2 /bin/true
```

**NOTE**: This container does not run, it simply exists to make sure you retain
all PMM data when you upgrade to a newer PMM Server image.  Do not remove
or re-create this container, unless you intend to wipe out all PMM data and
start over.

The previous command does the following:


* The `docker create` command instructs the Docker daemon
to create a container from an image.


* The `-v` options initialize data volumes for the container.


* The `--name` option assigns a custom name for the container
that you can use to reference the container within a Docker network.
In this case: `pmm-data`.


* `percona/pmm-server:2` is the name and version tag of the image
to derive the container from.


* `/bin/true` is the command that the container runs.

**NOTE**: PMM Server expects that the data volume initialized with the `-v` option will be
`/srv`.  Using any other value will result in data loss in an upgrade.

### Creating and Launching the PMM Server Container

To create and launch PMM Server in one command, use `docker run`:

```
docker run -d -p 80:80 -p 443:443 \
--volumes-from pmm-data --name pmm-server \
--restart always percona/pmm-server:2
```

This command does the following:


* The `docker run` command runs a new container based on the
`percona/pmm-server:2` image.


* The `-p` option maps the host port to the server port inside the docker
container for accessing the PMM Server web UI in the format of
`-p <hostPort>:<containerPort>`. For example, if port **80** is not
available on your host, you can map the landing page to port 8080 using
`-p 8080:80`, the same for port **443**: `-p 8443:443`.


* The `--volumes-from` option mounts volumes from the `pmm-data` container
created previously (see Creating the pmm-data Container).


* The `--name` option assigns a custom name to the container
that you can use to reference the container within the Docker network.
In this case: `pmm-server`.


* The `--restart` option defines the container’s restart policy.
Setting it to `always` ensures that the Docker daemon
will start the container on startup
and restart it if the container exits.


* `percona/pmm-server:2` is the name and version tag of the image
to derive the container from.


* A warning message is printed if invalid an environment variable name key is passed in via the command line option `-e <KEY>=<VALUE>`.

## Installing and using specific PMM Server version

To install a specific PMM Server version instead of the latest one, just put
desired version number after the colon. Also in this scenario it may be useful
to prevent updating PMM Server via the web interface with the `DISABLE_UPDATES` docker option.

Following docker tags are currently available to represent PMM Server versions:


* `:latest` currently means the latest release of the PMM 1.X


* `:2` is the latest released version of PMM 2


* `:2.X` can be used to refer any minor released version, excluding patch
releases


* `:2.X.Y` tag means specific patch release of PMM

For example, installing the latest 2.x version with disabled update button in
the web interface would look as follows:

```
docker create -v /srv --name pmm-data percona/pmm-server:2 /bin/true
docker run -d -p 80:80 -p 443:443 --volumes-from pmm-data \
--name pmm-server -e DISABLE_UPDATES=true \
--restart always percona/pmm-server:2
```

## Updating PMM Server Using Docker


1. Check the installed version of PMM Server. There are two methods.


    1. Use `docker ps`:

```
docker ps
```

This will show the version tag appended to the image name (e.g. `percona/pmm-server:2`).


    2. Use `docker exec`:

```
docker exec -it pmm-server curl -u admin:admin http://localhost/v1/version
```

This will print a JSON string containing version fields.


2. Check if there is a newer version of PMM Server.

Visit [https://hub.docker.com/r/percona/pmm-server/tags/](https://hub.docker.com/r/percona/pmm-server/tags/).


3. Stop the container and create backups.

Back-up the current container and its data so that
you can revert back to using them, and as a safeguard in case
the update procedure fails.

```
docker stop pmm-server
docker rename pmm-server pmm-server-backup
docker cp pmm-data pmm-data-backup
```


4. Pull the new PMM Server image.

You may specify an exact version number, or the latest.

To pull a specific version (2.9.0 in this example):

```
docker pull percona/pmm-server:2.9.0
```

To pull the latest version of PMM 2:

```
docker pull percona/pmm-server:2
```


5. Run the image.

```
docker run -d -p 80:80 -p 443:443 --volumes-from pmm-data --name pmm-server --restart always percona/pmm-server:2.9.0
```

(`pmm-data` is your existing data image.)


6. Check the new version.

Repeat step 1. You can also check the PMM Server web interface.

### Restoring the previous version


1. Stop and remove the running version.

```
docker stop pmm-server
docker rm pmm-server
docker rm pmm-data
```


2. Restore (rename) the backups.

```
docker rename pmm-server-backup pmm-server
docker rename pmm-data-backup pmm-data
```


3. Start (don’t `run`) the image.

```
docker start pmm-server
```

### Removing the backup container

If you stay with the new version and are sure you no longer need your backup containers, you can remove them.

```
docker rm pmm-server-backup
docker rm pmm-data-backup
```

## Backing Up PMM Data from the Docker Container

When PMM Server is run via Docker, its data are stored in the `pmm-data`
container. To avoid data loss, you can extract the data and store outside of the
container.

This example demonstrates how to back up PMM data on the computer where the
Docker container is run and then how to restore them.

To back up the information from `pmm-data`, you need to create a local
directory with essential sub folders and then run Docker commands to copy
PMM related files into it.


1. Create a backup directory and make it the current working directory. In this
example, we use *pmm-data-backup* as the directory name.

```
mkdir pmm-data-backup; cd pmm-data-backup
```


2. Create the essential sub directory:

```
mkdir srv
```

Run the following commands as root or by using the `sudo` command


1. Stop the docker container:

```
docker stop pmm-server
```


2. Copy data from the `pmm-data` container:

```
docker cp pmm-data:/srv ./
```

Now, your PMM data are backed up and you can start PMM Server again:

```
docker start pmm-server
```

## Restoring Backed-up Information to a PMM Data Container

You can restore a backup copy of your `pmm-data` container with these steps.


1. Stop the container:

```
docker stop pmm-server
```


2. Rename the container:

```
docker rename pmm-server pmm-server-backup
```


3. Rename the data container:

```
docker rename pmm-data pmm-data-backup
```


4. Create a new data container:

```
docker create -v /srv --name pmm-data percona/pmm-server:2 /bin/true
```

**NOTE**: This step creates a new data container based on the latest
`percona/pmm-server:2` image. All available versions of `pmm-server` images are listed at
[https://hub.docker.com/r/percona/pmm-server/tags/](https://hub.docker.com/r/percona/pmm-server/tags/).

Assuming that you have a backup copy of your `pmm-data` (see Backing Up PMM Data from the Docker Container), restore your data as follows:


1. Change to the directory where your `pmm-data` backup files are:

```
cd <path to>/pmm-data-backup
```


2. Copy data from your backup directory to the `pmm-data` container:

```
docker cp srv pmm-data:/srv
```


3. Apply correct ownership to `pmm-data` files:

```
docker run --rm --volumes-from pmm-data -it percona/pmm-server:2 chown -R pmm:pmm /srv
```


4. Run (create and launch) a new PMM server container:

```
docker run -d -p 80:80 -p 443:443 --volumes-from pmm-data \
--name pmm-server --restart always percona/pmm-server:2
```
