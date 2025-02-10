---
sidebar_position: 1
slug: /
---

# Introduction

MPCStats Demo Infrastructure provides a boilerplate for privacy-preserving statistical analysis and developers can use this boilerplate to build their own privacy-preserving applications by modifying existing components or extending functionality. We support use cases with this pattern:

1. Data providers generate a proof for their data from some website using [TLSNotary](https://tlsnotary.org/). This proof does not reveal their data.
2. Arbitrary computation can be performed on all data providers' masked data using MPC (we're using [MP-SPDZ](https://github.com/data61/MP-SPDZ/) as the MPC framework). Computation is defined with Python DSL and support common statistical operations.
3. Data consumers can query the results.

Data consumers can be convinced the statistical results are correct without revealing their data.

![client-interface](./client-interface.png)

The current implementation demonstrates our [Devcon demo](https://demo.mpcstats.org/) where participants collaboratively compute the statistical results from their ETH balance on Binance. This docs explain how everything works, how to modify the boilerplate, and how to deploy the infrastructure.
