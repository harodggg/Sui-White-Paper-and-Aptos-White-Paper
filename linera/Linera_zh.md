<h1 align="center">Linera: 一个用于高度可扩展的Web3应用程序的区块链基础设施</h1>
</br>

<h3 align="center">Version 1 – December 22, 2022</h3>
<h3 align="center">译者：moveworld社区-G-xbt</h3>
</br>

<p align="center">
    <b>Abstract</b>
</p>

&ensp;&ensp;&ensp;&ensp;我们提出了Linera，一个区块链基础设施，旨在通过在互联网规模上提供可预测的性能、安全性和响应性来支持最重要的web3应用。为此，Linera通过引入一种新的、集成的、多链范例来解决区块空间稀缺问题，该范例建立在弹性验证器之上。 Linera将用户置于协议的中心，允许他们管理自己的链(称为微链)中的块生产以获得最佳性能。为了帮助web3开发人员充分利用Linera基础设施，我们开发了一个丰富的、与语言无关的多链编程模型。 Linera应用程序使用异步消息跨链通信。在同一个微链中，应用程序是使用同步调用和临时会话(又名资源)组成的。得益于Wasm虚拟机，Linera的初始SDK将面向 Rust程序员。Linera基础设施基于委托权益证明。它将使用最先进的经济激励措施和社区的大规模审计来确保强有力的权力下放。[^1]

</br>

# Contents

