---
sidebar_position: 3
---

# Customization

Developers can modify and extend the boilerplate to create their own privacy-preserving applications by customizing these following components.

- Add new data sources: e.g.



## 1. Modify the TLSNotary prover and verifier for new data sources.

> This is a bit different from original TLSNotary, because in addition to being able to specify "redacted" parts of data where they are just not shown in the proof, users can also specify the "private" parts in the proof, which are not only not shown in the proof, but also having their sha3 commitment in the proof such that they can be seamlessly integrated with MP-SPDZ to make sure that the inputs of MP-SPDZ actually come from the private parts of these data from TLSNotary. This guide will mostly focus on customizing this additional "private" feature.

We have a few examples [here](https://github.com/ZKStats/tlsn/tree/mpspdz-compat/tlsn/examples), but to be fully compatible with end-to-end flow, let's take a look at how to customize this TLSNotary prover and verifier from Binance Eth Balance [Example](https://github.com/ZKStats/tlsn/tree/mpspdz-compat/tlsn/examples/binance) by modifying the following files:

#### binance_prover.rs

Here is the main file for creating proof, where you can make this following customizations for "private" part of data.

**In main()**:

- Specify "secret_file" format, the json format of the secret and its corresponding nonce to be written into the file containing all private information used to prove later that its sha3 hash is the sha3 commitment contained in the proof.

**In build_proof_with_redactions()**:

- Specify "redacted" parts in sent message.
  In this Binance example, we want to redact lots of things like user_agent, api_key and signature. We achieve so by both specifying private_seq and private_regexes (will become obvious once looking at the code) and then just combine them together to achieve sent_public_ranges.

- Specify "public" parts of the received message.
  We specify the part that we want to make it publicly shown in the proof (recv_public_ranges), by specifying the regex that must be "redacted" & "private"

- Specify "private" parts of the received message.
  We specify the private part (recv_private_ranges)that will be accompanied with sha3 commitment in the proof while being censored from the proof itself by specifying our preferred regex. In Binance example, we specify to make ETH free balance of only 2 decimlals precision private.

> With this structure, there will be some parts of received message that is not in either recv_public_ranges or recv_private_ranges. Those will be just redacted data that are censored without its correponding commitment (like in original TLSNotary)

> Since we decide which part to censor based on regex, it is very important to make sure that the returned data is formatted as you expect when you write regex or else there may result in unexpected data leaking. In our case, we enforce the check that recv transcript ends with uid because this is the assumption that we used to constrain regex in determining recv_public_ranges

> In getting data from API, it's recommended to specify as many arguments for API query as possible because we prefer the data sent back from API to be as smallest as possible.

#### binance_verifier.rs

Here, we just encapsulate the logic that distinguishes which part is just redacted, and which part is private.

```
    sent.set_redacted(b'X');
    recv.set_redacted(b'X');
    recv.set_private(b'Y');
```

## 2. Update the MPC program to include additional or modified statistical operations.

Use MPCStats library for statistical operations. Can see the example and instruction what/how to customize [here](https://github.com/ZKStats/MP-SPDZ/tree/mpcstats-lib/mpcstats)

## 3. Customize Coordination Server

This customization is to make sure we properly handle what data is allowed to participate in MPC process. In Binance example, we make sure that one "uid" (user id of binance account) can only submit one proof. Hence, with different data source, we need to adjust how we store & process data in coordination accordingly as follows.

- Specify what to store in coordination server.
  In Binance example, we store eth_address and uid [here](https://github.com/ZKStats/mpc-demo-infra/blob/e73b35aa487b8dc1efd403edddb80f10ebebf681/mpc_demo_infra/coordination_server/database.py#L31)
- Modify how we extract those data field from proof and make sure it wont repeat what's already in database.
  Just modify the code [here](https://github.com/ZKStats/mpc-demo-infra/blob/e73b35aa487b8dc1efd403edddb80f10ebebf681/mpc_demo_infra/coordination_server/routes.py#L142-L157)
- Modify how to process database i.e. creating task for sharing data MPC [here](https://github.com/ZKStats/mpc-demo-infra/blob/e73b35aa487b8dc1efd403edddb80f10ebebf681/mpc_demo_infra/coordination_server/routes.py#L233-L250)

## 4. Test locally

To quickly check that your infrastructure customization is working as intended, you can run the integration tests as follows:

```bash
poetry run pytest -s tests/test_integration.py
```

## 5. Deploy the application locally/remotely

To deploy the application locally, you can follow the steps in the [Deployment](/getting-started/deployment) section.
