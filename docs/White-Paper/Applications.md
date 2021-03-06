# 应用

总的来说，在以太坊之上有三种应用程序。
第一类是金融应用程序，为用户提供更有效的管理方式，并使用他们的资金签订合同。
这包括子货币，金融衍生产品，对冲合约，储蓄钱包，遗嘱以及最终甚至是一些类别的全面雇佣合约。
第二类是涉及金钱的半金融申请，但是在做什么方面也有非常重要的非货币方面;,一个完美的例子就是为计算问题提供解决方案的自我奖励。
最后，还有一些应用程序，例如在线投票和分散治理，根本没有财务。

## 令牌系统

区块链上的代币系统有许多应用程序，从代表美元或黄金等资产的子货币到公司股票，代表智能财产的个人代币，安全的不可伪造优惠券，甚至与常规值无关的代币系统,激励制度。
令牌系统在Ethereum中实现起来非常简单。
要理解的关键是，所有货币或标记系统从根本上都是一个数据库，只有一个操作：从A中减去X个单位，并将X个单位给予B，但条件是（i）A至少有X个单位,交易和（2）交易由A批准。
实现令牌系统所需的一切就是将这个逻辑合并到一个合约中。

用蛇来实现令牌系统的基本代码如下：

    def send(to, value):
        if self.storage[msg.sender] >= value:
            self.storage[msg.sender] = self.storage[msg.sender] - value
            self.storage[to] = self.storage[to] + value

这实质上是本文件上面进一步描述的“银行系统”状态转换功能的文字实现。
需要添加一些额外的代码行来提供首先分配货币单位的初始步骤以及其他一些边缘情况，理想情况下将增加一个函数让其他合同查询地址的余额,。
但这就是它的全部。
从理论上讲，作为子货币的以太坊代币系统可能会包含另一个重要的特征，即在比特币上的连锁货币所缺乏的一个重要特征：能够直接用该货币支付交易费用。
这样做的方式是，合同将保持乙醚余额，乙醚用来向发件人支付费用，然后通过收取其所需的内部货币单位来重新填充这个余额，,一个不断运行的拍卖。
用户因此需要用以太“激活”他们的帐户，但是一旦以太有一次就可以重复使用，因为合同每次都会退还。

## 金融衍生品和稳定价值货币

金融衍生产品是“智能合约”中最常见的应用，也是代码中最简单的一种。
实施金融合同的主要挑战是，其中大部分要求参考外部价格报价;,例如，一个非常理想的应用是一个智能的合约，针对美元对以太（或另一个加密货币）的波动进行套期保值，但这样做需要合约知道ETH / USD的价值。
最简单的方法是通过由特定方（例如，NASDAQ）维护的“数据馈送”合同，以便该方能够根据需要更新合同，并提供一个接口，允许其他合同发送,消息给那个合同并且得到回应提供价格。

鉴于这一关键因素，对冲合约将如下所示：

1. 等待A方输入1000以太。
2. 等B方输入1000乙醚。
3. 记录通过查询数据输入合同计算得出的1000乙醚的美元价值，在储存中，这是$ x。
4. 30天后，允许A或B“重新激活”合同，以便将价值$ x的以太（通过再次查询数据输入合同计算得出新价格）发送给A，其余的则发送给B.

这样的合同在密码电子商务中具有很大的潜力。
引用加密货币的一个主要问题是事实上它是不稳定的;,尽管许多用户和商家可能希望处理密码资产的安全性和便利性，但他们可能不希望在一天内面临失去23％的资金价值的前景。
到目前为止，最常见的解决方案是发行人支持的资产;,这个想法是，发行人创造一个子货币，他们有权发行和撤销单位，并提供一个单位的货币给谁（离线）与一个单位的指定标的资产（eg.gold ,， 美元）。
然后，发行人承诺向发回一个密码资产单元的任何人提供一个单位的基础资产。
这个机制允许任何非密码资产被“提升”成一个密码资产，只要发行者可以被信任。

然而，在实践中，发行人并不总是值得信赖的，在某些情况下，银行业基础设施太脆弱，或者太敌对，因为这种服务存在。
金融衍生产品提供了另一种选择
在这里，不是一个发行人提供资金来备份一个资产，而是一个投机者分散的市场，押注一个密码参考资产（例如.ETH）的价格将会上涨。
与发行人不同的是，投机者没有选择违约，因为套期保值合约持有他们的资金托管。
请注意，这种方法并不是完全分散的，因为仍然需要一个值得信赖的来源来提供价格报价，尽管有争议的是，在减少基础设施要求方面这是一个巨大的改进（与发行商不同，发行价格饲料不需要许可证,并可能被归类为言论自由），并减少欺诈的可能性。

