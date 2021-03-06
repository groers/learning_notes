# Fabric笔记

[TOC]

# Fabric特性

- Hyperledger Fabric 与其他区块链系统不同的地方是 **私有** 和 **许可** 。与允许未知身份参与网络的开放式非许可系统（需要诸如“工作量证明”之类的协议来验证交易并保护网络）不同，Hyperledger Fabric 网络的成员需要从可信赖的 **成员服务提供者（MSP）** 注册。

- Hyperledger Fabric 还提供创建 **通道** 的功能，允许一组参与者创建各自的交易账本。

- Hyperledger Fabric 有一个账本子系统，包括两个组件： **世界状态** 和 **交易日志** 。每个参与者都拥有他们所属的每个 Hyperledger Fabric 网络的账本副本。

  世界状态组件描述了在给定时间点的账本的状态。它是账本的数据库。交易日志组件记录产生世界状态中当前值的所有交易；这是世界状态的更新历史。然后，账本包括世界状态数据库和交易日志历史记录。在大多数情况下，链码只与账本的数据库、世界状态（例如，查询）交互，而不与交易日志交互。所有的交易，无论是**有效的**还是**无效的**，都会被添加到分布式账本中，但只有**有效**交易会更新世界状态。

# 关键概念

## 智能合约

为了支持以同样的方式更新信息，并启控制账本所有的功能（交易，查询等），区块链使用 **智能合约** 来提供对账本的受控访问。

智能合约不仅是在网络中封装和简化信息的关键机制，它还可以被编写成自动执行参与者的特定交易的合约。

例如，可以编写智能合约以规定运输物品的成本，其中运费根据物品到达的速度而变化。根据双方同意并写入账本的条款，当收到物品时，相应的资金会自动转手。

当智能合约被安装在 Peer 节点并且在通道上定义之后，它就可以被客户端应用调用了。客户端应用是通过发送交易提案给智能合约背书策略所指定的 Peer 的节点方式来调用智能合约的。这个交易的提案会作为智能合约的输入，智能合约会使用它来生成一个背书交易响应，这会由 Peer 节点返回给客户端应用。

## 共识

保持账本在整个网络中同步的过程称为 **共识** 。

交易必须按照发生的顺序写入账本，即使它们可能位于网络中不同的参与者集合之中。为此，必须建立交易的顺序，且必须采用一种方法来拒绝错误（或恶意）插入到账本中的非法交易。

## 链码定义

每个组织需要批准一个**链码定义**，和一系列参数来定义在一个通道中链码应该被如何使用。

在链码定义提供的信息中最重要的部分就是**背书策略**。它描述了在交易被其他的组织接受并存储在他们的账本副本上之前，哪些组织必须要同意此交易。

## 锚节点

如果一个 Peer 节点需要同另一个组织的 Peer 节点通信的话，它可以使用对方组织通道配置中定义的锚节点。一个组织可以拥有0个或者多个锚节点，并且一个锚节点能够帮助很多不同的跨组织间的通信

## 背书

Peer 节点通过向提案的响应添加自己的数字签名的方式提供背书，并且使用它的私钥为整个的负载提供签名。后这个背书会被用于证明这个组织的 Peer 节点生成了一个特殊的响应。在我们的例子中，如果 Peer 节点 P1 是属于组织 Org1 的话，背书 E1 就相当于一个数字证明——”在账本 L1 上的交易 T1 的响应 R1 已经被 Org1 的 Peer 节点 P1 同意了！”。

背书策略是 Hyperledger Fabric 与以太坊（Ethereum）或比特币（Bitcoin）等其他区块链的区别所在。**在这些区块链系统中，网络上的任何节点都可以生成有效的交易**。而 Hyperledger Fabric 更真实地模拟了现实世界；交易必须由 Fabric 网络中受信任的组织验证。例如，一个政府组织必须签署一个有效的 `issueIdentity` 交易，或者一辆车的 `买家` 和 `卖家` 都必须签署一个 `车辆` 转移交易。背书策略的设计旨在让 Hyperledger Fabric 更好地模拟这些真实发生的交互。

