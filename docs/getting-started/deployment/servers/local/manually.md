---
title: "Manually"
sidebar_position: 2
---

# Manual Deployment

## Steps
1. Install Poetry by following the instructions [here](https://python-poetry.org/docs/#installation).

1. Clone the repository and cd to the repository root:
   ```bash
   git clone https://github.com/ZKStats/mpc-demo-infra.git
   cd mpc-demo-infra
   ```

1. Set up the environment and install dependencies:

   ```bash
   ./setup_env.sh --setup-local
   ```

1. Create the following configuration files at the repository root:
   - `.env.coord`
     ```
     PORT=8005
     PARTY_HOSTS=["localhost", "localhost", "localhost"]
     PARTY_PORTS=[8006, 8007, 8008]
     PARTY_API_KEY=81f47c24b9fbe22421ea3ae92a9cc8f6
     PARTY_WEB_PROTOCOL=http
     PROHIBIT_MULTIPLE_CONTRIBUTIONS=False
     USER_QUEUE_HEAD_TIMEOUT=60
     FREE_PORTS_START=8010
     FREE_PORTS_END=8100
     ```

   - `.env.party`
     ```
     COORDINATION_SERVER_URL=http://localhost:8005
     PARTY_API_KEY=81f47c24b9fbe22421ea3ae92a9cc8f6
     PARTY_HOSTS=["localhost", "localhost", "localhost"]
     PARTY_PORTS=[8006, 8007, 8008]
     PARTY_WEB_PROTOCOL=http
     MAX_DATA_PROVIDERS=1000
     PERFORM_COMMITMENT_CHECK=False
     ```

   - `.env.consumer_api`
     ```
     COORDINATION_SERVER_URL=http://localhost:8005
     CERTS_PATH=certs
     PARTY_HOSTS=["localhost", "localhost", "localhost"]
     PARTY_PORTS=["8006","8007","8008"]
     PARTY_WEB_PROTOCOL=http
     PORT=8004
     ```

1. Start the `Coordination Server`
   Ensure that you are at the repository root before proceeding.

   ```bash
   source venv/bin/activate
   poetry run coord-run
   ```

1. Start three `Computation Party Server` instances
   Ensure that you are at the repository root before proceeding.

   1. Open a new terminal and start the `party-0` server:
   ```bash
   source venv/bin/activate
   PORT=8006 PARTY_ID=0 poetry run party-run
   ```

   2. Open a new terminal and start the `party-1` server:
   ```bash
   source venv/bin/activate
   PORT=8007 PARTY_ID=1 poetry run party-run
   ```

   3. Open a new terminal and start the `party-2` server:
   ```bash
   source venv/bin/activate
   PORT=8008 PARTY_ID=2 poetry run party-run
   ```

1. Start the `Data Consumer API Server`
   ```bash
   source venv/bin/activate
   poetry run consumet-api-run
   ```

1. Start the `Notary Server`
   ```bash
   cd ../tlsn/notary/target/release
   ./notary-server
   ```

