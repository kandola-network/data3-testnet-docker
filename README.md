# data3-testnet-docker
A bunch of docker compose configurations for easy data3 testnet onboarding of node providers

## Data3 Node Provider Service
The Data3 Node Provider Service is deployed once after a new Data3 Node Provider signs up at the Data3 Console.

You can deploy the Data3 Node Provider Service after its dependencies are deployed:
* Observability Server
* PubSub Platform

### Prerequisites
* MetaMask Wallet & Account with Test Kan Tokens (TTKAN) and Test Matic (reach out to us through our social media handles to get Test TTKAN Tokens)
* Linux AMD host (the Observability Server requires this)
* Public IP on the host with ability to expose ports 29092 (TCP), 8081 (HTTP) and 8443(HTTPS)
* Docker
- Your Docker containers must be able to resolve these publicly available hostnames:
    * testconsoleapi.data3.network (HTTPS/443)
    * testpubsub.data3.network (TCP/9092)
    * You can achieve this using one of the following methods (or similar):
```shell
# If you using an apt installation of docker
sudo dockerd --dns="8.8.8.8"
sudo systemctl restart docker
```

```shell
# or if you are using snap installation of docker
# Edit this file and add "dns":["8.8.8.8"] to the daemon.json
sudo vi /var/snap/docker/current/config/daemon.json
sudo systemctl restart snap.docker.dockerd
```

### Observability Server
This deploys the observability server:
```shell
cd data3-node-provider/node-provider-observability
# check configuration in docker-compose.yml
docker compose up -d
```

Once deployed, the observability web application will be running on port 8443 (if default port is not changed). 
* Access the observability web app using the browser
* Login using default credentials: admin/admin
* Reset admin password on first login
* Ensure that the grafana dashboard loads defaults

### PubSub Platform
This deploys the publish-subscribe platform:
```shell
cd data3-node-provider/node-provider-pubsub
# verify and edit configuration in docker-compose.yml
docker compose up -d
```
Once deployed, ensure that Kafka and Zookeeper docker containers are up successfully.

### All Node Provider Dependencies
Alternatively, you could deploy all the node provider dependencies in one go, using:
```shell
cd data3-node-provider/node-provider-all-deps
# verify and edit configuration in docker-compose.yml
docker compose up -d
```

### Node Provider Service
Once the above dependencies are deployed and running successfully:
```shell
cd data3-node-provider/node-provider
# verify and edit configuration in docker-compose.yml
docker compose up -d
```

## Data3 Storage Node
The Data3 Storage Node is deployed once for every customer deployment.

* Deploy the database instances as per the customer request
* Configure the database with users for monitoring, proxying and app.
* Configure proxysql.cnf (with the backend database IP/Hostname and port)

```shell
cd data3-storage-node
# verify and edit configuration in docker-compose.yml
docker compose up -d
```


