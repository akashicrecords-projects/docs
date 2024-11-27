**Akashic Validator Node Setup Manual**

-----
**Node Requirements**

1. **Validator Requirements**
   a. Anyone who stakes no less than 100,000 AKC is eligible to become a validator.
   b. There are 31 Akashic validator nodes, and only the top 31 by staking amount can become actual validators and earn block reward revenue.
1. **Delegator Requirements**
   Anyone can delegate AKC to a validator node.
1. **Hardware Requirements**

3\.1 **Node Specifications**
**Recommendation**: Use NVMe SSDs to avoid falling behind in block production and entering jail.

|**Node Type**|**CPU**|**Memory (GB)**|**Data Disk (GB)**|
| :- | :- | :- | :- |
|Core Validator Node|4 CPUs|16 GB|300+ GB Data Disk|
|Full Node|4 CPUs|12 GB|300+ GB Data Disk|
|Archive Node|4 CPUs|12 GB|3+ TB Data Disk|

3\.2 **Network Requirements**

|**Node Type**|**Port**|**Protocol**|**Public Required**|
| :- | :- | :- | :- |
|Validator Node|26656|TCP|Yes|
|CometBFT|26657|TCP|Optional|
|EVM RPC - HTTP|8545|TCP|Optional|
|EVM RPC - WSS|8546|TCP|Optional|
|Cosmos RPC|1317|TCP|Optional|

3\.3 **Operating System**
Ubuntu Server 24.04 LTS x86\_64

-----
**Validator Node Setup Process**

1. Create User
1. Create Working Directory
1. Download akashicored
1. Initialize Data
1. Synchronize Akashic Network Data
1. Configure Auto-Start Service
1. Set Limits for Open Files and Processes
1. Download and Modify Configuration Files
1. Monitor Network Health Status
1. Manually Upgrade the Akashic Network
1. Become a Validator Node

