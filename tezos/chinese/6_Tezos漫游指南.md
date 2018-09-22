# Tezos 漫游指南
文章来源: https://medium.com/cryptium/the-hitchhikers-guide-to-tezos-36f112662074
作者 Awa Sun Yin

本文的目的是为了帮助读者更进一步的了解Tezos项目中的各个关键环节，读者无需具备先前知识，通过阅读可以帮助读者更好地实现与Tezos网络的交互，理解Tezos系统的运行原理。这里涉及的主题包括从Tezos协议的介绍到Tezos系统里面包师(baker，即生产区块的人)和委托人(delegator)的经济学。

注意：本文着重于介绍Tezos项目本身及其生态系统。Tezos项目背后的故事，即项目创始人的轶事以及初始基金会的相关争议，请参阅[WIRED](https://www.wired.com/story/tezos-blockchain-love-story-horror-story)文章。
 
## Tezos是什么?
Tezos是一个公共分布式账本，或称为区块链，它是一个主要用[OCaml](https://en.wikipedia.org/wiki/OCaml)函数式编程语言编写的开源项目。 除了分布式记账以外，它还是一个智能合约平台，支持用Michelson语言编写的合约。 [Michelson](https://www.michelson-lang.com/)是强类型的，就像OCaml一样，适用于编写便于形式验证的智能合约（有助于提高系统的正确性，及时发现合约漏洞，例如臭名昭着的[DAO](https://en.wikipedia.org/wiki/The_DAO_%28organization%29)或[Multi-Sig Wallet](https://paritytech.io/parity-technologies-multi-sig-wallet-issue-update/)）。 Michelson仅限于编写与区块链相关的常见应用程序，例如 multisig wallets，vesting 或者 distribution rules，旨在更容易地检查，[究竟是什么命令被执行](https://www.michelson-lang.com/why-michelson.html)。

除语言选择外，Tezos的另一大特点还在于其独创的权益证明机制，即Liquid Proof-of-Stake ([LPoS](https://medium.com/tezos/liquid-proof-of-stake-aec2f7ef1da7))，用于确定用户群或节点必须满足哪些条件才能参与共识。 LPoS的使用与Nakamoto 共识相结合，该共识认为，最长的链才是正确的链，以激励实体维护基础设施，协调相同的无信任方形成Tezos网络。

Tezos项目的另一个不寻常的特征是所谓的链上治理。 区块链文献中的治理是指用于设计、提议、执行和实施对协议或项目进行更改的过程或系统。 “Tezos， 一种自我修正的加密分布式账本”，这个广泛传播的标签指的是，Tezos的目标不仅是支持源代码中的元升级或更改，还支持治理流程本身， 通过链上投票系统确定提议是否通过或被拒绝。

根据数据统计，Tezos的ICO从2017年7月1日开始，一直持续到2017年7月13日结束，总募资2.32亿美元，[累计发行763,306,929.69 XTZ，其中20％代币处于锁仓状态](https://medium.com/r/?url=https%3A%2F%2Fwww.tezos.ch%2Ffundraiser-statistics.html%23fundraiser-statistics)。 由于Tezos是一个开源项目，意味着任何人都可以提交对源代码的贡献，该项目主要由位于特拉华州的一个名为[Dynamic Ledger Solutions](https://medium.com/r/?url=https%3A%2F%2Fwww.linkedin.com%2Fcompany%2Fdynamic-ledger-solutions-inc%2F)的实体和[OCamlPro](https://medium.com/r/?url=https%3A%2F%2Fwww.ocamlpro.com)背后一个法国发展服务公司的团队负责开发、生产和维护。 

最近，Tezos共上线了三个版本：[zeronet](https://medium.com/r/?url=https%3A%2F%2Ftezos.gitlab.io%2Fzeronet)，[alphanet](https://medium.com/r/?url=https%3A%2F%2Ftezos.gitlab.io%2Falphanet)和[betanet](https://medium.com/r/?url=https%3A%2F%2Ftezos.gitlab.io%2Fbetanet)。 它们分别是：一个包含更多实验和以开发导向功能为主的测试网络，一个包含最新最稳定功能的测试网络，以及一个最接近生产、交易和起源（智能合约）的测试网络，它将持续存在于主网上。 Tezos基金会今天([9月17日](https://twitter.com/TezosFoundation/status/1040640714424508416))公布了主网的上线时间（又称为lunch date午餐约会），从Betanet测试网到主网的过渡将涉及标签的更改。 [Betanet测试网于2018年6月30日上线](http://tzscan.io/0)，[由8个Tezos 创世面包师运行至第7周期](http://tzscan.io/rolls-distribution)。到写作周期（第26周期）时，大约将会有441个不同的面包师。

Tezos时间以周期（约2-3个自然天）来衡量。Tezos有两种不同类型的账户：以`TZ1`开头用于持有资金的账户，和以`KT1`开头用于委托（智能合约）的账户。 Tezos是一个公共区块链，任何满足自我抵押要求的人都可以参与共识。 目前，只有两种方式可以获得XTZ：参与了众筹（只有通过了[KYC](https://tezos.com/betanet/)才能激活帐户使用里面的资金）或在Coinon或Gate.io两个交易所交易或场外交易。主网上线后，又有几个交易所宣布支持Tezos交易。

## Tezos的主要利益相关者

### 基金会 (The Tezos Foundation)
[Tezos基金会](https://tezos.foundation/)是一家受资助的非营利性实体，总部设在楚格，受瑞士法律管辖。 基金会的任务是促进和开发技术和应用，特别是开源的和去中心化的软件架构，其重点（但不限于）是Tezos协议。 原文（德文）说明如下：
> Förderung und Entwicklung von neuen Technologien und Applikationen, insbesondere in den Bereichen von neuen offenen und dezentralisierten Softwarearchitekturen; im Vordergrund — aber nicht ausschliesslich — steht dabei die Förderung und Entwicklung des so genannten Tezos Protokolls und der entsprechenden Technologien, sowie die Förderung und Unterstützung von Applikationen unter Anwendung des Tezos Protokolls; vollständige Zweckumschreibung gemäss Statuten — Tezos Stiftung’s purpose, retrieved from the [Commercial Register of Canton Zug](https://zg.chregister.ch/cr-portal/auszug/auszug.xhtml?uid=CHE-290.597.458)

促进和开发新技术和应用，特别是在新的、开源的和去中心化的软件架构领域。重点是 - 但不仅仅是- 促进和发展所谓的Tezos协议和相应的技术，以及促进和支持基于Tezos协议的应用。引用来源于楚格州商业登记处，根据Tezos基金会的章程简述

为了实现这一目的，[基金会启动了一项致力于研究](https://tezos.foundation/)、开发和社区工作的奖励计划（以Cryptium Labs为特色！）。

### Dynamic Ledger Solutions动态分布式账本解决方案
动态分布式账本解决方案是致力于开发和启动Tezos网络的实体。 作为回报，Tezos基金会将从众筹中拿出8.5％的资金，以及Genesis发行的总代币的10％作为奖励。

### XTZ持有人
实体可以通过众筹，Coinone或Gate.io等交易所或OTC交易获得XTZ。XTZ持有人可以自然地将资金用于多种目的，这其中最常见的就是转账，抵押（参见“面包师”一节），委托（参见“授权”一节），支出（例如捐赠给开源项目），或单纯的持有（或称为hodling）。 这里并不建议只是单纯的持有代币，因为通货膨胀会导致代币的相对价值被慢慢稀释（[XTZ属于通胀型代币，Tezos系统每年会增发≈5.51%奖励](http://tezos.gitlab.io/betanet/whitedoc/proof_of_stake.html#inflation)，后期为每年1%的增发）。主网上线后，又有几个交易所宣布支持Tezos交易。

### 面包师 (Bakers)
面包师可能是被委托人（Tezos的术语，当他们收到委托时）或验证者（PoS中常用的术语）。实体将最小数量的XTZ分配作为自我抵押（赌注），使他们有资格参与共识，从而接收烘焙（区块验证，奖励为16 XTZ + 交易手续费/每区块）和背书奖励（奖励为2或 1 XTZ每次背书）。面包师可以是单独的个体（又称个人烘焙），也可以是某些实体，他们通过收取服务费向代币持有人提供公共服务。这也意味着每一个烘培块的产生，XTZ的总供应量会增加16 XTZ。

* 更多相关信息请参阅 –你最喜欢的面包师是否被过度委托了？ 了解Tezos中的自我抵押要求：https://github.com/cryptiumlabs/library/blob/master/tezos/chinese/4_你最喜欢的烘焙师是否过度授权了%3F了解Tezos中的保证金要求.md  

[烘焙和背书权利根据一个类似彩票的系统](https://tezos.gitlab.io/zeronet/whitedoc/proof_of_stake.html)（a lottery-like system），采用优先级列表分配给面包师们。因此，与新晋面包师相比，高级面包师可能获得了更多的奖励。因为到目前为止，面包师的数量一直随着周期的增长而不断增加。

用于分配槽的（伪）随机数由多步系统和不同值的组合生成，例如在周期X结束时生成的256位数组成的随机种子，在前一周期中作出的最多32个启示的承诺，以及所有启示的散列。有传言说Tezos转向公共可验证秘密共享（例如Shamir的秘密共享）来产生随机数。

* 更多相关信息请参阅 –介绍Tezos的股权证明以及了解烘焙和背书奖励：https://github.com/cryptiumlabs/library/blob/master/tezos/chinese/5_你好管理员什么时候可以用Tez支付%3F介绍Tezos的权益证明(PoS)和理解烘焙(Baking)和认可奖励.md 
  
公共面包师不能拒绝委托，这些委托都是链上起源（智能合约），然后根据他们收到的授权总量进行最小自我抵押。 当自我抵押不能覆盖所有委托时，就意味着面包师被过度委托了。避免它的唯一方法是，不支付与导致过度委托的金额相对应的奖励，当然委托人并不被鼓励这样做。

根据协议，[奖励会直接支付给面包师](https://github.com/cryptiumlabs/library/blob/master/tezos/chinese/5_你好管理员什么时候可以用Tez支付%3F介绍Tezos的权益证明(PoS)和理解烘焙(Baking)和认可奖励.md)。这意味着面包师与委托人之间需要相互信任，因为后者必须相信，前者会支付所有与该委托相对应的奖励给他。

对应每个双重签名的区块面包师可能会损失至少512 XTZ（算上当前获得的所有奖励），而每个双重背书的区块可能会损失64 XTZ，他们还有可能损失自我抵押的所有金额。 当面包师受到惩罚时，这些代币会被直接烧毁，它们会从总供应中完全消失。 但是无论如何，双重烘焙和双重背书都不会影响被分配未来槽的可能性。 在烘焙或背书时不在线的面包师将会失去相应的收入，而其他的面包师将会取而代之（即被盗块stolen blocks）。

### Delegators委托人
委托人是指那些没有兴趣成为面包师的XTZ持有人，将其代币委托给其他的面包师进行烘焙。 理论上，委托人可以根据授权比例，从面包师所产生的总奖励中获得相应比例金额。 作为回报，被委托的面包师将收取％的费用作为服务费。

与交易相反，委托烘焙并不会使委托人丧失对他们资金的控制权。 一份委托就是一个起源（一份智能合约），合约所有者是委托人。在其他字段中，起源包含一个参数，该参数是目标面包师的地址，用于将委托分配给该特定面包师（同样，这并不意味着面包师将获得对这些资金的任何控制）。

与面包师的自我抵押不同，委托不会因为面包师进行双重烘焙或背书受到惩罚时而丢失。 [委托人面临的最大风险是委托给临时关闭其服务的面包师而丧失的机会成本](https://www.reddit.com/r/tezos/comments/94ulqc/xtezio_shutting_down/)。 在这种情况下，如果委托权没有在同一个周期内进行转换，除了7个基本周期之外，这笔委托资金还将在一个周期内没有效益（委托只有在新一批烘焙和背书操作时才需要）。其他可能存在的风险还包括，面包师不支付奖励的可能性，或是面包师无法完成指定烘焙的可能性，以及背书斑点导致面包师无法获得奖励。
* 更多相关信息请参阅 – 介绍Tezos的权益证明机制以及了解烘焙和背书奖励：https://github.com/cryptiumlabs/library/blob/master/tezos/chinese/5_你好管理员什么时候可以用Tez支付%3F介绍Tezos的权益证明(PoS)和理解烘焙(Baking)和认可奖励.md 
  
## The Tezos Ecosystem Tezos生态系统
### 钱包 (Wallets)
钱包是允许用户直接或间接与区块链交互的软件。 目前有两种类型的钱包：硬件钱包即冷钱包，以及在线钱包，又称为热钱包。钱包及其开发人员在区块链生态系统中发挥着至关重要的作用，因为代币的使用强烈依赖于它们。钱包在PoS网络中具有其特殊性，它支持委托和取消委托，这些都是比特币等传统网络不具备的。

面包师和开发人员通常使用Tezos客户端，这是一个基于命令行的钱包。 要获得Tezos客户端，[首先要在其设备上安装Tezos](https://tezos.gitlab.io/zeronet/introduction/howtouse.html)。 [此命令行工具支持转移资金或生成起源等操作](https://tezos.gitlab.io/zeronet/introduction/howtouse.html)。

* 更多相关信息请参阅 – Tezos客户端安装和委托指南：https://github.com/cryptiumlabs/library/blob/master/tezos/chinese/2_如何用Tezos客户端委托Tezos(XTZ)及如何运行你自己的节点betanet.md 
  
[Ledger Nano S](https://www.ledger.com/products/ledger-nano-s)是一个很受欢迎的支持与Tezos客户端进行交互的硬件钱包。 尽管要与命令行工具进行交互才能转移资金或生成起源，但将资金存放在硬件钱包上会极大的增加安全性，对于持有大量XTZ代币的用户建议使用硬件钱包。另外一款颇受欢迎的硬件钱包[Trezor](https://trezor.io/)也已经上线了[Tezos钱包](https://www.reddit.com/r/tezos/comments/8lk5vh/trezor_integration_for_tezos_demo/)。硬件钱包的最高风险来自于直接使用已经创建了的钱包，无论钱包是来自官方或非官方供应商，这意味着第三方可能已经备份了助记词并且可以访问存储在钱包中的资金。

* 更多相关信息请参阅 – Ledger Nano S委托指南：https://medium.com/cryptium/how-to-delegate-tezzies-tezos-xtz-with-your-ledger-nano-s-with-initial-setup-screenshots-519c9ae6654f

在Tezos生态系统中，有两个很受欢迎的由Stephen Andrew和Cryptonomic团队开发的热钱包：TezBox钱包和Cryptonomic的Galleon钱包。 随着Tezos社区的发展，有越来越多的开发者进入，为生态系统的实用性带来贡献。 [这是社区开发的Tezos钱包列表](https://djangobits.github.io/tezoswallets/)。 甚至还有来自远东的开发者加入，例如位于中国的[WeTez](http://www.wetez.cn/)。
* 更多相关信息请参阅 – TezBox委托指南：https://medium.com/cryptium/how-to-delegate-tezos-xtz-with-tezbox-online-wallet-96592e94e357 
* 更多相关信息请参阅 – Galleon委托指南：https://medium.com/cryptium/how-to-delegate-tezos-xtz-with-galleon-wallet-7bdc44e954a8

### 基本服务
除了协议和钱包开发以外，有些团队还为Tezos生态系统中的每一个用户或利益相关者提供了必不可少的服务。例如很多区块浏览器提供了免费的数据查询服务。它们直接从完整节点查询数据，通常用于检查一笔交易是否已经包含在下一个区块当中，检查我们当前处于哪个周期，检查哪些面包师正在工作，以及与他们的烘培相关的统计数据，甚至是关于网络的一般统计数据（总的代币供给或全网自我抵押的代币金额）。 最受欢迎的Tezos块浏览器是由OCamlPro团队开发的[TzScan](http://tzscan.io/)。

因为代币持有人需要在一定程度上信任烘焙服务，烘焙清单是PoS生态系统中基本服务的另一个代表。 清单列出了现有的公共烘焙服务以及显示了每个服务上的相关指标，在委托之前，通过清单你可以轻松地发现服务，并查找有关它们的更多信息。 最受欢迎的面包师列表有：[MyTezosBaker](http://mytezosbaker.com/)，[Tezos Rocks](https://tezos.rocks/)或TzScan等等。 还有些服务甚至可以为你提供面包师的打包成功率，例如[Bakendorse](https://bakendorse.com/#/)！

### Community社区
下表列出了全球主要的Tezos社区：
* https://gist.github.com/awasunyin/31c39f48aa8b23a68443d0336e0216b5 

在表格里还找不到你所认识的面包师、社区或开发团队吗？ 快通过PR来帮助我们完善这个列表吧！
* https://github.com/cryptiumlabs/library/tree/master/communities 