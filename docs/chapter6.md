## 6 Blockchain Transaction Fees for Bidirectional Channels

## 6 双向支付通道中的链上交易费用

It is possible for each participant to generate different versions of transac- tions to ascribe blame as to who broadcast the transaction on the blockchain. By having knowledge of who broadcast a transaction and the ability to as- cribe blame, a third party service can be used to hold fees in a 2-of-3 multisig escrow. If one wishes to broadcast the transaction chain instead of agreeing to do a Funding Close or replacement with a new Commitment Transaction, one would communicate with the third party and broadcast the chain to the blockchain. If the counterparty refuses the notice from the third party to cooperate, the penalty is rewarded to the non-cooperative party. In most instances, participants may be indifferent to the transaction fees in the event of an uncooperative counterparty.

> 在实际交易中，每个参与者都可以创建不同版本的交易记录，以此来确定是谁将交易广播到了区块链上。通过这种方式，可以明确是哪一方负有责任，进而可以使用第三方服务来管理交易费用，这些费用被存放在一个需要两方中的三方（2-of-3）共同签名的托管账户中。如果某方选择将整个交易链广播到区块链，而不是通过协商关闭资金（Funding Close）或者用新的承诺交易（Commitment Transaction）替换原交易，他们需要和第三方沟通并执行广播。如果对方拒绝第三方的合作提议，那么不合作的一方将面临惩罚。在大多数情况下，如果对方不合作，参与方可能对交易费用不太在意。

One should pick counterparties in the channel who will be cooperative, but is not an absolute necessity for the system to function. Note that this does not require trust among the rest of the network, and is only relevant for the comparatively minor transaction fees. The less trusted party may just be the one responsible for transaction fees.

> 个人都应该挑选通道中愿意合作的对手方，但这不是系统运行的绝对必要条件。需要注意的是，这并不需要网络的其余部分之间互相信任，并且最好应用在花费交易费用相对较少的场景中。低信任值的一方可能要自己负责交易费用。

The Lightning Network fees will likely be significantly lower than blockchain transaction fees. The fees are largely derived from the time-value of locking up funds for a particular route, as well as paying for the chance of channel close on the blockchain. These should be significantly lower than on-chain transactions, as many transactions on a Lightning Network chan- nel can be settled into one single blockchain transaction. With a sufficiently robust and interconnected network, the fees should asymptotically approach negligibility for many types of transactions. With cheap fees and fast trans- actions, it will be possible to build scalable micropayments, even amongst high-frequency systems such as Internet of Things applications or per-unit micro-billing.

> 闪电网络的交易费用将会大大低于链上交易费用。这些费用主要来自于锁定特定交易路径所占用资金的时间价值，以及关闭通道时链上广播的交易费用。这些加起来应该比链上交易的费用低得多，因为商店网络上的许多交易可以被归结到单独的一笔链上交易。有了这个足够稳健和相互连接的网络，对于许多类型的交易来说，交易费用应该会逐渐忽略不计了。随着交易费用越来越低，交易速度越来越快，将有可能构建小额支付系统，甚至是高频交易系统，例如物联网应用或微支付。
