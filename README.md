Configure a Standalone Software Message Broker
=====
This project provides instructions and tools to get a single [Solace PubSub+ software message broker](https://solace.com/products/software) Docker container up-and-running on a desktop using Docker Compose. 

If you are interested in setting up message brokers in an High Availability (HA) redundancy group, take a look at [Configure High-availability Groups Using Docker Compose](https://github.com/SolaceLabs/solace-ha-docker-compose).
## Contents
* [Before you begin](#before-you-begin)
* [Step 1: Download the Docker Compose Template](#Step1) 
* [Step 2: Create a PubSub+ Software Message Broker](#Step2) 
* [Step 3: Manage the Container](#Step3) 
* [Next Steps](#next-steps) 
<br><br>
<a name="before-you-begin"></a>
## Before you begin
The example shown, which makes use of Solace PubSub+ Standard, is suitable for use with up to 100 client connections. However, a maximum of 1,000 client connections can be configured on your platform, provided appropriate resources have been provisioned. For more information about system requirements, see [Editions of PubSub+ Event Broker: Software](https://docs.solace.com/Solace-SW-Broker-Set-Up/Setting-Up-SW-Brokers.htm) and [System Resource Requirements](https://docs.solace.com/Configuring-and-Managing/SW-Broker-Specific-Config/System-Resource-Requirements.htm).

You must have installed one of:

* [Docker Desktop for Windows](https://docs.docker.com/docker-for-windows/install/) with at least 2 GiB of memory dedicated to Docker Engine. For more information about allocating memory and swap space, refer to the [Docker Settings](https://docs.docker.com/docker-for-windows/#advanced) page.
* [Docker Desktop for Mac](https://docs.docker.com/docker-for-mac/install/) with at least 2 GiB of memory dedicated to Docker Engine. For more information about allocating memory and swap space, refer to the [Docker Settings](https://docs.docker.com/docker-for-mac/#advanced) page.

Ensure that Docker Compose is running by entering the following command:
```
docker-compose --help
```
If the help text is not displayed, you may need to install Docker Compose. See [Install Docker Compose](https://docs.docker.com/compose/install/) for instructions.

<a name="Step1"></a>
## Step 1: Download the Docker Compose Template
Clone the GitHub repository containing the Docker Compose template:
```
git clone https://github.com/SolaceLabs/solace-single-docker-compose.git
cd solace-single-docker-compose/template
```

<a name="Step2"></a>
## Step 2: Create a PubSub+ Software Message Broker
Run the following command to create a PubSub+ software message broker using the Compose template:
```
docker-compose -f PubSubStandard_singleNode.yml up -d
```
The Compose template runs a message broker named `pubSubStandardSingleNode`, using the `latest` PubSub+ Standard image pulled from Docker Hub, creates an `admin` user with global access permissions, and publishes the following message broker container ports to the same ports on the host:

* port 1443 -- Web Transport over TLS
* port 1883 -- MQTT Default VPN
* port 1943 -- SEMP over TLS
* port 2222 -- SSH connection to the Solace CLI
* port 5671 -- AMQP Default VPN over TLS
* port 5672 -- AMQP Default VPN
* port 8000 -- MQTT Default VPN over WebSockets
* port 8008 -- Web transport
* port 8080 -- SEMP / PubSub+ Manager
* port 8443 -- MQTT Default VPN over WebSockets/TLS
* port 8883 -- MQTT Default VPN over TLS
* port 9000 -- REST Default VPN
* port 9443 -- REST Default VPN over TLS
* port 55003 -- SMF Compressed
* port 55443 -- SMF over TLS
* port 55554 -- SMF


For more information about the default ports used for each service, refer to [Software Message Broker Configuration Defaults](https://docs.solace.com/Configuring-and-Managing/SW-Broker-Specific-Config/SW-Broker-Configuration-Defaults.htm).
Once the container is created, it will take about 60 seconds for the message broker to finish activating.

**Note:** On MacOS Big Sur and later, port 55555 (the default SMF port for the software broker) is blocked. If this port is mapped to a port on a Docker container, the container fails to start, either silently or with a "port in use" error. To avoid this problem with MacOS Big Sur, map the SMF port to a different port on the host.


<a name="Step3"></a>
## Step 3: Manage the PubSub+ Software Message Broker

You can access the Solace management tool, [PubSub+ Manager](https://docs.solace.com/Solace-PubSub-Manager/PubSub-Manager-Overview.htm), or the [Solace CLI](https://docs.solace.com/Solace-CLI/Using-Solace-CLI.htm) to start issuing configuration or monitoring commands on the message broker.

Solace PubSub+ Manager management access:
1. Open a browser and enter this url: _http://localhost:8080_.
2. Log in as user `admin` with default password `admin`.

Solace CLI management access:
1. Enter the following `docker exec` command:
```
docker exec -it pubSubStandardSingleNode /usr/sw/loads/currentload/bin/cli -A
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

To familiarize yourself with the some of the features of PubSub+ software message brokers and appliances, see [Best Practices](https://docs.solace.com/best-practices.htm) and the [Feature Index](https://docs.solace.com/feature-index.htm).

When you are feeling comfortable with your message broker, you can test messaging using the [Solace SDKPerf](https://docs.solace.com/SDKPerf/SDKPerf.htm?Highlight=SDKperf#Quick) application. You can download SDKPerf from the dev.solace.com [Downloads](https://dev.solace.com/downloads/) page.
