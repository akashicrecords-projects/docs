# 部署流程

## 1. chain部署
### 1.1 创世节点
- 修改 step2.sh 确定代币账号分配
```
sh step-1.sh
sh step-2.sh
```
- 按照提示操作

 ### 1.2 验证节点
```
sh node-init.sh
```
- 按照提示操作


### 1.3 出块
- 两个节点都运行
```
sh start.sh
```


## 2. observer client
### 2.1 创世节点的client
- 修改 client-init.sh 里的ip地址。然后执行
```
sh client-init.sh
sh start-client.sh
tail -f nohup.log
```
- 等待获取到peerID

### 2.2 验证节点client
- 修改 client-init.sh 将验证节点的client的ip和peerid填写到PEER字段
```
sh client-init.sh
sh start-client.sh
tail -f nohup.log
```
- 等待两个client连到一起

bin/akashacored tx observer update-keygen [Height] --from operator --fees 20aakc --home=./data


## 3. 部署合约
- 部署系统合约
- 部署eth/bnb合约
- 部署eth/bsc链参数到akashic
```
akashacored tx observer update-chain-params 1 1  --from operator --fees 1000aakc --gas 10000000
akashacored tx observer update-chain-params 56 56 --from operator --fees 1000aakc --gas 10000000
```
- 部署usdt到akashic白名单
```
akashacored tx crosschain whitelist-erc20 0xdAC17F958D2ee523a2206206994597C13D831ec7 1 USDT USDT.ETH 6 100000 --from operator --fees 1000aakc --gas 10000000

akashacored tx crosschain whitelist-erc20 0x55d398326f99059fF775485246999027B3197955 56 USDT USDT.BSC 18 100000 --from operator --fees 1000aakc --gas 10000000
```


## 4. 转移资产到module

## 5. 转移资产到fund

## 6. 其他命令
```
bin/akashacored tx fungible remove-foreign-coin  0x65a45c57636f9BcCeD4fe193A602008578BcA90b --from operator --fees 20aakc
bin/akashacored tx crosschain abort-stuck-cctx  0x7e8f0fdb62b1d86ea236c8caa0953809327ddfe22dacd3b3c727858ad58a9c44 --from operator --fees 20aakc
```
## 7. 测试
- eth从ethereum跨到akashic
- eth从akashic跨到ethereum
- usdt从ethereum跨到akashic
- usdt从akashic跨到ethereum
- bnb从bsc跨到akashic
- bnb从akashic跨到bsc
- usdt从bsc跨到akashic
- usdt从akashic跨到bsc
