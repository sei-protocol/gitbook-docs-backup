---
description: User docker container to start a sei chain cluster
---

# Docker Cluster

## Prerequisite

### Install Docker & Docker Compose

For MacOS:

```
# The easiest and recommended way to get Docker and
# Docker Compose is to install Docker Desktop here:
https://docs.docker.com/desktop/install/mac-install/
```

For Ubuntu:

```
# Follow the below link to install docker on ubuntu
https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository
# Follow the below link to install standalone docker compose
https://docs.docker.com/compose/install/other/
```

## Deploy Sei Chain Validators on Docker

Detailed instructions and commands can be found the `Makefile` in [this repo](https://github.com/sei-protocol/sei-chain/blob/master/Makefile)

#### Start a 4 Node Validator Cluster

This will start a 4 node sei chain cluster,  each validator node will be running in its own docker container, and each node will also run the oracle price feeder daemon

```bash
# If this is the first time or you want to rebuild the binary:
make docker-cluster-start

# If you have run docker-cluster-start and build/seid exist, 
# you can skip the build process to quick start by:
make docker-cluster-start-skipbuild
```

All the logs and genesis files will be generated under the temporary build/generated folder. To access the service log:

```bash
# Monitor logs after cluster is started for node0
tail -f build/generated/seid-0.log
```

#### **SSH into a single validator node**

```bash
# List all containers
docker ps -a
# SSH into a running container
docker exec -it [container_name] /bin/bash
```

## Deploy a State Sync Node

**Requirement**: Follow the above steps to start a 4 node docker cluster before starting any state sync node.

```bash
# Be sure to start up a 4-node cluster before you start a state sync node
make docker-cluster-start
# Wait for at least a few minutes till the latest block height exceed 500 (this can be changed via app.toml)
seid status |jq
# Start up a state sync node
make run-rpc-nodesh
```

## Local Docker for Debugging  & Testing

One of the fanciest thing of using docker is fast iteration. Here we support:

* Being able to make changes locally and start up the chain to see the immediate impact
* Being able to make changes to local dependency repo (Cosmo SDK/Tendermint) and start the chain with the latest changes without bumping or release any binary version

In order to make local debugging work, you can follow these steps:

```bash
# Clone your dependency repo and put them under the same path as sei-chain
cd sei-chain
cd ../
git clone https://github.com/sei-protocol/sei-tendermint.git
git clone https://github.com/sei-protocol/sei-cosmos.git

# Modify go.mod file to point to local repo, must use the exact same path as below:
cd sei-chain
go mod edit -replace github.com/cosmos/cosmos-sdk=../sei-cosmos
go mod edit -replace github.com/tendermint/tendermint=../sei-tendermint

# Start the docker cluster
make docker-cluster-start
# You are good to go now! Make changes as you wish to any of the dependency repo and run docker to test it out.

```

