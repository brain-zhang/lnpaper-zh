# 2.A Network of Micropayment Channels Can Solve Scalability
# 2.微支付通道网络可以解决可扩展性问题

“If a tree falls in the forest and no one is around to hear it, does it make a sound?”

> “如果一棵树倒在森林中，周围没有人听到，那么它发出过声音吗？”

The above quote questions the relevance of unobserved events —if nobody hears the tree fall, whether it made a sound or not is of no conse- quence. Similarly, in the blockchain, if only two participants care about an everyday recurring transaction, it’s not necessary for all other nodes in the bitcoin network to know about that transaction. It is instead preferable to only have the bare minimum of information on the blockchain. By defer- ring telling the entire world about every transaction, doing net settlement of their relationship at a later date enables Bitcoin users to conduct many transactions without bloating up the blockchain or creating trust in a cen- tralized counterparty. An effectively trustless structure can be achieved by using time locks as a component to global consensus.

> 上面的引文质疑了未观察事件的相关性——如果没有人听到树倒，它发声与否都无关紧要。同样，在区块链中，如果一笔日常往来只有两个参与方关心，就没必要让比特币网络中的所有节点都获知。
>
> 相反，区块链上只应保留最少的信息。通过推迟公布每笔交易，并在以后进行净额结算（Net Settlement），用户可以进行大量交易而不会导致区块链膨胀，也无需信任中心化对手方。
>
> 引入时间锁（Timelocks）作为全局共识的一部分，可以实现有效的去信任化（Trustless）结构。

Currently the solution to micropayments and scalability is to offload the transactions to a custodian, whereby one is trusting third party custodi- ans to hold one’s coins and to update balances with other parties. Trusting third parties to hold all of one’s funds creates counterparty risk and trans- action costs.

> 目前，微支付和扩容的方案通常是将交易转交给托管机构。这意味着需要信任第三方来持有资金，并由其维护与其他人的余额。信任第三方持有全部资金会产生交易对手风险（Counterparty Risk）和交易成本。

Instead, using a network of these micropayment channels, Bitcoin can scale to billions of transactions per day with the computational power available on a modern desktop computer today. Sending many payments inside a given micropayment channel enables one to send large amounts of funds to another party in a decentralized manner. These channels are not a separate trusted network on top of bitcoin. They are real bitcoin transactions.

> 相反，利用微支付通道（Micropayment Channels）网络，比特币可以支持每天数十亿笔交易，且仅需主流配置的家用电脑。
>
> 在微支付通道中连续发送多笔小额支付，可以去中心化的方式向另一方转移大额资金。这些通道并非凌驾于比特币之上的独立可信网络，它们本身就是真实的比特币交易。

Micropayment channels[3][4] create a relationship between two par- ties to perpetually update balances, deferring what is broadcast to the blockchain in a single transaction netting out the total balance between those two parties. This permits the financial relationships between two par- ties to be trustlessly deferred to a later date, without risk of counterparty default. Micropayment channels use real bitcoin transactions, only electing to defer the broadcast to the blockchain in such a way that both parties can guarantee their current balance on the blockchain; this is not a trusted overlay network —payments in micropayment channels are real bitcoin com- municated and exchanged off-chain.

> 微支付通道[3][4]在双方间建立一种关系来不断更新余额，并推迟向区块链广播，最终仅以单笔交易对总余额进行净额结算（Netting Out）。
>
> 这使得双方的财务关系能在去信任化（Trustlessly）的情况下延后处理，且无违约风险。微支付通道使用的是真实的比特币交易，仅选择推迟广播，使双方都能确保各自的链上余额。
>
> 这并非受信任的覆盖网络（Overlay Network）——通道内的支付是真实的比特币，只是在链下（Off-chain）进行传递和交换。

## 2.1 Micropayment Channels Do Not Require Trust
## 2.1 微支付通道不需要信任

Like the age-old question of whether the tree falling in the woods makes a sound, if all parties agree that the tree fell at 2:45 in the afternoon, then the tree really did fall at 2:45 in the afternoon. Similarly, if both counterparties agree that the current balance inside a channel is 0.07 BTC to Alice and 0.03 BTC to Bob, then that’s the true balance. However, without cryptography, an interesting problem is created: If one’s counterparty disagrees about the current balance of funds (or time the tree fell), then it is one’s word against another. Without cryptographic signatures, the blockchain will not know who owns what.

> 就像那个古老的问题：“树在森林中倒下是否发出了声音”一样，如果所有人都同意树是在下午2:45倒下的，那么树就是在下午2:45倒下的。
>
> 同样，如果交易双方（Alice和Bob）都同意通道内的当前余额是Alice 0.07 BTC，Bob 0.03 BTC，那么这就是真实的余额。
>
> 然而，如果没有密码学技术（Cryptography），就会产生一个有趣的问题：如果交易对手对当前的资金余额（或树倒下的时间）有异议，双方就会各执一词。如果没有加密签名，区块链就无法判断所有权的归属。

If the balance in the channel is 0.05 BTC to Alice and 0.05 BTC to Bob, and the balance after a transaction is 0.07 BTC to Alice and 0.03 BTC to Bob, the network needs to know which set of balances is correct. Blockchain transactions solve this problem by using the blockchain ledger as a timestamping system. At the same time, it is desirable to create a system which does not actively use this timestamping system unless absolutely necessary, as it can become costly to the network.

> 假设通道初始余额为每人0.05 BTC。交易后余额变为Alice 0.07 BTC，Bob 0.03 BTC。此时网络需要知道哪一组余额才是正确的。
>
> 区块链交易通过使用账本作为时间戳系统（Timestamping System）解决了这个问题。同时，我们希望建立一个系统，除非绝对必要，否则不主动调用此系统，以避免给网络带来高昂成本。

