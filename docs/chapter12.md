## 12. Conclusion

## 12. 结论

Creating a network of micropayment channels enables bitcoin scalability, micropayments down to the satoshi, and near-instant transactions. These channels represent real Bitcoin transactions, using the Bitcoin scripting op- codes to enable the transfer of funds without risk of counterparty theft, especially with long-term miner risk mitigations.

> 构建微支付通道网络实现了比特币的扩容，支持细化到聪（Satoshi）级别的微支付，并提供近乎瞬时的交易速度。
>
> 这种通道本质上是真实的比特币交易。通过利用比特币脚本操作码，资金转移无需承担对手方盗窃的风险。此外，长期来看，这也缓解了潜在的矿工风险。

If all transactions using Bitcoin were on the blockchain,  to enable  7 billion people to make two transactions per day, it would require 24GB blocks every ten minutes at best (presuming 250 bytes per transaction and 144 blocks per day). Conducting all global payment transactions on the blockchain today implies miners will need to do an incredible amount of computation, severely limiting bitcoin scalability and full nodes to a few centralized processors.

> 若所有比特币交易均在链上进行，为支持全球 70 亿人每天 2 笔交易的需求，区块容量每 10 分钟需达到 24GB（假设每笔交易 250 字节）。
>
> 在主链上承载全球所有支付行为，意味着矿工需承担天文数字般的计算量。这会严重制约比特币的扩展性，并导致全节点沦为少数中心化处理者的专利。

If all transactions using Bitcoin were conducted inside a network of micropayment channels, to enable 7 billion people to make two channels per year with unlimited transactions inside the channel, it would require 133 MB blocks (presuming 500 bytes per transaction and 52560 blocks per year). Current generation desktop computers will be able to run a full node with old blocks pruned out on 2TB of storage.

> 相比之下，若全球支付均通过微支付通道网络进行，假设 70 亿人每年仅需开启 2 个通道（通道内交易次数不限），则仅需 133MB 的区块容量（假设每笔交易 500 字节）。
>
> 在这种规模下，目前的主流个人电脑即可胜任全节点工作，若配合区块修剪（Pruning）技术，仅需约 2TB 存储空间。

With a network of instantly confirmed micropayment channels whose payments are encumbered by timelocks and hashlock outputs, Bitcoin can scale to billions of users without custodial risk or blockchain centralization when transactions are conducted securely off-chain using bitcoin scripting, with enforcement of non-cooperation by broadcasting signed multisignature transactions on the blockchain.

> 通过构建由即时确认、且受哈希锁与时间锁约束的支付构成的微支付通道网络，比特币可在无托管风险且不导致中心化的前提下，支撑数十亿用户。
>
> 借助比特币脚本，各方能在互不信任的环境下安全地进行链下交易，并在发生分歧时通过广播已签署的多重签名交易来强制执行合约。
