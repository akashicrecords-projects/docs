# Pump文档



## 一、Pump介绍<br>
&ensp; &ensp;Pump 是一个为Memecoin创作者和交易者设计的创新平台，简化了代币创建和交易流程。用户仅需支付0.02个【NativeToken】的费用，输入代币名称并上传图片，即可轻松发布新代币。其他用户可以在定价曲线上买卖这些代币，从而形成公平启动的市场环境。当代币市值达到 $42,500时，平台会自动向Swap注入流动性并销毁，以确保安全性和流动性。此外，完成绑定曲线的创作者还能获得0.5个【NativeToken】作为奖励。

&ensp; &ensp; 1个【NativeToken】=1 *1000 个AKC

&ensp; &ensp; AKC按均价0.5美元计算

## 二、Pump的特点<br>
&ensp; &ensp;1、低成本：仅需0.02个【NativeToken】即可发币。

&ensp; &ensp;2、无需创建流动性：市值达 $42,500时，平台自动在DEX创建流动性池。

&ensp; &ensp;3、流程自动化：市值达标后自动上线交易，无需手动操作。

&ensp; &ensp;4、公平启动：无代币预留、无税费，权限完全放弃，代币直接公开流通。

&ensp; &ensp;Pump降低了代币创建门槛，使代币发行更加简单便捷。<br>

## 三、发布流程<br>
&ensp; &ensp;在Pump平台上发布代币的步骤如下：

&ensp; &ensp;1、选择一个独特的代币名称（Name）和符号（Ticker）。

&ensp; &ensp;2、为代币编写简单描述。

&ensp; &ensp;3、上传代币的图片或视频。

&ensp; &ensp;4、可选项：添加社交媒体链接（如Twitter、Telegram、网站）。

&ensp; &ensp;5、输入你想购买的数量（创建者可优先购买一些）。

&ensp; &ensp;6、支付0.02个【NativeToken】的创建费用及所购代币的花费。

## 四、计算公式<br>

&ensp; &ensp;我们提前在 Pump的定价系统中设置了一个名为 virtualSolReserves 的前置虚拟池。该虚拟池初始包含30枚【NativeToken】和 1,073,000,191枚代币，其定价公式遵循 x * y = k 的恒定乘积规则。经过数据拟合计算，初始的 k 值 为 32,190,005,730，每枚代币的初始价格约为 0.000000028个【NativeToken】。

&ensp; &ensp;定价公式如下：k=（xn+x）*（yn-y）

&ensp; &ensp;xn和yn，表示当前池子中已有的数量值，

&ensp; &ensp;x表示当前要花费多少【NativeToken】的数量，y表示当前兑换出来代币的数量

&ensp; &ensp;y = yn - k /（ xn + x）

&ensp; &ensp;初始状态时，1个x，可以兑换y=34612909.38709677个代币

&ensp; &ensp;x/y 就是每枚代币的初始价格约为 0.000000028个【NativeToken】

## 五、定价曲线<br>

&ensp; &ensp;Pump 的代币定价系统采用光滑的绑定曲线，Pump 所有 Memecoin 的代币总量固定为 10 亿，且共享相同经济模型。用户可以通过绑定曲线以合理价格铸造部分代币，剩余代币和募集的资金将组成流动性池（LP）。

&ensp; &ensp;联合曲线定价函数为 y= 1073000191 - 32190005730/( 30+x) ，x 为买入【NativeToken】数量，y 为对应得到代币数量，求导可得每枚代币的价格。以下为函数曲线，蓝色为定价曲线，红色为价格曲线。
其中 x 为用户购买的【NativeToken】数量，y为获得的代币数量，随着 x 增加，代币数量减少，价格上升。绑定曲线的初期价格陡峭，适合 Memecoin 的高波动性特点，早期用户可享受较低价格，而后期随着购买增多，价格逐渐平稳上升。这种模型为早期投资者提供了激励，同时确保了代币的流动性。Pump的绑定曲线提供了公平、透明的价格增长机制，保障了所有代币在固定总量下的供需平衡，不仅简化了发行流程，还为用户提供了持续的流动性，提升了平台的吸引力和参与度。

## 六、代币发布和募资流程<br>
&ensp; &ensp;Pump上的代币总发行量都是10亿，精度18，成功发射后 LP 的自动燃烧和合约权限的完全放弃。用户募资与代币分配用户通过Pump平台的买卖操作，共募集到85 个【NativeToken】，兑换得到 8 亿枚 Memecoin 代币。初始流动性池的建立募集的 85 个【NativeToken】中，79 个【NativeToken】用于初始流动性池（6个【NativeToken】 作为上币费）。额外2 亿枚代币，和79个【NativeToken】 一起添加至 Swap的流动性池，使最终代币总量达到 10 亿枚。Swap上市后的价格与市值代币在Swap上线时的每枚代币价格为 0.00000028个【NativeToken】，是初始虚拟池价格的 10 倍。<br>

&ensp; &ensp;代币创建成本与平台费用<br>

&ensp; &ensp;1、创建代币0.02个【NativeToken】。

&ensp; &ensp;2、在募资过程中，Pump 平台收取 1% 的交易费用。

&ensp; &ensp;3、在代币上线 Swap 流动性池时，平台收取 6个【NativeToken】 的上币费，以支持流动性池的配置。Pump的整个流程设计透明、去中心化，且费用结构合理，为代币项目的公开流通和流动性提供了有力支持。

&ensp; &ensp;4、如果代币成功发射至Swap，平台会优先从收取的6个【NativeToken】中扣除0.5个【NativeToken】奖励发布者。


## 七、注意：<br>
&ensp; &ensp;如果买方购买超了85个【NativeToken】(如用100个【NativeToken】去购买)： 依然用79个【NativeToken】 与 当前所剩下的 代币 在swap中创建交易对。

&ensp; &ensp;这样的话是可以拉高在Swap中的价格：因为 AKC数量不变，但是token数量少了。那么，swap中，每个token就更值钱了。