Instead, both parties can commit to signing a transaction and not broadcasting this transaction. So if Alice and Bob commit funds into a 2- of-2 multisignature address (where it requires consent from both parties to create spends), they can agree on the current balance state. Alice and Bob can agree to create a refund from that 2-of-2 transaction to themselves, 0.05 BTC to each. This refund is not broadcast on the blockchain. Either party may do so, but they may elect to instead hold onto that transaction, knowing that they are able to redeem funds whenever they feel comfortable doing so. By deferring broadcast of this transaction, they may elect to change this balance at a future date.

> 相反，双方可以共同签署一笔交易但不予广播。如果Alice和Bob将资金存入一个2/2多重签名地址（Multisignature Address，需双方同意方可动用），他们就能对当前的余额状态达成一致。
>
> Alice和Bob可以从该2/2交易中创建一笔给各自退款（Refund）的交易，金额各为0.05 BTC。这笔退款不立即上链广播。
>
> 任何一方都可以随时广播，但他们通常选择保留这笔交易，因为他们知道随时可以赎回资金。通过推迟广播，他们可以在未来进一步更改余额。

To update the balance, both parties create a new spend from the 2-of-2 multisignature address, for example 0.07 to Alice and 0.03 to Bob. Without proper design, though, there is the timestamping problem of not knowing which spend is correct: the new spend or the original refund.

> 为了更新余额，双方从2/2多重签名地址创建一笔新的花费交易（Spend），例如Alice 0.07 BTC，Bob 0.03 BTC。
>
> 然而，如果没有适当的设计，就会出现时间戳问题：无法确定哪笔交易才是正确的——是新的花费交易，还是最初的退款交易。

The restriction on timestamping and dates, however, is not as com- plex as full ordering of all transactions as in the bitcoin blockchain. In the case of micropayment channels, only two states are required: the current correct balance, and any old deprecated balances. There would only be a single correct current balance, and possibly many old balances which are deprecated.

> 相比比特币区块链中对所有交易进行完全排序（Full Ordering），对通道时间戳和日期的限制并不复杂。
>
> 在微支付通道中，只需区分两个状态：当前正确的余额状态，以及废弃的旧状态。在任何时刻，只会有一个正确的当前余额，以及若干个已废弃的旧余额。

Therefore, it is possible in bitcoin to devise a bitcoin script whereby all old transactions are invalidated, and only the new transaction is valid. Invalidation is enforced by a bitcoin output script and dependent trans- actions which force the other party to give all their funds to the channel counterparty. By taking all funds as a penalty to give to the other, all old transactions are thereby invalidated.

> 因此，可以设计一种比特币脚本，使所有旧交易失效（Invalidated），仅保留最新交易有效。
>
> 这种失效机制通过比特币输出脚本（Output Script）和依赖交易（Dependent Transactions）强制执行。如果一方作弊，该机制会强制其将所有资金赔偿给对手方。通过将全部资金作为违约罚金，所有旧交易由此失效。

This invalidation process can exist through a process of channel con- sensus where if both parties agree on current ledger states (and building new states), then the real balance gets updated. The balance is reflected on the blockchain only when a single party disagrees. Conceptually, this system is not an independent overlay network; it is more a deferral of state on the current system, as the enforcement is still occurring on the blockchain itself (albeit deferred to future dates and transactions).

> 这种失效过程通过通道共识（Channel Consensus）实现：只要双方对比特币账本状态（及新状态的构建）达成一致，真实的余额即告更新。
>
> 只有当一方产生分歧时，余额才会反映到区块链上。从概念上讲，该系统并非独立的覆盖网络（Overlay Network），而更像是当前系统状态的推迟（Deferral of State），因为强制执行权最终仍归属于区块链本身（尽管推迟到了未来的交易中）。

## 2.2 A Network of Channels 
## 2.2 通道网络

Thus, micropayment channels only create a relationship between two parties. Requiring everyone to create channels with everyone else does not solve the scalability problem. Bitcoin scalability can be achieved using a large network of micropayment channels.

> 因此，微支付通道仅在两方之间建立关系。要求每个人都与所有人建立通道并不能解决扩容问题。比特币的可扩展性需要通过庞大的微支付通道网络来实现。

If we presume a large network of channels on the Bitcoin blockchain, and all Bitcoin users are participating on this graph by having at least one channel open on the Bitcoin blockchain, it is possible to create a near-infinite amount of transactions inside this network. The only transactions that are broadcasted on the Bitcoin blockchain prematurely are with uncooperative channel counterparties.

> 假设比特币区块链上存在一个庞大的通道网络，所有用户都通过至少开启一个通道来参与这个网络图（Graph），那么该网络内部就能产生近乎无限的交易。
>
> 只有在通道对手方不合作时，才需要提前将交易广播到比特币区块链上。

By encumbering the Bitcoin transaction outputs with a hashlock and timelock, the channel counterparty will be unable to outright steal funds and Bitcoins can be exchanged without outright counterparty theft. Fur- ther, by using staggered timeouts, it’s possible to send funds via multiple intermediaries in a network without the risk of intermediary theft of funds.

> 通过在比特币交易输出中加入哈希锁（Hashlock）和时间锁（Timelock），通道对手将无法直接窃取资金，从而实现在无盗窃风险下的比特币交换。
>
> 此外，利用交错的超时时间（Staggered Timeouts），资金可以通过网络中的多个中介节点发送，且无需担心被中介窃取。