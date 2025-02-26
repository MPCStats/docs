---
sidebar_position: 1
slug: /
---

# Introduction

MPCStats Demo Infrastructure is designed for privacy-preserving statistical survey using Multi-Party Computation (MPC). It enables multiple data providers to share inputs without exposing them to others while allowing the computation of aggregate statistics. The system is adaptable, supporting various data sources and statistical operations.

## Why MPCStats Demo Infrastructure?

Traditional methods of computing statistics from multiple data sources often require sending raw data to a central server:
- **Privacy Risk**: Centralized access to individual data points can expose sensitive information.
- **Input Integrity Concerns**: Without verification, data providers could submit incorrect or falsified data.

We provide these features:
- **Data Privacy**: Using MPC (built on the [MP-SPDZ](https://github.com/data61/MP-SPDZ/) framework), the system enables computations without exposing individual inputs, ensuring no single party gains access to others’ raw data.
- **Input Validity**: Through [TLSNotary](https://tlsnotary.org/), data providers can prove their data is authentically sourced from verified websites, preventing falsified inputs.
- **Developer and User Experience**:
  - **For developers**: The infrastructure is easy to set up and modify for various scenarios. While existing frameworks can conduct such surveys, they often require understanding complex underlying technologies and custom implementations. MPCStats provides boilerplate code and a statistical library to simplify customization.
  - **For data providers and consumers**: By delegating MPC to three non-colluding servers, participants experience a workflow similar to traditional statistical survey systems. This design allows users to submit data and disconnect, enhancing usability without compromising privacy.

MPCStats Demo Infrastructure ensures privacy-preserving and verifiable results, making it suitable for sensitive applications such as financial analysis, health data aggregation, and collaborative statistics.
<!--
MPCStats Demo Infrastructure is the demo built by the [MPCStats](https://pse.dev/en/projects/mpc-stats) team for [Devcon 7](https://www.youtube.com/watch?v=wCp7Zsjou7w). Data providers can share their data privately, and data consumers can learn statistical results from all shared data.

![graph: data providers -> data consumers]


provides a boilerplate to perform statistical analysis on data shared by data providers, without data providers revealing their data. Developers can use this boilerplate to build their own privacy-preserving applications by modifying existing components or extending functionality. We support use cases with this pattern:

1. Data providers generate a proof for their data from some website using [TLSNotary](https://tlsnotary.org/), which does not reveal their data.
2. Arbitrary computation can be performed on all data providers' masked data using MPC: we're using [MP-SPDZ](https://github.com/data61/MP-SPDZ/) as our underlying MPC framework.

Data consumers can be convinced the statistical results are correct without revealing their data.

![client-interface](./client-interface.png)

TODO: should be two examples: simple and binance

The current implementation demonstrates our [demo at Devcon 7](https://pse-team.notion.site/MPCStats-Devcon-Demo-Report-3055bb69afd24d60bf8ee8d4fa5f774c) where participants collaboratively compute the statistical results from their ETH balance on Binance. This docs explain how everything works, how to modify the boilerplate, and how to deploy the infrastructure. See our talk [here](https://www.youtube.com/watch?v=wCp7Zsjou7w).

-->
