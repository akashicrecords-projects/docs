# Akashic Observation Node Setup Manual

Node Requirements\
\
1. Hardware Requirements\
\
1.1 Node Specifications\
- Node Type: Full Node\
- CPU: 4 CPUs\
- Memory (GB): 12 GB\
- Data Disk (GB): 300+ GB Data Disk\
\
1.2 Network Requirements\
- Node Type: Validator Node\
- Port: 26656 (TCP), must be public.\
- CometBFT: Port 26657 (TCP), optional.\
- EVM RPC - HTTP: Port 8545 (TCP), optional.\
- EVM RPC - WSS: Port 8546 (TCP), optional.\
- Cosmos RPC: Port 1317 (TCP), optional.\
\
1.3 Operating System\
- Ubuntu Server 24.04 LTS (x86_64)\
\
Observation Node Process\
\
1. Create User\
2. Create Working Directory\
3. Download \`akashicored\`\
4. Initialize Data\
5. Download and Modify Configuration Files\
6. Set File and Process Limits\
7. Configure Auto-Start Service\
8. Synchronize Akashic Network Data\
9. Monitor Network Health Status\
10. Manual Upgrades of the Akashic Network
