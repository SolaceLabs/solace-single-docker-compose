Set Up Single Software Message Broker
=====
This project provides instructions and tools to use Docker Compose to configure a single Solace PubSub+ software message broker Docker container on a desktop. 
## Contents
* [Before you begin](#before-you-begin)
* [Step 1: Get an Image](#Step1) 
* [Step 2: Run the Container](#Step2) 
* [Step 3: Manage the Container](#Step3) 
* [Next Steps](#next-steps) 
<br><br>
<a name="before-you-begin"></a>
## Before you begin
The example shown, which makes use of Solace PubSub+ Standard, is suitable for use with up to 100 client connections. However, a maximum of 1,000 client connections can be configured on your platform, provided appropriate resources have been provisioned. For information on client connection scaling, refer to Scaling Tiers for Software Message Brokers.

It's assumed you have:

* If you are using Windows:
 * Windows 10 Pro.
 * Docker for Windows installed, with at least 2 GiB of memory dedicated to Docker for Windows. For more information about allocating memory and swap space, refer to the [Docker Settings](https://docs.docker.com/docker-for-windows/#advanced) page.
* If you are using macOS:
 * Mac OS X Yosemite 10.10.3 or higher.
 * Docker for Mac installed, with at least 2 GiB of memory dedicated to Docker for Mac. For more information about allocating memory and swap space, refer to the [Docker Settings](https://docs.docker.com/docker-for-mac/#advanced) page.

 
<a name="Step1"></a>
## Step 1: Get an Image
Pull the message broker image from the Docker repository using these two steps:
1. Start Docker and opne a command-line terminal
2. Pull the image:
```
> docker pull solace/solace-pubsub-standard
```
When loading is inished, you can check the iamge with the `images` command.
```
docker images
REPOSITORY                      TAG     IMAGE ID      CREATED       SIZE
solace/solace-pubsub-standard   latest  b8a61124d92f  10 days ago   1.11GB      
```
This example assumes that you are using **Solace PubSub+ Standard**. If you want to use another edition, you need to obtain the appropriate message broker package:
* **Solace PubSub+ Enterprise Evaluation Edition**: Go to the Solace [Downloads](https://dev.solace.com/downloads/) page. Then select Download in the Solace PubSub+ Enterprise Evaluation Edition Docker Images section. You will be able to download a compressed archive file called `solace-pubsub-evaluation-<version>-docker.tar.gz`.
* **Solace PubSub+ Enterprise**: If you have purchased a Docker image of Solace PubSub+ Enterprise, Solace will give you information for how to download the compressed tar archive package from a secure Solace server. Contact Solace Support at support@solace.com if you require assistance.
* Once you've obtained the package, start Docker and [load](https://docs.docker.com/engine/reference/commandline/load/) the image.
For example:
```
> docker load -i C:\Users\username\Downloads\solace-pubsub-evaluation-8.10.x.x-docker.tar.gz
```
<a name="Step2"></a>
## Step 2: Run the Container
Once you have loaded the image, you can run the container on your Windows host using Docker's `run` command:
```
> docker run -d -p 8080:8080 -p 55555:55555 --shm-size=2g --env username_admin_globalaccesslevel=admin --env username_admin_password=admin --name=solace solace/solace-pubsub-standard
```
This example runs a message broker named `solace`, using the `latest` PubSub+ Standard image pulled from Docker Hub, creates an `admin` user with global access permissions, and publishes the following message broker container ports to the same ports on the Windows host:
* port 8080—enables SEMP management traffic to the container. Use this port when connecting to the container using the PubSub+ Manager (refer to [PubSub+ Manager Overview](https://docs.solace.com/Solace-PubSub-Manager/PubSub-Manager-Overview.htm) for more information).
* port 55555—enables SMF data to pass through the container.

To use additional services, you must publish each corresponding port. For example, to pass AMQP encrypted traffic, you need to publish port 5671. For more information about the default ports used for each service, refer to [Software Message Broker Configuration Defaults](https://docs.solace.com/Configuring-and-Managing/SW-Broker-Specific-Config/SW-Broker-Configuration-Defaults.htm).

You can use Dcoker's `ps` commant to check on the status of the message vroker container one it's running.
```
> docker ps
```
The `STATUS` column associate with the message broker displays if the container is running and for how long.
The `PORTS` column associate with the message broker displays the port mapping from the Windows host to the container.

**NOTE**:  If you loaded the image from a compressed tar archive, replace `solace/solace-pubsub-standard` in the example with the repository and tag that corresponds with your image. For example, if you loaded version 8.10.0.1022 of the Solace PubSub+ Evaluation Edition, use `solace-pubsub-evaluation:8.10.0.1022`.

<a name="Step3"></a>
## Step 3: Manage the Container

You can access the Solace management tool, PubSub+ Manager, or the Solace CLI to start issuing configuration or monitoring commands on the message broker.

Solace PubSub+ Manager management access:
1. Open a browser and enter this url: http://localhost:8080.
2. Log in as user `admin` with password `admin`.
Solace CLU management access:
1. Enter the following `docker exec` command:
```
docker exec -it solace /usr/sw/loads/currentload/bin/cli -A
```
2. Enter the following commands to enter configuration mode:
```
solace> enable
solace# config
solace(configure)#
```
3. Issue configuration or monitoring commands. For a list of commands currently supported on the message broker, refer to [Software Message Broker CLI Commands](https://docs.solace.com/Solace-CLI/CLI-Reference/VMR_CLI_Commands.html).

<a name="next-steps"></a>
## Next Steps
You now have a message broker Docker container with a basic configuration that is ready for messaging tasks.

There are additional configuration tasks you can make use of in the following topics:
* [Software Message Broker Configuration Defaults](https://docs.solace.com/Configuring-and-Managing/SW-Broker-Specific-Config/SW-Broker-Configuration-Defaults.htm)—Go through the default port numbers for message broker services.
* [Scaling Tiers for Software Message Brokers](https://docs.solace.com/Configuring-and-Managing/SW-Broker-Specific-Config/Configuring-Conn-Scale-Tiers.htm)—Learn about message broker connection scaling tiers.
* [Docker Image Specific Topics](https://docs.solace.com/Configuring-and-Managing/SW-Broker-Specific-Config/Docker-Tasks/Container-Configuration-Tasks.htm)—Learn to prepare the message broker Docker container for a variety of messaging functions.

Also, in order to fully utilize the message broker's features, you should familiarize yourself with the configuration operations common to both Solace PubSub+ software message brokers and appliances. For information, see the topics in the [Configuration](https://docs.solace.com/Configuration.htm) section.

When you are feeling comfortable with your message broker, you can test messaging using the Solace SDKPerf application. You can download SDKPerf from the Other Software section of the dev.solace.com [Downloads](https://dev.solace.com/downloads/) page.
