---
title: "Using Docker"
sidebar_position: 1
---

# Docker Deployment

## Assumptions
Below instructions assume that the servers will be installed on:
- `192.168.0.100` (Coordination Server)
- `192.168.0.101` (Computation Party Server 0)
- `192.168.0.102` (Computation Party Server 1)
- `192.168.0.103` (Computation Party Server 2)
- `192.168.0.104` (Data Consumer API Server)

Note: You can use domain names such as `coord.example.com` instead of IP addresses.

## Common Environment Configuration
1. Install `Docker` by following the instructions [here](https://docs.docker.com/engine/install/).

1. Clone the `mpc-demo-infra` repository and cd to the repository root:
   ```bash
   git clone https://github.com/ZKStats/mpc-demo-infra.git
   cd mpc-demo-infra
   ```

## Deploying the Coordination Server
Make sure that you are at the repository root before proceeding.

1. Change the `PARTY_HOSTS` in `./mpc_demo_infra/coordination_server/docker/.env.coord` to:
   ```bash
   PARTY_HOSTS=["192.168.0.101", "192.168.0.102", "192.168.0.103"]
   ```

1. If you intend to use HTTPS, also:
   1. Set the `PARTY_WEB_PROTOCOL` to `https`
   1. Rename the private key and certificate files of your domain as `privkey.pem` and `fullchain.pem` respectively and add them to `mpc_demo_infra/data_consumer_api/docker/ssl_certs` directory.

1. Start the server
   ```bash
   docker build -t coord -f ./mpc_demo_infra/coordination_server/docker/Dockerfile
   docker run --init -it -v coord-data:/root/mpc-demo-infra/ -p 8005-8100:8005-8100 coord
   ```

## Deploying the Computation Party Servers
Perform the following steps on each of the three servers running a Computation Party Server instance.
Make sure that you are at the repository root before proceeding.
 
1. Change the `PARTY_HOSTS` in `./mpc_demo_infra/computation_party_server/docker/.env.party` to:
   ```bash
   PARTY_HOSTS=["192.168.0.101", "192.168.0.102", "192.168.0.103"]
   ```

1. If you intend to use HTTPS, also:
   1. Set `PARTY_WEB_PROTOCOL` to `https`
   1. Rename the private key and certificate files of your domain as `privkey.pem` and `fullchain.pem` respectively and add them to `mpc_demo_infra/computation_party_server/docker/ssl_certs` directory.

   
1. Change the `COORDINATION_SERVER_URL` to:
   ```
   COORDINATION_SERVER_URL=http://192.168.0.100:8005
   ```
1. Run the following commands to start a server instance. Replace %PARTY_ID% with the party ID assigned to the server (0, 1, or 2). Also, replace %PORT% with 8005, 8006 and 8007 for Party 0, Party 1 and Party 2, respectively.

   ```bash
   cd <mpc-demp-infra repository root>
   export PORT=%PORT%
   export PARTY_ID=%PARTY_ID%
   export NUM_PARTIES=3
   docker build --build-arg PORT=${PORT} --build-arg PARTY_ID=${PARTY_ID} --build-arg NUM_PARTIES=${NUM_PARTIES} -t party -f ./mpc_demo_infra/computation_party_server/docker/Dockerfile
   docker run --init -it -v party-data:/root/MP-SPDZ/ -p 8000-8100:8000-8100 -e PARTY_ID=${PARTY_ID} party
   ```

## Deploying the Data Consumer API Server
Make sure that you are at the repository root before proceeding.

1. Change the `PARTY_HOSTS` in `mpc_demo_infra/coordination_server/docker/.env.coord` to:
   ```bash
   PARTY_HOSTS=["192.168.0.101", "192.168.0.102", "192.168.0.103"]
   ```

1. If you intend to use HTTPS, also:
   1. Set `PARTY_WEB_PROTOCOL` to `https`
   1. Rename the private key and certificate files of your domain as `privkey.pem` and `fullchain.pem` respectively and add them to `mpc_demo_infra/data_consumer_api/docker/ssl_certs` directory.

1. Start the server
   ```bash
   cd <mpc-demp-infra repository root>
   docker build -t data_consumer_api -f ./mpc_demo_infra/data_consumer_api/docker/Dockerfile
   docker run --init -it -p 8004:8004 data_consumer_api
   ```

## Deploying the Notary Server
Make sure that you are at the repository root before proceeding.

1. If you intend to run the Notary Server for your domain, replace `notary.key` and `notary.crt` in the `./mpc_demo_infra/notary_server/docker/ssl_certs` directory with your domain's private key and certificate files respectively.

1. Start the server
Replace %NOTARY_IP% with the IP address of the server on which the Notary server runs:

```bash
docker build --build-arg NOTARY_IP=%NOTARY_IP% -t notary -f ./mpc_demo_infra/notary_server/docker/Dockerfile
docker run --init -it -p 8003:8003 notary
```

