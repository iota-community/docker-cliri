# WARNING

I take no responsability about eventual damage! This project includes following alpha and beta software:
* CLIRI - Coordinator Less IOTA Reference Implementation

# cliri - znet - testnet node with docker-compose

## Getting Started

These instructions will get you a copy of CLIRI - the znet Testnet Tangle node up and running on your local machine using docker and docker-compose.

### Prerequisites

It is expected that you have already installed docker, docker-compose and know how to start and use it.
Knowledge about your operating system (Windows, Linux, MacOS).

## Usage

### Clone the repository
```
git clone https://gitlab.com/antonionardella/cliri-znet.git
```

### The following part has to be reviewed


#### Choose RAM for Java in docker-compose.yml

Edit `docker-compose.yml` which is located in the cliri-znet folder to choose the RAM to run Java for CLIRI, for example using 4GB for java uncomment by removing `#` next to JAVA_MAX_MEMORY=4096m
```
environment:
 # Use the following for 8GB; comment following line if other setting is chosen 
  - JAVA_MAX_MEMORY=8192m
 # Uncomment the following for 4GB
 #- JAVA_MAX_MEMORY=4096m
  - JAVA_MIN_MEMORY=256m
```

#### Define the environment details
comment ports, uncomment environment part and insert necessary information e.g.:
```
    expose:
      - "14266"
    ports:
      - "14600:14600/udp" # change to another port if you already have IRI running e.g. "14601:14600/udp" and change it under volumes/cliri/cliri.ini
      - "15600:15600/tcp" # change to another port if you already have IRI running e.g. "15601:15600/tcp"
      - "14265:14265" # change to another port if you already have IRI running e.g. "14267:14265"
# To get things started
    command: ["-c", "/cliri/conf/cliri.ini"]
```
### Start the node

Enter the cliri-znet folder
```
cd cliri-znet
```

Run it with:
```
docker-compose up -d
```
### Check the logs

Check the IRI logs with
```
docker logs iota_cliri
```

## Warnings

### Ports

The ports setup in the docker-compose.yml file opens following container ports

Port/Type | Use 
--- | ---
14265 | IOTA/IRI API port
14600/udp | IOTA/IRI UDP connection port
15600/tcp | IOTA/IRI TCP connection port

Please assure yourself to set your firewall accordingly, the ports are opened on 0.0.0.0 (all IP adresses, internal and external)
Change ports accordingly if the ports in the docker-compose have been changed.

### Remote API limits

**At this point NO API limits are now default!**

Following API limits are to be set as best practice (see iota.partners site or discussions on discord), but are not enabled as explained in the following table

parameter | explaination 
--- | ---
getNeighbors|No one can see the data of your neighbors
addNeighbors|No one can add neighbors to your node
removeNeighbors|No one can remove neighbors from your node
setApiRateLimit|This will prevent external connections from being able to use this command
interruptAttachingToTangle| To prevent users to do the PoW on your node
attachToTangle| To prevent users to do the PoW on your node

### Firewall (ufw) rules

The following rules have been used on my node, please adapt accordingly to your setup!
```
sudo ufw default allow outgoing
sudo ufw default deny incoming
sudo ufw allow ssh
sudo ufw allow 80
sudo ufw allow 443
sudo ufw allow 14265
sudo ufw allow 14600/udp
sudo ufw allow 15600/tcp
sudo ufw enable
```
Change ports accordingly if the ports in the docker-compose have been changed.

## More information

For more information about the projects please refer to the following github repositories:

* [CLIRI - Coordinator Less IOTA Node](https://github.com/iotaledger/cliri)