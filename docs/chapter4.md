# 4. Hashed Timelock Contract (HTLC)

# 4. 哈希时间锁合约（HTLC）

A bidirectional payment channel only permits secure transfer of funds inside a channel. To be able to construct secure transfers using a network of channels across multiple hops to the final destination requires an additional construction, a Hashed Timelock Contract (HTLC).

> 双向支付通道仅支持通道内的安全资金转移。若要通过跨越多个节点的通道网络实现向最终目的地的安全转账，需要引入一种额外的结构：哈希时间锁合约（Hashed Timelock Contract, HTLC）。

The purpose of an HTLC is to allow for global state across multiple nodes via hashes. This global state is ensured by time commitments and time-based unencumbering of resources via disclosure of preimages. Trans- actional “locking” occurs globally via commitments, at any point in time a single participant is responsible for disclosing to the next participant whether they have knowledge of the preimage R. This construction does not require custodial trust in one’s channel counterparty, nor any other participant in the network.

> HTLC 的目的是通过哈希在多个节点间维持全局状态。这种全局状态通过时间承诺以及基于原像（Preimage）披露的资源解锁机制来保障。
>
> 交易通过承诺实现全局“锁定”，在任意时间点，参与者负责向下一方披露其是否掌握原像 $R$。该结构无需信任通道对手方或网络中的任何其他参与者。

In order to achieve this, an HTLC must be able to create certain transactions which are only valid after a certain date, using nLockTime, as well as information disclosure to one’s channel counterparty. Additionally, this data must be revocable, as one must be able to undo an HTLC.

> 为此，HTLC 必须能利用 `nLockTime` 创建仅在特定日期后才有效的交易，并向对手方披露信息。此外，这些数据必须是可撤销的，以便能够取消 HTLC。

An HTLC is also a channel contract with one’s counterparty which is enforcible via the blockchain. The counterparties in a channel agree to the following terms for a Hashed Timelock Contract:

> HTLC 是由区块链强制执行的通道合约。双方约定如下条款：

1. If Bob can produce to Alice an unknown 20-byte random input data R from a known hash H, within three days, then Alice will settle the contract by paying Bob 0.1 BTC.

2. If three days have elapsed, then the above clause is null and void and the clearing process is invalidated, both parties must not attempt to settle and claim payment after three days.

3. Either party may (and should) pay out according to the terms of this contract in any method of the participants choosing and close out this contract early so long as both participants in this contract agree.

4. Violation of the above terms will incur a maximum penalty of the funds locked up in this contract, to be paid to the non-violating counterparty as a fidelity bond.

> 1. 若 Bob 能在三天内向 Alice 出示哈希 $H$ 对应的未知 20 字节随机原像 $R$，则 Alice 需向 Bob 支付 0.1 BTC 以结算合约。
>
> 2. 若三天期限已满，上述条款即告失效，清算流程作废。任何一方不得在三天后尝试结算或索赔。
>
> 3. 只要双方达成一致，任何一方均可（且应当）按合同条款提前终止合约。
>
> 4. 违反上述条款者，将面临没收合约内全部资金的最高处罚，该资金将作为“诚实保证金”赔付给未违约方。

For clarity of examples, we use days for HTLCs and block height for RSMCs. In reality, the HTLC should also be defined as a block height (e.g. 3 days is equivalent to 432 blocks).

> 为便于说明，我们在 HTLC 示例中使用“天”为单位，在 RSMC 中使用区块高度。实际上，HTLC 也应定义为区块高度（如 3 天相当于 432 个区块）。

In effect, one desires to construct a payment which is contingent upon knowledge of R by the recipient within a certain timeframe. After this timeframe, the funds are refunded back to the sender.

> 本质上，我们希望构建一种基于接收方在特定时间内披露 $R$ 为条件的支付方式。若逾期未披露，资金将退回给发送方。

Similar to RSMCs, these contract terms are programatically enforced on the Bitoin blockchain and do not require trust in the counterparty to adhere to the contract terms, as all violations are penalized via unilaterally enforced fidelity bonds, which are constructed using penalty transactions spending from commitment states. If Bob knows R within three days, then he can redeem the funds by broadcasting a transaction; Alice is unable to withhold the funds in any way, because the script returns as valid when the transaction is spent on the Bitcoin blockchain.

