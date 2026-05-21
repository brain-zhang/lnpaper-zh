## 6 Blockchain Transaction Fees for Bidirectional Channels

## 6 双向支付通道中的链上交易费用

It is possible for each participant to generate different versions of transac- tions to ascribe blame as to who broadcast the transaction on the blockchain. By having knowledge of who broadcast a transaction and the ability to as- cribe blame, a third party service can be used to hold fees in a 2-of-3 multisig escrow. If one wishes to broadcast the transaction chain instead of agreeing to do a Funding Close or replacement with a new Commitment Transaction, one would communicate with the third party and broadcast the chain to the blockchain. If the counterparty refuses the notice from the third party to cooperate, the penalty is rewarded to the non-cooperative party. In most instances, participants may be indifferent to the transaction fees in the event of an uncooperative counterparty.

> 参与方通过生成不同版本的交易来明确由谁承担广播责任。鉴于可以归咎责任，可利用第三方服务在 2-of-3 多重签名托管（Escrow）中预留手续费。
>
> 若一方希望广播交易链而非协作关闭通道或更新承诺交易，可联系该第三方。若对手方拒绝协作，则可能面临惩罚。在大多数情况下，面对不合作的对手，参与者可能并不太在意手续费的微小差异。

One should pick counterparties in the channel who will be cooperative, but is not an absolute necessity for the system to function. Note that this does not require trust among the rest of the network, and is only relevant for the comparatively minor transaction fees. The less trusted party may just be the one responsible for transaction fees.

> 应当挑选愿意协作的对手方建立通道，但这并非系统运行的先决条件。
>
> 注意，这并不要求全网互信，且仅涉及相对较小的交易费用问题。信任度较低的一方可被要求承担交易费用。

The Lightning Network fees will likely be significantly lower than blockchain transaction fees. The fees are largely derived from the time-value of locking up funds for a particular route, as well as paying for the chance of channel close on the blockchain. These should be significantly lower than on-chain transactions, as many transactions on a Lightning Network chan- nel can be settled into one single blockchain transaction. With a sufficiently robust and interconnected network, the fees should asymptotically approach negligibility for many types of transactions. With cheap fees and fast trans- actions, it will be possible to build scalable micropayments, even amongst high-frequency systems such as Internet of Things applications or per-unit micro-billing.

> 闪电网络的手续费预计将远低于链上费用。手续费主要源于锁定特定路径资金的时间价值，以及支付潜在链上关闭通道的成本。
>
> 由于闪电网络中的海量支付最终可合并为单笔链上交易，其成本必然极低。随着网络稳健性和互联度的提升，许多交易的手续费将趋近于零。
>
> 低廉的费用与极速的交易将催生可扩展的微支付（Micropayments），甚至支持物联网（IoT）应用或单位微计费等高频系统。