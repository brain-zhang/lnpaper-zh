## 9. 风险

主要风险与时间锁到期相关。此外，核心节点和部分商家为降低路由延迟需保持私钥在线。不过，终端用户和普通节点仍可将私钥存放在冷存储中。

### 9.1 Improper Timelocks

### 9.1 时间锁设置不当

Participants must choose timelocks with sufficient amounts of time. If insuf- ficient time is given, it is possible that timelocked transactions believed to be invalid will become valid, enabling coin theft by the counterparty. There is a trade-off between longer timelocks and the time-value of money. When writing wallet and Lightning Network application software, it is necessary to ensure that sufficient time is given and users are able to have their trans- actions enter into the blockchain when interacting with non-cooperative or malicious channel counterparties.

> 参与者必须选择足够长的锁定时间。若时长不足，已过期的交易可能被误认为仍然有效，从而给对手方窃取资金的机会。
>
> 需在更长的锁定期与资金时间价值之间进行权衡。钱包和闪电网络应用在开发时，必须确保预留充裕时间，以便用户在面对不协作或恶意的对手时，能顺利将交易广播至区块链以收回资金。

### 9.2 Forced Expiration Spam

### 9.2 强制过期引发的垃圾交易攻击

Forced expiration of many transactions may be the greatest systemic risk when using the Lightning Network. If a malicious participant creates many channels and forces them all to expire at once, these may overwhelm block data capacity, forcing expiration and broadcast to the blockchain. The result would be mass spam on the bitcoin network. The spam may delay transactions to the point where other locktimed transactions become valid.

> 大规模交易的强制过期是闪电网络面临的最大系统性风险。恶意参与者若创建海量通道并强制它们同时过期，可能导致广播数据超出区块容量，迫使比特币网络充斥垃圾交易。
>
> 这种拥堵可能导致其他受时间锁限制的交易因无法及时处理而失效。

This may be mitigated by permitting one transaction replacement on all pending transactions. Anti-spam can be used by permitting only one transaction replacement of a higher sequence number by the inverse of an even or odd number. For example, if an odd sequence number was broad- cast, permit a replacement to a higher even number only once. Transactions would use the sequence number in an orderly way to replace other trans- actions. This mitigates the risk assuming honest miners. This attack is extremely high risk, as incorrect broadcast of Commitment Transactions entail a full penalty of all funds in the channel.

> 缓解措施之一是允许对挂起（Pending）交易进行单次替换。例如，通过奇偶数反转来限制序列号的提升：若已广播奇数序列号交易，仅允许替换为更高的偶数序列号一次。
>
> 这种有序替换机制在矿工诚实的前提下能降低风险。由于恶意广播承诺交易会触发全额罚没，此类攻击对攻击者而言风险极高。

Additionally, one may attempt to steal HTLC transactions by forcing a timeout transaction to go through when it should not. This can be easily mitigated by having each transfer inside the channel be lower than the total transaction fees used. Since transactions be extremely cheap and do not hit the blockchain with cooperative channel counterparties, large transfers of value can be split into many small transfers. This attempt can only work if the blocks are completely full for a long time. While it is possible to mitigate it using a longer HTLC timeout duration, variable block sizes may become common, which may need mitigations.

> 此外，攻击者可能尝试通过强制 HTLC 超时来窃取资金。缓解方法是确保通道内每笔转账金额低于总交易手续费。
>
> 由于协作时交易不上链，大额转账可拆分为多笔小额交易。此类攻击仅在区块长期满载时可能奏效。延长 HTLC 超时或采用可变区块大小（Variable Block Size）方案可进一步降低风险。

If this type of transaction becomes the dominant form of transactions which are included on the blockchain, it may become necessary to increase the block size and run a variable blocksize structure and timestop flags   as described in the section below. This can create sufficient penalties and disincentives to be highly unprofitable and unsuccessful for attackers, as attackers lose all their funds from broadcasting the wrong transaction, to the point where it will never occur.

> 若此类交易成为链上主流，则有必要通过增加区块容量、引入可变区块结构及 Timestop 标记来应对。
>
> 严厉的惩罚机制将使此类攻击无利可图，从而从根本上遏制攻击动机。


### 9.3 Coin Theft via Cracking

### 9.3 遭遇入侵导致资金被盗

As parties must be online and using private keys to sign, there is a possibility that, if the computer where the private keys are stored is compromised, coins will be stolen by the attacker. While there may  be methods to mitigate  the threat for the sender and the receiver, the intermediary nodes must be online and will likely be processing the transaction automatically. For this reason, the intermediary nodes will be at risk and should not be holding  a substantial amount of money in this “hot wallet.” Intermediary nodes which have better security will likely be able to out-compete others in the long run and be able to conduct greater transaction volume due to lower fees. Historically, one of the largest component of fees and interest in the financial system are from various forms of counterparty risk – in Bitcoin it is possible that the largest component in fees will be derived from security risk premiums.

