# Eichain节点搭建命令手册

## 验证节点流程
### 1.创建用户
如果当前登录用户是root账号，则建议创建eichain账号，并切换到eichain账号；
/>useradd -m -s /bin/bash eichain
/>su eichain
如果当前登录用户非root账号，则忽略本步骤操作
（本文档以eichain账号举例说明）

### 2. 创建工作目录
/>cd /opt
/>sudo mkdir eichain
/>sudo chown –R eichain:eichain eichain
/>cd eichain
/>mkdir/opt/eichain/bin/opt/eichain/data

### 3. 下载eicored
/>wget https://github.com/eichain-projects/eichain/releases/download/v0.0.2/eichain.tar.gz-o/opt/eichain/bin/eichain.tar.gz
/>cd/opt/eichain/bin/
/>tar zvfx eichain.tar.gz
/>chmod a+x /opt/eichain/bin/eicored

### 4.  初始化数据
/>cd/opt/eichain
/>bin/eicored init MONIKER --chain-id eichain_9071-1  --home=./data
    eichain_9070-1是主网； eichain_9071-1是测试网；eichain_9072-1是研发网
    MONIKER改为所在节点的名字，只能是字母+数字组成

### 5. 下载并修改配置文件
/>wget https://raw.githubusercontent.com/eichain-projects/network-config/main/serenity/genesis.json -O ./data/config/genesis.json
/>wget https://raw.githubusercontent.com/eichain-projects/network-config/main/serenity/client.toml -O ./data/config/client.toml
/>wget https://raw.githubusercontent.com/eichain-projects/network-config/main/serenity/config.toml -O ./data/config/config.toml
/>wget https://raw.githubusercontent.com/eichain-projects/network-config/main/serenity/app.toml -O ./data/config/app.toml

### 6.设置打开文件和进程数量限制
/>sudo vi /etc/security/limits.conf
*       soft    nproc   262144
*       hard    nproc   262144
*       soft    nofile  262144
*       hard    nofile  262144
/>sudo vi /etc/sysctl.conf
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
ExecStart=/opt/eichain/bin/eicored start --home=/opt/eichain/data/ --log_format=json
/>sudo systemctl daemon-reload
/>sudo systemctl enable --now eicored

### 8. 同步数据
#### 8.1 快照方法
#### 8.2 KSYNC
#### 8.3 检查状态
/>curl localhost:26657/status

### 9. 成为验证人节点
#### 9.1 创建验证人账号（以operator名为例）
/>bin/eicored keys add operator --algo secp256k1 --home=./data
/>bin/eicored query bank balances $(bin/eicored keys show operator –a --home=./data) --home=./data

#### 9.2 向该账户转账
必须使用eichain原生网络转账，不能使用evm转账
bin/eicored tx bank send < from address > < to address > xxxaeic -- fees=3000000000000000aeic -- home=./date
举例说明：
//转账
bin/eicored tx bank send eic1qpvcrrlg6nucku5zxm88lxfa0cfnms99elcu5 eic197s5jdgq0tpra3gzypcqp4de3xjfz5sltujkps 500000000000000000000aeic--fees=300000000000000aeic --home=./data
//查余额
bin/eicored query bank balances eic197s5jdgq0tpra3gzypcqp4de3xjfz5sltujkps --home=data

#### 9.3 成为验证人
/>bin/eicored tx staking create-validator \
  --amount=1000000000000000000aeic \
  --pubkey=$(bin/eicored tendermint show-validator --home=./data) \
  --moniker="MY_NODE_NAME" \
  --chain-id=eichain_9070-1 \
  --commission-rate="0.10" \
  --commission-max-rate="0.20" \
  --commission-max-change-rate="0.01" \
  --min-self-delegation="1000000" \
  --gas="600000" \
  --fees=6000000000000000aeis \
  --from=<验证人地址>\
  --home=./data
返回交易hash

#### 9.4 检查上述交易状态
/>bin/eicored  query tx <your transaction hash> --home=./data

#### 9.5 检查验证人是否创建
/> bin/eicored query staking validator $(bin/eicored keys show <验证人地址> --bech val –a --home=./data) --home=./data

#### 9.6 其他用户向节点质押
/>bin/eicored tx staking delegate $(bin/eicored keys show operator --bech val --a --home=./data) 1000000aeic --from=<user> --home=./data

#### 9.7 检查是否在验证列表中
/>bin/eicored query tendermint-validator-set | grep $(bin/eicored tendermint show-address --home=./data) --home=./data

#### 9.8 检查验证人签名状态
/>bin/eicored query slashing signing-info $(bin/eicored tendermint show-validator --home=./data)  --home=./data

#### 9.9 出监狱
/>bin/eicored tx slashing unjail --from=operator --home=./data
