---
title: "Troubleshooting"
sidebar_position: 4
---

# Troubleshooting and Support

## Common Issues
If you encounter issues:

1. Ensure all dependencies are correctly installed.
2. Check that the required ports (8005-8008, 8010-8100) are not in use.
3. Verify that TLSN and MP-SPDZ exit in the correct locations (`../tlsn` and `../MP-SPDZ`).
4. For MP-SPDZ issues, ensure that
  - you've added `MOD = -DGFP_MOD_SZ=5` to `CONFIG.mine`.
  - you've generated certificates for computation parties. If not, run `Scripts/setup-ssl.sh` under `../MP-SPDZ`.
  - you've rebuilt the VM. If not, run `make replicated-ring-party.x` under `../MP-SPDZ`.
5. If you run into Docker-related issues, try these commands to clean up your environment:
   ```bash
   docker system prune -a --volumes
   docker rm -f $(docker ps -aq)
   docker volume prune -a -f
   docker image prune -f
   ```

## Getting Help
If you encounter any issues during installation, feel free to open an issue in our [GitHub repository](https://github.com/ZKStats/mpc-demo-infra).

