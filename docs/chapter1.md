
# 1. The Bitcoin Blockchain Scalability Problem

# 比特币区块链的可扩展性问题

The Bitcoin[1] blockchain holds great promise for distributed ledgers, but the blockchain as a payment platform, by itself, cannot cover the world’s commerce anytime in the near future. The blockchain is a gossip protocol whereby all state modifications to the ledger are broadcast to all partic- ipants. It is through this “gossip protocol” that consensus of the state, everyone’s balances, is agreed upon. If each node in the bitcoin network must know about every single transaction that occurs globally, that may create a significant drag on the ability of the network to encompass all global financial transactions. It would instead be desirable to encompass all transactions in a way that doesn’t sacrifice the decentralization and security that the network provides.

> 比特币[1]区块链在分布式账本领域大有可为。但作为一个支付平台，它本身无法在短期内覆盖全球商业交易。
>
> 区块链是一种“传闻广播”（gossip）协议，账本的所有状态修改都会广播给所有参与者。通过这种协议，大家对账户余额的状态达成共识。
>
> 如果比特币网络中的每个节点都必须获知全球发生的每一笔交易，这会严重拖累网络承载全球金融交易的能力。因此，我们需要一种既能承载所有交易，又不牺牲网络去中心化和安全性的方案。

The payment network Visa achieved 47,000 peak transactions per sec- ond (tps) on its network during the 2013 holidays[2], and currently averages hundreds of millions per day. Currently, Bitcoin supports less than 7 trans- actions per second with a 1 megabyte block limit. If we use an average of 300 bytes per bitcoin transaction and assumed unlimited block sizes, an equiva- lent capacity to peak Visa transaction volume of 47,000/tps would be nearly 8 gigabytes per Bitcoin block, every ten minutes on average. Continuously, that would be over 400 terabytes of data per year.

> 2013年节假日期间，Visa支付网络实现了每秒4.7万笔交易（TPS）的峰值，目前平均每天处理数亿笔交易。
>
> 目前，比特币网络受限于1MB的区块上限，每秒支持不到7笔交易。假设每笔交易平均300字节，若要达到Visa 47000 TPS的峰值（且假设区块大小不受限），比特币区块平均每十分钟需达到8GB，每年将产生超过400TB的数据。

Clearly, achieving Visa-like capacity on the Bitcoin network isn’t fea- sible today. No home computer in the world can operate with that kind of bandwidth and storage. If Bitcoin is to replace all electronic payments in the future, and not just Visa, it would result in outright collapse of the Bit- coin network, or at best, extreme centralization of Bitcoin nodes and miners to the only ones who could afford it. This centralization would then defeat aspects of network decentralization that make Bitcoin secure, as the abil- ity for entities to validate the chain is what allows Bitcoin to ensure ledger accuracy and security.

> 显然，目前在比特币网络上实现Visa级的处理能力并不现实。目前尚无家用电脑能支持这种带宽和存储需求。
>
> 如果比特币未来要取代所有电子支付系统，而非仅仅是Visa，这会导致网络彻底崩溃。或者在最好的情况下，导致节点和矿工高度中心化——只有极少数负担得起成本的人才能运行。这种中心化会破坏网络的去中心化特质，削弱比特币确保账本准确性和安全性的能力。

Having fewer validators due to larger blocks not only implies fewer individuals ensuring ledger accuracy, but also results in fewer entities that would be able to validate the blockchain as part of the mining process, which results in encouraging miner centralization. Extremely large blocks, for example in the above case of 8 gigabytes every 10 minutes on average, would imply that only a few parties would be able to do block validation. This creates a great possibility that entities will end up trusting centralized parties. Having privileged, trusted parties creates a social trap whereby the central party will not act in the interest of an individual (principal- agent problem), e.g. rentierism by charging higher fees to mitigate the incentive to act dishonestly. In extreme cases, this manifests as individuals sending funds to centralized trusted custodians who have full custody of customers’ funds. Such arrangements, as are common today, create severe counterparty risk. A prerequisite to prevent that kind of centralization from occurring would require the ability for bitcoin to be validated by a single consumer-level computer on a home broadband connection. By ensuring that full validation can occur cheaply, Bitcoin nodes and miners will be able to prevent extreme centralization and trust, which ensures extremely low transaction fees.

> 较大的区块会导致验证节点减少。这不仅意味着监督账本准确性的个人减少，也导致参与挖矿验证的实体减少，从而加剧矿工中心化。
>
> 以平均每十分钟8GB的区块为例，这意味着只有极少数方能验证区块。这极可能导致系统最终被中心化实体把控。
>
> 拥有特权的受信任方会引发“社会陷阱”，即中心化参与者可能不会为了大众利益行事（委托代理问题，Principal-Agent Problem）。例如，他们可能通过收取高额租金（Rentierism/食利行为）来抵消不诚实动机。
>
> 在极端情况下，个人会将资金存放在拥有完全控制权的中心化托管机构。这种安排在今天很常见，但会带来严重的交易对手风险（Counterparty Risk）。
>
> 防止此类中心化的先决条件是，比特币必须能在一台连接家用宽带的消费级计算机上进行验证。通过确保低成本的全节点验证，比特币节点和矿工才能避免中心化，并保持极低的交易费用。

While it is possible that Moore’s Law will continue indefinitely, and the computational capacity for nodes to cost-effectively compute multi- gigabyte blocks may exist in the future, it is not a certainty.

> 虽然摩尔定律可能无限延续，未来节点或许能低成本验证数GB规模的区块，但这并非必然。

To achieve much higher than 47,000 transactions per second using Bitcoin requires conducting transactions off the Bitcoin blockchain itself. It would be even better if the bitcoin network supported a near-unlimited num- ber of transactions per second with extremely low fees for micropayments. Many micropayments can be sent sequentially between two parties to en- able any size of payments. Micropayments would enable unbunding, less trust and commodification of services, such as payments for per-megabyte internet service. To be able to achieve these micropayment use cases, how- ever, would require severely reducing the amount of transactions that end up being broadcast on the global Bitcoin blockchain.

> 要实现远超47000 TPS的性能，需要在比特币链下（off-blockchain）进行交易。
>
> 如果比特币网络能以极低费用支持近乎无限的微支付（Micropayments），效果将更佳。双方可以连续发送多笔微支付来合成任意金额。
>
> 微支付能实现服务的拆解和商品化，并减少对信任的需求，例如按兆字节付费的互联网服务。但要实现这些应用，必须大幅减少广播到比特币主链上的交易数量。

While it is possible to scale at a small level, it is absolutely not possible to handle a large amount of micropayments on the network or to encompass all global transactions. For bitcoin to succeed, it requires confidence that if it were to become extremely popular, its current advantages stemming from decentralization will continue to exist. In order for people today to believe that Bitcoin will work tomorrow, Bitcoin needs to resolve the issue of block size centralization effects; large blocks implicitly create trusted custodians and significantly higher fees.

> 虽然小规模扩展可行，但在现有网络上处理海量微支付或全球交易是绝无可能的。
>
> 比特币若要成功，必须让人们确信，即使在大规模普及后，其去中心化的优势依然存在。
>
> 为了维持对比特币未来的信心，必须解决大区块带来的中心化效应。大区块会催生受信任的托管人，并显著提高交易费用。
