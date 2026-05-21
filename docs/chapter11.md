## 11. Use Cases

## 11. 应用场景

In addition to helping bitcoin scale, there are many uses for transactions on the Lightning Network:

> 除了助力比特币扩容外，闪电网络交易还拥有广泛的应用场景：

• Instant Transactions. Using Lightning, Bitcoin transactions are now nearly instant with any party. It is possible to pay for a cup of coffee with direct non-revocable payment in milliseconds to seconds.

> • **即时交易**。利用闪电网络，比特币交易几乎可以在瞬间完成。用户能在毫秒到秒级的时间内完成一单不可撤销的咖啡支付。

• Exchange Arbitrage. There is presently incentive to hold funds on exchanges to be ready for large market moves due to 3-6 block con- firmation times. It is possible for the exchange to participate in this network and for clients to move their funds on and off the exchange for orders nearly instantly. If the exchange does not have deep market depth and commits to only permitting limit orders close to the top of the order book, then the risk of coin theft becomes much lower. The exchange, in effect, would no longer have any need for a cold storage wallet. This may substantially reduce thefts and the need for trusted third party custodians.

> • **交易所套利**。目前由于充值需 3-6 个区块确认，投资者往往需将资金存放在交易所内以应对市场波动。若交易所接入闪电网络，用户可实现资金的秒级充提。
>
> 这种模式下，交易所甚至无需维持庞大的冷钱包，从而显著降低资金被盗风险，减少对第三方托管机构的依赖。

• Micropayments. Bitcoin blockchain fees are far too high to accept micropayments, especially with the smallest of values. With this sys- tem, near-instant micropayments using Bitcoin without a 3rd party custodian would be possible. It would enable, for example, paying per-megabyte for internet service or per-article to read a newspaper.

> • **微支付**（Micropayments）。比特币链上手续费过高，难以承载超小额支付。闪电网络实现了无需第三方托管的即时微支付，支持诸如按兆字节计费的上网服务或按篇计费的新闻阅读。

• Financial Smart Contracts and Escrow. Financial contracts are espe- cially time-sensitive and have higher demands on blockchain computa- tion. By moving the overwhelming majority of trustless transactions off-chain, it is possible to have highly complex transaction contract terms without ever hitting the blockchain.

> • **金融智能合约与托管**。金融合约对时效性和链上计算有极高要求。通过将绝大部分去信任化（Trustless）交易转移至链下，我们可以实现复杂的合约条款而无需将其广播至区块链。

• Cross-Chain Payments. So long as there are similar hash-functions across chains, it’s possible for transactions to be routed over multi- ple chains with different consensus rules. The sender does not have to trust or even know about the other chains – even the destination chain. Simiarly, the receiver does not have to know anything about the sender’s chain or any  other chain.  All the receiver cares about  is a conditional payment upon knowledge of a secret on their chain. Payment can be routed by participants in both chains in the hop. E.g. Alice is on Bitcoin, Bob is on both Bitcoin and X-Coin and Carol is on a hypothetical X-Coin, Alice can pay Carol without understanding the X-Coin consensus rules.

> • **跨链支付**。只要不同区块链采用相似的哈希函数，交易即可在具有不同共识规则的链间进行路由。发送方无需了解甚至无需感知目标链的存在；接收方亦然。
>
> 接收方仅关心基于其本链原像披露的条件支付。支付可在跨链跳点由同时参与两条链的节点进行转发。例如，Alice 在比特币链上，Carol 在 X 币链上，Alice 可通过跨链节点 Bob 向 Carol 发起支付，而无需理解 X 币的共识规则。
