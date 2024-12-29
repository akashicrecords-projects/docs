# 多方式转账

本文档仅描述命令行转账，且仅支持原生资产(目前仅为akc)的转账
命令行转账要求账号本地化保管

## 立即到账转账

```
bin/akashacored tx bank send  \
    <from address> <to address> <amount> \
    --gas=200000 \
    --fees=1000000aakc \
    --chain-id=akchain_9070-1 \
    --home=./data
```

## 延迟到账 - 永不到账

```
bin/akashacored tx vesting create-permanent-locked-account  \ 
    <to address> <amount> \
    --from=<from address>
    --gas=200000 \
    --fees=1000000aakc \
    --chain-id=akchain_9070-1 \
    --home=./data
```

## 延迟到账 - 到期立即到账

```
bin/akashacored tx vesting create-vesting-account  \ 
    <to address> <amount> <end_timeunix timestamp>\
    --delayed=true \
    --from=<from address> \
    --gas=200000 \
    --fees=1000000aakc \
    --chain-id=akchain_9070-1 \
    --home=./data
```

The `end_time` must be provided as a UNIX epoch timestamp 

## 延迟到账 - 到期线性到账

```
bin/akashacored tx vesting create-vesting-account  \ 
    <to address> <amount> <end_time>\
    --from=<from address> \
    --gas=200000 \
    --fees=1000000aakc \
    --chain-id=akchain_9070-1 \
    --home=./data
```

## 延迟到账 - 到期周期到账

```
bin/akashacored tx vesting create-periodic-vesting-account  \ 
    <to address> <periods_json_file> \
    --from=<from address> \
    --gas=200000 \
    --fees=1000000aakc \
    --chain-id=akchain_9070-1 \
    --home=./data

```