> 与 RSMC 类似，这些条款由比特币区块链自动执行，无需信任对手方。违约行为会触发单方面惩罚，罚金来源于承诺状态下的支出交易。
>
> 若 Bob 在三天内出示 $R$，他即可广播交易赎回资金。Alice 无法拦截，因为该支出脚本在区块链上是合法有效的。

An HTLC is an additional output in a Commitment Transaction with a unique output script:

> HTLC 表现为承诺交易中的一个具有特殊脚本的额外输出：

```
OP IF
    OP HASH160 <Hash160 (R)> OP EQUALVERIFY 2  <Alice2 > <Bob2> OP CHECKMULTISIG
OP ELSE
    2  <Alice1>  <Bob1> OP CHECKMULTISIG
OP ENDIF
```

Conceptually, this script has two possible paths spending from a single HTLC output. The first path (defined in the OP IF) sends funds to Bob if Bob can produce R. The second path is redeemed using a 3-day timelocked refund to Alice. The 3-day timelock is enforced using nLockTime from the spending transaction.

> 从概念上讲，该脚本为单个 HTLC 输出提供了两条支出路径：
>
> 第一条（`OP_IF` 部分）：若 Bob 出示 $R$，资金支付给 Bob。
>
> 第二条（`OP_ELSE` 部分）：Alice 在 3 天锁定期满后可获得退款。该 3 天锁定期由支出交易中的 `nLockTime` 强制执行。


### 4.1 Non-revocable HTLC Construction

### 4.1 不可撤销的HTLC构建

![Figure11](figures/figure11.png?raw=true "Figure11")

Figure 11: This is a non-functional naive implementation of an HTLC. Only the HTLC path from the Commitment Transaction is displayed. Note that there are two possible spends from an HTLC output. If Bob can produce the preimage R within 3 days and he can redeem path 1. After three days, Alice is able to broadcast path 2. When 3 days have elapsed either is valid. This model, however, doesn’t work with multiple Commitment Transactions.

> 图 11：这是一个无法正常工作的简单 HTLC 实现。图中仅展示了从承诺交易发出的 HTLC 路径。
>
> 注意，HTLC 输出有两种支出可能：若 Bob 在 3 天内提供原像 $R$，可按路径 1 赎回；若 3 天后 $R$ 未披露，Alice 可按路径 2 广播。
>
> 然而当 3 天期满后，两者皆为有效。此外，该模型无法与多个承诺交易协同工作。

If R is produced within 3 days, then Bob can redeem the funds by broadcast- ing the “Delivery” transaction. A requirement for the “Delivery” transaction to be valid requires R to be included with the transaction. If R is not in- cluded, then the “Delivery” transaction is invalid. However, if 3 days have elapsed, the funds can be sent back to Alice by broadcasting transaction “Timeout”. When 3 days have elapsed and R has been disclosed, either transaction may be valid.

> 若 Bob 在 3 天内出示 $R$，即可通过广播“交付”（Delivery）交易赎回资金。该交易必须包含 $R$ 方为有效。
>
> 若 3 天期满，Alice 可通过广播“超时”（Timeout）交易收回资金。注意，若 3 天已过且 $R$ 已披露，这两笔交易可能同时有效。

It is within both parties individual responsibility to ensure that they can get their transaction into the blockchain in order to ensure the balances are correct. For Bob, in order to receive the funds, he must either broadcast the “Delivery” transaction on the Bitcoin blockchain, or otherwise settle with Alice (while cancelling the HTLC). For Alice, she must broadcast the “Timeout” 3 days from now to receive the refund, or cancel the HTLC entirely with Bob.

> 双方均有责任确保自己的交易上链以维护权益。Bob 必须广播“交付”交易，或与 Alice 协作结算（并取消 HTLC）。Alice 则必须在 3 天后广播“超时”交易以收回资金，或与 Bob 协作取消 HTLC。

Yet this kind of simplistic construction has similar problems as an incorrect bidirectional payment channel construction. When an old Com- mitment Transaction gets broadcast, either party may attempt to steal funds as both paths may be valid after the fact. For example, if R gets disclosed 1 year later, and an incorrect Commitment Transaction gets broadcast, both paths are valid and are redeemable by either party; the contract is not yet enforcible on the blockchain. Closing out the HTLC is absolutely necessary, because in order for Alice to get her refund, she must terminate the contract and receive her refund. Otherwise, when Bob discovers R after 3 days have elapsed, he may be able to steal the funds which should be going to Alice. With uncooperative counterparties it’s not possible to terminate an HTLC without broadcasting it to the bitcoin blockchain as the uncooperative party is unwilling to create a new Commitment Transaction.