> 由于参与方需在线使用私钥签名，若存钥计算机被攻破，资金可能被盗。发送方和接收方尚有防御手段，但必须在线处理交易的中间节点面临较大风险，因此其“热钱包”不应存放过多资金。
>
> 长远来看，安全性更高的中间节点将因能提供更低廉的费率而胜出。传统金融体系的手续费大多源于交易对手风险，而在比特币中，费用的主要组成部分可能是安全风险溢价。

A Funding Transaction may have multiple outputs with multiple Com- mitment Transactions, with the Funding Transaction key and some Commit- ment Transactions keys stored offline. It is possible to create an equivalent of a “Checking Account” and “Savings Account” by moving funds between outputs from a Funding Transaction, with the “Savings Account” stored offline and requiring additional signatures from security services.

> 筹资交易可设置多个输出对应不同的承诺交易，其部分私钥可离线存放。
>
> 通过在筹资交易的输出间转移资金，可以构建类似于“活期”和“定期”账户的体系，其中“定期账户”离线存储并需要额外的安全签名。

### 9.4 Data Loss

### 9.4 数据丢失

When one party loses data, it is possible for the counterparty to steal funds. This can be mitigated by having a third party data storage service where encrypted data gets sent to this third party service which the party cannot decrypt. Additionally, one should choose channel counterparties who are responsible and willing to provide the current state, with some periodic tests of honesty.

> 若某方丢失数据，对手方可能趁机窃取资金。对策是采用第三方加密存储服务（第三方无法解密）。
>
> 此外，应选择负责且配合的对手方，并定期进行诚信测试。

### 9.5 Forgetting to Broadcast the Transaction in Time 

### 9.5 未能及时广播交易

If one does not broadcast a transaction at the correct time, the counterparty may steal funds. This can be mitigated by having a designated third party to send funds. An output fee can be added to create an incentive for this third party to watch the network. Further, this can also be mitigated by implementing OP CHECKSEQUENCEVERIFY.

> 若未能在规定时间内广播交易，对手方可能窃取资金。
>
> 解决方法是委托第三方监控并在必要时代为发送。通过在输出中设置手续费来激励第三方。此外，`OP_CHECKSEQUENCEVERIFY` 也能缓解此类问题。

### 9.6 Inability to Make Necessary Soft-Forks

### 9.6 软分叉无法实施

Changes are necessary to bitcoin, such as the malleability soft-fork. Addi- tionally, if this system becomes popular, it will be necessary for the system to securely transact with many users and some kind of structure like a blockheight timestop will be desirable. This system assumes such changes to enable Lightning Network to exist entirely, as well as soft-forks ensuring the security is robust against attackers will occur. While the system may continue to operate with only some time lock and malleability soft-forks, there will be necessary soft-forks regarding systemic risks. Without proper community foresight, an inability to establish a timestop or similar func- tion will allow systemic attacks to take place and may not be recognized as imperative until an attack actually occurs.

> 闪电网络需要比特币进行软分叉以解决延展性问题。此外，大规模普及还需引入如 Timestop 等机制来保障安全。
>
> 若仅解决延展性而不考虑长期的系统性风险，系统虽能运行，但在遭受攻击时可能缺乏足够的防御手段。若社区缺乏预见性，未能及时建立 Timestop 等功能，系统性攻击可能在被意识到必要性前就已发生。

### 9.7 Colluding Miner Attacks

### 9.7 勾结矿商发动攻击

Miners may elect to refuse to enter in particular transactions (e.g. Breach Remedy transactions) in order to assist in timeout coin theft. An attacker can pay off all miners to refuse to include certain transactions in their mem- pool and blocks. The miners can identify their own blocks in an attempt to prove their behavior to the paying attacker.

> 矿工可能拒绝打包特定交易（如违约补救交易），以协助他人窃取超时资金。攻击者可能通过贿赂全体矿工，使其在内存池和区块中排除特定交易。
>
> 矿工可通过在区块中添加标识向攻击者邀功。

This can be mitigated by encouraging miners to avoid identifying their own blocks. Further, it should be expected that this kind of payment to miners is malicious activity and the contract is unenforcible. Miners may then take payment and surreptitiously mine a block without identifying the block to the attacker. Since the attacker is paying for this, they will quickly run out of money by losing the fee to the miner, as well as losing all their money in the channel. This attack is unlikely and fairly unattractive as it is far too difficult and requires a high degree of collusion with extreme risk. 

> 缓解对策包括鼓励矿工匿名打包。此外，此类交易本质上是恶意违规且不受法律保障。
>
> 矿工可能在拿走贿赂后依然打包了该交易，却不向攻击者透露。对于攻击者而言，这不仅损失了贿赂金，还会丢掉通道内的全部资金。此类攻击极难实施且风险巨大，对攻击者缺乏吸引力。

The risk model of this attack occurirng is similar to that of miners colluding to do reorg attacks: Extremely unlikely with many uncoordinated miners.

> 这种勾结攻击的风险模型类似于矿工勾结发起 51% 重组攻击：在存在大量独立矿工的环境下，这几乎是不可能的。