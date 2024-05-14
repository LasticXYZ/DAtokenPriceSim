# Tokenomics Model

### Tokenomics Model Description

#### Background
- **Data Availability Competition**: With data availability (DA) becoming highly affordable and accessible, DA providers are in fierce competition to attract Layer 2 solutions to use their platforms.
- **Current Incentive Mechanisms**: DA providers currently rely on business development teams to incentivize Layer 2 solutions with tokens, a process that is lengthy and expensive.
- **Automating the Process with Lastic**: Lastic aims to streamline and automate this onboarding process, reducing costs and increasing efficiency.

#### Model Overview
This tokenomics model is designed to function from three key perspectives, each with specific expectations and interactions:

1. **Users (Token Holders)**
   - **Participation**: Normal users who hold DA tokens can participate by depositing their `DAtoken` into the DA provider's pool.
   - **Incentive**: In return for locking their `DAtoken`, users receive `LasticDA`, a reward token which is gradually minted over time. When they have enough of `LasticDA` token they can at any time switch back for the `DAtoken`, thus `LasticDA` represents `DAtoken`'s locked liquidity inside the DA pool.
   - **Benefit**: This mechanism incentivizes users to deposit their DA tokens, thereby increasing the pool's value and contributing to the overall ecosystem. In the early stages these users will be especially crucial, especially for jump starting the whole economic game. Thus it's quite important that we design the mechanics in such a way that we incentivize early or long term backers.

2. **DA Providers**
   - **Streamlined Incentives**: Instead of manually negotiating with Layer 2 solutions, DA providers can leverage the automated tokenomics model.
   - **Deposits and Proofs**: DA providers facilitate the process by allowing deposits and managing the generation of `DAtokenProofs`.
   - **Foundation Contributions**: The DA foundation can contribute to the pool by depositing its currency, which in turn burns `DAtokenProofs`, maintaining the system's balance.

3. **Layer 2 Solutions**
   - **Engagement**: Layer 2 solutions connect to the Lastic platform, triggering the minting of `DAtokenProofs` based on their submission duration.
   - **Spam Prevention**: To prevent abuse, the creation of `DAtokenProofs` is based on the length of the Layer 2's engagement, with longer commitments yielding more tokens.
   - **Rewards**: The model ensures that genuine, long-term Layer 2 engagements are rewarded proportionally, encouraging sustainable and meaningful participation.

#### Incentive Mechanics
- **User Deposits and Rewards**: Users deposit `DAtoken` into the DA pool and receive `LasticDA` tokens, minted in proportion to the pool’s value and the number of `DAtokenProofs` generated.
- **Foundation Deposits**: The DA foundation's deposits burn `DAtokenProofs`, ensuring a controlled supply and supporting the overall token economy.
- **Layer 2 Contributions**: Layer 2 solutions’ contributions are incentivized through a tiered system, where longer participation periods result in higher rewards of `DAtokenProofs`.

#### Combining Multiple DA Providers
- **Unified Token**: The `LasticDA` token aggregates the value of `LasticDA` tokens from various DA providers, creating a cohesive and scalable ecosystem.

This tokenomics model aims to automate and optimize the onboarding process for Layer 2 solutions, incentivize user participation, and streamline the interaction between DA providers, foundations, and Layer 2 solutions, ultimately fostering a more efficient and competitive DA marketplace.

### Key Terms
- **DA Provider**: Example - DA1
- **DA Token**: Example - DAtoken
- **Reward Token**: LasticDA
- **Proof Token**: DAtokenProof

### User Flow

1. **Deposit Tokens**
    - Users deposit `DAtoken` into the DA1 pool.
2. **Lock Tokens**
    - The deposited `DAtoken` is locked in the pool.
3. **Mint Reward Tokens**
    - Users receive `LasticDA` in exchange for locking their tokens.
    - `LasticDA` is minted when `DAtokenProofs` are generated.
    - The amount of `LasticDA` minted depends on:
        - The total value in the pool.
        - The quantity of `DAtokenProofs` generated.

```mermaid
flowchart TD
    A[User] -->|Deposits DAtoken| C[DA Token Pool]
    C -->|Tokens Locked| C
    C -->|Mints LasticDA| A
```

### Token Unlock Flow

1. **Burn Reward Tokens**
    - Users burn `LasticDA`.
2. **Unlock Tokens**
    - Users receive their original `DAtoken` from the pool.

```mermaid
flowchart TD
    A[User] -->|Burns LasticDA| C[DA Token Pool]
    C -->|Unlocks DAtoken| A
```

### DA Foundation Flow

1. **Deposit Currency**
    - The DA foundation deposits its currency into the pool.
2. **Burn Proof Tokens**
    - `DAtokenProofs` are burned when the foundation deposits currency.

```mermaid
flowchart TD
    A[Foundation] -->|Deposits Currency| B[DA Token Pool]
    B -->|Burns DAtokenProofs| B
```

### Layer 2 Flow

1. **Mint Proof Tokens**
    - `DAtokenProofs` are minted based on the duration of Layer 2 submissions to the DA provider.
2. **Spam Prevention**
    - To prevent spam, the minting of `DAtokenProofs` considers the submission duration:
        - Short-term submissions yield fewer tokens.
        - Longer submissions yield more tokens (e.g., 2-4 months: 10% of `DAtoken`).

```mermaid
flowchart TD
    A[Layer 2] -->|Connects to Lastic| B[Mints DAtokenProofs]
    B -->|2 Months| C[0% DAtoken]
    B -->|2-4 Months| D[10% of DAtoken]
    B -->|4+ Months| E[30% of DAtoken]
    B -->|1 Year+| F[100%+ of DAtoken]
    C --> G[Layer 2]
    D --> G[Layer 2]
    E --> G[Layer 2]
    F --> G[Layer 2]
```

### Combined View

```mermaid
flowchart TD
    A[User] -->|Locks DAtoken| C1[DA Token Pool]
    C1 -->|LasticDA Minted| A
    D[Foundation] -->|Deposits Currency| C1
    E[Layer 2] -.Triggers Minting .-> F[DAtokenProofs]
    D -.Burns .-> F

    subgraph C1
        C[Pool]
        F
    end
```

### Integration with Multiple DA Providers

The `LasticDA` token represents the aggregate value of `LasticDA` tokens from various DA providers.

## Modeling Dynamics with Python

To model the value of the `LasticDA` token over time:

1. **Linear Growth of DAtoken**: The value of `DAtoken` rises linearly.
2. **User Inflows**: User deposits cause small deviations, appearing as noise that never falls below zero.
3. **Foundation Influence**: The DA foundation's deposits cause step-like increases in the `DAtoken` pool value.

```