> 然而，这种简单结构存在类似于错误双向通道的问题。若旧的承诺交易被广播，由于两条路径可能在事后同时有效，任何一方都可能尝试窃取资金。
>
> 例如，若 $R$ 在一年后被披露，而旧交易被广播，此时两条路径均有效。该合约尚无法在区块链上强制执行。
>
> 及时关闭 HTLC 至关重要：Alice 必须终止合约以获得退款；否则，若 Bob 在 3 天后才发现 $R$，他可能窃取本该属于 Alice 的退款。对于不合作的对手方，如果不将其广播至区块链，就无法终止 HTLC。

### 4.2 Off-chain Revocable HTLC
### 4.2 链下可撤销HTLC

To be able to terminate this contract off-chain without a broadcast to the Bitcoin blockchain requires embedding RSMCs in the output, which will have a similar construction to the bidirectional channel.

> 为了不用广播交易到区块链就能在链下终止合约，需要在输出中嵌入RSMCs，其结构与双向通道类似。

![Figure12](figures/figure12.png?raw=true "Figure12")

Figure 12:  If Alice broadcasts C2a,  then the left half will execute.  If Bob broadcasts  C2b, then the right half will execute. Either party may broadcast their Commitment transaction at any time. HTLC Timeout is only valid after 3 days. HTLC Executions can only be broadcast if the preimage to the hash R is known. Prior Commitments (and their dependent transactions) are not displayed for brevity.

> 图 12：若 Alice 广播 C2a，则执行左半部分；若 Bob 广播 C2b，则执行右半部分。双方均可随时广播各自的承诺交易。
>
> HTLC 超时仅在 3 天后有效。只有掌握原像 $R$ 才能执行 HTLC。为简明起见，图中省略了旧承诺交易及其依赖交易。

Presume Alice and Bob wish to update their balance in the channel at Commitment 1 with a balance of 0.5 to Alice and 0.5 to Bob.
Alice wishes to send 0.1 to Bob contingent upon knowledge of R within 3 days, after 3 days she wants her money back if Bob does not produce R.

> 假设在承诺交易 1 中，Alice 和 Bob 各持有 0.5 BTC。Alice 希望向 Bob 支付 0.1 BTC，前提是 Bob 能在 3 天内出示原像 $R$；否则，Alice 将收回这笔钱。

The new Commitment Transaction will have a full refund of the cur-
rent balance to Alice and Bob (Outputs 0 and 1), with output 2 being the HTLC, which describes the funds in transit. As 0.1 will be encumbered in an HTLC, Alice’s balance is reduced to 0.4 and Bob’s remains the same at 0.5.

> 新的承诺交易将包含三个输出：输出 0 和 1 是分配给 Alice 和 Bob 的当前余额，输出 2 是表示在途资金的 HTLC。
>
> 由于 0.1 BTC 被锁定在 HTLC 中，Alice 的余额降至 0.4 BTC，Bob 的余额保持 0.5 BTC 不变。

This new Commitment Transaction (C2a/C2b) will have an HTLC output with two possible spends. Each spend is different depending on each counterparty’s version of the Commitment Transaction. Similar to the bidirectional payment channel, when one party broadcasts their Commit- ment, payments to the counterparty will be assumed to be valid and not invalidated. This can occur because when one broadcasts a Commitment Transaction, one is attesting this is the most recent Commitment Transac- tion. If it is the most recent, then one is also attesting that the HTLC exists and was not invalidated before, so potential payments to one’s counterparty should be valid.

> 新承诺交易（C2a/C2b）的 HTLC 输出有两种支出路径，且在双方的版本中各不相同。
>
> 与双向通道类似，当某方广播其承诺交易时，给对方的支付被假定为有效。这是因为广播承诺交易即代表声明其为最新版本。
>
> 若确实为最新版本，则意味着该 HTLC 依然存续且未被作废，因此给对方的支付是合法有效的。

