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

## Steps
1. Install `Docker` by following the instructions [here](https://docs.docker.com/engine/install/).

1. Configure and start the `Coordination Server`  
   1. Change the `PARTY_HOSTS` in `mpc_demo_infra/coordination_server/docker/.env.coord` to:
      ```bash
      PARTY_HOSTS=["192.168.0.101", "192.168.0.102", "192.168.0.103"]
      ```

   1. If you intend to use HTTPS, also:
      1. Set `PARTY_WEB_PROTOCOL` to `https`
      1. Rename the private key and certificate files of your domain as `privkey.pem` and `fullchain.pem` respectively and add them to `mpc_demo_infra/data_consumer_api/docker/ssl_certs` directory.


   1. Start the server
      ```bash
      cd ./mpc_demo_infra/coordination_server/docker/
      docker build -t coord .
      docker run --init -it -v coord-data:/root/mpc-demo-infra/ -p 8005-8100:8005-8100 coord
      ```

1. Configure the `Computation Party Server`
   1. Clone the `MP-SPDZ` repository to any preferred location and cd to the repository root:
      ```bash
      git clone git@github.com:ZKStats/MP-SPDZ.git
      cd MP-SPDZ
      ```

   1. Create `P{0,1,2}.pem` and `P{0,1,2}.key` files
      ```bash
      ./Scripts/setup-ssl.sh 3
      ```

   1. Copy the created pem and key files to `<mpc-demo-infra repository root>/mpc_demo_infra/computation_party_server/docker/` directory:
      ```bash
      cp Player-Data/P*.{pem,key} <mpc-demo-infra repository root>/mpc_demo_infra/computation_party_server/docker/
      ```
   1. If you intend to use HTTPS, also:
      1. Set `PARTY_WEB_PROTOCOL` to `https`
      1. Rename the private key and certificate files of your domain as `privkey.pem` and `fullchain.pem` respectively and add them to `mpc_demo_infra/computation_party_server/docker/ssl_certs` directory.

   1. Change the `PARTY_HOSTS` in `mpc_demo_infra/coordination_server/docker/.env.coord` to:
      ```bash
      PARTY_HOSTS=["192.168.0.101", "192.168.0.102", "192.168.0.103"]
      ```
   
   1. Change the `COORDINATION_SERVER_URL` to:
      ```
      COORDINATION_SERVER_URL=http://192.168.0.100:8005
      ```

1. Start three `Computatoin Party Server` instances
   1. cd to the Docker directory:
      ```bash
      cd mpc-demo-infra/mpc_demo_infra/computation_party_server/docker
      ```

   1. Run the following commands three times to start three server instances, replacing %PORT% and %PARTY_ID%. Use ports 8005, 8006, and 8007 for Party 0, Party 1, and Party 2, respectively.
      ```bash
      export PORT=%PORT%
      export PARTY_ID=%PARTY_ID%
      export NUM_PARTIES=3
      docker build --build-arg PORT=${PORT} --build-arg PARTY_ID=${PARTY_ID} --build-arg NUM_PARTIES=${NUM_PARTIES} -t party .
      docker run --init -it -v party-data:/root/MP-SPDZ/ -p 8000-8100:8000-8100 -e PARTY_ID=${PARTY_ID} party
      ```

1. Configure and start the `Data Consumer API Server`
   1. Change the `PARTY_HOSTS` in `mpc_demo_infra/coordination_server/docker/.env.coord` to:
      ```bash
      PARTY_HOSTS=["192.168.0.101", "192.168.0.102", "192.168.0.103"]
      ```

   1. If you intend to use HTTPS, also:
      1. Set `PARTY_WEB_PROTOCOL` to `https`
      1. Rename the private key and certificate files of your domain as `privkey.pem` and `fullchain.pem` respectively and add them to `mpc_demo_infra/data_consumer_api/docker/ssl_certs` directory.

   1. Start the server
      ```bash
      cd ./mpc_demo_infra/data_consumer_api/docker/
      docker build -t data_consumer_api .
      docker run --init -it -p 8004:8004 data_consumer_api
      ```
