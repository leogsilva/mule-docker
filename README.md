# Mule ESB Community Edition Docker Image
Docker image with Mule ESB on Alpine Linux and Oracle Java 8.

In order for the time of the container to be synchronized (using ntpd), it must be run with the SYS_TIME capability.
In addition you may want to add the SYS_NICE capability, in order for ntpd to be able to modify its priority.

Example:
```
docker run --cap-add=SYS_TIME --cap-add=SYS_NICE leogsilva/mule-docker:latest
```

## Volumes
- /opt/mule-standalone/logs       - Log output directory.
- /opt/mule-standalone/config     - Directory containing configuration for applications, domains etc.
- /opt/mule-standalone/apps       - Deployment directory for Mule applications.
- /opt/mule-standalone/domains    - Deployment directory for Mule domains.

## Environment
- SET_CONTAINER_TIMEZONE - Whether to set the timezone of the Docker container when starting it. Default is true.
- CONTAINER_TIMEZONE - Timezone to use in container. Default is Europe/Stockholm.
- MULE_EXTERNAL_IP - External IP address of the Docker container. On Mac and Windows this will be the Docker machine's IP address.
This IP address is used to expose JMX of the Mule ESB instance running in the Docker container.

Example:
```
docker run -e "SET_CONTAINER_TIMEZONE=true" -e "CONTAINER_TIMEZONE=Europe/Stockholm" -e "CONTAINER_EXTERNAL_IP=192.168.99.100" leogsilva/mule-docker:latest
```

## Exposed ports
- 8081  -   Default HTTP port.
- 1099  - JMX port.

## JXM Monitoring
To monitor a Mule ESB instance running in a Docker container, use the following JMX service URL:<br/>
```
service:jmx:rmi:///jndi/rmi://192.168.99.100:1099/jmxrmi
```
<br/>Note that the IP address may need to be updated and the port number depends on the port mapping configuration when the container was launched.<br/>

## Pre-configured example app
This container also contains a pre-compiled application that exposes an API endpoint like 
```
http://<host ip>:8081/world?language=English
```

## Running with Jolokia 
Only exposing logs dir and with Jolokia port exposed
```
sudo docker run -d --cap-add=SYS_TIME --cap-add=SYS_NICE -p 8080:8899 -p 1099:1099 -p 8081:8081 -v $HOME/mule-docker/mule-standalone/logs:/opt/mule-standalone/logs mule
```

To validate jolokia, invoke the following endpoint using any web browser:
```
http://34.205.250.219:8080/jolokia/read/Mule.basic_tutorial-1.0.0-SNAPSHOT:Application=!%22application%20totals!%22,type=org.mule.Statistics
```

## Calling jolokia to validate installation


## Note!
* The best use of docker with mule is creating a container with application pre-deployed and not exposing apps dir on host. In my tests, exposing app 
directory on host causes application to being redeployed multiple times
* Mule ESB on Alpine Linux has not, to my knowledge, received extensive testing.


