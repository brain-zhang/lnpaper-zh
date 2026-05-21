## 10. Block Size Increases and Consensus

## 10. 区块大小与共识

If  we  presume that a decentralized payment network exists and one user will make 3 blockchain transactions per year on average, Bitcoin will be able to support over 35 million users with 1MB blocks in ideal circumstances (assuming 2000 transactions/MB, or 500 bytes/Tx). This is quite limited, and an increase of the block size may be necessary to support everyone in the world using Bitcoin. A simple increase of the block size would be a hard fork, meaning all nodes will need to update their wallets if they wish to participate in the network with the larger blocks.

> 假设在一个去中心化支付网络中，每位用户每年平均发起 3 笔链上交易。在理想情况下（假设每笔交易 500 字节，即 2000 笔/MB），1MB 的区块仅能支持约 3500 万用户。
>
> 这一容量显然不足以支撑全球规模，因此长期来看，增加区块容量是必要的。简单的扩容通常涉及硬分叉（Hard Fork），这意味着所有希望参与大区块网络的节点都必须更新其钱包软件。

While it may appear as though this system will mitigate the block size increases in the short term, if it achieves global scale, it will necessitate a block size increase in the long term. Creating a credible tool to help prevent blockchain spam designed to encourage transactions to timeout becomes imperative.

> 虽然闪电网络在短期内能缓解扩容压力，但若要实现全球普及，长期仍需增加区块容量。此外，必须建立可靠的机制，以防止区块链遭受旨在诱发时间锁超时的垃圾交易攻击。

To  mitigate timelock spam vulnerabilities, non-miner and miners’ con- sensus rules may also differ if the miners’ consensus rules are more restrictive. Non-miners may accept blocks over 1MB, while miners may have different soft-caps on block sizes. If a block size is above that cap, then that is viewed as an invalid block by other miners, but not by non-miners. The miners will only build the chain on blocks which are valid according to the agreed-upon soft-cap. This permits miners to agree on raising the block size limit with- out requiring frequent hard-forks from clients, so long as the amount raised by miners does not go over the clients’ hard limit. This mitigates the risk of mass expiry of transactions at once. All transactions which are not re- deemed via Exercise Settlement (ES) may have a very high fee attached, and miners may use a consensus rule whereby those transactions are exempted from the soft-cap, making it very likely the correct transactions will enter the blockchain.

> 为了防范时间锁垃圾交易攻击，可以允许非矿工与矿工在共识规则上存在差异，通常矿工规则更为严格。
>
> 例如，非矿工可接受 1MB 以上的区块，而矿工则约定一个更小的“软上限”（Soft-cap）。超出此上限的区块将被其他矿工视为无效，但非矿工仍会接受。
>
> 矿工仅在符合软上限的区块上构建链。这使得矿工能在不触发客户端频繁硬分叉的前提下，协商提升区块容量（只要不超过客户端的硬上限）。
>
> 这降低了大规模交易同时过期的风险。所有未通过“执行结算”（Exercise Settlement）赎回的交易可附加极高手续费，矿工可约定此类交易不受软上限限制，从而确保合法交易优先入链。

When transactions are viewed as circuits and contracts instead of transaction packets, the consensus risks can be measured by the amount of time available to cover the UTXO set controlled by hostile parties. In effect, the upper bound of the UTXO size is determined by transaction fees and the standard minimum transaction output value. If the bitcoin miners have a deterministic mempool which prioritizes transactions respecting a “weak” local time order of transactions, it could become extremely unprofitable and unlikely for an attack to succeed. Any transaction spam time attack by broadcasting the incorrect Commitment Transaction is extremely high risk for the attacker, as it requires an immense amount of bitcoin and all funds committed in those transactions will be lost if the attacker fails.

> 当我们将交易视为电路和合约而非单纯的数据包时，共识风险可通过评估攻击者控制的 UTXO 集的变现时间来衡量。
>
> 实际上，UTXO 规模的上限受限于交易手续费和标准最小输出值。若矿工采用一种能按“弱”本地时间顺序优先处理交易的确定性内存池，攻击将变得极其困难且无利可图。
>
> 任何通过广播旧承诺交易发动的攻击对攻击者而言都是生死博弈：这不仅需要巨量比特币，一旦失败，投入的所有资金都将全数罚没。
