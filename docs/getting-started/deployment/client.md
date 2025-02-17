---
title: "Client"
sidebar_position: 1
---

# Client CLI
Client CLI provides the following functionalities:

- Sharing ETH Balance 
You can anonymously share your ETH balance at Binance as follows:

- Query Computation
You can retrieve statistical metrics such as mean, median, max, min, and Gini coefficient based on users’ historical ETH balance contributions with the following command:

## Configuration
To configure the `Client CLI`, create a `.env.client_cli` file in the configuration directory.

### Default Configuration (Single-Server, Local HTTP)
```
COORDINATION_SERVER_URL=http://127.0.0.1:8005
PARTY_HOSTS=["127.0.0.1", "127.0.0.1", "127.0.0.1"]
PARTY_PORTS=[8006,8007,8008]
PARTY_WEB_PROTOCOL=http
NOTARY_SERVER_HOST=127.0.0.1
```

- If using HTTPS
  1. Set PARTY_WEB_PROTOCOL=https and update COORDINATION_SERVER_URL to use https.
  1. Rename the private key and certificate files of your domain as `privkey.pem` and `fullchain.pem` respectively and add them to `ssl_certs` directory at the repository root.

- If using multiple servers, update both COORDINATION_SERVER_URL and PARTY_HOSTS with the appropriate IP addresses.

## Installation
There are three ways to install the client CLI:

### 1. From Source Code
This method involves building the client entirely from the source code, offering the most security through full transparency and control.

- Bandwidth requirement: Moderate
- Downloads one repository and dependencies for a Rust executable and a Python3 script. Additionally, it may require downloading Rust, Python3, and Poetry if not already installed.

#### Steps
1. Clone the repository and cd to the repository root:
   ```bash
   git clone https://github.com/ZKStats/mpc-demo-infra.git
   cd mpc-demo-infra
   ```

1. Set up the environment:

   ```bash
   ./setup_env.sh --client
   ```

### 2. Using Pre-Built Binary
This method relies on trusting pre-compiled binary. It is the least secure but the simplest and fastest way to install the client.

- Bandwidth requirement: Low
- Downloads a shell script and two binaries (less than 50MB).

Note: The binaries are built directly from the source code using public GitHub workflows, defined and available in this repository. This ensures participants can verify the process and confirm binary integrity.
#### Steps
1. Download the script to fetch and execute appropriate binary
   ```
   curl -L -o share-data.sh https://github.com/ZKStats/mpc-demo-infra/releases/latest/download/share-data.sh
   ```
1. Follow the instructions in Client CLI Configuration to create an appropriate `.env.client_cli` file in the current directory.

### 3. Using Docker
This method builds the client in an isolated environment using publicly available images, providing moderate security.

- Bandwidth requirement: Very High
- Downloads dependent Docker images (more than 6GB) and Docker itself if not already installed.

Note: The Dockerfile used for building the client is included in this repository, ensuring transparency.

#### Steps
1. Install `Docker` by following the instructions [here](https://docs.docker.com/engine/install/).

1. Clone the repository and cd to the repository root:
   ```bash
   git clone https://github.com/ZKStats/mpc-demo-infra.git
   cd mpc-demo-infra
   ```

1. Follow the instructions in Client CLI Configuration to create a `.env.client_cli` in `./mpc_demo_infra/client_cli/docker/` directory

1. Build Docker image
   ```
   cd mpc-demo-infra/mpc_demo_infra/client_cli/docker
   ./build.sh
   ```

## Execution

### Sharing ETH Balance
1. Get the Binance API key and secret, following the instructions in [Get Your Binance API Key](https://github.com/ZKStats/mpc-demo-infra/blob/main/mpc_demo_infra/client_cli/docker/README.md#step-1-get-your-binance-api-key)

1. Follow the instructions in Client CLI Configuration to create a `.env.client_cli` in the repository root directory

1. Execute `Client CLI`:

   - Using Source Code

     Make sure that you are at the repository root before proceeding.

     - Single-Server Local Configuration
       1. Get the container ID of the Notary Server container:
          ```bash
          CONTAINER=$(docker ps | grep notary | awk '{print $1}')
          ```
       1. Copy `notary.crt` from the container:
          ```bash
          docker cp $CONTAINER:/root/tlsn/notary/target/release/fixture/tls/notary.crt .
          ```
       1. Follow the instructions in Client CLI Configuration to create a `.env.client_cli` in the repository root directory

       1. Execute `Client CLI`:
          ```bash
          poetry run client-share-data <eth-address> <binance-api-key> <binance-api-secret>  --notary-crt-path $(pwd)/notary.crt
          ``` 
     - Multi-Server Configuration
       ```bash
       poetry run client-share-data <eth-address> <binance-api-key> <binance-api-secret>
       ```

   - Using Pre-Built Binary
     ```bash
     chmod +x share-data.sh
     ./share-data.sh <eth-address> <binance-api-key> <binance-api-secret>
     ```

   - Using Docker
     Make sure that you are at the repository root before proceeding.

     ```bash
     cd ./mpc_demo_infra/client_cli/docker/
     ./share-data.sh <eth-address> <binance-api-key> <binance-api-secret>
     ```

### Query Computation
Make sure that you are at the repository root before proceeding.

1. Follow the instructions in Client CLI Configuration to create a `.env.client_cli` in the repository root directory

1. Execute `Client CLI`:
   ```bash
   poetry run client-query
   ```