Note that HTLC transaction names (beginning with the letter H) will begin with the number 1, whose values do not correlate with Commitment Transactions. This is simply the first HTLC transaction. HTLC transac- tions may persist between Commitment Transactions. Each HTLC has 4 keys per side of the transaction (C2a and C2b) for a total of 8 keys per counterparty.

> 注意，HTLC 交易（以字母 H 开头）的编号从 1 开始，与承诺交易的编号不直接相关。这仅代表第一个 HTLC 交易。
>
> HTLC 交易可跨越多个承诺交易存续。每个 HTLC 为双方（C2a 和 C2b）各分配 4 个密钥，共计 8 个密钥。

The HTLC output in the Commitment Transaction has two sets of keys per counterparty in the output.

> 承诺交易中的 HTLC 输出为每方关联两组密钥。

For Alice’s Commitment Transaction (C2a), the HTLC output script requires multisig(PAlice2, PBob2) encumbered by disclosure of R, as well as multisig(PAlice1, PBob1) with no encumbering.

> 在 Alice 的承诺交易（C2a）中，HTLC 输出脚本包含两部分：一是需提供原像 $R$ 的 `multisig(PAlice2, PBob2)`，二是无附加条件的 `multisig(PAlice1, PBob1)`。

For Bob’s Commitment Transaction (C2b), the HTLC output script requires multisig(PAlice6, PBob6) encumbered by disclosure of R, as well as multisig(PAlice5, PBob5) with no encumbering.

> 同样，在 Bob 的承诺交易（C2b）中，脚本包含需提供 $R$ 的 `multisig(PAlice6, PBob6)` 和无附加条件的 `multisig(PAlice5, PBob5)`。

The HTLC output states are different depending upon which Com- mitment Transaction is broadcast.

> 根据具体广播的是哪一份承诺交易，HTLC 的输出状态也会有所不同。

#### 4.2.1 HTLC when the Sender Broadcasts the Commitment Transaction

#### 4.2.1 当支付方广播承诺交易时的HTLC状态

For the sender (Alice), the “Delivery” transaction is sent as an HTLC Exe- cution Delivery transaction (HED1a), which is not encumbered in an RSMC. It assumes that this HTLC has never been terminated off-chain, as Alice is attesting that the broadcasted Commitment Transaction is the most recent. If Bob can produce the preimage R, he will be able to redeem funds from the HTLC after the Commitment Transaction is broadcast on the blockchain.

> 对于发送方（Alice）而言，“交付”交易表现为“HTLC 执行交付交易”（HED1a），该交易不受 RSMC 约束。
>
> 这基于一个假设：该 HTLC 尚未在链下终止，且 Alice 声明其广播的是最新承诺交易。若 Bob 能提供原像 $R$ ，他即可在承诺交易上链后从 HTLC 中赎回资金。

This transaction consumes multisig(PAlice2, PBob2) if Alice broadcasts her Commitment C2a. Only Bob can broadcast HED1a since only Alice gave her signature for HED1a to Bob.

> 若 Alice 广播其 C2a，HED1a 将花费 `multisig(PAlice2, PBob2)` 输出。只有 Bob 能广播 HED1a，因为 Alice 已预先将签名交给了 Bob。

However, if 3 days have elapsed since forming the  HTLC,  then Alice will be able broadcast a “Timeout” transaction,  the HTLC Time-  out transaction  (HT1a).  This  transaction  is  an  RSMC.  It  consumes  the output multisig(PAlice1, PBob1) without requiring disclosure  of  R  if  Alice broadcasts C2a. This transaction cannot enter into the blockchain until 3 days have elapsed. The output for this transaction is an RSMC  with multisig(PAlice3, PBob3) with relative maturity of 1000 blocks, and multisig(PAlice4, PBob4) with no requirement for  confirmation  maturity. Only Alice can broadcast HT1a since only Bob gave his signature for HT1a to Alice.

> 然而，若 HTLC 创建已满 3 天，Alice 即可广播“HTLC 超时交易”（HT1a）。HT1a 是一笔 RSMC 交易。
>
> 若 Alice 广播 C2a，HT1a 将花费 `multisig(PAlice1, PBob1)`，无需披露 $R$。该交易必须在 3 天期满后方可上链。
>
> HT1a 的输出是一个 RSMC：其一路径需 `multisig(PAlice3, PBob3)` 且等待 1000 个确认；另一路径为 `multisig(PAlice4, PBob4)` 无等待要求。只有 Alice 能广播 HT1a。

