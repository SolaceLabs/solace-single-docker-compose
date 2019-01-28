Set Up Single Software Message Broker
=====
This project provides instructions and tools to use Docker Compose to configure a single Solace PubSub+ software message broker Docker container on a desktop. 
## Contents
* [Before you begin](#before-you-begin)
* [Step 1: Download the Docker Compose Template](#Step1) 
* [Step 2: Create a PubSub+ Software Message Broker](#Step2) 
* [Step 3: Manage the Container](#Step3) 
* [Next Steps](#next-steps) 
<br><br>
<a name="before-you-begin"></a>
## Before you begin
The example shown, which makes use of Solace PubSub+ Standard, is suitable for use with up to 100 client connections. However, a maximum of 1,000 client connections can be configured on your platform, provided appropriate resources have been provisioned. For information on client connection scaling, refer to Scaling Tiers for Software Message Brokers.

It's assumed you have:

* If you are using Windows:
  * Windows Pro 10.
  * Docker for Windows installed, with at least 2 GiB of memory dedicated to Docker for Windows. For more information about allocating memory and swap space, refer to the [Docker Settings](https://docs.docker.com/docker-for-windows/#advanced) page.
* If you are using macOS:
  * Mac OS X Yosemite 10.10.3 or higher.
  * Docker for Mac installed, with at least 2 GiB of memory dedicated to Docker for Mac. For more information about allocating memory and swap space, refer to the [Docker Settings](https://docs.docker.com/docker-for-mac/#advanced) page.

 
<a name="Step1"></a>
## Step 1: Download the Docker Compose Template
Clone the GitHub repository containing the Docker Compose template:
```
git clone https://github.com/SolaceLabs/solace-single-software-message-broker.git
git cd solace-single-software-message-broker/template
```

<a name="Step2"></a>
## Step 2: Create a PubSub+ Software Message Broker
Run the following command to create a PubSub+ software message broker using the Compose template:
```
docker-compose -f PubSubStandard_singleNode.yml up -d
```
The Compose template runs a message broker named `pubSubStandardSingleNode`, using the `latest` PubSub+ Standard image pulled from Docker Hub, creates an `admin` user with global access permissions, and publishes the following message broker container ports to the same ports on the macOS host:
* port 8080—enables SEMP management traffic to the container. Use this port when connecting to the container using the  PubSub+ Manager (refer to [Solace PubSub+ Manager] for more information).
* port 55555—enables SMF data to pass through the container. 
To use additional services, you can edit the compose template to publish each corresponding port. For example, to  enable AMQP over TLS, uncomment the appropriate line in the compose template (`- '5671:5671'`). For more information about the default ports used for each service, refer to [Configuration Defaults]().
Once the container is created, it will take about 60 seconds for the message broker to finish activating. 


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