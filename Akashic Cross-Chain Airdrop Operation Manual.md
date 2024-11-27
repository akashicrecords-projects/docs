**Akashic Cross-Chain Airdrop Operation Manual**

1.  **Introduction**\
    As is well known, users must have AKC assets to pay transaction fees
    for any transactions on the Akashic chain.\
    To encourage cross-chain asset trading activity, users transferring
    assets cross-chain to Akashic for the first time will receive AKC
    airdrops, allowing them to trade further on Akashic.\
    **Principle**: Airdrops are provided **only** for users transferring
    external chain assets to Akashic for the first time.

2.  **Airdrop Management Principles**\
    2.1 Anyone can donate AKC assets to the airdrop contract address for
    airdrops.\
    2.2 Only the **Owner** account can withdraw unused assets.\
    2.3 Only the **Owner** account can set the amount of AKC per
    airdrop.\
    2.4 Only the **Owner** account can designate **Operator** accounts.\
    2.5 Only **Operator** accounts can notify the contract to execute an
    airdrop for a specific account.\
    2.6 Each account can only receive an airdrop **once**.

3.  **Airdrop Service Products**\
    The cross-chain airdrop service consists of two parts: **Smart
    Contract** and **Monitoring Service**.

-   **Smart Contract**:\
    The smart contract manages the AKC assets for airdrops, the amount
    per airdrop, account authorization for eligibility, withdrawal of
    unused AKC assets, and checks to verify whether an address has
    already received an airdrop.\
    Management roles are divided into two types: **Owner** and
    **Operator**.\
    (1) **Owner**: Responsible for staking AKC assets for airdrops,
    setting the amount per airdrop, withdrawing unused AKC assets, and
    authorizing Operators.\
    (2) **Operator**: Responsible for notifying the contract to execute
    an airdrop for specific addresses. Operators are authorized by the
    Owner.\
    (3) By default, the **Owner** and **Operator** share the same
    address, which is the contract deployment address.

-   **Monitoring Service**:\
    The monitoring service tracks inbound cross-chain transactions and
    notifies the smart contract to execute an airdrop for the to address
    whenever a first-time cross-chain transaction is detected.

4.  **Airdrop Management**\
    Airdrop management is supported on PC and mobile devices:

-   **PC**: Enter https://airdrop-testnet.akashicrecords.io/ in a
    browser to open the airdrop interface.

-   **Mobile**: Open the **TP Wallet** app and enter
    https://airdrop-testnet.akashicrecords.io/ to access the airdrop
    interface.

**Note**: Before opening the airdrop interface, log in to MetaMask or TP
Wallet. The following instructions use the **TP Wallet** app as an
example.

**3.1 Viewing the Current Airdrop Service Status**\
3.1.1 The **Owner** opens the **TokenPocket Wallet** app and logs into
the wallet.\
3.1.2 Go to **Discover**, enter
https://airdrop-testnet.akashicrecords.io/, and open the airdrop
interface.

**3.3 Staking AKC Assets for Airdrops**\
3.3.1 The **Owner** opens the **TokenPocket Wallet** app.\
3.3.2 Select the **Assets** page in the wallet and switch to the AKC
wallet.\
3.3.3 Choose **Transfer** → **Direct Transfer** to enter the transfer
page.\
3.3.4 Set the recipient address as the airdrop contract address and
specify the amount of AKC to stake.\
3.3.5 After submitting the transfer, a security confirmation pop-up will
appear. Follow the prompts to continue.\
3.3.6 Confirm the transaction and enter the corresponding transaction
password to complete the transfer.\
3.3.7 Switch to the **Discover** section to view the airdrop status
(refer to Section 3.1).

**3.4 Withdrawing Unused AKC Assets**\
Enter the airdrop page (see Section 3.1), select the **Withdraw**
button, and follow the prompts to withdraw all unused AKC assets from
the contract address back to the **Owner** account.

**3.5 Setting the Amount of AKC Per Airdrop**\
Enter the airdrop page, set the amount of AKC for each airdrop, and
click the **Set** button. Follow the prompts to complete the
configuration.
