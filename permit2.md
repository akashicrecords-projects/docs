# permit2
Permit2 引入了一种低开销的下一代代币授权 / 元交易系统，使代币授权在不同应用之间更加简单、更安全且更加一致。

#Features（功能）
## 基于签名的授权：任何 ERC20 代币，即使不支持 EIP-2612，也可以使用类似 permit 的授权方式。这使应用在集成 Permit2 合约时，可以通过在交易数据中附带一个 permit 签名，实现单笔交易流程。
批量代币授权：通过一次签名即可为不同代币设置不同的授权对象（spender）。
基于签名的代币转账：代币持有者可以通过签名消息，直接将代币转给指定的接收方，而无需设置 allowance。这意味着应用在接收代币时不再需要提前授权，也不会存在“悬挂授权”的问题。该签名仅在被使用的交易期间有效。
批量代币转账：通过一次签名即可将不同的代币转给不同的接收地址。
安全的任意数据验证：可以通过传入 witness hash 和 witness type 来验证任意附加数据。其中 type 字符串必须符合 EIP-712 标准。
合约签名验证：所有签名验证均支持 EIP-1271，因此合约账户也可以通过签名完成代币授权和转账。
非单调重放保护：基于签名的转账使用无序、非单调递增的 nonce，从而签名的 permit 不需要按照特定顺序执行。
可过期授权：授权可以设置有效期，从而避免钱包中对全部代币余额的长期授权带来的安全风险。这也意味着，当授权过期后无需额外发起交易进行撤销。
批量撤销授权：可以在一次交易中撤销多个代币和多个授权对象的 allowance。
#Architecture（架构）
Permit2 由两个合约组成：AllowanceTransfer 和 SignatureTransfer。
SignatureTransfer 合约：处理所有基于签名的转账。它会绕过代币的 allowance 机制，授权仅在该一次性签名被使用的交易期间有效。
AllowanceTransfer 合约：负责设置代币的 allowance，为指定的 spender 在指定时间内授予指定额度的使用权限。只有在正确设置权限的情况下，通过该合约发起的转账才会成功。
Integrating with Permit2（集成Permit2）
在集成之前，如果合约希望通过 Permit2 请求用户的代币，用户必须先在对应的代币合约中对 Permit2 合约进行授权。更详细的技术参考可以查看 Uniswap 官方文档。

关于 viaIR 编译的说明
Permit2 使用 viaIR 编译方式，因此在集成测试中导入和部署时，集成项目也需要启用 viaIR 编译。该方式通常较慢，可以通过使用预编译的 DeployPermit2 工具来避免。

import {DeployPermit2} from "permit2/test/utils/DeployPermit2.sol";

contract MyTest is DeployPermit2 {
address permit2;
function setUp() public {
permit2 = deployPermit2();
}
}