背书节点验证（1）交易提案的格式完整，（2）且验证该交易提案之前没有被提交过（重放攻击保护），（3）验证签名是有效的（使用 MSP），（4）验证发起者（在这个例子中是客户端 A）有权在该通道上执行该操作（也就是说，每个背书节点确保发起者满足通道 *Writers* 策略）。背书节点将交易提案输入作为调用的链码函数的参数。然后根据当前状态数据库执行链码，生成交易结果，包括响应值、读集和写集（即表示要创建或更新的资产的键值对）。目前没有对账本进行更新。这些值以及背书节点的签名会一起作为“提案响应”返回到 SDK，SDK 会为应用程序解析该响应。

## 交易

- 在第一个阶段，应用程序会跟*背书节点*的子集一起工作，其中的每个节点都会向应用程序为提案的账本更新提供背书，但是不会将提案的更新应用到他们的账本副本上。
- 在第二个阶段，这些分散的背书会被搜集到一起当做交易被打包进区块中。
- 在最后一个阶段，这些区块会被分发回每个 Peer 节点，在这些 Peer 节点上每笔交易在被应用到 Peer 节点的账本副本之前会被验证。

### 阶段3：验证和提交

如果一笔交易被正确的背书，Peer 节点会尝试将它应用到账本中。为了做这个，Peer 节点必须要进行账本一致性检查来验证当前账本中的状态同应用了更新提案后的账本是能够兼容的。这可能不是每次都是有可行的，即使交易已经被完整地背书过了。比如，另外一笔交易可能会更新账本中的同一个资产，这样的话交易的更新就不会是有效的了，因此也就不会被应用。通过这种方式，账本在通道中的每个 Peer 节点都是保持一致的，因为他们中每个都在遵守相同的验证规则。

一项交易被分发给网络中的所有节点，各节点通过两个阶段对其进行**验证**。首先，根据背书策略检查交易，确保该交易已被足够的组织签署。其次，继续检查交易，以确保当该交易在受到背书节点签名时它的交易读集与世界状态的当前值匹配，并且中间过程中没有被更新。如果一个交易通过了这两个测试，它就被标记为**有效**。所有交易，不管是**有效的**还是**无效的**，都会被添加到区块链历史中，但是仅**有效的**交易才会更新世界状态。

## 区块

它由三个部分组成