After HT1a enters into the blockchain and 1000 block confirmations occur, an HTLC Timeout Revocable  Delivery  transaction  (HTRD1a)  may  be  broadcast  by  Alice  which   consumes   multisig(PAlice3, PBob3). Only Alice can broadcast HTRD1a 1000 blocks after HT1a is broadcast since only Bob gave his signature for HTRD1a  to  Alice.  This  trans- action can be revocable when another transaction supersedes HTRD1a using multisig(PAlice4, PBob4) which does not have any block maturity requirements.

> 在 HT1a 获得 1000 个确认后，Alice 可广播“HTLC 超时可撤销交付交易”（HTRD1a），花费 `multisig(PAlice3, PBob3)` 输出。
>
> 同样，该交易是可撤销的：若有人用 `multisig(PAlice4, PBob4)` 发起无等待期的替代交易，则 HTRD1a 被撤销。

#### 4.2.2 HTLC when the Receiver Broadcasts the Commitment Transaction
#### 4.2.2 接收方广播承诺交易时的HTLC状态

For the potential receiver (Bob), the “Timeout” of receipt is refunded as an HTLC Timeout Delivery transaction (HTD1b). This transaction directly refunds the funds to the original sender (Alice) and is not encumbered in an RSMC. It assumes that this HTLC has never been terminated off-chain, as Bob is attesting that the broadcasted Commitment Transaction (C2b)  is the most recent. If 3 days have elapsed,  Alice can broadcast HTD1b  and take the refund. This transaction consumes multisig(PAlice5, PBob5) if Bob broadcasts C2b. Only Alice can broadcast HTD1b since Bob gave his signature for HTD1b to Alice.

> 对潜在接收方（Bob）而言，超时退款通过“HTLC 超时交付交易”（HTD1b）实现。
>
> 该交易直接向发送方（Alice）退款，且不受 RSMC 约束。它假设 HTLC 尚未在链下终止，且 Bob 广播的是最新版本。
>
> 若 3 天期满，Alice 即可广播 HTD1b 收回退款。若 Bob 广播 C2b，该交易将花费 `multisig(PAlice5, PBob5)`。只有 Alice 能广播此交易。

However, if HTD1b is not broadcast (3 days have not elapsed) and Bob knows the preimage R, then Bob will be able to broadcast the HTLC Execution transaction (HE1b) if he can produce R. This transaction is an RSMC. It consumes the output multisig(PAlice6, PBob6) and requires dis- closure of R if Bob broadcasts C2b. The output for this transaction is an RSMC with multisig(PAlice7, PBob7) with relative maturity of 1000 blocks, and multisig(PAlice8, PBob8) which does not have any block maturity require- ments. Only Bob can broadcast HE1b since only Alice gave her signature for HE1b to Bob.

> 若 3 天内 Bob 掌握了 $R$，他即可广播“HTLC 执行交易”（HE1b）。这是一个 RSMC 交易。
>
> 若 Bob 广播 C2b，HE1b 将花费 `multisig(PAlice6, PBob6)` 且必须披露 $R$。其输出同样是 RSMC：路径一需 `multisig(PAlice7, PBob7)` 且等待 1000 个确认；路径二无等待。只有 Bob 能广播 HE1b。

After HE1b enters into the blockchain and 1000 block confirmations occur, an HTLC Execution Revocable Delivery transaction  (HERD1b) may  be  broadcast  by  Bob   which   consumes   multisig(PAlice7, PBob7).  Only Bob can broadcast HERD1b 1000 blocks after HE1b is broadcast since only Alice gave her signature for HERD1b to Bob.  This  transaction can be revocable when another transaction supersedes HERD1b using multisig(PAlice8, PBob8) which does not have any block maturity requirements.

> 在 HE1b 获得 1000 个确认后，Bob 可广播“HTLC 执行可撤销交付交易”（HERD1b），花费 `multisig(PAlice7, PBob7)` 输出。
>
> 同样，该交易是可撤销的，可由无等待期的 `multisig(PAlice8, PBob8)` 支出交易取代。


### 4.3 HTLC Off-chain Termination

### 4.3 链下终止HTLC

