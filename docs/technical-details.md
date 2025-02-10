# Technical Details

## Input Authentication

![input-authentication](./input-authentication.png)

- Data providers prove their Binance balance using TLSNotary.
- During MPC, the balance is used as a private input, and a TLSNotary commitment is calculated and revealed alongside the stats result.
- All parties verify the TLSNotary proof to ensure that the private input matches the commitment, providing input correctness.

## Client Interface

![client-interface](./client-interface.png)

In vanilla MPC, for data consumers to know the results from a computation, all data providers and consumers must stay online all the time, but that's not practical for

- Data Providers: Share masked balances with 3 computation parties via secret sharing. This allows them to go offline after providing their data.
- Computation Parties: Perform MPC to compute the statistical result.
- Data Consumers: Query results from the computation parties, ensuring asynchronous participation.

## Benchmarks

MPC Protocol: Settled on [`mal-rep-ring`](https://mp-spdz.readthedocs.io/en/latest/readme.html#honest-majority), a malicious secure, honest majority protocol, i.e. it tolerates up to 1/3 malicious parties. We benchmarked through all MPC protocols provided by MP-SPDZ and found it's the most practical option for our demo:

- TLSNotary Data Commitment: Identified as a bottleneck operation but performed efficiently, taking roughly 1 second for 1-byte data.
- Mean Calculation: Computes the mean of 10,000 numbers in under 0.2 seconds.

See [here](https://pse-team.notion.site/Choosing-a-Suitable-MPC-Protocol-fffd57e8dd7e8034b4d7c75b02d79ed3) for more details.

## Comparison with Existing Works

(To-be-completed)