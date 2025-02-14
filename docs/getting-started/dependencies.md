# Dependencies

- python 3
- poetry
- cargo
- [TLSN](https://github.com/ZKStats/tlsn)
  - branch: `mpspdz-compat`
- [MP-SPDZ](https://github.com/ZKStats/MP-SPDZ) (only required for computation party server)
  - branch: `demo_client`
  - need to add `MOD = -DGFP_MOD_SZ=5 -DRING_SIZE=257` to `CONFIG.mine`
  - install: `make setup`
  - build vm: `make replicated-ring-party.x`

## Note
`TLSN` and `MP-SPDZ` are included in the `mpc-demo-infra` repository to allow users to customize them, such as using a different MPC scheme or developing a new prover and verifier.
