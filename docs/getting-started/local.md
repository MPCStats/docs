---
title: "Local Deployment and Testing"
slug: /getting-started/local-deployment
sidebar_position: 1
---

# Local Deployment and Testing

## 1. Deploy Servers Locally

You have two options for deploying the servers locally:

### 1.1. Manual Deployment

1. Install Poetry by following the instructions [here](https://python-poetry.org/docs/#installation).

2. Clone the repository and navigate to the repository root:
   ```bash
   git clone --recursive https://github.com/MPCStats/mpc-demo-infra.git
   cd mpc-demo-infra
   ```

3. Set up the environment:
   ```bash
   ./setup_env.sh
   ```

4. Start all of the servers:
   ```bash
   ./run_servers.sh
   ```
   Shutdown the servers with `Ctrl+C`.


### 2.1. Sharing ETH Balance

1. Get the Binance API key and secret, following the instructions in [Get Your Binance API Key](https://github.com/MPCStats/mpc-demo-infra/blob/main/mpc_demo_infra/client_cli/docker/README.md#step-1-get-your-binance-api-key)

2. Run the client CLI:
    ```bash
    poetry run client-share-data <eth-address> <binance-api-key> <binance-api-secret>
    ```
    where `<eth-address>` can be any random Ethereum address, `<binance-api-key>` is your Binance API key, `<binance-api-secret>` is your Binance API secret.


### 2.2. Querying Results

Query statistical results.
```bash
poetry run client-query
```

> Please make sure you have run `client-share-data` first, otherwise you will get an error like
>    ```bash
>    2025-02-27 17:36:46 [ERROR] Failed to query computation: Failed to query computation: response.status=400, await response.text()='{"detail":"No MPC session found"}'
>    ```

You can also query results with Client API:
```bash
curl http://127.0.0.1:8004/query-computation
```
