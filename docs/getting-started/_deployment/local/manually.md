---
title: "Manually"
sidebar_position: 2
---

# Manual Deployment

## Steps
1. Install Poetry by following the instructions [here](https://python-poetry.org/docs/#installation).

1. Clone the repository and cd to the repository root:

   ```bash
   git clone https://github.com/MPCStats/mpc-demo-infra.git
   cd mpc-demo-infra
   ```

1. Set up the environment:

   Make sure that you are at the repository root before proceeding.

   ```bash
   ./setup_env.sh
   ```

1. Start all the infrastructure servers:

   Make sure that you are at the repository root before proceeding.

   ```bash
   ./run_servers.sh
   ```

1. Shutdown all the infrastructure servers:

   Make sure that you are at the repository root before proceeding.

   ```bash
   ./shutdown_all_servers.sh
   ```
