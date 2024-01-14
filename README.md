# StakeAcross 

CCIP cross chain staking protocol

## Use Case Description

### Smart Contracts Overview

- **StakeAcrossSender.sol**: This is the Sender Contract deployed on Fuji, the source chain.
- **StakeAcrossVaultProtocol**: Operating on Sepolia, the destination chain, this contract is a key component of the protocol.

The `StakeAcrossVaultProtocol` is an extension of both the `CrossChainReceiver` Contract and the `ERC4626Vault` Contract. It leverages the functionality of `CrossChainReceiver` for sending and receiving messages and tokens across blockchain networks, while also incorporating the features of the `ERC4626Vault` contract, which adheres to the ERC4626 standard. The ERC4626 standard represents a tokenized vault framework, where ERC20 tokens are used to signify shares in an asset pool.

> **Note:** In this example, we use the **CCIP-BnM** token for sending and receiving via CCIP, as it is one of the two available tokens on the testnet for testing purposes. For more information, please refer to [CCIP Test Tokens](https://docs.chain.link/ccip/supported-networks/v1_2_0/testnet).

> **Note:** The **CCIP-BnM** token serves as a placeholder for any other ERC20 token that we might want to use on the mainnet in the future. To find out which tokens are currently available on various mainnet networks and how to add your own, check out this resource:  [CCIP Mainnet Tokens](https://docs.chain.link/ccip/supported-networks/v1_0_0/mainnet).

### Workflow

![StakesAcross-CCIP-Workflow](https://github.com/Stake-Across/ccip-staking-protocol/assets/330947/c771de4a-e3e0-4573-bc0d-a0beb990823d)


1. **Token Deposit**: A DeFi user deposits tokens into the `StakeAcrossSender` contract on the source chain.
2. **Cross-Chain Transfer**: The user then employs [Chainlink CCIP](https://docs.chain.link/ccip) to transfer these tokens, along with accompanying message data, to the `StakeAcrossVaultProtocol` on the destination chain.
3. **Share Minting and Transfer**: The `StakeAcrossVaultProtocol` mints shares equivalent to the deposited token value and transfers these shares to the user's account on the destination chain.
4. **Asset Redemption**: At a subsequent time, the user can redeem these token shares through the `StakeAcrossVaultProtocol` contract. This enables the user to reclaim the original asset tokens on the source chain via CCIP.
5. **Yield Benefits**: If the asset tokens in the vault have appreciated (due to a successful investment strategy), the user can withdraw a greater amount of asset tokens than the initial deposit, benefiting from the yield generated by the protocol.

This use case demonstrates the integration of cross-chain capabilities with yield-generating strategies in DeFi, utilizing the robustness of Chainlink's CCIP and the efficiency of the ERC4626 standard for a comprehensive DeFi solution.

Chainlink CCIP fees are paid using LINK tokens. They can also be paid in the [chain's native token](https://documentation-private-git-ccip-documentation-chainlinklabs.vercel.app/ccip/architecture#ccip-billing) but in this example we pay CCIP fees in LINK.

## Stake Across


### Buy and Stake $Coq Across multiple blockchains


#### Context

The $Coq Token is available on a limited number of Decentralized Exchanges (Dexs) and Centralized Exchanges (Cexs), but this can potentially restrict new users from easily acquiring $Coq tokens. We believe that bridges serve as a common method for transferring tokens across different blockchain networks. However, using bridges carries inherent risks, as they are often vulnerable to hacking attempts, resulting in the depletion of liquidity. Constant efforts are being made to explore alternative options and establish secure methods for bridging tokens and messages between blockchain networks.

#### Proposed Solution

An innovative approach to enhance liquidity for $Coq across various blockchain networks involves leveraging the Cross Chain Interoperability Protocol (CCIP). With CCIP, users gain the ability to send messages, transfer tokens, and trigger actions seamlessly across different blockchains.


#### How it works? 

This protocol utilizes Chainlink's Cross Chain Interoperability Protocol (CCIP) technology, enabling users to purchase and/or stake $Coq across various blockchain networks. The CCIP burn and mint mechanism facilitate this process. By simultaneously burning the token on the source chain and minting an equivalent token on the destination chain, the protocol seamlessly transfers assets between different blockchains. The same method can be employed to transfer tokens back and forth between either chain.
CCIP offers audited token pool contracts that manage the intricacies of burning/minting or locking/minting tokens across chains. Importantly, token sponsors retain complete control over their token pool contract while leveraging CCIP.
The well-established reputation of Chainlink in the realm of oracles positions CCIP as a technology with high adoption potential. In contrast to various blockchain bridges, CCIP is an infrastructure designed for integration within applications. Its primary focus is not the end user, hence a user-friendly graphical interface from Chainlink is not provided.
While CCIP technology is in its early stages and evolving daily, it currently supports only a limited number of blockchains and tokens. Testing on the supported testnets is possible.
For $Coq to be supported by CCIP, the project team must engage with the Chainlink team, obtain approval, and provide token liquidity to the CCIP protocol.
Our protocol enables users across multiple chains to buy, hold, stake, and secure $Coq. Stake Across allows users to buy, stake, and receive staking rewards. This initiative contributes to the growth of the Coq Inu community across the broader blockchain ecosystem, offering users the seamless ability to purchase, stake, receive rewards, and transfer $Coq across different networks.


## Project Setup Guide

### Overview

This project leverages [Hardhat tasks](https://hardhat.org/hardhat-runner/docs/guides/tasks-and-scripts) for streamlined operations. This is because we interact directly with the CCIP contracts deployed on the testnet. The task files are conveniently named with sequential number prefixes, indicating the order in which they should be executed for this specific use case.

#### Initial Steps

1. **Clone the Repository**: Start by cloning the project repository to your local machine.
2. **Install Dependencies**: Run `npm install` in your project directory to install necessary dependencies.

### Preparing the Chains

#### Source Chain: Fuji

For the source chain, where `StakeAcrossSender.sol` is deployed, you will need:

- **LINK Tokens**: Acquire LINK tokens for the Fuji chain. Detailed instructions can be found [here](https://docs.chain.link/resources/link-token-contracts).
- **CCIP-BnM Tokens**: Obtain Burn & Mint Tokens for the Fuji chain using the `drip()` function. More information is available [here](https://docs.chain.link/ccip/test-tokens#mint-test-tokens).
- **Fuji AVAX**: A small amount of Fuji AVAX is required, which can be obtained from [this faucet](https://faucets.chain.link/fuji).

#### Destination Chain: Sepolia

For the destination chain, where `StakeAcrossVaultProtocol` is deployed, you will need:

- **LINK Tokens**: Similar to the Fuji chain, acquire LINK tokens for Sepolia. Ensure you switch networks and interact with the correct LINK token contract. The same URL as above can be used.
- **Sepolia ETH**: A small amount of Sepolia ETH is necessary, available from [this faucet](https://faucets.chain.link/sepolia).

### Configuration Details

- **Network Configurations**: The `./networks.js` file contains all the configuration details. This file exports configurations used by the tasks in `./tasks`.

### Managing Environment Variables

For enhanced security:

- **Avoid Human-Readable Storage**: We advise against storing environment variables in a `.env` file or any human-readable format.
- **Encrypted Environment Variables**: Utilize the [@chainlink/env-enc NPM package](https://www.npmjs.com/package/@chainlink/env-enc) for encryption.
- **Required Variables**:
  - `PRIVATE_KEY1`: Your development wallet's private key for deployment.
  - `PRIVATE_KEY2`: Additional account to test deposits from a different one.
  - `SEPOLIA_RPC_URL`: The JSON-RPC URL from services like Alchemy or Infura for Sepolia.
  - `AVALANCHE_FUJI_RPC_URL`: The RPC URL for the Avalanche Fuji test network, typically `https://api.avax-test.network/ext/bc/C/rpc`.

## Executing the Use Case Steps

This guide outlines the process of deploying and interacting with the `StakeAcrossSender.sol` and `StakeAcrossVaultProtocol` contracts for our use case, using the Avalanche Fuji C Chain as the source chain and Sepolia as the destination chain.

### Deployment and Interaction Process

### Step 1: Deploy and Fund Sender on Fuji

- **Task**: Deploy the `StakeAcrossSender.sol` contract to the Avalanche Fuji C Chain.
- **Command**: `npx hardhat setup-sender --network fuji`
- **Note**: Record the contract address from the console output. This step also funds your contract if your environment variables are correctly set up.

### Step 2: Deploy & Fund Protocol on Sepolia

- **Task**: Deploy the `StakeAcrossVaultProtocol` contract to Sepolia.
- **Command**: `npx hardhat setup-protocol --network sepolia`
- **Note**: Take note of the contract address. `StakeAcrossVaultProtocol` also manages the **vSTA** ERC20 tokens that represent Platform Shares. With this address the tokens can be imported into the user's wallet

### Step 3: Transfer Tokens and Data from Fuji to Sepolia

- **Task**: Send  tokens and data from `StakeAcrossSender.sol` to `StakeAcrossVaultProtocol`. Amount in "wei" units - i.e. 500000000000000000 = 0.5 CCIP-BnM tokens.
- **Command**:
  ```
  npx hardhat transfer-token
  --network fuji
  --sender <<Sender Contract Address on Fuji>>
  --protocol <<Protocol Contract Address on Sepolia>>
  --dest-chain sepolia \                // Sepolia destination chain selector
  --amount 500000000000000000           // Units of BnM
  ```
- **Note**: Record the Source Tx Hash from your console. The transfer can take between 5 to 15 minutes due to the cross-chain nature of CCIP.

### Step 4: Verify Message Receipt on Destination Chain

- **Task**: Check if the message has been successfully received on Sepolia.
- **Tools**: Use the [CCIP explorer page](https://ccip.chain.link) to monitor the transaction status. Wait for it to show "Success".
- **Note**: Record the message Id when successful.

### Step 5: Read Protocol Contract Balances

- **Command**:
  ```
  npx hardhat read-protocol \
  --network <<network>> \
  --address <<Protocol contract address>>
  --user <<user address>>
  ```
  - **Note**: This task will provide the following information: **Total Assets** deposited in the protocol, **Total Share**s minted, **Number of vSTA Shares** allocated to the user, and **Preview** the current value of Assets that the user is entitled to if they redeem all their Shares. This amount varies based on the outcomes of the investment strategy."
  ```
  Vault Total Assets: 1.32
  Vault Total Supply: 1.1636

  User vSTA Balance: 0.8
  Preview Redeem for User: 0.9075
  ```

### Step 6: Redeem Shares from Protocol Contract

- **Task**: Redeem shares for equivalent assets, which are then transferred back to the user's EOA on the source chain via CCIP.
- **Command**:
  ```
  npx hardhat redeem-protocol \
  --network <<network>> \
  --address <<Protocol contract address>> \
  --dest-chain <<Sender contract address>> \
  --receiver <<Reciever EOA address>> \
  --amount 800000000000000000  // amount of shares to redeem
  ```

### Steps 7 and 8: Withdraw Funds
- **Note**: These tasks, which are optional and only relevant during testing, allow us to retrieve the remaining balances of the various tokens used to fund contracts for testing purposes, enabling their reuse in subsequent tests.
- **Withdraw from Sender**:

```
  `npx hardhat withdraw-sender-funds --network fuji --address <<Sender contract address on Fuji>>`
```

- **Withdraw from Protocol**:

```
  `npx hardhat withdraw-protocol-funds --network sepolia --address <<Protocol address on Sepolia>>`
```

## Additional Information

Each step in this process is a Hardhat Task, located in separate, sequentially numbered files in `./tasks`. Follow the sequence and pay attention to the console outputs for a smooth execution of the use case.


# Project Roadmap: TODO List

As part of our ongoing efforts to enhance and optimize our project, we have identified key areas for improvement and expansion. The following items are prioritized in our development roadmap:

1. **Development of Dynamic NFT System**:
   - We plan to create a dynamic Non-Fungible Token (NFT) system. This system will track and represent each user's reputation, investment history, and earnings, serving as a unique digital asset that reflects the user's engagement and success within the platform.

2. **Strategy Protocol Deployment**:
   - We aim to deploy the Protocol on a blockchain network that strikes an optimal balance between cost-efficiency and reliability. The chosen network should minimize transaction costs while maintaining high standards of performance and security.

3. **Expanding Sender Contract Deployment**:
   - To enhance the protocol’s reach and interoperability, we will deploy the `Sender` contract across all Ethereum Virtual Machine (EVM) compatible networks supported by Chainlink's Cross-Chain Interoperability Protocol (CCIP). This expansion will significantly increase the protocol's accessibility across diverse blockchain ecosystems.

4. **Best Practices**:
   - Complete the addition of the recommended CCIP Best Practices, particularly those related to security: [CCIP Best Practices](https://docs.chain.link/ccip/best-practices)

These initiatives are integral to the project's advancement and will be actively pursued in our development cycle. We look forward to updating our community as we achieve these milestones.