After an HTLC is constructed, to terminate an HTLC off-chain requires both parties to agree on the state of the channel. If the recipient can prove knowledge of R to the counterparty, the recipient is proving that they are able to immediately close out the channel on the Bitcoin blockchain and re- ceive the funds. At this point, if both parties wish to keep the channel open, they should terminate the HTLC off-chain and create a new Commitment Transaction reflecting the new balance.

> HTLC 建立后，链下终止需双方共识。若接收方能向对手方出示 $R$，即证明其有能力立即在链上结算并获取资金。
>
> 此时，若双方希望维持通道开启，应在链下终止该 HTLC，并创建一个反映新余额的新承诺交易。

![Figure13](figures/figure13.png?raw=true "Figure13")

Figure 13: Since Bob proved to Alice he knows R by telling Alice R, Alice is willing to update the balance with a new Commitment Transaction. The payout will be the same whether C2 or C3 is broadcast at this time.

> 图 13：由于 Bob 向 Alice 披露了 $R$ 从而证明其已知晓原像，Alice 愿意通过新的承诺交易更新余额。此时无论广播 C2 还是 C3，最终支付金额均相同。

Similarly, if the recipient is not able to prove knowledge of R by disclosing R, both parties should agree to terminate the HTLC and create a new Commitment Transaction with the balance in the HTLC refunded to the sender.

> 同样地，若接收方无法出示 $R$，双方应协商终止 HTLC，并创建新的承诺交易，将 HTLC 中的资金退还给发送方。

If the counterparties cannot come to an agreement or become other- wise unresponsive, they should close out the channel by broadcasting the necessary channel transactions on the Bitcoin blockchain.

> 若双方无法达成一致或某方失去响应，则应通过在链上广播必要的交易来关闭通道。

However, if they are cooperative, they can do so by first generat-  ing a new Commitment Transaction with the new balances, then inval- idate the prior Commitment by exchanging Breach Remedy transactions (BR2a/BR2b). Additionally, if they are terminating a particular HTLC, they should also exchange some of their own private keys used in the HTLC transactions.

> 若双方配合，可先创建包含新余额的承诺交易，再通过交换违约补救交易（BR2a/BR2b）作废旧版本。
>
> 此外，在终止特定 HTLC 时，双方还应交换该 HTLC 交易中使用的部分私钥。

For example, Alice wishes to terminate the HTLC, Alice will disclose KAlice1 and KAlice4 to Bob. Correspondingly if Bob wishes to terminate the HTLC, Bob will disclose KBob6 and KBob8  to Alice.  After the private keys  are disclosed to the counterparty, if Alice broadcasts C2a, Bob will be able to take all the funds from the HTLC immediately. If Bob broadcasts C2b, Alice will be able to take all funds from the HTLC immediately. Note that when an HTLC is terminated, the older Commitment Transaction must be revoked as well.

> 例如，Alice 为终止 HTLC 需向 Bob 披露 `KAlice1` 和 `KAlice4`；Bob 则披露 `KBob6` 和 `KBob8`。
>
> 私钥披露后，若 Alice 广播 C2a，Bob 可立即提取 HTLC 中的资金；若 Bob 广播 C2b，Alice 同理。注意，终止 HTLC 时必须同时撤销旧的承诺交易。

![Figure14](figures/figure13.png?raw=true "Figure14")

Figure 14: A fully revoked Commitment Transaction and terminated HTLC. If either party broadcasts Commitment 2, they will lose all their money to the counterparty. Other commitments (e.g. if Commitment 3 is the current Commitment) are not displayed for brevity.

> 图 14：已终止的 HTLC 及其对应的已撤销承诺交易。若任一方广播承诺交易 2，其资金将全数罚没给对方。为简明起见，图中省略了其他承诺交易（如当前的 C3）。

Since both parties are able to prove the current state to each other, they can come to agreement on the current balance inside the channel. Since they may broadcast the current state on the blockchain, they are able to come to agreement on netting out and terminating the HTLC with a new Commitment Transaction.

> 由于双方均能互证当前状态且随时可上链强制执行，因此他们能够就余额达成一致，并通过新的承诺交易完成净额结算并终止 HTLC。

### 4.4 HTLC Formation and Closing Order

### 4.4 HTLC的构造以及终止次序

To create a new HTLC, it is the same process as creating a new Commitment Transaction, except the signatures for the HTLC are exchanged before the new Commitment Transaction’s signatures.

