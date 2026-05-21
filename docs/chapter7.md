## 7. Pay to Contract

## 7. 向合约支付

It is possible construct a cryptographically provable “Delivery Versus Pay- ment” contract, or pay-to-contract[18], as proof of payment. This proof can be established as knowledge of the input R from hash(R) as payment of a certain value. By embedding a clause into the contract between the buyer and seller stating that knowing R is proof of funds sent, the recipient of funds has no incentive to disclose R unless they have certainty that they will receive payment. When the funds eventually get pulled from the buyer by their counterparty in their micropayment channel, R is disclosed as part of that pull of funds. One can design paper legal documents that specify that knowledge or disclosure of R implies fulfillment of payment. The sender can then arrange a cryptographically signed contract with knowledge of in- puts for hashes treated as fulfillment of the paper contract before payment occurs.

> 我们可以构建一种加密可证明的“货到付款”合约，即“向合约支付”（Pay-to-Contract）[18]，作为支付证明。
>
> 该证明基于对 $hash(R)$ 对应原像 $R$ 的掌握。通过在买卖双方的合同中加入条款，声明“掌握 $R$ 即视为已支付款项”，接收方只有在确信能收到款项时才会披露 $R$。
>
> 当款项最终通过微支付通道被提取时，$R$ 会作为提取资金的一部分被披露。甚至可以设计法律文件，规定披露 $R$ 即意味着履行了支付义务。发送方可据此在支付发生前，将对哈希原像的掌握作为纸质合同履行的加密依据。
