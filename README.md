
# STX-HypeCoin - Memecoin Token Contract

This is a smart contract for a **Memecoin** token that includes a set of advanced features such as:

* **Manual Block Height Handling**: The contract includes the ability to track and manipulate the block height for various operations such as transfers, staking, and governance proposals.
* **Transfer Cooldown**: Transfers are subject to a cooldown period, which ensures that users cannot transfer tokens too frequently.
* **Staking Mechanism**: Users can stake tokens with an explicit block height-based lock period.
* **Governance Proposal System**: A governance system allows token holders to propose and vote on decisions with a block height-based voting period.

## Features

* **Token Configuration**:

  * The token is configured with the name `MemeToken` and symbol `MEME`.
  * The total supply is capped at `1,000,000,000` tokens.

* **Transfer Cooldown**:

  * There is a cooldown period of **10 blocks** between transfers for any given sender.

* **Staking**:

  * Users can stake their tokens with a configurable lock period (in blocks).
  * Tokens are not retrievable until the unlock block is reached.

* **Governance**:

  * Token holders can create governance proposals, which are voted on by the community.
  * Proposals are active for a user-defined voting period (in blocks).

## Contract Functions

### 1. `update-block-height`

* **Purpose**: Increments the current block height manually.
* **Usage**: This function is called to update the block height (typically by the contract owner).
* **Returns**: The updated block height.

```clojure
(update-block-height)
```

### 2. `get-block-height`

* **Purpose**: Returns the current block height.
* **Usage**: This is a read-only function to check the current block height.

```clojure
(get-block-height)
```

### 3. `transfer`

* **Purpose**: Transfers tokens from the sender to a recipient, with a cooldown period.
* **Arguments**:

  * `amount (uint)`: The number of tokens to transfer.
  * `recipient (principal)`: The address of the recipient.
* **Cooldown**: Transfers are subject to a **10-block cooldown** between consecutive transfers by the same sender.
* **Returns**: A success message if the transfer is successful.

```clojure
(transfer amount recipient)
```

### 4. `stake-tokens`

* **Purpose**: Allows users to stake tokens with a specific lock period.
* **Arguments**:

  * `amount (uint)`: The number of tokens to stake.
  * `lock-period (uint)`: The number of blocks the tokens will be locked for.
* **Returns**: A success message once tokens are staked.

```clojure
(stake-tokens amount lock-period)
```

### 5. `unstake-tokens`

* **Purpose**: Allows users to unstake tokens after the lock period has passed.
* **Returns**: A success message if the tokens are unstaked and returned to the sender.

```clojure
(unstake-tokens)
```

### 6. `create-governance-proposal`

* **Purpose**: Allows users to create a governance proposal.
* **Arguments**:

  * `description (string-utf8 200)`: A description of the proposal.
  * `voting-period (uint)`: The voting period in blocks.
* **Returns**: The ID of the created proposal.

```clojure
(create-governance-proposal description voting-period)
```

### 7. `vote-on-proposal`

* **Purpose**: Allows users to vote on an active governance proposal.
* **Arguments**:

  * `proposal-id (uint)`: The ID of the proposal.
  * `vote (bool)`: `true` for voting in favor, `false` for voting against.
* **Returns**: A success message if the vote is successfully recorded.

```clojure
(vote-on-proposal proposal-id vote)
```

### 8. `get-governance-proposal`

* **Purpose**: Retrieves information about a specific governance proposal.
* **Arguments**:

  * `proposal-id (uint)`: The ID of the proposal.
* **Returns**: Details about the proposal, including the description, votes for, votes against, and voting deadline.

```clojure
(get-governance-proposal proposal-id)
```

---

## Contract Overview

### Token Configuration

* **Token Name**: `MemeToken`
* **Token Symbol**: `MEME`
* **Total Supply**: `1,000,000,000`
* **Maximum Supply**: `1,000,000,000`
* **Current Block Height**: A variable that can be updated manually by the contract owner to keep track of the blockchain's progression.

### Transfer Cooldown

A **10-block cooldown** is enforced between consecutive transfers by the same sender. This is controlled by the `transfer-last-block` map, which stores the block height of the sender’s last transfer. The cooldown can be adjusted if necessary by modifying the code.

### Staking Mechanism

The staking mechanism allows users to lock tokens for a fixed number of blocks (defined by `lock-period`). Once the lock period has expired (as determined by the `unlock-block`), users can unstake their tokens. The staking information is stored in the `staking-deposits` map, which tracks the amount of tokens staked, the block in which the tokens were staked (`stake-block`), and the unlock block height (`unlock-block`).

### Governance Proposal System

The governance system enables the creation of proposals that token holders can vote on. Each proposal has a voting period, and votes can be cast by token holders. The `governance-proposals` map stores active proposals, including the proposer’s address, description, votes, and voting deadline.

---

## Error Codes

* `ERR-OWNER-ONLY (u100)`: Operation restricted to the contract owner only.
* `ERR-INSUFFICIENT-BALANCE (u101)`: Insufficient balance for transfer.
* `ERR-TRANSFER-COOLDOWN (u102)`: Transfer cooldown period not met.
* `ERR-MAX-SUPPLY-REACHED (u103)`: Attempt to mint more than the maximum supply.
* `ERR-AIRDROP-FAILURE (u104)`: AirDrop failed.
* `ERR-TOKEN-BURN-FAILED (u105)`: Token burn failed.
* `ERR-INVALID-STAKE (u111)`: Invalid staking operation (stake information not found).
* `ERR-UNLOCK-BLOCK-REACHED (u112)`: Unlock block not reached for unstaking.

---