- [1 介绍](#1-介绍)
  - [1.1 Web3 中对可预测性能和响应能力的需求](#11-web3-中对可预测性能和响应能力的需求)
  - [1.2 区块空间稀缺问题](#12-区块空间稀缺问题)
  - [1.3 现有方法的缺点](#13-现有方法的缺点)
  - [1.4 我们的任务](#14-我们的任务)
  - [1.5 项目概况](#15-项目概况)
    - [1.5.1 具有弹性验证器的集成多链系统](#151-具有弹性验证器的集成多链系统)
    - [1.5.2 让多链编程成为主流](#152-让多链编程成为主流)
    - [1.5.3 弹性验证器的稳健去中心化](#153-弹性验证器的稳健去中心化)
- [2 Linera多链协议](#2-linera多链协议)
  - [2.1 参与者：用户、验证者、链所有者](#21-参与者用户验证者链所有者)
  - [2.2 安全模型](#22-安全模型)
  - [2.3 符号](#23-符号)
  - [2.4 微链](#24-微链)
  - [2.5 跨链请求](#25-跨链请求)
  - [2.6 链状态](#26-链状态)
  - [2.7 块执行](#27-块执行)
  - [2.8 客户端/验证器交互](#28-客户端验证器交互)
  - [2.9 核心协议的扩展](#29-核心协议的扩展)
- [3 多链协议分析](#3-多链协议分析)
  - [3.1 响应能力](#31-响应能力)
  - [3.2 可扩展性](#32-可扩展性)
  - [3.3 安全](#33-安全)
- [4 在 Linera中构建Web3应用程序](#4-在-linera中构建web3应用程序)
  - [4.1 创建应用程序](#41-创建应用程序)
  - [4.2 多链部署](#42-多链部署)
  - [4.3 跨链通信](#43-跨链通信)
  - [4.4 本地可组合性](#44-本地可组合性)
  - [4.5 临时链](#45-临时链)
- [5 去中心化](#5-去中心化)
  - [5.1 委托权益证明](#51-委托权益证明)
  - [5.2 可审计性](#52-可审计性)
- [6 结论](#6-结论)

# 1 介绍

## 1.1 Web3 中对可预测性能和响应能力的需求

得益于区块链技术，下一代互联网web3将为用户提供新一代资产感知应用程序，并让他们对数字经济有更民主的控制。然而，开发具有良好用户体验的web3应用程序目前是一项具有挑战性的任务。问题之一是大规模的可靠性和响应能力：当太多用户活跃时，区块链可能会停止响应或要求惩罚性费用。一般来说，应用程序开发人员希望他们的基础设施编程接口易于使用和可预测，而忽略其他应用程序引起的流量。集中式API提供者[^29]已被提议用于促进在流行区块链之上的编程，但此类提供者需要被信任并且不会提高底层区块链的性能和费用。 Linera旨在通过提供可大规模保证性能和响应能力的区块链基础设施来缩小集中式和分散式应用程序之间的差距。

## 1.2 区块空间稀缺问题

传统区块链在费用和延迟方面具有不可预测的最坏情况结果的主要原因可以解释为区块空间稀缺问题。即，在由单个块链组成的区块链中，用户必须竞争才能将他们的交易选择到下一个块中。然而，与此同时，区块的生产率和大小受到共识协议、网络和执行层性能的限制。因此，在流量高峰期间(例如，NFT空投)，用户可能会被其他人高估或被延迟很长时间——在此期间，他们实际上无法使用基础设[^21]。

## 1.3 现有方法的缺点

不出所料，多年来已经提出了许多区块链基础设施，并考虑到了可扩展性的改进。我们在这里提供了最常见方法的高级摘要，但并未力求面面俱到。

<b>更快的单链。</b>单个链中块的生产速率通常受到验证器之间的数据传播延迟的限制 [^17]。从历史上看，块大小一直是第一个要调整的参数，以根据安全要求和网络约束 [^17、^19] 最大化交易吞吐量。由于BFT共识协议的最新进展（例如 [^21]），如今交易速率的新瓶颈似乎是交易的顺序执行而不是共识排序。

&ensp;&ensp;&ensp;&ensp;预计一个区块中包含的许多交易在实践中应该是独立的，最近的几个项目已经开发了能够在几个处理单元上并行执行交易的子集的架构[^18]。虽然这肯定会导致更高的交易率，但这种系统的特点仍然是每秒钟最大交易数在6位数以下。此外，有效的交易率在很大程度上取决于每个区块中实际独立交易的比例[^25]。总而言之，这使得在不对其他用户的活动进行任何假设的情况下，不可能事先为一个用户保证费用和/或延迟。

&ensp;&ensp;&ensp;&ensp;最后，在高吞吐量链中，由于执行的CPU需求和数据同步的网络需求，审计验证器变得更加困难。具体地说，连续交易的绝对数量可能会阻止只使用普通硬件的社区成员以足够快的速度快速验证交易，从而以有意义的方式验证验证器的工作[^23]。

<b>区块链分片。</b>解决区块链可扩展性的另一个流行方向是将执行状态划分为固定数量的并行链，每个并行链由一组单独的验证器独立运行。这称为*区块链分片*。

&ensp;&ensp;&ensp;&ensp;虽然这种方法仍在不断改进，但它在历史上一直面临着一些挑战。首先，使用不同的验证器集会产生安全权衡，因为攻击者可能会选择性地攻击系统中最弱的一组（例如，铸造硬币）。其次，重组分片，即用户帐户跨链分布的方式，是一项复杂的操作，需要广泛的网络通信[^31]。最后，当分片数量增加以支持额外流量时，需要交换的跨链消息数量也会增加[^25]。在每个分片都有一组独立的验证器的系统中，跨链消息会产生显着的延迟，最终抵消添加新链的影响 [^29][^31]。

<b>rollups。</b>最后，解决区块空间稀缺性的一种流行方法是rollup协议，无论是乐观的还是基于有效性证明（又名ZK rollup）[^10]。在高层次上，乐观和有效性（“ZK”）汇总都包含一个第2层协议，该协议构建一系列大块，旨在在第 1层执行、压缩和确认。不幸的是，确认交易的过程在这两种情况下，第1层都需要很长时间。乐观汇总必须等待几天才能解决争议。有效性汇总必须一次压缩许多第 2层交易以支付第1层gas。在实践中，收集足够的第2层交易、计算有效性证明和归档交易以强制执行严格的数据可用性需要每个第2层块几个小时。

&ensp;&ensp;&ensp;&ensp;较长的第1层确认时间可能会鼓励某些用户接受安全权衡并相信某些应用程序的第2层的最终性。一般来说，汇总必须被信任以执行协议(*i.e. for liveness*)并公平地选择交易（参见矿工可提取价值 [^14]）。在最近设计去中心化汇总协议的努力中可以看出这种担忧 [^27]。

## 1.4 我们的任务

受这些观察的启发，Linera 项目的创建是为了基于三个关键原则开发一种新型的web3基础设施：

    （i.） 构建具有可预测性能和响应能力的安全基础架构——通过在一组弹性验证器中运行多个链；
    （ii.） 启用可扩展 web3 应用程序的丰富生态系统——通过在新的执行层上工作，使多链编程成为主流；
    （iii.） 最大化去中心化——通过确保弹性验证器得到最佳激励和社区大规模审计。

## 1.5 项目概况

Linera致力于为区块链社区提供以下创新。

### 1.5.1 具有弹性验证器的集成多链系统

为了实现我们对具有可预测性能和大规模响应能力的 web3 基础设施的愿景，我们开发了一种新的多链协议，旨在利用现代云基础设施：

（1）在 Linera 中，验证器是一种类似 web2 的弹性服务，可并行验证和执行多条链中的交易块。因为 Linera 系统中存在的链（活动和非活动）的数量是无限的，所以我们也称它们为微链。

（2）使用新区块积极扩展微链的任务与验证或执行是分开的，由每条链的所有者承担。鼓励每个 Linera 用户创建一个他们拥有的链并将他们的帐户放在那里。

（3）每个验证者都管理着所有的微链。（我们称之为集成多链方法。微链使用异步消息进行交互，否则独立运行。因此，验证者可以通过在许多内部工作人员（又名分片）之间分配他们的工作量来弹性扩展。使用每个验证器的内部网络可以有效地实现链之间的异步消息。

（4）微链在接受新区块的方式上可能有所不同。在扩展自己的链时，用户使用受可靠广播 [^6][^11] 启发的低延迟、无内存池协议直接向验证器提交新块。需要用户之间更复杂交互的应用程序也可能依赖于按需创建的临时微链。在实践中，只有 Linera基础设施拥有的公共微链才需要完整的拜占庭容错共识协议[^11]。

（5）通常，验证者不进行交互——基础设施拥有的公共链除外。验证者之间的微链同步委托给链所有者。这意味着不活跃的微链（那些不创建块的）除了存储之外对验证者没有成本。

&ensp;&ensp;&ensp;&ensp;使用弹性验证器是Linera的一个独特假设。我们打算为Linera社区支持新验证者可以选择的各种云提供商。Linera最初受到Meta[^6]开发的学术低延迟支付协议FastPay的启发。 Linera通过将用户帐户转变为微链、添加智能合约以及支持链间的任意异步消息，显着推广了FastPay。 Linera多链协议的更详细描述在第 2节中给出。我们在第3节中分析了该协议。

### 1.5.2 让多链编程成为主流

Linera将许多链集成到一组独特的验证器中。由于每个验证器的内部网络，这极大地促进了跨链通信。第一次，各种 web3 应用程序有机会通过利用廉价高效的多链架构进行弹性扩展。为了促进多链编程的采用，我们做出了以下设计选择：

（6）Linera 的执行模型被设计为与语言无关且对开发人员友好。 Linera的初始 SD将基于Wasm并以Rust编程语言为目标。

（7）Linera应用程序是可组合的和多链的。一旦创建了应用程序，它就可以在任何链上按需运行。同一应用程序的运行实例使用异步消息和发布/订阅渠道跨链协调。在同一微链中运行的应用程序使用跨合约调用和临时会话对象进行交互。

&ensp;&ensp;&ensp;&ensp;Linera 中的会话对象的灵感来自Move语言[^8]中的资源。 Move中的静态类型资源已被提议用于帮助实现可组合性[^24]。在Linera中，类资源的可组合性是通过使用会话句柄和运行时检查来实现的。例如，为了发送代币，Linera合约将能够转移包含代币的临时会话的所有权。

&ensp;&ensp;&ensp;&ensp;一般来说，建立一个大型的开发者社区是采用区块链基础设施的一个主要因素。由于Wasm 态系统不断改进其多语言工具 [^2]，它为 Linera 提供了长期服务于多个开发者社区的可能性。有关 Linera 编程模型的更详细讨论，请参阅第4节。

### 1.5.3 弹性验证器的稳健去中心化

经典的“区块链三难困境”[^9] 断言同时实现可扩展性、安全性和去中心化是困难的。虽然这种观察对于固定容量的验证器肯定是成立的，但我们认为在定义和实施令人满意的弹性验证器去中心化概念方面还没有做出足够的努力。

（8）“区块链” [^9]断言实现扩展性，安全性去中心化是是困难困难的的。虽然虽然这种观察对于对于固定固定固定容量容量的的验证器肯定和实施令人们满意的弹性试验去中心化概念方面还没有做足足够的努力。

（9）微链被设计为可独立审计。这意味着Linera作为一个整体将由社区以分布式方式进行审计，仅使用商品硬件。

&ensp;&ensp;&ensp;&ensp;使用大型验证器提高性能并使用社区维护去中心化区块链社区在汇总[^9]的背景下讨论了驱动审计员。随着Linera项目的进展，我们将继续关注有效性（“ZK”）证明和链压缩领域的技术进步。Linera 的去中心化将在第5节中进一步讨论。

# 2 Linera多链协议

我们现在介绍位于Linera基础设施核心的多链协议。该技术描述旨在说明协议的主要思想，但并非详尽无遗。我们在第3节中非正式地分析协议，并在第4节中讨论编程模型。

## 2.1 参与者：用户、验证者、链所有者

Linera 协议旨在提供一个计算基础设施，开发人员可以在其中创建去中心化应用程序，最终用户可以安全高效地与它们进行交互。

&ensp;&ensp;&ensp;&ensp;与区块链系统一样，Linera中应用程序的状态在多个称为验证器的部分受信任节点之间进行复制。修改应用程序的状态是通过将交易插入新块并将新块提交给验证器来完成的。

&ensp;&ensp;&ensp;&ensp;为了支持可扩展性要求，Linera从一开始就被设计为一个集成的多链系统：块不是使用单链，而是组织在许多平行的块链中，称为微链。这意味着Linera应用程序的状态通常跨链分布。重要的是，除非正在进行重新配置（第 2.9 节），否则所有微链都使用一组验证器。

在Linera中，使用新区块扩展链的任务与区块验证是分开的。它由链的所有者承担。实际上，链的所有者可以是协议中的任何参与者。因为Linera验证器充当区块验证服务，所以链所有者也可以称为客户。链所有者的例子包括：

- 希望在不同应用程序中更好地控制其帐户的最终用户；
- 希望操作临时链（例如原子交换）的最终用户；
- 希望发布代码或管理应用程序的开发人员；
- 验证者出于基础设施目的共同运行一条链。
  
最后一个用例是Linera如何管理当前的一组验证器，也称为委员会。 Liner的编程模型在第4节中介绍。审计员的额外角色在第 5 节中讨论。

## 2.2 安全模型

## 2.3 符号

## 2.4 微链

## 2.5 跨链请求

## 2.6 链状态

## 2.7 块执行

## 2.8 客户端/验证器交互

## 2.9 核心协议的扩展

# 3 多链协议分析

在本节中，我们分析了Linera区块链设定的设计目标，包括响应能力、可扩展性和安全保证。

## 3.1 响应能力

## 3.2 可扩展性

## 3.3 安全

# 4 在 Linera中构建Web3应用程序

Linera的编程模型旨在为应用程序开发人员提供丰富的、与语言无关的可组合性，同时利用微链进行扩展。

## 4.1 创建应用程序

## 4.2 多链部署

## 4.3 跨链通信

## 4.4 本地可组合性

## 4.5 临时链

# 5 去中心化

Linera鼓励验证者使用云基础设施来解锁弹性扩展并从标准生产环境中受益。为了最大化去中心化，Linera依赖于两个关键特性：委托权益证明 (DPoS) 和社区审计。

## 5.1 委托权益证明

为了确保系统的长期安全，Linera依赖于委托权益证明 (DPoS)：验证者的投票权是他们在系统中的权益以及最终用户委托给他们的权益的函数。为了使DPo 正常运行，用户必须能够更改他们的委托偏好，并且验证者必须有一个自动程序来加入和离开系统。这两种操作都需要一个公共链，任何用户都可以在其中提交交易。重新配置验证器还需要为每条链精心设计迁移协议。这两种机制都在第2.9节中进行了概述。

&ensp;&ensp;&ensp;&ensp;代币授权和经济学将在单独的文件中更加精确。为了解决远程攻击——旧委员会变得腐败[^16]——，Linera 允许微链拒绝来自不再受信任的委员会的跨链消息（*例如支付*）（参见第 2.9 节）。

## 5.2 可审计性

审计区块链传统上需要运行一个完整的节点，该节点在本地保存整个交易历史的副本。但是，在高吞吐量系统的情况下，这可能需要大量的磁盘空间和CPU资源。当普通用户（使用商品硬件的用户）需要数天或数周时间来全面审核去中心化系统时，社区可能无法可靠地阻止流氓验证者联盟更改协议。轻客户端 [^13] 减少资源使用，但只检查块头，不提供相同级别的验证。

&ensp;&ensp;&ensp;&ensp;相比之下，微链方法使社区可以持续审计 Linera验证器。在Linera中，审计员类似于客户端（第 2.8 节），因为它只需要跟踪一小部分微链。因为 Linera 的可扩展性依赖于拥有许多链而不是更大的块，所以在商品硬件上实时重放单个链的执行总是可行的。

&ensp;&ensp;&ensp;&ensp;为了让Linera社区持续验证所有链，可以在共享分布式存储（例如 IPFS [3]之上放置一个分布式协议，如下所示。执行链中的块允许验证执行状态和传出消息。通常应将块标记为已审核，并将传出消息在分布式存储中建立索引。要完成链的验证，客户端还必须验证每个传入消息确实是由其发送者链产生的。这可以通过在共享存储中查找传入消息以查看它们是否已经过验证来完成，否则，安排它们的验证。最后,Linera微链的分布式验证类似于经典的图索引算法，例如 PageRank [4]。

# 6 结论

Linera旨在提供首个在互联网规模上具有可预测性能、响应能力和安全性的多链基础设施。为此，Linera引入了在同一组验证器中运行许多称为微链的平行链的想法，并使用每个验证器的内部网络在链之间快速传递异步消息。这种架构有很多优点：

- __弹性缩放。__ 在Linera中，可扩展性是通过添加链而不是通过增加块的大小或速率来获得的。每个验证者都可以随时添加和删除容量（也称为内部工作人员），以维持多链应用程序的标称性能。
- __响应能力。__ 当微链由单个用户操作时，Linera使用受可靠广播[^6][^11] 启发的简化的无内存池共识协议。这减少了块延迟并最终使Web3应用程序更具响应性。
- __可组合性。__ 与其他多链系统相比，低块延迟也有助于可组合性：它允许来自另一个链的异步消息的接收者通过添加新块来快速响应。
- __链安全。__ 与传统的多链系统相比，在同一组验证器中运行所有微链的一个好处是创建链不会影响Linera的安全模型。
- __去中性化。__ Linera 依靠委托权益证明 (DPoS) 来确保安全。每个微链都可以在商品硬件上单独执行。这允许客户和审计员持续运行他们自己的验证并让验证者负责。
- __语言无关。__ Linera的编程模型不依赖于特定的编程语言。经过深思熟虑，我们决定在Linera的初始执行层集中精力在Wasm和Rust上。
  
在未来的报告中，我们将正式化协议以支持多所有者链以及第2.9节中提到的其他扩展。特别是，我们计划利用跨链消息在我们现有的多链基础设施之上整合最新的基于DAG的共识机制[^15][^21]。我们还计划分别描述验证者公平报酬和用户激励的经济模型。Linera停用和存档微链的能力提供了一个优雅的场所来控制未来验证器的存储成本。总的来说，我们预计Linera的集成架构和验证器交互的最小化在优化大规模运行验证器的成本方面将非常有帮助。


[^1]: 法律免责声明：本文件及其内容不是出售任何代币的要约，也不是购买任何代币的要约邀请。我们发布本白皮书只是为了接收公众的反馈和意见。本文件中的任何内容都不应被阅读或解释为对 Linera 基础设施或其代币（如果有）将如何开发、利用或增值的保证或承诺。 Linera 仅概述了其当前的计划，该计划可能会自行决定更改，其成功将取决于其无法控制的许多因素。此类前瞻性陈述必然涉及已知和未知的风险，可能导致未来期间的实际业绩和结果与我们在本文件中描述或暗示的内容存在重大差异。 Linera 不承担更新其计划的义务。无法保证文件中的任何陈述将被证明是准确的，因为实际结果和未来事件可能存在重大差异。请不要过分依赖未来的陈述。

[^2]: WebAssembly. <https://webassembly.org/>.

[^3]: The Bytecode Alliance. <https://bytecodealliance.org/>, 2022.

[^4]: The InterPlanetary File System. <https://ipfs.tech/>, 2022.

[^5]: The PageRank algorithm. <https://en.wikipedia.org/wiki/PageRank>, 2022.

[^6]: Gul Agha. Actors: a model of concurrent computation in distributed systems. MIT press, 1986.

[^7]: Mathieu Baudet, George Danezis, and Alberto Sonnino. Fastpay: High-performance Byzantine fault tolerant
settlement. In Proceedings of the 2nd ACM Conference on Advances in Financial Technologies, pages 163–177, 2020.

[^8]: Mathieu Baudet, Alberto Sonnino, Mahimna Kelkar, and George Danezis. Zef: Low- latency, scalable, private payments. arXiv preprint arXiv:2201.05671, 2022.

[^9]: Sam Blackshear, Evan Cheng, David L. Dill, Victor Gao, Ben Maurer, Todd Nowacki, Alistair Pott, Shaz Qadeer, Rain, Dario Russi, Stephane Sezer, Tim Zakian, and Runtian Zhou. Move: A language with programmable resources. <https://diem-developers-components.netlify.app/papers/> diem-move-a-language-with-programmable-resources/2020-05-26.pdf, 2020.

[^10]: Vitalik Buterin. Endgame. <https://vitalik.ca/general/2021/12/06/endgame>. html, 2021.

[^11]: Vitalik Buterin. An incomplete guide to rollups. <https://vitalik.ca/general/> 2021/01/05/rollup.html, 2021.

[^12]: Christian Cachin, Rachid Guerraoui, and Lu ́ıs Rodrigues. Introduction to reliable and secure distributed programming. Springer Science & Business Media, 2011.

[^13]: Miguel Castro, Barbara Liskov, et al. Practical Byzantine fault tolerance. In OsDI, volume 99, pages 173–186, 1999.

[^14]: Panagiotis Chatzigiannis, Foteini Baldimtsi, and Konstantinos Chalkias. Sok: Blockchain light clients. Cryptology ePrint Archive, 2021.

[^15]: Philip Daian, Steven Goldfeder, Tyler Kell, Yunqi Li, Xueyuan Zhao, Iddo Bentov, Lorenz Breidenbach, and Ari Juels. Flash boys 2.0: Frontrunning in decentralized ex- changes, miner extractable value, and consensus instability. In 2020 IEEE Symposium on Security and Privacy (SSP’20), pages 910–927. IEEE, 2020.

[^16]: George Danezis, Lefteris Kokoris-Kogias, Alberto Sonnino, and Alexander Spiegelman. Narwhal and Tusk: a DAG-based mempool and efficient BFT consensus. In Proceedings of the Seventeenth European Conference on Computer Systems, pages 34–50, 2022.

[^17]: Evangelos Deirmentzoglou, Georgios Papakyriakopoulos, and Constantinos Patsakis. A survey on long-range attacks for proof of stake protocols. IEEE Access, 7:28712–28725, 2019.

[^18]: Ittay Eyal, Adem Efe Gencer, Emin Gu ̈n Sirer, and Robbert Van Renesse. {Bitcoin- NG}: A scalable blockchain protocol. In 13th USENIX symposium on networked sys- tems design and implementation (NSDI 16), pages 45–59, 2016.

[^19]: Rati Gelashvili, Alexander Spiegelman, Zhuolun Xiang, George Danezis, Zekun Li, Dahlia Malkhi, Yu Xia, and Runtian Zhou. Block-STM: Scaling blockchain execution by turning ordering curse to a performance blessing, 2022.

[^20]: Arthur Gervais, Ghassan O Karame, Karl Wu ̈st, Vasileios Glykantzis, Hubert Ritzdorf, and Srdjan Capkun. On the security and performance of proof of work blockchains. In Proceedings of the 2016 ACM SIGSAC conference on computer and communications security, pages 3–16, 2016.

[^21]: Arthur Gervais, Hubert Ritzdorf, Ghassan O Karame, and Srdjan Capkun. Tampering with the delivery of blocks and transactions in bitcoin. In Proceedings of the 22nd ACM SIGSAC Conference on Computer and Communications Security, pages 692–705, 2015.

[^22]: Neil Giridharan, Lefteris Kokoris-Kogias, Alberto Sonnino, and Alexander Spiegelman. Bullshark: DAG BFT protocols made practical. arXiv preprint arXiv:2201.05677, 2022.

[^23]: Andreas Haas, Andreas Rossberg, Derek L Schuff, Ben L Titzer, Michael Holman, Dan Gohman, Luke Wagner, Alon Zakai, and JF Bastien. Bringing the web up to speed with webAssembly. In Proceedings of the 38th ACM SIGPLAN Conference on Programming Language Design and Implementation, pages 185–200, 2017.

[^24]: Jae-Yun Kim, Junmo Lee, Yeonjae Koo, Sanghyeon Park, and Soo-Mook Moon. Ethanos: efficient bootstrapping for full nodes on account-based blockchain. In Pro- ceedings of the Sixteenth European Conference on Computer Systems, pages 99–113, 2021.

[^25]: Jae-Yun Kim, Junmo Lee, Yeonjae Koo, Sanghyeon Park, and Soo-Mook Moon. Ethanos: efficient bootstrapping for full nodes on account-based blockchain. In Pro- ceedings of the Sixteenth European Conference on Computer Systems, pages 99–113, 2021.

[^26]: Michal Kr ́ol, Onur Ascigil, Sergi Rene, Alberto Sonnino, Mustafa Al-Bassam, and Etienne Rivi`ere. Shard scheduler: object placement and migration in sharded account- based blockchains. In Proceedings of the 3rd ACM Conference on Advances in Financial Technologies, pages 43–56, 2021.

[^27]: Du Mingxiao, Ma Xiaofeng, Zhang Zhe, Wang Xiangwei, and Chen Qijun. A review on consensus algorithm of blockchain. In 2017 IEEE international conference on systems, man, and cybernetics (SMC), pages 2567–2572. IEEE, 2017.

[^28]: nanfengpo. A design of decentralized ZK-rollups based on EIP-4844. https:// ethresear.ch/t/a-design-of-decentralized-zk-rollups-based-on-eip-4844/ 12434, 2022.

[^29]: Slashdot. Best blockchain apis of 2022. <https://slashdot.org/software/> blockchain-apis/, 2022.

[^30]: Alberto Sonnino. Chainspace: A sharded smart contract platform. In Network and Distributed System Security Symposium 2018 (NDSS 2018), 2018.

[^31]: Gavin Wood et al. Ethereum: A secure decentralised generalised transaction ledger. Ethereum project yellow paper, 151(2014):1–32, 2014.

[^32]: Mahdi Zamani, Mahnush Movahedi, and Mariana Raykova. Rapidchain: Scaling blockchain via full sharding. In Proceedings of the 2018 ACM SIGSAC conference on computer and communications security, pages 931–948, 2018.