## 身份和声誉系统

所有人[Namecoin] [11]最早的替代性加密货币都试图使用类似比特币的区块链来提供名称注册系统，用户可以在公共数据库中将他们的名字与其他数据一起注册。
主要引用的用例是[DNS] [12]系统，将域名（比如“bitcoin.org”）（或者在Namecoin的例子中是“bitcoin.bit”）映射到IP地址。
其他用例包括电子邮件认证和潜在的更高级的信誉系统。
以下是在以太坊提供类似Namecoin的名称注册系统的基本合同：

    def register(name, value):
        if !self.storage[name]:
            self.storage[name] = value

合同很简单，,所有这一切都是以太坊网络中的一个数据库，可以添加到以太网中，但不能修改或删除。
任何人都可以注册一个有一定价值的名字，那么注册就会永远坚持下去。
一个更复杂的名称注册合同也将有一个“函数条款”，允许其他合同进行查询，以及一个名称的“所有者”（即第一个注册者）更改数据或转让所有权的机制。
甚至可以在顶部添加信誉和Web信任功能。

## 无心文件存储

在过去几年中，已经出现了一些流行的在线文件存储创业公司，其中最着名的是Dropbox，它试图允许用户上传硬盘的备份，并让服务存储备份并允许用户访问,换取月费。
但是，目前文件存储市场有时效率较低，,粗略地看一下各种[现有的解决方案] [13]表明，特别是在20-200GB的“不可思议的谷”水平上，既没有免费配额也没有企业级的折扣，每月主流文件存储成本的价格是这样的,您在一个月内支付的费用超过了整个硬盘的成本。
以太坊合同可以允许开发一个分散的文件存储生态系统，个人用户可以通过租用自己的硬盘来赚取少量的钱，未使用的空间可以用来进一步降低文件存储成本。

这样一个设备的关键支撑将是我们所说的“分散式Dropbox合同”。
该合同的工作如下。
首先，将所需的数据分成块，对每个块进行隐私加密，然后从中构建Merkle树。
然后用合约规则规定，每N个块，合约将在Merkle树中选择一个随机索引（使用之前的块散列，可以从合同代码访问，作为随机源），并给出X,第一个实体提供一个简单的支付验证的交易，就像树中特定索引处块的所有权证明一样。
当用户想重新下载文件时，可以使用微支付通道协议（例如每32千字节1个字节）来恢复文件;,最节省费用的方法是付款人不要把交易发布到最后，而应该在每32千字节之后用一个稍微更有利可图的交易来替换交易。

该协议的一个重要特点是，虽然看起来好像一个人相信许多随机节点不会决定忘记文件，但可以通过秘密共享将文件分成许多部分将风险降低到接近零,看合同，看看每件还在一些节点的占有。
如果一份合同还在付钱，那么就提供了一个密码证据，证明某人还在存储这个文件。

### 无心自组织

“分散的自治组织”的一般概念是，拥有一定数量的成员或股东的虚拟实体，可能拥有67％的多数，有权使用该实体的资金并修改其代码。
成员们将共同决定组织如何分配资金。
分配DAO资金的方法可以从赏金，工资到更为独特的机制，如内部货币，以奖励工作。
这基本上复制了传统公司或非营利组织的法律标志，但是只使用加密区块链技术来实施。
到目前为止，关于DAO的大部分讨论围绕着“分权自主化公司”（DAC）的“资本主义”模式，即分红股东和流通股。,另一种可能被称为“分散的自治社区”的替代方案，将使所有成员在决策过程中享有平等的份额，并要求67％的现有成员同意增加或删除成员。
一个人只能拥有一个会员的要求就需要集体强制执行。

如何编写DAO的一般概要如下。
最简单的设计只是一个自我修改的代码，如果有三分之二的成员同意修改，这个代码就会改变。
尽管代码在理论上是不可变的，但是人们可以很容易地解决这个问题，并且通过在不同的合同中包含大量的代码，并将调用哪些合同的地址存储在可修改的存储中，从而具有事实上的可变性。
在这种DAO合同的简单实现中，将会有三种交易类型，由交易中提供的数据所区分:

* `[0,i,K,V]`用索引`i`注册提议，将存储索引`K`中的地址更改为值`V`
* `[0,i]`投票赞成提案`i`
* `[2,i]`如果提出了足够的表决，则可以最终确定提案`i`