- **区块头**

  这个部分包含三个字段，这些字段是在创建一个区块时候被写入的。

  - **区块编号**：编号从0（初始区块）开始，每在区块链上增加一个新区块，编号的数字都会加1。
  - **当前区块的哈希值**：当前区块中包含的所有交易的哈希值。
  - **前一个区块头的哈希值**：区块链中前一个区块头的哈希值。

  这些字段是通过在内部对区块数据进行加密哈希而生成的。它们确保了每一个区块和与之相邻的其他区块紧密相连，从而组成一个不可更改的账本。

  ![ledger.blocks](https://hyperledger-fabric.readthedocs.io/zh_CN/latest/_images/ledger.diagram.4.png) *区块头详情：区块 B2 的区块头 H2 包含了区块编号 2，当前区块数据 D2 的哈希值 CH2，以及前一个区块头 H1 的哈希值。*

- **区块数据**

  这部分包含了一个有序的交易列表。区块数据是在排序服务创建区块时被写入的。这些交易的结构很复杂但也很直接，我们会在[后边](https://hyperledger-fabric.readthedocs.io/zh_CN/latest/ledger/ledger.html#交易)进行讲解。

- **区块元数据**

  这个部分包含了区块被写入的时间，还有区块写入者的证书、公钥以及签名。随后，区块的提交者也会为每一笔交易添加一个有效或无效的标记，但由于这一信息与区块同时产生，所以它不会被包含在哈希中。

### 交易部分

正如我们所看到的，交易记录了世界状态发生的更新。让我们来详细了解一下这种把交易包含在区块中的**区块数据**结构。

![ledger.transaction](https://hyperledger-fabric.readthedocs.io/zh_CN/latest/_images/ledger.diagram.5.png) *交易详情：交易 T4 位于区块 B1 的区块数据 D1 中，T4包括的内容如下：交易头 H4，一个交易签名 S4，一个交易提案 P4，一个交易响应 R4 和一系列背书 E4。*

在上面的例子中，我们可以看到以下字段：

- **头**

  这部分用 H4 表示，它记录了关于交易的一些重要元数据，比如，相关链码的名字以及版本。

- **签名**

  这部分用 S4 表示，它包含了一个由客户端应用程序创建的加密签名。该字段是用来检查交易细节是否未经篡改，因为交易签名的生成需要用到应用程序的私钥。

- **提案**

  这部分用 P4 表示，它负责对应用程序供给智能合约的输入参数进行编码，随后该智能合约生成提案账本更新。在智能合约运行时，这个提案提供了一套输入参数，这些参数同当前的世界状态一起决定了新的账本世界状态。

- **响应**

  这部分用 R4 表示，它是以**读写集** （RW-set）的形式记录下世界状态之前和之后的值。交易响应是智能合约的输出，如果交易验证成功，那么该交易会被应用到账本上，从而更新世界状态。

- **背书**

  就像 E4 显示的那样，它指的是一组签名交易响应，这些签名都来自背书策略规定的相关组织，并且这些组织的数量必须满足背书策略的要求。你会注意到，虽然交易中包含了多个背书，但它却只有一个交易响应。这是因为每个背书都对组织特定的交易响应进行了有效编码，那些不完全满足背书的交易响应肯定会遭到拒绝、被视为无效，而且它们也不会更新世界状态，所以没必要放进交易中。

  在交易中只包含一个交易响应，但是会有多个背书。这是因为每个背书包含了它的组织特定的交易响应，这意味着不需要包含任何没有有效的背书的交易响应，因为它会被作为无效的交易被拒绝，并且不会更新世界状态。

# gossip

不是每个 Peer 节点都需要连接到排序节点——Peer 节点可以使用 **gossip** 协议将区块的信息发送给其他 Peer 节点

基于 gossip 的数据传播协议在 Fabric 网络中有三个主要功能：

1. 通过持续的识别可用的成员节点来管理节点发现和通道成员，还有检测离线节点。
2. 向通道中的所有节点传播账本数据。所有没有和当前通道的数据同步的节点会识别丢失的区块，并将正确的数据复制过来以使自己同步。
3. 通过点对点的数据传输方式，使新节点以最快速度连接到网络中并同步账本数据。

Peer 节点基于 gossip 的数据广播操作接收通道中其他的节点的信息，然后将这些信息随机发送给通道上的一些其他节点，随机发送的节点数量是一个可配置的常量。Peer 节点可以用“拉”的方式获取信息而不用一直等待。这是一个重复的过程，以使通道中的成员、账本和状态信息同步并保持最新。在分发新区块的时候，通道中**主节点**从排序服务拉取数据然后分发给它所在组织的节点。

## 锚节点

gossip 利用锚节点来保证不同组织间的互相通信。

当提交了一个包含锚节点更新的配置区块时，Peer 节点会连接到锚节点并获取它所知道的所有节点信息。一个组织中至少有一个节点连接到了锚节点，锚节点就可以获取通道中所有节点的信息。因为 gossip 的通信是固定的，而且 Peer 节点总会被告知它们不知道的节点，所以可以建立起一个通道上成员的视图。

例如，假设我们在一个通道有三个组织 A、B 和 C，组织 C 定义了锚节点 peer0.orgC。当 peer1.orgA 连接到 peer0.orgC 时，它将会告诉 peer0.orgC 有关 peer0.orgA 的信息。稍后等 peer1.orgB 连接到 peer0.orgC 时，后者也会告诉前者关于 peer0.orgA 的信息。在这之后，组织 A 和组织 B 可以开始直接交换成员信息而无需借助 peer0.orgC 了。

由于组织间的通信依赖于 gossip，所以在通道配置中必须至少有一个锚节点。为了系统的可用性和冗余性，我们强烈建议每个组织都提供自己的一些锚节点。注意，锚节点不一定和主节点是同一个节点。

### Gossip消息

在线的节点通过持续广播“存活”消息来表明其处于可用状态，如果没有节点收到某个节点的存活信息，这个“死亡”的节点会被从通道成员关系中剔除。

除了自动转发接收到的消息之外，状态协调进程还会在每个通道上的 Peer 节点之间同步 **世界状态**。每个 Peer 节点都持续从通道中的其他节点拉取区块，来修复他们缺失的状态。



# 私有数据

如果一个通道上的一组组织需要对该通道上的其他组织保持数据私有，则可以选择创建一个新通道，其中只包含需要访问数据的组织。但是，在每种情况下创建单独的通道会产生额外的管理开销（维护链码版本、策略、MSP等），并且不能在保留一部分数据私有的同时，可以让所有通道参与者看到该事务。

这就是为什么从v1.2开始，Fabric 提供了创建**私有数据集合**的功能，它允许在通道上定义的组织子集能够背书、提交或查询私有数据，而**无需创建单独的通道**。

## 私有数据集合

集合是两个元素的组合:

1. **实际的私有数据**，通过 [Gossip 协议](https://hyperledger-fabric.readthedocs.io/zh_CN/latest/gossip.html)点对点地发送给授权可以看到它的组织。私有数据保存在被授权的组织的节点上的私有数据库上，它们可以被授权节点的链码访问。排序节点不能影响这里也不能看到私有数据。注意，由于 gossip 以点对点的方式向授权组织分发私有数据，所以必须设置通道上的锚节点，也就是每个节点上的 CORE_PEER_GOSSIP_EXTERNALENDPOINT 配置，以此来引导跨组织的通信。
2. **该数据的 hash 值**，该 hash 值被背书、排序之后写入通道上每个节点的账本。Hash 值作为交易的证明用于状态验证，并可用于审计。

下面的图表分别说明了被授权和未被授权拥有私有数据的节点的账本内容。

![private-data.private-data](https://hyperledger-fabric.readthedocs.io/zh_CN/latest/_images/PrivateDataConcept-2.png)

如果集合成员陷入争端，或者他们想把资产转让给第三方，他们可能决定与其他参与方共享私有数据。然后，第三方可以计算私有数据的 hash，并查看它是否与通道账本上的状态匹配，从而证明在某个时间点，集合成员之间存在该状态。

### 什么时候使用一个通道内的组织集合，什么时候使用单独的通道

- 当必须将整个账本在属于通道成员的组织中保密时，使用**通道**比较合适。
- 当账本必须共享给一些组织，但是只有其中的部分组织可以在交易中使用这些数据的一部分或者全部时，使用**集合**比较合适。此外，由于私有数据是点对点传播的，而不是通过块传播的，所以在交易数据必须对排序服务节点保密时，应该使用私有数据集合。

### 私有数据交易流程

1. 客户端应用程序提交一个提案请求，让属于授权集合的背书节点执行链码函数（读取或写入私有数据）。 私有数据，或用于在链码中生成私有数据的数据，被发送到提案的 `transient`（瞬态）字段中。
2. 背书节点模拟交易，并将私有数据存储在 `瞬态数据存储`（ transient data store ，节点的本地临时存储）中。它们根据组织集合的策略将私有数据通过[gossip](https://hyperledger-fabric.readthedocs.io/zh_CN/latest/gossip.html)分发给授权的 Peer 节点。
3. 背书节点将提案响应发送回客户端。提案响应中包含经过背书的读写集，这其中包含了公共数据，还包含任何私有数据键和值的 hash。*私有数据不会被发送回客户端*。更多关于带有私有数据的背书的信息，请查看[这里](https://hyperledger-fabric.readthedocs.io/zh_CN/latest/private-data-arch.html#endorsement)。
4. 客户端应用程序将交易（包含带有私有数据 hash 的提案响应）提交给排序服务。带有私有数据 hash 的交易同样被包含在区块中。带有私有数据 hash 的区块被分发给所有节点。这样，通道中的所有节点就可以在不知道真实私有数据的情况下，用同样的方式来验证带有私有数据 hash 值的交易。
5. 在区块提交的时候，授权节点会根据集合策略来决定它们是否有权访问私有数据。如果节点有访问权，它们会先检查自己的本地 `瞬态数据存储` ，以确定它们是否在链码背书的时候已经接收到了私有数据。如果没有的话，它们会尝试从其他已授权节点那里拉取私有数据，然后对照公共区块上的 hash 来验证私有数据并提交交易和区块。当验证或提交结束后，私有数据会被移动到这些节点私有数据库和私有读写存储的副本中。随后 `瞬态数据存储` 中存储的这些私有数据会被删除。