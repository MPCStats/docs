---
title: "Local Deployment and Testing"
slug: /getting-started/local-deployment
sidebar_position: 1
---

# Local Deployment and Testing

## 1. Deploy Servers Locally

You have two options for deploying the servers locally:

### 1.1. Manual Deployment

1. Clone the repository and navigate to the repository root:
   ```bash
   git clone https://github.com/ZKStats/mpc-demo-infra.git
   cd mpc-demo-infra
   ```

2. Set up the environment:
   ```bash
   ./setup_env.sh
   ```

3. Start the servers:
   ```bash
   ./start-all-servers.sh
   ```

4. Shutdown the servers:
   ```bash
   ./shutdown-all-servers.sh
   ```

### 1.2. Or with Docker

Alternatively, you can use Docker for a simpler deployment experience, though it might take time to build the images:

1. Install `Docker` by following the instructions [here](https://docs.docker.com/engine/install/).

2. Install `Docker Compose` by following the instructions [here](https://docs.docker.com/compose/install/).

3. Build and start all servers with:
   ```bash
   ./config-docker-compose.py
   docker-compose up --build
   ```

   To stop the servers, press `Ctrl+C` or run:
   ```bash
   docker-compose down
   ```


## 2. Sharing ETH Balance

1. Get the Binance API key and secret, following the instructions in [Get Your Binance API Key](https://github.com/ZKStats/mpc-demo-infra/blob/main/mpc_demo_infra/client_cli/docker/README.md#step-1-get-your-binance-api-key)

2. Create a `.env.client_cli` file in the repository root with the following content:
    ```bash
    cp .env.client_cli.example .env.client_cli
    ```

3. Run the client CLI:
    ```bash
    poetry run client-share-data <eth-address> <binance-api-key> <binance-api-secret>
    ```
    where `<eth-address>` can be any random Ethereum address, `<binance-api-key>` is your Binance API key, `<binance-api-secret>` is your Binance API secret.


## 3. Querying Results

Query with Client CLI
```bash
poetry run client-query
```

or query with Client API:
```bash
curl http://127.0.0.1:8004/query-computation
```
