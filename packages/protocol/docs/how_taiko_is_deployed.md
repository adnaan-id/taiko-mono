# How Taiko is deployed

The Taiko protocol smart contracts are deployed on L1 and L2. The L2 contracts are pre-deployed first by creating a genesis block, and then the L1 contracts are deployed using a script. The general flow is like this:

1. A `genesis.json` is generated, which includes the L2 contracts (see: [generate genesis](../utils/generate_genesis/main.ts)).
2. The `genesis.json` is used as input to generate the genesis block (see: https://geth.ethereum.org/docs/fundamentals/private-network#creating-genesis-block).
TL;DR:
- **Creating the Genesis Block**:
  - Every blockchain begins with a genesis block, typically configured with specific parameters.
  - Parameters include enabling Ethereum features, setting initial block gas limit, and allocating initial ether.
  - Customizing the genesis block involves creating a genesis.json file with desired configurations.

- **Configurations for Different Consensus Mechanisms**:
  - For a Proof of Authority (PoA) network, the genesis.json file configures the 'clique' engine for consensus.
  - Ethash, the default consensus algorithm, requires fewer configurations since it's used primarily in public Ethereum networks.
  - Each consensus mechanism requires specific parameters for initialization and operation.

- **Network Setup and Node Initialization**:
  - Initializing Geth nodes involves importing the genesis block and configuring the network.
  - Setting up a peer-to-peer network includes designating a bootstrap node for network entry.
  - Member nodes can be connected to the network using the bootstrap node, and additional configurations are needed for mining and signing blocks based on the chosen consensus mechanism.
3. The L1 smart contracts are deployed by executing the L1 deployment script, [DeployOnL1.s.sol](../script/DeployOnL1.s.sol). The L1 deployment script takes in artifacts from the L2 deployment such as the deployed contract addresses, and genesis block hash.
