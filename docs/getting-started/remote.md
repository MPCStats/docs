---
title: "Remote Deployment and Testing"
slug: /getting-started/remote-deployment
sidebar_position: 2
---

# Remote Deployment and Testing

## 1. Deploy Servers Remotely

## Assumptions
Below instructions assume that the servers will be installed on:
- `192.168.0.100` (Coordination Server, Data Consumer API Server)
- `192.168.0.101` (Computation Party Server 0)
- `192.168.0.102` (Computation Party Server 1)
- `192.168.0.103` (Computation Party Server 2)

## 1.1. Deploy Coordination Server and Data Consumer API Server
1. Install Poetry by following the instructions [here](https://python-poetry.org/docs/#installation).

2. Clone the repository and cd to the repository root:
   ```bash
   git clone https://github.com/ZKStats/mpc-demo-infra.git
   cd mpc-demo-infra
   ```

3. Create the configuration file at the repository root:
   - `.env.coord`
     ```
     PORT=8005
     PARTY_HOSTS=["192.168.0.101", "192.168.0.102", "192.168.0.103"]
     PARTY_PORTS=[8006, 8007, 8008]
     PARTY_API_KEY=81f47c24b9fbe22421ea3ae92a9cc8f6
     PARTY_WEB_PROTOCOL=http
     PROHIBIT_MULTIPLE_CONTRIBUTIONS=False
     USER_QUEUE_HEAD_TIMEOUT=60
     FREE_PORTS_START=8010
     FREE_PORTS_END=8100
     ```
   - `.env.consumer_api`
     ```
     COORDINATION_SERVER_URL=http://192.168.0.100:8005
     CERTS_PATH=certs
     PARTY_HOSTS=["192.168.0.101", "192.168.0.102", "192.168.0.103"]
     PARTY_PORTS=["8006","8007","8008"]
     PARTY_WEB_PROTOCOL=http
     PORT=8004
     ```

4. Set up the environment:
   ```bash
   ./setup_env.sh --coord --consumer
   ```

5. Start the coordination server:
   ```bash
   poetry run coord-run
   ```

6. Start the data consumer API server:
   ```bash
   poetry run consumer-api-run
   ```

## 1.2. Deploy Computation Party Servers

For each party server, perform the following steps:
1. Install Poetry by following the instructions [here](https://python-poetry.org/docs/#installation).

2. Clone the repository and cd to the repository root:
   ```bash
   git clone https://github.com/ZKStats/mpc-demo-infra.git
   cd mpc-demo-infra
   ```

3. Create the configuration file at the repository root:
   - `.env.party`
     ```
     COORDINATION_SERVER_URL=http://192.168.0.100:8005
     PARTY_API_KEY=81f47c24b9fbe22421ea3ae92a9cc8f6
     PARTY_HOSTS=["192.168.0.101", "192.168.0.102", "192.168.0.103"]
     PARTY_PORTS=[8006, 8007, 8008]
     PARTY_WEB_PROTOCOL=http
     MAX_DATA_PROVIDERS=1000
     PERFORM_COMMITMENT_CHECK=True
     ```

4. Set up the environment:
   ```bash
   ./setup_env.sh --party
   ```

5. Start the party server:
    For party 0, run:
   ```bash
   PORT=8006 PARTY_ID=0 poetry run party-run
   ```
   For party 1, run:
   ```bash
   PORT=8007 PARTY_ID=1 poetry run party-run
   ```
   For party 2, run:
   ```bash
   PORT=8008 PARTY_ID=2 poetry run party-run
   ```


## 2. Sharing ETH Balance
1. Install Poetry by following the instructions [here](https://python-poetry.org/docs/#installation).

2. Get the Binance API key and secret, following the instructions in [Get Your Binance API Key](https://github.com/ZKStats/mpc-demo-infra/blob/main/mpc_demo_infra/client_cli/docker/README.md#step-1-get-your-binance-api-key)

3. Clone the repository and cd to the repository root:
   ```bash
   git clone https://github.com/ZKStats/mpc-demo-infra.git
   cd mpc-demo-infra
   ```

4. Set up Client and Notary Server:
    ```bash
    ./setup_env.sh --client --notary
    ```

5. Start Notary Server
    ```bash
    ./run_servers --notary-only
    ```

4. Modify the configuration file at the repository root:
   - `.env.client_cli`
     ```
     COORDINATION_SERVER_URL=http://192.168.0.100:8005
     PARTY_HOSTS=["192.168.0.101", "192.168.0.102", "192.168.0.103"]
     PARTY_PORTS=["8006","8007","8008"]
     PARTY_WEB_PROTOCOL=http
     NOTARY_SERVER_HOST=127.0.0.1
     ```

5. Execute `Client CLI`:
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
curl http://192.168.0.100:8004/query-computation
```
