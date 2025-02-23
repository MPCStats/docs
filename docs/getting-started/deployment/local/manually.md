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

1. Set up the environment:

   ```bash
   ./setup_env.sh
   ```

1. Start all the infrastructure servers:

   ```bash
   ./start-all-server.sh
   ```

1. Shutdown all the infrastructure servers:

   ```bash
   ./shut-down-all-servers.sh
   ```
