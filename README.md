<img src="https://cloud.githubusercontent.com/assets/668093/12567089/0ac42774-c372-11e5-97eb-00baf0fccc37.jpg" alt="OpenMRS"/>

# OpenMRS Reference Application Docker (Alpine Linux)

[![Build Status](https://travis-ci.org/psbrandt/openmrs-contrib-refappdockeralpine.svg?branch=master)](https://travis-ci.org/psbrandt/openmrs-contrib-refappdockeralpine)
[![MySQL Image](https://omrs-shields.psbrandt.io/custom/mysql/150_MB_%20%7C%207_Layers/blue?style=flat)](https://bintray.com/psbrandt/openmrs-refapp-alpine/mysql/view)
[![Tomcat Image](https://omrs-shields.psbrandt.io/custom/tomcat/266_MB_%20%7C%208_Layers/blue?style=flat)](https://bintray.com/psbrandt/openmrs-refapp-alpine/tomcat/view)

> Docker containers for [OpenMRS](http://openmrs.org) Reference Application

This repository contains the necessary infrastructure code and related resources
required to compose and run Docker containers that start an instance
of the OpenMRS Reference Application running on [Alpine Linux](https://www.alpinelinux.org/).

> :bulb: See the [`Debian` repo](https://github.com/psbrandt/openmrs-contrib-refappdockerdebian) for images
that run on Debian Linux.

For more information about the OpenMRS Reference Application visit [openmrs.org](http://www.openmrs.org/).

## Running

### Prerequisites

Make sure you have [Docker](https://docs.docker.com/) and [Docker Compose](https://docs.docker.com/compose/install/) installed.

### Composition

To run containers use the prebuilt images, first get the compose file:

```
wget https://raw.githubusercontent.com/psbrandt/openmrs-contrib-refappdockeralpine/master/docker-compose-prebuilt.yml
```

Then pull the images:

```
docker-compose -f docker-compose-prebuilt.yml pull
```

Once the pull is complete, you'll have to make sure that the mysql container
is initialized due to [this limitation](https://github.com/docker-library/mysql/issues/81).
Do this by running the following:

````
docker-compose -f docker-compose-prebuilt.yml up openmrs-refapp-mysql
````

When it's done you can press <kbd>ctrl+c</kbd> to stop the container. You will only have
to do this the first time.

After this you can run the OpenMRS Reference Application by executing the following:

````
docker-compose -f docker-compose-prebuilt.yml up
````

## Access

To log into OpenMRS Reference Application, use the following details:

* **Host**: `DOCKER_HOST:8080/openmrs`
* **User**: admin
* **Pass**: Admin123

## Building

### Prerequisites

Make sure you have [Docker](https://docs.docker.com/) and [Docker Compose](https://docs.docker.com/compose/install/) installed.

### Setup

Start by cloning this repository:

````
git clone https://github.com/psbrandt/openmrs-contrib-refappdockeralpine
````

Enter the directory and build the images:

````
cd openmrs-contrib-refappdockeralpine
docker-compose build
````

Once the build is complete, you'll have to make sure that the mysql container
is initialized due to [this limitation](https://github.com/docker-library/mysql/issues/81).
Do this by running the following:

````
docker-compose up openmrs-refapp-mysql
````

When it's done you can press <kbd>ctrl+c</kbd> to stop the container. You will only have
to do this the first time.

After this you can run the OpenMRS Reference Application by executing the following:

````
docker-compose up
````

## Troubleshooting

> Could not create connection to database server. Attempted reconnect 3 times. Giving up.

Since it's not currently possible to order the startup of Docker containers,
sometimes the Tomcat container will start before the MySQL container. As a result,
OpenMRS might not get a database connection on start up. To work around this,
stop the containers and restart them:

````
docker-compose stop
docker-compose start
````

Or if you are using the prebuilt images:

````
docker-compose -f docker-compose-prebuilt.yml stop
docker-compose -f docker-compose-prebuilt.yml start
````

## License

[MPL 2.0 w/ HD](http://openmrs.org/license/)
