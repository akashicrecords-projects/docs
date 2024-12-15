**Deployment Process**

**1. Chain Deployment**

**1.1 Genesis Node**

-   Modify step2.sh to confirm token account allocation

sh step-1.sh

sh step-2.sh

-   Follow the instructions provided

**1.2 Validator Node**

sh node-init.sh

-   Follow the instructions provided

**1.3 Block Production**

-   Run the following on both nodes

sh start.sh

**2. Observer Client**

**2.1 Genesis Node Client**

-   Modify the IP address in client-init.sh, then execute the following:

sh client-init.sh

sh start-client.sh

tail -f nohup.log

-   Wait until the peerID is obtained

**2.2 Validator Node Client**

-   Modify client-init.sh to set the IP and peerID of the validator
    node\'s client in the PEER field

sh client-init.sh

sh start-client.sh

tail -f nohup.log

-   Wait until both clients are connected

Run the following to update the observer key:

bin/akashacored tx observer update-keygen \[Height\] \--from operator
\--fees 20aakc \--home=./data

**3. Deploy Smart Contracts**

-   Deploy the system contracts

-   Deploy ETH/BNB contracts

-   Deploy ETH/BSC chain parameters to Akashic

akashacored tx observer update-chain-params 1 1 \--from operator \--fees
1000aakc \--gas 10000000

akashacored tx observer update-chain-params 56 56 \--from operator
\--fees 1000aakc \--gas 10000000

-   Add USDT to the Akashic whitelist

akashacored tx crosschain whitelist-erc20
0xdAC17F958D2ee523a2206206994597C13D831ec7 1 USDT USDT.ETH 6 100000
\--from operator \--fees 1000aakc \--gas 10000000

akashacored tx crosschain whitelist-erc20
0x55d398326f99059fF775485246999027B3197955 56 USDT USDT.BSC 18 100000
\--from operator \--fees 1000aakc \--gas 10000000

**4. Transfer Assets to Module**

**5. Transfer Assets to Fund**

**6. Other Commands**

bin/akashacored tx fungible remove-foreign-coin
0x65a45c57636f9BcCeD4fe193A602008578BcA90b \--from operator \--fees
20aakc

bin/akashacored tx crosschain abort-stuck-cctx
0x7e8f0fdb62b1d86ea236c8caa0953809327ddfe22dacd3b3c727858ad58a9c44
\--from operator \--fees 20aakc

**7. Testing**

-   Transfer ETH from Ethereum to Akashic

-   Transfer ETH from Akashic to Ethereum

-   Transfer USDT from Ethereum to Akashic

-   Transfer USDT from Akashic to Ethereum

-   Transfer BNB from BSC to Akashic

-   Transfer BNB from Akashic to BSC

-   Transfer USDT from BSC to Akashic

-   Transfer USDT from Akashic to BSC

\# Deployment Process

\## 1. Chain Deployment

\### 1.1 Genesis Node

\- Modify step2.sh to confirm token account allocations

\`\`\`

sh step-1.sh

sh step-2.sh

\`\`\`

\- Follow the instructions provided.

\### 1.2 Validator Node

\`\`\`

sh node-init.sh

\`\`\`

\- Follow the instructions provided

\### 1.3 Block Production

\- Run the following command on both nodes.

\`\`\`

sh start.sh

\`\`\`

\## 2. Observer Client

\### 2.1 Genesis Node Client

\- Modify the IP address in client-init.sh, then execute.

\`\`\`

sh client-init.sh

sh start-client.sh

tail -f nohup.log

\`\`\`

\- Wait until the peerID is retrieved

\### 2.2 Validator Node Client

\- Modify client-init.sh to set the IP and peerID of the validator node
client in the PEER field

\`\`\`

sh client-init.sh

sh start-client.sh

tail -f nohup.log

\`\`\`

\- Wait until both clients are connected

bin/akashacored tx observer update-keygen \[Height\] \--from operator
\--fees 20aakc \--home=./data

\## 3. Deploy Smart Contracts

\- Deploy the system contracts.

\- Deploy ETH/BNB contracts

\- Deploy ETH/BSC chain parameters to Akashic

\`\`\`

akashacored tx observer update-chain-params 1 1 \--from operator \--fees
1000aakc \--gas 10000000

akashacored tx observer update-chain-params 56 56 \--from operator
\--fees 1000aakc \--gas 10000000

\`\`\`

\- Add USDT to the Akashic whitelist

\`\`\`

akashacored tx crosschain whitelist-erc20
0xdAC17F958D2ee523a2206206994597C13D831ec7 1 USDT USDT.ETH 6 100000
\--from operator \--fees 1000aakc \--gas 10000000

akashacored tx crosschain whitelist-erc20
0x55d398326f99059fF775485246999027B3197955 56 USDT USDT.BSC 18 100000
\--from operator \--fees 1000aakc \--gas 10000000

\`\`\`

\## 4. Transfer Assets to Module

\## 5. Transfer Assets to Fund

\## 6. Other Commands

\`\`\`

bin/akashacored tx fungible remove-foreign-coin
0x65a45c57636f9BcCeD4fe193A602008578BcA90b \--from operator \--fees
20aakc

bin/akashacored tx crosschain abort-stuck-cctx
0x7e8f0fdb62b1d86ea236c8caa0953809327ddfe22dacd3b3c727858ad58a9c44
\--from operator \--fees 20aakc

\`\`\`

\## 7. Testing

\- Transfer ETH from Ethereum to Akashic

\- Transfer ETH from Akashic to Ethereum

\- Transfer USDT from Ethereum to Akashic

\- Transfer USDT from Akashic to Ethereum

\- Transfer BNB from BSC to Akashic

\- Transfer BNB from Akashic to BSC

\- Transfer USDT from BSC to Akashic

\- Transfer USDT from Akashic to BSC
