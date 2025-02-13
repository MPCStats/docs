---
title: "Client"
---

# Client CLI Deployment

## Assumptions
Below instructions assume that the servers have been installed on:
- `192.168.0.100` (Coordination Server)
- `192.168.0.101` (Computation Party Server 0)
- `192.168.0.102` (Computation Party Server 1)
- `192.168.0.103` (Computation Party Server 2)

## Installation
There are three ways to install the client CLI:

### 1. Using Pre-Built Binaries
This method relies on trusting pre-compiled binaries. It is the least secure but the simplest and fastest way to install the client.

- Bandwidth requirement: Low
- Downloads a shell script and two binaries (less than 50MB).

Note: The binaries are built directly from the source code using public GitHub workflows, defined and available in this repository. This ensures participants can verify the process and confirm binary integrity.


#### Steps
1. Execute the following:
   ```
   curl -L https://github.com/ZKStats/mpc-demo-infra/releases/latest/download/share-data.sh | bash -s -- <eth-address> <binance-api-key> <binance-api-secret>
   ```

### 2. Using Docker
This method builds the client in an isolated environment using publicly available images, providing moderate security.

- Bandwidth requirement: Very High
- Downloads dependent Docker images (more than 6GB) and Docker itself if not already installed.

Note: The Dockerfile used for building the client is included in this repository, ensuring transparency.

#### Steps
1. Install `Docker` by following the instructions [here](https://docs.docker.com/engine/install/).

1. Execute the following:
   ```
   git clone https://github.com/ZKStats/mpc-demo-infra.git
   cd mpc-demo-infra/mpc_demo_infra/client_cli/docker
   ./build.sh
   ./share-data.sh <eth-address> <binance-api-key> <binance-api-secret>
   ```

### 3. From Source Code

This method involves building the client entirely from the source code, offering the most security through full transparency and control.

- Bandwidth requirement: Moderate
- Downloads one repository and dependencies for a Rust executable and a Python3 script. Additionally, it may require downloading Rust, Python3, and Poetry if not already installed.

#### Steps
1. Clone the `mpc-demo-infra` repository and cd to the repository root:
   ```bash
   git clone https://github.com/ZKStats/mpc-demo-infra.git
   cd mpc-demo-infra
   ```

2. Install Rust
   ```
   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
   ```

3. Install Binance Prover

   Ensure that you are at the repository root before proceeding.
   ```
   cd tlsn/tlsn
   cargo build --release --example binance_prover
   ```

4. Install Python3
   - Ubuntu 24.04
   ```
   apt-get update && apt-get install -y python3 python3-venv python3-pip curl pipx git automake build-essential clang cmake git libboost-dev libboost-iostreams-dev libboost-thread-dev libgmp-dev libntl-dev libsodium-dev libssl-dev libtool pkg-config libmpfr-dev libmpc-dev && apt-get clean 
   ```

   - MacOS (Intel/Apple Silicon)

   ```
   brew install python
   ```

5. Install Poetry
   ```
   python3 -m venv venv
   source venv/bin/activate
   venv/bin/pip install -U pip setuptools
   venv/bin/pip install poetry
   ```

6. Install the MPC demo infrastructure

   Ensure that you are at the repository root before proceeding.
   ```
   poetry install
   ```

## Configuration
1. Create a `.env.client_cli` file at the repository root with the following contents:
   ```
   COORDINATION_SERVER_URL=http://192.168.0.100:8005
   PARTY_HOSTS=["192.168.0.101","192.168.0.102","192.168.0.103"]
   PARTY_PORTS=[8006,8007,8008]
   PARTY_WEB_PROTOCOL=http
   ```
2. If you intend to use HTTPS, also:
   1. Set `PARTY_WEB_PROTOCOL=https` in the `.env.client_cli` file.
   2. Rename the private key and certificate files of your domain as `privkey.pem` and `fullchain.pem` respectively and add them to `ssl_certs` directory at the repository root.

3. Move `.env.client_cli` to the appropriate location based on your installation method:
   - Pre-built binaries: The directory where the binaries were downloaded
   - Docker: Under `./mpc_demo_infra/client_cli/docker/` directory
   - Source code: The repository root directory

## Using Client CLI
Client CLI provides two main functionalities: sharing data and querying computation.

### Share Data
You can anonymously share your ETH balance at Binance as follows:

1. Get the Binance API key and secret, following the instructions in [Get Your Binance API Key](https://github.com/ZKStats/mpc-demo-infra/blob/main/mpc_demo_infra/client_cli/docker/README.md#step-1-get-your-binance-api-key)

1. Get a voucher from the Coordination Server

1. Share your ETH balance at Binance with the voucher: 

   ```bash
   poetry run client-share-data <eth_address> <binance_api_key> <binance_api_secret>
   ```

### Query Computation
You can retrieve statistical metrics such as mean, median, max, min, and Gini coefficient based on users’ historical ETH balance contributions with the following command:

```bash
poetry run client-query
```
