---
sidebar_position: 2
---

# Customizing the Infrastructure

Developers can modify and extend the boilerplate to create their own privacy-preserving applications.

- Customization Areas:

  1. TLSNotary Integration: Adjust the prover and verifier to handle new data sources.
  2. MPC Program: Modify or extend the statistical computations to fit specific needs.
  3. Database and Storage: Customize how data is stored or integrated into external systems.

- Steps to Customize:
  1. Modify the TLSNotary prover and verifier for new data sources. Can look at a few examples [here](https://github.com/ZKStats/tlsn/tree/mpspdz-compat/tlsn/examples), specifically to be compatible with end-to-end flow, pls follow the instruction [here](https://github.com/ZKStats/tlsn/tree/mpspdz-compat/tlsn/examples/binance)
  2. Update the MPC program to include additional or modified statistical operations. Use MPCStats library for statistical operations. Can see the example and instruction what/how to customize [here](https://github.com/ZKStats/MP-SPDZ/tree/mpcstats-lib/mpcstats)
  3. Deploy and test the customized application locally before scaling to a remote setup.
