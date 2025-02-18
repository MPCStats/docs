---
title: "Troubleshooting"
sidebar_position: 4
---

# Troubleshooting and Support

## Common Issues
If you encounter issues:

1. Ensure all dependencies are correctly installed.
1. Check that the required ports (8005-8008, 8010-8100) are not in use.
1. Verify that symbolic links to `tlsn` and `MP-SPDZ` exist in the expected locations (`../tlsn` and `../MP-SPDZ` relative to the repository root).
4. For `MP-SPDZ` issues, ensure that:
   - you've added `MOD = -DGFP_MOD_SZ=5` to `MP-SPDZ/CONFIG.mine` at the repository root.
   - you've generated certificates for computation parties. If not, run `Scripts/setup-ssl.sh` under `MP-SPDZ` directory at the repository root.
   - you've rebuilt the VM. If not, run `make replicated-ring-party.x` under `MP-SPDZ` directory at the repository root.
1. If you run into Docker-related issues, try these commands to clean up your environment:
   ```bash
   docker system prune -a --volumes
   docker rm -f $(docker ps -aq)
   docker volume prune -a -f
   docker image prune -f
   ```
1. If you are trying single-server local configuration and getting `UnknownCA` error from `binance_prover`, make sure you don't have the `binance_prover` in the repository root directory.
1. If you get an error like 'P0.pem' is empty, try running `./Scripts/setup-ssl.sh 3` under `MP-SPDZ` directory at the repository root.
## Getting Help
1. If Notary Server's SSL certificate has expired, you can generate a new one by running the following commands from the repository root:
   ```bash
   cd tlsn/notary/server/fixture/tls 
   openssl genpkey -algorithm RSA -out notary.key -pkeyopt rsa_keygen_bits:2048 \
     && openssl req -new -key notary.key -out request.csr -subj "/C=US/ST=State/L=City/O=Organization/OU=Department/CN=127.0.0.1" \
     && openssl x509 -req -in request.csr -signkey notary.key -out notary.crt -days 365 -extfile openssl.cnf -extensions v3_req
   ```

If you encounter any issues during installation, feel free to open an issue in our [GitHub repository](https://github.com/ZKStats/mpc-demo-infra).