然后合同将有这些条款。
它将保存所有开放存储更改的记录，以及谁投他们的清单。
它也将有一个所有成员的名单。
当任何存储变更得到三分之二的成员投票时，最终交易可以执行变更。
一个更复杂的骨架也会具有内置的投票功能，如发送交易，增加成员和删除成员，甚至可以提供[Liquid Democracy] [14]式投票代表 (即任何人都可以指定某人投票给他们，并且任务是传递的，所以如果A分配了B并且B分配了C，那么C就决定了A的投票).
即任何人都可以指定某人投票给他们，并且任务是传递的，所以如果A分配了B并且B分配了C，那么C就决定了A的投票

这种设计可以使DAO作为一个分散的社区而有机地发展，使人们最终可以将专家的成员筛选出来，尽管与“现有系统”不同，专家们可以随时间轻易地进入和退出,因为个别社区成员改变了路线。
这种设计可以使DAO作为一个分散的社区而有机地发展，使人们最终可以将专家的成员筛选出来，尽管与“现有系统”不同，专家们可以随时间轻易地进入和退出,因为个别社区成员改变了路线。(最好是在合同内部有一个订单匹配机制).
代表团也将存在流动民主式，推广“董事会”的概念。

## 其它应用

**1. 储蓄钱包**.
假设爱丽丝希望保证自己的资金安全，但担心自己会输，否则有人会破解自己的私钥。
她把乙醚与一家银行的鲍勃合同如下：

* 爱丽丝可以每天最多提取1％的资金。
* 鲍勃一个人可以每天最多提取1％的资金，但是爱丽丝有能力通过她的钥匙进行交易，关闭这个能力。
* 爱丽丝和鲍勃一起可以撤回任何东西

通常情况下，爱丽丝每天1％就足够了，如果爱丽丝想要更多地收回，她可以联系鲍勃寻求帮助。
如果Alice的密钥被黑客入侵，她就跑到Bob身上，将资金转移到新的合同中。
如果她失去了钥匙，鲍勃将最终获得资金。
如果鲍勃原来是恶意的，那么她可以关闭他的退出能力。

**2. 农作物保险**.
人们可以很容易地制定金融衍生品合约，但是使用天气的数据馈送而不是任何价格指数。
如果爱荷华州的一个农民购买了一种能够根据爱荷华州降雨量反向支付的衍生物，那么如果出现干旱，农民就会自动收到钱，如果雨水充足，农民就会因为作物收成好而开心。
这一般可以扩大到自然灾害保险。

**3. 分散的数据馈送**.
通过一个称为“[SchellingCoin] [15]”的协议，实际上可以分散数据馈送。
SchellingCoin的基本工作原理如下：N方将所有数据的价值（例如ETH / USD价格）都存入系统，然后对这些值进行排序，所有在第25和第75百分位之间的人都可以得到一个令牌作为奖励。
每个人都有动力提供其他人将会提供的答案，而大量球员能够实际达成一致的唯一价值就是明显的违约：事实。
这创建了一个分散的协议，理论上可以提供任何数量的值，包括ETH / USD价格，柏林的温度，甚至是特定的硬计算结果。

**4. 智能多重签名托管**.
比特币允许多重签名交易合约，例如，给定五个密钥中的三个可以花费资金。
以太坊允许更多的粒度;,例如，五分之四的人可以消费一切，五分之三的人每天可以花费高达10％，五分之二的人每天花费高达0.5％。
此外，以太坊multisig是异步的 - 双方可以在不同的时间在区块链上注册他们的签名，最后的签名会自动发送交易。

**5. 云计算**.
EVM技术还可用于创建可验证的计算环境，允许用户请求其他人进行计算，然后可以选择要求提供某些随机选择的检查点的计算正确完成的证据。
这允许创建云计算市场，任何用户都可以通过他们的台式机，笔记本电脑或专用服务器参与，并且可以使用安全存款的抽查以确保该系统是可信的(即节点不能盈利地作弊).
虽然这样的制度可能不适合所有的任务，,例如，需要高级别进程间通信的任务不能容易地在大量节点上完成。
但是，其他任务更容易并行化; ,SETI @ home，folding @ home和遗传算法等项目可以很容易地在这样的平台上实现。

**6. 点对点赌博**.
任何数量的点对点博弈协议，如Frank Stajano和Richard Clayton的[Cyber​​dice] [16]，都可以在以太坊区块链上实现。
最简单的赌博协议实际上只是下一个块哈希差异的合同，并且可以从那里建立更高级的协议，从而以几乎为零的费用创建赌博服务，这些服务无法作弊。

**7. 预测市场**.
提供预言市场也很容易实现，预测市场与谢林币一起可能被证明是第一个主流应用[futarchy] [17]作为分散组织的治理协议。

**8. 在连锁分散的市场**, 以身份和名誉系统为基础.