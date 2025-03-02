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

## Demo at Devcon 2024

We had a demo at [Devcon 2024](https://www.youtube.com/watch?v=wCp7Zsjou7w) where participants can prove and share their Binance ETH balance in their spot account using the Binance API. We then derive the mean, median, max, and gini index of the balances of all participants, without revealing their individual balances.

This demo demonstrates how real-world data sources like Binance can be securely integrated into privacy-preserving statistical computations using MPC and TLSNotary. It's a foundational step toward building tools that enable collaborative data analysis while preserving user privacy.

You can explore our [implementation code](https://github.com/ZKStats/mpc-demo-infra) and read our detailed [Devcon Demo Report](https://pse-team.notion.site/MPCStats-Devcon-Demo-Report-3055bb69afd24d60bf8ee8d4fa5f774c) to learn more about the technical details and outcomes of this demonstration. For the rest of the documentation, we'll focus on the components and workflow of the demo.

Below is what our stats page looked like:

![Devcon demo interface](./devcon-demo.png)

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
