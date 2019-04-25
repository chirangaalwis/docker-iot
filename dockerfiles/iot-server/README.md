# Dockerfile for WSO2 IoT Server #

This section defines the step-by-step instructions to build an [Ubuntu](https://hub.docker.com/_/ubuntu/) Linux based Docker image for WSO2 IoT Server 3.3.1.

## Prerequisites

* [Docker](https://www.docker.com/get-docker) v17.09.0 or above


## How to build an image and run

##### 1. Checkout this repository into your local machine using the following Git command.

```
git clone https://github.com/wso2/docker-iot.git
```

>The local copy of the `dockerfiles/iot-server` directory will be referred to as `IOT_DOCKERFILE_HOME` from this point onwards.

##### 2. Add WSO2 IoT Server distribution and MySQL connector to `<IOT_DOCKERFILE_HOME>/files`.

- Download [WSO2 IoT Server v3.3.1](https://wso2.com/iot/install)
distribution and extract it to `<IOT_DOCKERFILE_HOME>/files`.
- Download [MySQL Connector/J](https://downloads.mysql.com/archives/c-j)
and copy that to `<IOT_DOCKERFILE_HOME>/files`.
- Once all of these are in place, it should look as follows:

  ```bash
  <IOT_DOCKERFILE_HOME>/files/wso2iot-3.3.1/
  <IOT_DOCKERFILE_HOME>/files/mysql-connector-java-<version>-bin.jar
  ```
  
>Please refer to [WSO2 Update Manager documentation]( https://docs.wso2.com/display/WUM300/WSO2+Update+Manager)
in order to obtain latest bug fixes and updates for the product.

##### 3. Build the Docker image.

- Navigate to `<IOT_DOCKERFILE_HOME>` directory. <br>
  Execute `docker build` command as shown below.
    + `docker build -t wso2iot-server:3.3.1 .`
    
##### 4. Running the Docker image.

- `docker run -it -p 9443:9443 wso2iot-server:3.3.1`

>Here, only port 9443 (HTTPS servlet transport) has been mapped to a Docker host port.
You may map other container service ports, which have been exposed to Docker host ports, as desired.

##### 5. Accessing management console.

- To access the management console, use the docker host IP and port 9443.
    + `https://<DOCKER_HOST>:9443/carbon`
    
>In here, <DOCKER_HOST> refers to hostname or IP of the host machine on top of which containers are spawned.


## How to update configurations

Configurations would lie on the Docker host machine and they can be volume mounted to the container. <br>
As an example, steps required to change the port offset using `carbon.xml` is as follows.

##### 1. Stop the IoT Server container if it's already running.
In WSO2 IoT Server v3.3.1 product distribution, `carbon.xml` configuration file <br>
can be found at `<DISTRIBUTION_HOME>/conf`. Copy the file to some suitable location of the host machine, <br>
referred to as `<SOURCE_CONFIGS>/carbon.xml` and change the offset value under ports to 1.

##### 2. Grant read permission to `other` users for `<SOURCE_CONFIGS>/carbon.xml`
```
chmod o+r <SOURCE_CONFIGS>/carbon.xml
```

##### 3. Run the image by mounting the file to container as follows.
```
docker run \
-p 9444:9444 \
--volume <SOURCE_CONFIGS>/carbon.xml:<TARGET_CONFIGS>/carbon.xml \
wso2iot-server:3.3.1
```

>In here, <TARGET_CONFIGS> refers to /home/wso2carbon/wso2iot-3.3.1/conf folder of the container.


## Docker command usage references

* [Docker build command reference](https://docs.docker.com/engine/reference/commandline/build/)
* [Docker run command reference](https://docs.docker.com/engine/reference/run/)
* [Dockerfile reference](https://docs.docker.com/engine/reference/builder/)
