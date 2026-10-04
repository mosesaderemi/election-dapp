# Election DApp

A transparent election smart contract written in Solidity, with Brownie scripts and tests for deployment and verification. An administrator registers candidates and controls the voting window, and each wallet address can cast exactly one vote.

## How It Works

The contract moves through three states: `NOTOPENYET`, `OPEN` and `ENDED`.

1. The deploying account becomes the administrator.
2. The administrator adds candidates while the election has not started.
3. The administrator opens voting for a chosen duration.
4. Each address votes once for a candidate by ID.
5. After the duration passes, the administrator ends the election and the result can be read on chain.

## Contract Reference (`contracts/Election.sol`)

| Function | Access | Description |
| --- | --- | --- |
| `addCandidate(name)` | Administrator | Register a candidate and emit `CandidateAdded` |
| `openVoting(duration)` | Administrator | Start voting for the given duration and emit `VoteOpened` |
| `vote(candidateId)` | Any address, voting open | Cast one vote, rejected if the address has already voted |
| `endVoting()` | Administrator | Close the election after the deadline and emit `VoteEnded` |
| `getVoteCount(candidateId)` | Public view | Votes for a candidate |
| `getCandidatesList()` | Public view | All candidates with their vote counts |
| `getUserVote(address)` | Public view | The candidate an address voted for |
| `getResult()` | Public view | The election outcome |

## Tech Stack

| Area | Technology |
| --- | --- |
| Smart contract | Solidity ^0.8.0 |
| Framework | Brownie (Python) |
| Testing | pytest through Brownie |
| Web3 helpers | web3.py |

## Project Structure

```text
.
├── contracts/Election.sol        Election contract
├── scripts
│   ├── deploy_election.py        Deployment script
│   └── helper_scripts.py         Account selection and unit helpers
└── tests/test_election.py        Contract tests
```

## Getting Started

### Prerequisites

- Python 3.8 or later
- Brownie: `pip install eth-brownie`
- Ganache (optional) for a local blockchain

### Run the tests

```bash
git clone https://github.com/mosesaderemi/election-dapp.git
cd election-dapp
brownie test
```

The tests cover deployment, adding candidates, opening and closing the election, voting, closing before the deadline and reading the result.

### Deploy

Local development network:

```bash
brownie run scripts/deploy_election.py
```

Public or test network: create a `brownie-config.yaml` that defines `wallets.from_key` (read from an environment variable) and the network settings, store the private key in a `.env` file, and run:

```bash
brownie run scripts/deploy_election.py --network <network-name>
```

Never commit private keys. `.env` is ignored by Git.

## Author

Aderemi Moses Timileyin
GitHub: [@mosesaderemi](https://github.com/mosesaderemi)
LinkedIn: [linkedin.com/in/mosesaderemi-6a70a5405](https://www.linkedin.com/in/mosesaderemi-6a70a5405)

## License

Released under the MIT License. See the `LICENSE` file for details.
