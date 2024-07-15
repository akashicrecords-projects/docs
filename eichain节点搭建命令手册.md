# Eichain节点搭建命令手册

## 观察节点搭建
### 1.创建用户
如果当前登录用户是root账号，则建议创建eichain账号，并切换到eichain账号；如果当前登录用户非root账号，则忽略本步骤操作  
  
    />useradd -m -s /bin/bash eichain  
    />su eichain  
  
(本文档以eichain账号为例)  
  
### 2. 创建工作目录
    />cd /opt  
    />sudo mkdir eichain  
    />sudo chown –R eichain:eichain eichain  
    />cd eichain  
    />mkdir /opt/eichain/bin /opt/eichain/data  
  
### 3. 下载可执行文件
    />cd /opt/eichain/bin/ 
    />wget https://github.com/eichain-projects/eichain/releases/download/v0.0.2/eichain.tar.gz-o/opt/eichain/bin/eichain.tar.gz  
    />tar zvfx eichain.tar.gz  
    />chmod a+x /opt/eichain/bin/eicored  
  
### 4.  初始化链
    />cd  /opt/eichain  
    />bin/eicored init <MONIKER> --chain-id eichain_9071-1 --home=./data  
    
| 网络类型 | ChainId |  
|:-------:|:------:|  
| mainnet | eichain_9070-1 |  
|testnet | eichain_9071-1 |  
| mocknet | eichain_19070-1 |  
  
**MONIKER改为所在节点名字，只能是"字母+数字"组成**  
  
### 5. 下载配置文件
    />wget https://raw.githubusercontent.com/eichain-projects/network-config/main/serenity/genesis.json -O ./data/config/genesis.json  
    />wget https://raw.githubusercontent.com/eichain-projects/network-config/main/serenity/client.toml -O ./data/config/client.toml  
    />wget https://raw.githubusercontent.com/eichain-projects/network-config/main/serenity/config.toml -O ./data/config/config.toml  
    />wget https://raw.githubusercontent.com/eichain-projects/network-config/main/serenity/app.toml -O ./data/config/app.toml  
  
### 6.设置打开文件和进程的数量限制
    />sudo vi /etc/security/limits.conf  
    
    文件末尾添加：  
  
    *       soft    nproc   262144
    *       hard    nproc   262144
    *       soft    nofile  262144
    *       hard    nofile  262144  
    
    />sudo vi /etc/sysctl.conf  
    
    文件末尾添加：  

    fs.file-max=262144  
  
### 7.配置自动启动服务
    />sudo vi /etc/systemd/system/eicored.service  
    
    [Unit]  
    Description=eicored  
    After=multi-user.target  
    StartLimitIntervalSec=0  
    [Install]  
    WantedBy=multi-user.target  
    [Service]  
    WorkingDirectory=/opt/eichain  
    Type=simple  
    Restart=always  
    RestartSec=600  
    User=eichain  
    LimitNOFILE=262144  
    ExecStart=/opt/eichain/bin/eicored start --home=/opt/eichain/data/  
   
    />sudo systemctl daemon-reload  
    />sudo systemctl enable --now eicored  
  
### 8. 同步数据
检查状态  

    />curl localhost:26657/status  
    
返回值中，如果"catching_up":false 表示正在同步； 如果"catching_up":false 表示同步完成  
  
## 成为验证节点
### 9. 验证人拥有eic资产
#### 9.1 创建验证人账号(以operator名为例)  

     />bin/eicored keys add operator --algo eth_secp256k1 --home=./data  
  
#### 9.2 向验证人转账

    />bin/eicored tx bank send < from address > < to address > < amount > --fees=3000000000000000aeic --home=./date  
举例说明：  

    //转账(以转移500eic为例)  
    />bin/eicored tx bank send eic1qpvcrrlg6nucku5zxm88lxfa0cfnms99elcu5 eic197s5jdgq0tpra3gzypcqp4de3xjfz5sltujkps 500000000000000000000aeic--fees=300000000000000aeic --home=./data  
    //查余额  
    />bin/eicored query bank balances eic197s5jdgq0tpra3gzypcqp4de3xjfz5sltujkps --home=./data  
  
#### 9.3 成为验证人
    />bin/eicored tx staking create-validator \
    --amount=< 质押数量 > \
    --pubkey=$(bin/eicored tendermint show-validator --home=./data) \
    --moniker="MY_NODE_NAME" \
    --chain-id=<chain_id> \
    --commission-rate="0.10" \
    --commission-max-rate="0.20" \
    --commission-max-change-rate="0.01" \
    --min-self-delegation="1000000" \
    --gas="600000" \
    --fees=<手续费> \
    --from=<验证人地址>\
    --home=./data  

举例说明：  

    />bin/eicored tx staking create-validator \
    --amount=10000000000000000000000aeic \
    --pubkey=$(bin/eicored tendermint show-validator --home=./data) \
    --moniker="eichain" \
    --chain-id=eichain_9070-1 \
    --commission-rate="0.10" \
    --commission-max-rate="0.20" \
    --commission-max-change-rate="0.01" \
    --min-self-delegation="1000000" \
    --gas="600000" \
    --fees=6000000000000000aeis \
    --from=eic197s5jdgq0tpra3gzypcqp4de3xjfz5sltujkps\
    --home=./data  
    
返回交易hash  

#### 9.4 检查状态
检查成为验证人交易是否成功  

    />bin/eicored query tx <your transaction hash> --home=./data
  
检查验证人是否创建

    />bin/eicored query staking validator $(bin/eicored keys show <验证人地址> --bech val -a --home=./data) --home=./data  

检查节点是否在验证列表中

    />bin/eicored query tendermint-validator-set | grep $(bin/eicored tendermint show-address --home=./data) --home=./data  
  
检查验证人签名状态  

    />bin/eicored query slashing signing-info $(bin/eicored tendermint show-validator --home=./data) --home=./data  
  
### 9.5 节点出狱
如果节点进入监狱，则执行命令出监狱  

    />bin/eicored tx slashing unjail --from=<验证人地址> --home=./data

#### 9.6 其他用户向节点质押EIC  

    />bin/eicored tx staking delegate $(bin/eicored keys show operator --bech val -a --home=./data) < amount > --from=<user> --home=./data
  
