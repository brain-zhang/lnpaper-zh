## Appendix A  Resolving Malleability 

## 附录A 解决交易延展性问题

In order to create these contracts in Bitcoin without a third party trusted service, Bitcoin must fix the transaction malleability problem. If transac- tions can be mutated, then signatures can be invalidated, thereby making refund transactions and commitment bonds invalidated. This creates an opportunity for hostile actors to use it as an opportunity for a negotiating tactic to steal coins, in effect, a hostage scenario.

> 若要在比特币系统中实现无需第三方的合约，必须解决交易延展性（Malleability）问题。若交易数据可被改动，签名即告失效，从而导致退款交易和承诺保证金无效。
>
> 这为恶意行为者提供了可乘之机，使其能通过改变交易 ID 来勒索资金。

To mitigate malleability, it is necessary to make a soft-fork change to bitcoin. Older clients would still work, but miners would need to update. Bitcoin has had several soft forks in the past, including pay-to-script-hash (P2SH).

> 解决延展性问题需通过软分叉。老旧客户端仍可运行，但矿工必须升级。比特币历史上曾多次成功实施软分叉，如 P2SH 功能。

To mitigate malleability, it requires changing which contents are signed by the participants. This is achieved by creating new sighash types. In order to accommodate this new behavior, a new P2SH type or new OP CHECKSIG is necessary to make it a soft-fork rather than a hard-fork.

> 修复延展性需要调整签名覆盖的内容，这可通过引入新的签名哈希（Sighash）类型来实现。为确保其为软分叉而非硬分叉，可引入新型 P2SH 或新的 `OP_CHECKSIG` 操作码。

If a new P2SH was defined, it would use a different output script such as:

> 如果定义了新的P2SH，它将使用不同的输出脚本，如:

```
OP DUP OP HASH160 <20-byte hash> OP EQUALVERIFY
```

Since this will always resolve to true provided a valid redeemScript, all existing clients will return true. This allows the scripting system to construct new rules, including new signature validation rules. At least one new sighash would need to exist.

> 只要提供有效的赎回脚本（RedeemScript），此类脚本在旧客户端上始终返回 True。这使得我们能在现有脚本系统框架下建立新规则（如新的签名验证规则）。这至少需要一种新的 Sighash 类型。

SIGHASH NOINPUT would neither sign any input transactions IDs nor sign the index. By using SIGHASH NOINPUT, one can be assured that one’s counterparty cannot invalidate entire trees of chained transactions of potential contract states which were previously agreed upon, using transac- tion ID mutation. With the new sighash flags, it is possible to spend from a parent transaction even though the transaction ID has changed, so long as the script evaluates as true (i.e. a valid signature).

> `SIGHASH_NOINPUT` 既不签署输入交易 ID，也不签署索引。通过此标志，参与方可确保对手方无法通过修改交易 ID 来破坏预先达成的合约交易链。
>
> 即使父交易 ID 改变，只要脚本验证通过（即签名有效），子交易依然可以成功支出。

SIGHASH NOINPUT implies significant risk with address reuse, as it can work with any transaction in which the sigScript returns as valid, so multiple transactions with the same outputs are redeemable (provided the output values are less).

> `SIGHASH_NOINPUT` 在地址重用时存在巨大风险：只要 `sigScript` 有效，它就可用于支出任何具有相同输出脚本的交易（前提是输出金额足够）。

Further, and just as importantly, SIGHASH NOINPUT permits par- ticipants to sign spends of transactions without knowing the signatures of the transaction being spent. By solving malleability in the above manner, two parties may build contracts and spend transactions without either party having the ability to broadcast that original transaction on the blockchain until both parties agree. With the new sighash type, participants may build potential contract states and potential payout conditions and agree upon all terms, before the contract may be paid, broadcast, and executed upon without the need for a trusted third party.

> 同样重要的是，`SIGHASH_NOINPUT` 允许参与者在不知道父交易签名的情况下，预先为支出交易签名。
>
> 这种修复延展性的方式使得双方能构建并支出尚未上链的交易。参与者可预设合约状态和支付条件，待条件成熟后再执行支付和广播，全程无需第三方参与。

Without SIGHASH NOINPUT, one cannot build outputs before the transaction can be funded. It is as if one cannot make any agreements without committing funds without knowing what one is committing to. SIGHASH NOINPUT allows one to build redemption for transactions which do not yet exist. In other words, one can form agreements before funding the transaction if the output is a 2-of-2 multisignature transaction.

> 若无 `SIGHASH_NOINPUT`，则无法在筹集资金前构建支出。这好比在不知道资金来源时就无法签署支付协议。
>
> `SIGHASH_NOINPUT` 使得为“尚不存在”的交易构建赎回路径成为可能。换言之，若输出为 2-of-2 多重签名，双方可在筹资前就达成协议。

To use SIGHASH NOINPUT, one builds a Funding  Transaction, and does not yet sign it. This Funding Transaction does not need to use SIGHASH NOINPUT if it is spending from a transaction which has already been entered into the blockchain. To spend from a Funding Transaction with a 2-of-2 multisignature output which has not yet been signed and broadcast, however, requires using SIGHASH NOINPUT.

> 使用 `SIGHASH_NOINPUT` 时，需先构建未签名的筹资交易。若筹资交易花费的是已上链的资金，则其本身不一定需要 `SIGHASH_NOINPUT`。
>
> 但若要花费一笔尚未签署或广播的 2-of-2 筹资交易输出，则必须使用 `SIGHASH_NOINPUT`。

A further stop-gap solution using OP_CHECKSEQUENCEVERIFY or a less-optimal use of OP_CHECKLOCKTIMEVERIFY will be described in a future paper by Rusty Russell. An updated version of this paper will also include these constructions.

> 采用 `OP_CHECKSEQUENCEVERIFY` 或 `OP_CHECKLOCKTIMEVERIFY` 的替代方案由 Rusty Russell 提出。本文的后续版本将包含这些临时解决方案的详细描述。
