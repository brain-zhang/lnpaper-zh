## 8. 比特币闪电网络

利用带有哈希锁（Hashlocks）和时间锁（Timelocks）的微支付通道，可以通过一系列递减的时间锁在多跳支付网络上完成交易清算，而无需依赖中心化清算机构。

> 传统金融市场通过中心化机构转移交付义务，并通过该中心枢纽完成结算以转移所有权。银行电汇、资金转账系统（如 ACH 和 Visa 网络）以及证券清算所（如 DTCC）均按此模式运行。

As Bitcoin enables programmatic money, it is possible to create trans- actions without contacting a central clearinghouse. Transactions can execute off-chain with no third party which collects all funds before disbursing it – only transactions with uncooperative channel counterparties become auto- matically adjudicated on the blockchain.

> 比特币实现了货币的可编程性，因此无需联系中心清算所即可创建交易。
>
> 交易可以在链下执行，无需第三方先归集再分发资金——只有在对手方不配合时，才由区块链自动裁决。

The obligation to deliver funds to an end-recipient is achieved through a process of chained delegation. Each participant along the path assumes the obligation to deliver to a particular recipient. Each participant passes on this obligation to the next participant in the path. The obligation of each subsequent participant along the path, defined in their respective HTLCs, has a shorter time to completion compared to the prior participant. This way each participant is sure that they will be able to claim funds when the obligation is sent along the path.

> 向最终收款人支付资金是通过“链式委托”实现的。路径上的每位参与者都承担向特定收款人交付的义务，并将此义务传递给下一位参与者。
>
> 路径上各后续参与者在 HTLC 中定义的完成时间依次递减。这样，每位参与者都能确保在履行义务后顺利收回资金。

Bitcoin Transaction Scripting, a form of what some call an implemen- tation of “Smart Contracts”[19], enables systems without trusted custodial clearinghouses or escrow services.

> 比特币交易脚本是“智能合约”[19]的一种实现形式，它构建了无需信任第三方清算或托管服务的系统。

### 8.1 Decrementing Timelocks

### 8.1 递减时间锁

Presume Alice wishes to send 0.001 BTC to Dave. She locates a route through Bob and Carol. The transfer path would be Alice to Bob to Carol to Dave.

> 假设 Alice 想要通过 Bob 和 Carol 向 Dave 转账 0.001 BTC。转账路径为：Alice -> Bob -> Carol -> Dave。

![Figure15](figures/figure15.png?raw=true "Figure15")

Figure 15: Payment over the Lightning Network using HTLCs.

> 图 15：利用 HTLC 在闪电网络上进行支付。

When Alice sends payment to Dave through Bob and Carol, she re- quests from Dave hash(R) to use for this payment. Alice then counts the amount of hops until the recipient and uses that as the HTLC expiry. In this case, she sets the HTLC expiry at 3 days. Bob then creates an HTLC with Carol with an expiry of 2 days, and Carol does the same with Dave with an expiry of 1 day. Dave is now free to disclose R to Carol, and both parties will likely agree to immediate settlement via novation with a replacement Com- mitment Transaction. This then occurs step-by-step back to Alice. Note that this occurs off-chain, and nothing is broadcast to the blockchain when all parties are cooperative.

> 当 Alice 通过 Bob 和 Carol 向 Dave 转账时，她先向 Dave 索要 $hash(R)$。Alice 根据跳数设置 HTLC 到期时间，此处设为 3 天。
>
> 随后，Bob 为 Carol 创建 2 天到期的 HTLC，Carol 为 Dave 创建 1 天到期的 HTLC。
>
> Dave 随后向 Carol 披露 $R$，双方通过更替（Novation）更新承诺交易完成即时结算。该过程会逐步逆向传回至 Alice。
>
> 注意，在各方配合时，该过程完全在链下进行。

![Figure16](figures/figure16.png?raw=true "Figure16")

Figure 16:  Settlement of HTLC, Alice’s funds get sent to Dave.

> 图 16：HTLC 的结算，Alice 的资金发送给 Dave。

Decrementing timelocks are used so that all parties along the path know that the disclosure of R will allow the disclosing party to pull funds, since they will at worst be pulling funds after the date whereby they must receive R. If Dave does not produce R within 1 day to Carol, then Carol will be able to close out the HTLC. If Dave broadcasts R after 1 day, then he will not be able to pull funds from Carol. Carol’s responsibility to Bob occurs on day 2, so Carol will never be responsible for payment to Dave without an ability to pull funds from Bob provided that she updates her transaction with Dave via transmission to the blockchain or via novation.

