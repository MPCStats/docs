# Dependencies

- python 3
- poetry
- cargo
- [TLSN](https://github.com/ZKStats/tlsn)
  - branch: `mpspdz-compat`
  - clone it as `../tlsn`
- [MP-SPDZ](https://github.com/ZKStats/MP-SPDZ) (only required for computation party server)
  - branch: `demo_client`
  - clone it as `../MP-SPDZ`
  - need to add `MOD = -DGFP_MOD_SZ=5 -DRING_SIZE=257` to `CONFIG.mine`
  - install: `make setup`
  - build vm: `make replicated-ring-party.x`