> 创建新 HTLC 的过程与创建新承诺交易类似，区别在于 HTLC 的签名需在承诺交易签名之前交换。

To close out an HTLC, the process is as follows (from C2 to C3):

> 关闭 HTLC 的步骤如下（从 C2 迁移至 C3）：

1. Alice signs and sends her signature for RD3b and C3b. At this point Bob can elect to broadcast C3b or C2b (with the HTLC) with the same payout. Bob is willing after receiving C3b to close out C2b.

> 1. Alice 签署 `RD3b` 和 `C3b` 并发给 Bob。此时 Bob 可选择广播 `C3b` 或带 HTLC 的 `C2b`，两者的支付额相同。收到 `C3b` 后，Bob 愿意关闭 `C2b`。

2. Bob signs and sends his signature for RD3a and C3a, as well as his private keys used for Commitment 2 and the HTLC being terminated; he sends Alice KBobRSMC2, KBob5,  and  KBob8.  At  this  point  Bob should only broadcast C3b and should not broadcast C2b as he will lose all his money if he does so. Bob has fully revoked C2b and the HTLC. Alice is willing after receiving C3a to close out C2b.

> 2. Bob 签署 `RD3a` 和 `C3a` 发给 Alice，并移交 C2 及相关 HTLC 的私钥（`KBobRSMC2`, `KBob5`, `KBob8`）。此时 Bob 只能广播 `C3b`，若广播 `C2b` 则会面临罚款。此时 `C2b` 和 HTLC 已被 Bob 撤销。Alice 收到 `C3a` 后愿意关闭 `C2a`。

3. Alice signs and sends her signature for RD3b and C3b, as well as her private keys used for Commitment 2 and the HTLC being terminated; she sends Bob KAliceRSMC2, KAlice1, and KAlice4. At this point  neither party should broadcast Commitment 2, if they do so, their funds will be going to the counterparty. The old Commitment and old HTLC are now revoked and fully terminated. Only the new Commitment 3 remains, which does not have an HTLC.

> 3. Alice 签署 `RD3b` 和 `C3b` 发给 Bob，并移交其 C2 相关的私钥（`KAliceRSMC2`, `KAlice1`, `KAlice4`）。至此，双方均不应再广播 C2，否则资金将归对方。旧的承诺和 HTLC 已被彻底撤销，仅保留不含 HTLC 的 C3。

When the HTLC has been closed, the funds are updated so that the present balance in the channel is what would occur had the HTLC contract been completed and broadcast on the blockchain. Instead, both parties elect to do off-chain novation and update their payments inside the channel.

> HTLC 关闭后，通道余额将更新为该合约执行后的状态。双方选择通过链下“更替”（Novation）来更新通道内的支付分配，而非广播至区块链。

It is absolutely necessary for both parties to complete off-chain nova- tion within their designated time window. For the receiver (Bob), he must know R and update his balance with Alice within 3 days (or whatever time was selected), else Alice will be able to redeem it within 3 days. For Alice, very soon after her timeout becomes valid, she must novate or broadcast the HTLC Timeout transaction. She must also novate or broadcast the HTLC Timeout Revocable Delivery transaction as soon as it becomes valid. If the counterparty is unwilling to novate or is stalling, then one must broadcast the current channel state, including HTLC transactions) onto the Bitcoin blockchain.

> 双方必须在指定时间窗口内完成链下更替。接收方（Bob）必须在 3 天内（或约定期限内）披露 $R$ 以更新余额，否则 Alice 将能收回资金。
>
> 对 Alice 而言，一旦超时生效，她必须立即进行更替或广播“HTLC 超时”交易，并在后续广播“HTLC 超时可撤销交付”交易。
>
> 若对手方拒绝更替或拖延，则必须将包含 HTLC 交易的当前通道状态广播至区块链。

The amount of time flexibility with these offers to novate are depen- dent upon one’s contingent dependencies on the hashlock R. If one estab- lishes a contract that the HTLC must be resolved within 1 day, then if the transaction times out Alice must resolve it by day 4 (3 days plus 1), else Alice risks losing funds.

> 更替操作的时间灵活性取决于对哈希锁 $R$ 的依赖程度。若合约规定 HTLC 必须在 1 天内解决，那么在 3 天锁定到期后，Alice 必须在第 4 天（3+1）前解决，否则将面临资金风险。