> 采用递减时间锁是为了确保参与者能通过披露 $R$ 提取资金。若 Dave 未能在 1 天内向 Carol 提供 $R$，Carol 即可关闭该 HTLC。
>
> 若 Dave 在 1 天后才广播 $R$，他将无法从 Carol 处提取资金。
>
> 由于 Carol 需在第 2 天对 Bob 负责，她绝不会在无法从 Bob 处收回资金的情况下向 Dave 支付，前提是她通过上链或更替更新了与 Dave 的交易。

In the event that R gets disclosed to the participants halfway through expiry along the path (e.g. day 2), then it is possible for some parties along the path to be enriched. The sender will be able to know R, so due to Pay to Contract, the payment will have been fulfilled even though the receiver did not receive the funds. Therefore, the receiver must never disclose R unless they have received an HTLC from their channel counterparty; they are guaranteed to receive payment from one of their channel counterparties upon disclosure of the preimage.

> 若 $R$ 在到期中途（如第 2 天）被披露，路径上的某些方可能会获利。发送方得知 $R$ 后，基于“向合约支付”原则，支付即视为已完成，即便接收方尚未收到资金。
>
> 因此，接收方在收到其通道对手方的 HTLC 之前绝不能披露 $R$。披露原像的前提是已获得支付保障。

In the event a party outright disconnects, the counterparty will be re- sponsible for broadcasting the current Commitment Transaction state in the channel to the blockchain. Only the failed non-responsive channel state gets closed out on the blockchain, all other channels should continue to update their Commitment Transactions via novation inside the channel. Therefore, counterparty risk for transaction fees are only exposed to direct channel counterparties. If a node along the path decides to become unresponsive, the participants not directly connected to that node suffer only decreased time- value of their funds by not conducting early settlement before the HTLC close.

> 若某方掉线，其对手方负责将通道状态广播至区块链。仅掉线通道需在链上关闭，其他通道应继续在链下更替。
>
> 因此，手续费风险仅存在于直接相邻的对手方之间。若路径中某个节点无响应，非直连参与者仅会因无法提前结算而遭受资金时间价值的损失。

![Figure17](figures/figure17.png?raw=true "Figure17")

Figure 17: Only the non-responsive channels get broadcast on the blockchain, all others are settled off-chain via novation.

> 图 17：只有无响应的通道会在链上广播，其他通道均通过更替在链下结算。

## 8.2 Payment Amount

## 8.2 付款金额

It is preferable to use a small payment per HTLC. One should not use an extremely high payment, in case the payment does not fully route to its destination. If the payment does not reach its destination and one of the participants along the path is uncooperative, it is possible that the sender must wait until the expiry before receiving a refund. Delivery may be lossy, similar to packets on the internet, but the network cannot outright steal funds in transit. Since transactions don’t hit the blockchain with cooperative channel counterparties, it is recommended to use as small of a payment as possible. A tradeoff exists between locking up transaction fees on each hop versus the desire to use as small a transaction amount as possible (the latter of which may incur higher total fees). Smaller transfers with more intermediaries imply a higher percentage paid as Lightning Network fees to the intermediaries.

> 建议每笔 HTLC 采用小额支付。不应发送大额款项，以免路由失败。若支付未达目的地且中间人非协作，发送方必须等待到期方可获退款。
>
> 交付可能存在类似网络“丢包”的损耗，但资金绝不会在途中被窃取。在各方协作时交易不上链，这鼓励尽量减小单笔金额。
>
> 需在各跳锁定的手续费与交易金额之间权衡：金额越小、中间人越多，支付给中间人的费率占比就越高。

## 8.3 Clearing Failure and Rerouting

## 8.3 排障以及重新路由

If a transaction fails to reach its final destination, the receiver should send an equal payment to the sender with the same hash, but not disclose R. This will net out the disclosure of the hash for the sender, but may not for the receiver. The receiver, who generated the hash, should discard R and never broadcast it. If one channel along the path cannot be contacted, then the channels may elect to wait until the path expires, which all participants will likely close out the HTLC as unsettled without any payment with a new Commitment Transaction.

