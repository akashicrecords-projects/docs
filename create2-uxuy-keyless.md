# UXUY Keyless Create2 基础设施预置

本文通过一项治理提案，在 AKC 主网预置 UXUY 所需的三个固定地址合约。完整提案内容见同目录的 [uxuy-keyless-create2-preinstall-proposal.json](uxuy-keyless-create2-preinstall-proposal.json)。

| 名称 | 地址 | Runtime 长度 |
| --- | --- | --- |
| ImmutableCreate2Factory (0age keyless) | `0x0000000000ffe8b47b3e2130213b802212439497` | 2099 bytes |
| ImmutableCreate2Factory (0age intermediate) | `0xcfA3A7637547094fF06246817a35B8333C315196` | 2099 bytes |
| Keyless CREATE2 proxy | `0x7A0D94F55792C434d74a40883C6ed8545E406D12` | 31 bytes |

两个 factory 的 runtime Keccak256 均为：

```text
0x767db8f19b71e367540fa372e8e81e4dcb7ca8feede0ae58a0c0bd08b7320dee
```

## 前置检查

在持有治理提案密钥的节点执行。以下命令使用默认 RPC 和默认生产密钥库。

```bash
cd /opt/akchain/core

./bin/akashacored status
./bin/akashacored query auth module-account gov -o json
./bin/akashacored query gov params -o json
./bin/akashacored query staking params -o json
```

提案文件中的 `authority` 必须等于 `query auth module-account gov` 返回的 `account.value.address`。当前 AKC 的值应为：

```text
akasha10d07y265gmmuvt4z0w9aw880jnsr700jty3m57
```

提交前，从任意 EVM RPC 查询三个地址，确认均未被占用：

```bash
for ADDRESS in \
  0x0000000000ffe8b47b3e2130213b802212439497 \
  0xcfA3A7637547094fF06246817a35B8333C315196 \
  0x7A0D94F55792C434d74a40883C6ed8545E406D12; do
  curl -s -X POST http://127.0.0.1:8545 \
    -H 'content-type: application/json' \
    --data "{\"jsonrpc\":\"2.0\",\"method\":\"eth_getCode\",\"params\":[\"${ADDRESS}\",\"latest\"],\"id\":1}"
done
```

预期每项均返回 `"result":"0x"`。若地址已有 Cosmos 账户或已有不同 runtime，注册会失败，不能覆盖。

## 提交提案

将 JSON 文件放到节点的 `/opt/akchain/core` 目录。先核对 `deposit` 不低于 `query gov params` 返回的 `min_deposit`；AKC 当前提案文件使用 `10000000aakc`。

```bash
./bin/akashacored tx gov submit-proposal uxuy-keyless-create2-preinstall-proposal.json \
  --from <proposer-key> \
  --home ./data \
  --chain-id akchain_9070-1 \
  --gas auto \
  --gas-adjustment 1.5 \
  --fees 5000000aakc \
  -y
```

记录返回的交易哈希和提案 ID。查询投票中的提案：

```bash
./bin/akashacored query gov proposals --proposal-status voting-period -o json
```

## 验证人投票

每个有投票权的验证人执行一次：

```bash
./bin/akashacored tx gov vote <proposal_id> yes \
  --from <validator-key> \
  --home ./data \
  --chain-id akchain_9070-1 \
  --gas auto \
  --gas-adjustment 1.5 \
  --fees 5000000aakc \
  -y
```

投票期结束后查询结果：

```bash
./bin/akashacored query gov proposal <proposal_id> -o json
```

预期状态为 `PROPOSAL_STATUS_PASSED`。治理会在通过时调用 `MsgRegisterPreinstalls`；无需重启节点或替换二进制。

## 验证代码

在每个公开 RPC 节点执行三次 `eth_getCode`。第一个和第二个地址的返回值必须相同，且 Keccak256 为上文的 factory hash；第三个地址必须返回：

```text
0x60003681823780368234f58015156014578182fd5b80825250506014600cf3
```

可用 Foundry 验证 factory runtime：

```bash
cast code 0x0000000000ffe8b47b3e2130213b802212439497 --rpc-url <rpc-url> | cast keccak
cast code 0xcfA3A7637547094fF06246817a35B8333C315196 --rpc-url <rpc-url> | cast keccak
```

两项都应输出：

```text
0x767db8f19b71e367540fa372e8e81e4dcb7ca8feede0ae58a0c0bd08b7320dee
```

## 说明

- 当前预置接口创建的账户 nonce 为 `0`，不会设置为以太坊原始部署后的 `1`。这不影响这些合约的 runtime、固定地址和 CREATE2 部署结果。
- 三个地址位于同一条 `MsgRegisterPreinstalls` 中。任意一项无法注册时，整条消息失败，不会只写入部分合约。
- 不采用无 EIP-155 的原始交易方案。该方案除单节点 JSON-RPC 配置外，还要求链上 `allow_unprotected_txs=true`，会扩大整条链接受可重放 legacy 交易的范围。
