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

## 1.2 区块空间稀缺问题

## 1.3 现有方法的缺点

## 1.4 我们的任务

## 1.5 项目概况

### 1.5.1 具有弹性验证器的集成多链系统

### 1.5.2 让多链编程成为主流

### 1.5.3 弹性验证器的稳健去中心化

# 2 Linera多链协议

## 2.1 参与者：用户、验证者、链所有者

## 2.2 安全模型

## 2.3 符号

## 2.4 微链

## 2.5 跨链请求

## 2.6 链状态

## 2.7 块执行

## 2.8 客户端/验证器交互

## 2.9 核心协议的扩展

# 3 多链协议分析

## 3.1 响应能力

## 3.2 可扩展性

## 3.3 安全

# 4 在 Linera中构建Web3应用程序

## 4.1 创建应用程序

## 4.2 多链部署

## 4.3 跨链通信

## 4.4 本地可组合性

## 4.5 临时链

# 5 去中心化

## 5.1 委托权益证明

## 5.2 可审计性

# 6 结论
[^2]
[^3]
[^4]
[^5]
[^6]
[^7]
[^8]
[^9]
[^10]
[^11]
[^12]
[^13]
[^14]
[^15]
[^16]
[^17]
[^18]
[^19]
[^20]
[^21]
[^22]
[^23]
[^24]
[^25]
[^26]
[^27]
[^28]
[^29]
[^30]
[^31]
[^32]



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








