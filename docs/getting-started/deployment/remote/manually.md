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

1. Install Poetry by following the instructions [here](https://python-poetry.org/docs/#installation).

1. Clone the repository and cd to the repository root:
   ```bash
   git clone https://github.com/ZKStats/mpc-demo-infra.git
   cd mpc-demo-infra
   ```

1. Install the dependencies:

   `Ubuntu`:
   ```bash
   ./setup_env.sh --setup-mpspdz
   ```

   Other environments:

   (TO BE ADDED)


## Deploying the Coordination Server
### Steps
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

1. Start the server
   ```bash
   poetry run coord-run
   ```

## Deploying Computation Party Server instances
### Steps
1. Create the configuration file at the repository root:
   - `.env.party`
     ```
     COORDINATION_SERVER_URL=http://192.168.0.199:8005
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

1. Start three `Computation Party Server` instances
   1. Open a new terminal and start the `party-0` server:
   ```bash
   PORT=8006 PARTY_ID=0 poetry run party-run
   ```

   2. Open a new terminal and start the `party-1` server:
   ```bash
   PORT=8007 PARTY_ID=1 poetry run party-run
   ```

   3. Open a new terminal and start the `party-2` server:
   ```bash
   PORT=8008 PARTY_ID=2 poetry run party-run
   ```

## Deploying the Data Consumer API Server
### Steps
1. Create the configuration file at the repository root:
   - `.env.consumer_api`
     ```
     COORDINATION_SERVER_URL=http://192.168.0.199:8005
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

1. Start the server
   ```bash
   poetry run consumet-api-run
   ```


