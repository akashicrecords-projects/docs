# Akashichain节点搭建命令手册

## 观察节点搭建
### 1.创建用户
如果当前登录用户是root账号，则建议创建akashichain账号，并切换到akashichain账号；如果当前登录用户非root账号，则忽略本步骤操作  
/>useradd -m -s /bin/bash akashichain  
/>su akashichain  
  
(本文档以akashichain账号为例)  
  
### 2. 创建工作目录
/>cd /opt  
/>sudo mkdir akashichain  
/>sudo chown –R akashichain:akashichain akashichain  
/>cd akashichain  
/>mkdir /opt/akashichain/bin /opt/akashichain/data  
  
### 3. 下载可执行文件
/>cd /opt/akashichain/bin/ 
/>wget https://github.com/akashicrecords-projects/akchain/releases/download/v2.0.3/akashacored-linux-amd64    
/>mv akashacored-linux-amd64 akashicored
/>chmod a+x /opt/akashichain/bin/akashicored  
  
### 4.  初始化链
/>cd  /opt/akashichain  
/>bin/akashicored init *MONIKER* --chain-id=*id* --home=./data  
    id: mainnet - akchain_9070-1； testnet - akchain_9071-1； mocknet - akchain_19070-1;  
    MONIKER改为所在节点名字，只能是"字母+数字"组成  
  
### 5. 下载配置文件
配置文件在https://github.com/akashicrecords-projects/network-config/blob/main/mainnet目录
  
### 6.设置打开文件和进程的数量限制
/>sudo vi /etc/security/limits.conf  
文件末尾添加：  <br>
```
\*       soft    nproc   262144
\*       hard    nproc   262144
\*       soft    nofile  262144
\*       hard    nofile  262144
```
/>sudo vi /etc/sysctl.conf  
文件末尾添加：  <br>
```
fs.file-max=262144
```
  
### 7.配置自动启动服务
/>sudo vi /etc/systemd/system/akashicored.service  
[Unit]  
Description=akashicored  
After=multi-user.target  
StartLimitIntervalSec=0  
[Install]  
WantedBy=multi-user.target  
[Service]  
WorkingDirectory=/opt/akashichain  
Type=simple  
Restart=always  
RestartSec=600  
User=akashichain  
LimitNOFILE=262144  
ExecStart=/opt/akashichain/bin/akashicored start --home=/opt/akashichain/data/  
/>sudo systemctl daemon-reload  
/>sudo systemctl enable --now akashicored  

### 8. 同步数据
检查状态  
/>curl localhost:26657/status  
返回值中，如果"catching_up":false 表示正在同步； 如果"catching_up":false 表示同步完成  
  
## 成为验证节点
### 9. 验证人拥有AKC资产
#### 9.1 创建验证人账号(以operator名为例)  
/>bin/akashicored keys add operator --algo eth_secp256k1 --home=./data  
  
#### 9.2 向验证人转账
/>bin/akashicored tx bank send < from address > < to address > < amount > --fees=3000000000000000aakc --home=./date  
举例说明：  
//转账(以转移500akc为例)  
/>bin/akashicored tx bank send aksha1qpvcrrlg6nucku5zxm88lxfa0cfnms99elcu5 akasha197s5jdgq0tpra3gzypcqp4de3xjfz5sltujkps 500000000000000000000aakc--fees=300000000000000aakc --home=./data  
//查余额  
/>bin/akashicored query bank balances akasha197s5jdgq0tpra3gzypcqp4de3xjfz5sltujkps --home=./data  
  
#### 9.3 成为验证人
```
/>bin/akashicored tx staking create-validator \
  --amount=< 质押数量 > \
  --pubkey=$(bin/akashicored tendermint show-validator --home=./data) \
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
```
举例说明：  
```
/>bin/akashicored tx staking create-validator \
  --amount=10000000000000000000000aakc \
  --pubkey=$(bin/akashicored tendermint show-validator --home=./data) \
  --moniker="akashichain" \
  --chain-id=akashichain_9070-1 \
  --commission-rate="0.10" \
  --commission-max-rate="0.20" \
  --commission-max-change-rate="0.01" \
  --min-self-delegation="1000000" \
  --gas="600000" \
  --fees=6000000000000000aeis \
  --from=akasha197s5jdgq0tpra3gzypcqp4de3xjfz5sltujkps\
  --home=./data 
```  
返回交易hash  

#### 9.4 检查状态
检查成为验证人交易是否成功  
```
/>bin/akashicored query tx <your transaction hash> --home=./data
```  
检查验证人是否创建  
```
/>bin/akashicored query staking validator $(bin/akashicored keys show <验证人地址> --bech val -a --home=./data) --home=./data  
```
检查节点是否在验证列表中  
```
/>bin/akashicored query tendermint-validator-set | grep $(bin/akashicored tendermint show-address --home=./data) --home=./data  
```  
检查验证人签名状态  
```
/>bin/akashicored query slashing signing-info $(bin/akashicored tendermint show-validator --home=./data) --home=./data  
```  
### 9.5 节点出狱
如果节点进入监狱，则执行命令出监狱  
```
/>bin/akashicored tx slashing unjail --from=<验证人地址> --home=./data
```
#### 9.6 其他用户向节点质押AKC
```
/>bin/akashicored tx staking delegate $(bin/akashicored keys show operator --bech val -a --home=./data) < amount > --from=<user> --home=./data
```
  
说明： 以上命令均可通过hub UI操作完成

