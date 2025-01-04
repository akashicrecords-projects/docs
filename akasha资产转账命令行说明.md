# 多方式转账

本文档仅描述命令行转账，且仅支持原生资产(目前仅为akc)的转账<br>
命令行转账要求账号本地化保管

## 立即到账转账

```
bin/akashacored tx bank send \
    <from address> <to address> <amount> \
    --gas=200000 \
    --fees=100aakc \
    --chain-id=akchain_9070-1 \
    --home=./data
```
例如:<br>
```
/>bin/akashacored tx bank send \
    akasha18rl6mfx3lap3fkn5alwxdwxcmhfcalj7t9t8u2 akasha1kkk8j83up5g47285d8ckeur88ux5y2evtdlgvh 10akc \
    --gas=200000 \
    --fees=100aakc \
    --chain-id=akchain_9070-1 \
    --home=./data

/>bin/akashacored q bank balances akasha1kkk8j83up5g47285d8ckeur88ux5y2evtdlgvh --home=./data
balances:
- amount: "10000000000000000000"
  denom: aakc
pagination:
  next_key: null
  total: "0"
```

## 延迟到账 - 永不到账

```
bin/akashacored tx vesting create-permanent-locked-account \
    <to address> <amount> \
    --from=<from address> \
    --gas=200000 \
    --fees=100aakc \
    --chain-id=akchain_9070-1 \
    --home=./data
```
例如:<br>
```
/>bin/akashacored tx vesting create-permanent-locked-account \
    akasha1xpg30e6pvl42j5xcr2m68e9g57kh5d2mffnjcp 10akc \
    --from=akasha18rl6mfx3lap3fkn5alwxdwxcmhfcalj7t9t8u2 \
    --gas=200000 \
    --fees=100aakc \
    --chain-id=akchain_9070-1 \
    --home=./data

/>bin/akashacored q bank balances akasha1xpg30e6pvl42j5xcr2m68e9g57kh5d2mffnjcp --home=./data
balances:
- amount: "10000000000000000000"
  denom: aakc
pagination:
  next_key: null
  total: "0"

/>bin/akashacored q bank spendable-balances akasha1xpg30e6pvl42j5xcr2m68e9g57kh5d2mffnjcp --home=./data
balances:
- amount: "0"
  denom: aakc
pagination:
  next_key: null
  total: "0"
```

## 延迟到账 - 到期立即到账

```
bin/akashacored tx vesting create-vesting-account \
    <to address> <amount> <end_time> \
    --delayed=true \
    --from=<from address> \
    --gas=200000 \
    --fees=100aakc \
    --chain-id=akchain_9070-1 \
    --home=./data
```
The `end_time` must be provided as a UNIX epoch timestamp [在线工具](https://www.sjczhq.com/)

举例：<br>
```
/>bin/akashacored tx vesting create-vesting-account \
    akasha1ek84hzyrhzaya3x0qr7grnct28hfu2fd0zzeql 100akc 1735989317 \
    --delayed=true \
    --from=akasha18rl6mfx3lap3fkn5alwxdwxcmhfcalj7t9t8u2 \
    --gas=200000 \
    --fees=100aakc \
    --chain-id=akchain_9070-1 \
    --home=./data

/>bin/akashacored q bank balances akasha1ek84hzyrhzaya3x0qr7grnct28hfu2fd0zzeql --home=./data
balances:
- amount: "100000000000000000000"
  denom: aakc
pagination:
  next_key: null
  total: "0"

// 中间时间
/>bin/akashacored q bank spendable-balances akasha1ek84hzyrhzaya3x0qr7grnct28hfu2fd0zzeql --home=./data
balances:
- amount: "0"
  denom: aakc
pagination:
  next_key: null
  total: "0"

// 结束时间
/>bin/akashacored q bank spendable-balances akasha1ek84hzyrhzaya3x0qr7grnct28hfu2fd0zzeql --home=./data
balances:
- amount: "100000000000000000000"
  denom: aakc
pagination:
  next_key: null
  total: "0"
```

## 延迟到账 - 到期线性到账

```
bin/akashacored tx vesting create-vesting-account \
    <to address> <amount> <end_time>\
    --from=<from address> \
    --gas=200000 \
    --fees=100aakc \
    --chain-id=akchain_9070-1 \
    --home=./data
```
举例:<br>
```
/>bin/akashacored tx vesting create-vesting-account \
    akasha1l07ec3ytgzxsplm25e0vgwjqmpemulaxvs4265 100akc 1735989900 \
    --from=akasha18rl6mfx3lap3fkn5alwxdwxcmhfcalj7t9t8u2 \
    --gas=200000 \
    --fees=100aakc \
    --chain-id=akchain_9070-1 \
    --home=./data

/>bin/akashacored q bank balances akasha1l07ec3ytgzxsplm25e0vgwjqmpemulaxvs4265 --home=./data
balances:
- amount: "100000000000000000000"
  denom: aakc
pagination:
  next_key: null
  total: "0"

//中间时间
/>bin/akashacored q bank spendable-balances akasha1l07ec3ytgzxsplm25e0vgwjqmpemulaxvs4265 --home=./data
balances:
- amount: "46244131455399061000"
  denom: aakc
pagination:
  next_key: null
  total: "0"

//最后时间
/>bin/akashacored q bank spendable-balances akasha1l07ec3ytgzxsplm25e0vgwjqmpemulaxvs4265 --home=./data
balances:
- amount: "100000000000000000000"
  denom: aakc
pagination:
  next_key: null
  total: "0"

```

## 延迟到账 - 到期周期到账(deprecated)

```
bin/akashacored tx vesting create-periodic-vesting-account  \ 
    <to address> <periods_json_file> \
    --from=<from address> \
    --gas=200000 \
    --fees=1000000aakc \
    --chain-id=akchain_9070-1 \
    --home=./data

```
