---
title: "Manually"
sidebar_position: 2
---

# Manual Deployment

## Assumptions
Below instructions assume that the servers will be installed on:
- `192.168.0.100` (Coordination Server)
- `192.168.0.101` (Computation Party Server 0)
- `192.168.0.102` (Computation Party Server 1)
- `192.168.0.103` (Computation Party Server 2)
- `192.168.0.104` (Data Consumer API Server)

Note: You can use domain names such as `coord.example.com` instead of IP addresses.

## Common Environment Configuration
Follow the steps below on each server before deploying:

1. Clone the `mpc-demo-infra` repository and cd to the repository root:
   ```bash
   git clone https://github.com/MPCStats/mpc-demo-infra.git
   ```

## `setup_env.sh` Options
The `setup_env.sh` script can take multiple options to set up an environment for running multiple servers. For example, if you intend to run `Coordination Server`, `Data Compumer API Server` and `Notary Server` on one server, you can execute the following:
```
./setup_env.sh --coord --consumer --notary
```

instead of executing:

```
./setup_env.sh --coord
./setup_env.sh --consumer
./setup_env.sh --notary
```

## Deploying the Coordination Server
1. Create the configuration file at the repository root:
   - `.env.coord`
     ```
     PORT=8005
     PARTY_HOSTS=["192.168.0.100", "192.168.0.101", "192.168.0.102"]
     PARTY_PORTS=[8006, 8007, 8008]
     PARTY_API_KEY=81f47c24b9fbe22421ea3ae92a9cc8f6
     PARTY_WEB_PROTOCOL=http
     PROHIBIT_MULTIPLE_CONTRIBUTIONS=False
     USER_QUEUE_HEAD_TIMEOUT=60
     FREE_PORTS_START=8010
     FREE_PORTS_END=8100
     ```
1. If you intend to use HTTPS:
   1. Add the following lines to the `.env.coord` file:
      ```
      PRIVKEY_PEM_PATH=ssl_certs/privkey.pem
      FULLCHAIN_PEM_PATH=ssl_certs/fullchain.pem
      ```

   1. Update the existing `PARTY_WEB_PROTOCOL` line with:
      ```
      PARTY_WEB_PROTOCOL=https

      ```
   1. Rename the private key and certificate files of your domain as `privkey.pem` and `fullchain.pem` respectively and add them to `mpc_demo_infra/ssl_certs` directory.

1. Set up the environment:
  ```bash
  ./setup_env.sh --coord
  ```

1. Start the server
   Make sure that you are at the repository root before proceeding.

   ```bash
   poetry run coord-run
   ```

## Deploying Computation Party Server instances
Perform the following steps on each of the three servers running a Computation Party Server instance.

1. Create the configuration file at the repository root:
   - `.env.party`
     ```
     COORDINATION_SERVER_URL=http://192.168.0.100:8005
     PARTY_API_KEY=81f47c24b9fbe22421ea3ae92a9cc8f6
     PARTY_HOSTS=["192.168.0.100", "192.168.0.101", "192.168.0.102"]
     PARTY_PORTS=[8006, 8007, 8008]
     PARTY_WEB_PROTOCOL=http
     MAX_DATA_PROVIDERS=1000
     PERFORM_COMMITMENT_CHECK=False
     ```

1. If you intend to use HTTPS:
   1. Add the following lines to the `.env.party` file:
      ```
      PRIVKEY_PEM_PATH=ssl_certs/privkey.pem
      FULLCHAIN_PEM_PATH=ssl_certs/fullchain.pem
      ```

   1. Update the existing `PARTY_WEB_PROTOCOL` line with:
      ```
      PARTY_WEB_PROTOCOL=https
      ```

   1. Rename the private key and certificate files of your domain as `privkey.pem` and `fullchain.pem` respectively and add them to `mpc_demo_infra/ssl_certs` directory.

1. Set up the environment:
  ```bash
  ./setup_env.sh --party
  ```

1. Start the `Computation Party Server`
   Make sure that you are at the repository root before proceeding.

   In the command below, replace `%PARTY_ID%` with the party ID assigned to the server (0, 1, or 2) and replace `%PORT%` with 8006, 8007, or 8008 for Party 0, Party 1, and Party 2, respectively.

   ```bash
   PORT=%PORT% PARTY_ID=%PARTY_ID% poetry run party-run
   ```

## Deploying the Data Consumer API Server
1. Create the configuration file at the repository root:
   - `.env.consumer_api`
     ```
     COORDINATION_SERVER_URL=http://192.168.0.100:8005
     CERTS_PATH=certs
     PARTY_HOSTS=["192.168.0.100", "192.168.0.101", "192.168.0.102"]
     PARTY_PORTS=["8006","8007","8008"]
     PARTY_WEB_PROTOCOL=http
     PORT=8004
     ```

1. If you intend to use HTTPS:
   1. Add the following lines to the `.env.party` file:
      ```
      PRIVKEY_PEM_PATH=ssl_certs/privkey.pem
      FULLCHAIN_PEM_PATH=ssl_certs/fullchain.pem
      ```

   1. Update the existing `PARTY_WEB_PROTOCOL` line with:
      ```
      PARTY_WEB_PROTOCOL=https
      ```

   1. Rename the private key and certificate files of your domain as `privkey.pem` and `fullchain.pem` respectively and add them to `mpc_demo_infra/ssl_certs` directory.

1. Set up the environment:
  ```bash
  ./setup_env.sh --consumer
  ```

1. Start the server
   Make sure that you are at the repository root before proceeding.

   ```bash
   poetry run consumer-api-run
   ```

## Deploying the Notary Server
Make sure that you are at the repository root before proceeding.

1. Set up the environment:
  ```bash
  ./setup_env.sh --notary
  ```

1. Generate self-signed certificate
   Replace `%NOTARY_IP%` below with the IP address of the server on which the Notary Server runs.

   ```bash
   cp ./mpc_demo_infra/notary_server/docker/openssl.cnf ../tlsn/notary/server/fixture/tls/
   pushd ../tlsn/notary/server/fixture/tls/
   sed -i "s/XXX/%NOTARY_IP%/" openssl.cnf
   openssl genpkey -algorithm RSA -out notary.key -pkeyopt rsa_keygen_bits:2048
   openssl req -new -key notary.key -out request.csr -subj "/C=US/ST=State/L=City/O=Organization/OU=Department/CN=%NOTARY_IP%"
   openssl x509 -req -in request.csr -signkey notary.key -out notary.crt -days 365 -extfile openssl.cnf -extensions v3_req
   popd
   ```

1. Copy config and cert files to release directory
   ```bash
   cp -R ../tlsn/notary/server/fixture ../tlsn/notary/target/release
   cp -R ../tlsn/notary/server/config ../tlsn/notary/target/release
   cp ./mpc_demo_infra/notary_server/docker/ssl_certs/notary.crt ../tlsn/notary/target/release/fixture/tls
   cp ./mpc_demo_infra/notary_server/docker/ssl_certs/notary.key ../tlsn/notary/target/release/fixture/tls
   cp ./mpc_demo_infra/notary_server/docker/config.yaml ../tlsn/notary/target/release/config
   ```
1. Start the server
   ```bash
   cd ../tlsn/notary/target/release
   ./notary-server

   ```