> 若交易失败，收款人应向发送人发起一笔哈希值相同但金额相等的反向支付（且不披露 $R$）。
>
> 这将抵消发送方的哈希披露压力。生成哈希的收款人应废弃 $R$。若路径中某通道失联，其他通道可选择等待到期，届时各方将通过新承诺交易关闭未结算的 HTLC。

![Figure18](figures/figure18.png?raw=true "Figure18")

Figure 18: Dave creates a path back to Alice after Alice fails to send funds to Dave, because Carol is uncooperative. The input R from hash(R) is never brodcast by Dave, because Carol did not complete her actions. If R was broadcast, Alice will break-even. Dave, who controls R should never broadcast R because he may not receive funds from Carol, he should let the contracts expire. Alice and Bob have the option to net out and  close the contract early, as well, in this diagram.

> 图 18：由于 Carol 不配合，Alice 支付失败，Dave 建立了一条回拨路径。Dave 绝不应广播 $R$ ，因为 Carol 未履行义务。
>
> 若广播 $R$，Alice 将能收回资金（收支相抵），但 Dave 却收不到 Carol 的钱。控制 $R$ 的 Dave 应让合约自然过期。
>
> 在此图中，Alice 和 Bob 也可选择提前对冲并关闭合约。

If the refund route is the same as the payment route, and there are no half-signed contracts whereby one party may be able to steal funds, it is possible to outright cancel the transaction by replacing it with a new Com- mitment Transaction starting with the most recent node who participated in the HTLC.

> 若退款路径与原路径一致，且不存在可能导致资金被窃的半签名合同，则可以通过从最近的节点开始，逐一更新承诺交易来彻底取消该笔交易。

It is also possible to clear out a channel by creating an alternate route path in which payment will occur in the opposite direction (netting out   to zero) and/or creating an entirely alternate route for the payment path. This will create a time-value of money for disclosing inputs to hashes on the Lightning Network. Participants may specialize in high connectivity between nodes and offering to offload contract hashlocks from other nodes for a fee. These participants will agree to payments which net out to zero (plus fees), but are loaning bitcoins for a set time period. Most likely, these entities with low demand for channel resources will be end-users who are already connected to multiple well-connected nodes. When an end-user connects to a node, the node may ask the client to lock up their funds for several days to another channel the client has established for a fee. This can be achieved by having the new transactions require a new hash(Y) from input Y in addition to the existing hash which may be generated by any participant, but must disclose Y only after a full circle is established. The new participant has the same responsibility as well as the same timelocks as the old participant being replaced. It is also possible that the one new participant replaces multiple hops.

> 还可以通过创建反向路径或完全备用的路径来清理（Clear out）通道。这在闪电网络中创造了哈希原像披露的货币时间价值。
>
> 有些参与者可能专注于提高节点连接度，并通过收费来承接其他节点的哈希锁合约（Offload）。
>
> 最终用户可能付费请用户将其资金在另一通道中锁定数日。实现方式是要求新交易除了原有的哈希外，还需包含一个新的由参与者生成的 $hash(Y)$。
>
> 原像 $Y$ 仅在完整环路建立后披露。新参与者承担被替换者的全部责任和时间锁要求。

![Figure19](figures/figure19.png?raw=true "Figure19")

Figure 19: Erin is connected to both Bob and Dave. If Bob wishes to free up his channel with Carol, since that channel is active and very profitable, Bob can offload the payment  to Dave  via Erin.  Since Erin has extra bitcoin available, she will be able to collect some  fee for offloading the channel between Bob and Carol as well as between Carol and Dave. The channels between Bob and Carol as well as Carol and Dave are undone and no longer have the HTLC, nor has payment occurred on  that  path.  Payment  will  occur  on  the path involving Erin. This is achieved by creating a new payment from Dave to Carol to  Bob contingent upon Erin constructing an HTLC. The payment in dashed lines (red) are netted out to zero and settled via a new Commitment Contract.

> 图 19：Erin 与 Bob 和 Dave 均有连接。由于 Bob 与 Carol 的通道非常活跃，他希望将支付通过 Erin 路径“分流”（Offload）。
>
> Bob-Carol 及 Carol-Dave 间的通道随后撤销 HTLC。这通过在 Erin 建立 HTLC 后，再发起一笔 Dave-Carol-Bob 的支付来实现。
>
> 红色虚线部分最终冲抵为零，并通过新承诺合约结算。