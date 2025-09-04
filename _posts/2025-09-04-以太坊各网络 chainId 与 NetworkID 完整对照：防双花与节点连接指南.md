---
layout:     post
title:      以太坊各网络 chainId 与 NetworkID 完整对照：防双花与节点连接指南
date:       2025-09-04
header-img: img/post-bg-desk.jpg
catalog: true
---

> 本文整理了以太坊、以太坊经典以及主流以太坊兼容链的 **chainId** 和 **NetworkID** 速查清单，附带应用场景与常见问答，帮助开发者和普通用户快速确认链上身份，避免误转或多链冲突。

---

## 一、为什么要区分 chainId 与 NetworkID？

| 概念 | 作用 | 触发时机 |
| --- | --- | --- |
| **chainId** | 防 **双花攻击**，在签名阶段告诉交易仅能发送到指定链 | 发起交易 |
| **NetworkID** | 保障 **节点 P2P 连接** 的质量与安全，不同 ID 无法握手 | 节点同步 |

理解这两个值，就等于给你准备空投、桥接、开发合约或搭建节点时上了 **双重保险**。

---

## 二、以太坊官方链 ID 速览

以下数据为 **官方长期维护**、非实验性质的网络列表。

- **以太坊主网 Mainnet**
  - `chainId`: 1  
  - `NetworkID`: 1

- **Ropsten 测试网（PoW）**
  - `chainId`: 3  
  - `NetworkID`: 3

- **Rinkeby 测试网（PoA）**
  - `chainId`: 4  
  - `NetworkID`: 4

- **Goerli 测试网（PoA）**
  - `chainId`: 5  
  - `NetworkID`: 5

- **Kovan 测试网（PoA）**
  - `chainId`: 42  
  - `NetworkID`: 42

> 👉 [多测试网逐步退役，Goerli 成为主力测试链后，下一步应该如何确保安全又免费的开发体验？](https://okxdog.com/)

---

## 三、以太坊经典与早期分叉网络

- **Ethereum Classic Mainnet**
  - `chainId`: 61  
  - `NetworkID`: 1

- **Morden 经典测试网**
  - `chainId`: 62  
  - `NetworkID`: 2

值得注意：ETC 与 ETH 主网的 `NetworkID` 相同，但 `chainId` 不同，**签名时依旧可避免重复交易**，在跨链桥或重放防护场景下尤其重要。

---

## 四、扩容与兼容链常用 ID（精选 Top 20）

为方便日常操作，以下仅保留活跃度高、工具文档完善的网络。

1. **Optimism**：`chainId: 10` / `NetworkID: 10`
2. **Arbitrum One**：`chainId: 42161` / `NetworkID: 42161`
3. **Polygon**：`chainId: 137` / `NetworkID: 137`
4. **BNB Smart Chain**：`chainId: 56` / `NetworkID: 56`
5. **Avalanche C-Chain**：`chainId: 43114` / `NetworkID: 1`
6. **Fantom**：`chainId: 250` / `NetworkID: 250`
7. **Moonbeam**（Polkadot）：`chainId: 1284` / `NetworkID: 1284`
8. **Moonriver**（Kusama）：`chainId: 1285` / `NetworkID: 1285`
9. **xDai / Gnosis**：`chainId: 100` / `NetworkID: 100`
10. **Klaytn**（Cypress）：`chainId: 8217` / `NetworkID: 8217`
11. **Celo**（Mainnet）：`chainId: 42220` / `NetworkID: 42220`
12. **RSK**（Bitcoin Sidechain）：`chainId: 30` / `NetworkID: 30`
13. **OKT Chain**：`chainId: 66` / `NetworkID: 66`
14. **TomoChain**：`chainId: 88` / `NetworkID: 88`
15. **Wanchain**：`chainId: 888` / `NetworkID: 888`
16. **Heco**（原火币生态链）：`chainId: 128` / `NetworkID: 128`
17. **Energy Web Chain**：`chainId: 246` / `NetworkID: 246`
18. **Cronos**（Crypto.org）：`chainId: 25` / `NetworkID: 25`
19. **Metis**：`chainId: 1088` / `NetworkID: 1088`
20. **zkSync Era**：`chainId: 324` / `NetworkID: 324`

> 👉 [想快速为自己的 RPC 节点补全所有常用链的 chainId？这份脚本一键搞定](https://okxdog.com/)

---

## 五、使用场景与踩坑提示

1. **MetaMask 添加自定义网络**  
   填写 **chainId** 必须与目标链一致，否则前端会报错 `Incorrect chain ID`。

2. **Graph Node / The Graph 部署子图**  
   在 `network` 字段中一般填写 **NetworkID**；若官方索引器缺数据，可能需要手动调整。

3. **空投猎人**  
   不同链的地址格式相同，若 `chainId` 不匹配将导致 **代币永久锁定**。

4. **跨链桥**  
   多数桥会再次验证 `chainId` 与收款链是否相符，误填将导致 **压力测试式的赎回流程**。

---

## 常⻅问题 FAQ

**Q1：为何有时看到 Arbitrum 的 NetworkID 是 0？**  
A：Arbitrum 在后端同步以太坊主网络（NetworkID=1），但其 `chainId` 独立为 42161，以防止重放。

**Q2：NetworkID 和 P2P port 有何区别？**  
A：NetworkID 负责 **握手身份识别**，P2P port 只是 TCP 端口；前者决定了“能连”，后者决定了“可通”。

**Q3：钱包里没有 chainId 字段的历史交易会受影响吗？**  
A：早在 **EIP-155** 已实现跨链回放保护，旧交易在未升级的链上才可被重放，现今基本无需担心。

**Q4：能否随机定义新的 chainId？**  
A：技术可行，但**EIP-155** 鼓励通过 [chainid.network](https://chainid.network/) 申报，避免碰撞。

**Q5：Layer2 网络如何查看官方声明的 chainId？**  
A：在官方桥的连接文档、GitHub release note 或区块浏览器首页底部，几乎都会给出明确值。

**Q6：测试网水龙头总说 “wrong network”，怎么办？**  
A：重点核对 `chainId` + `RPC 域名 + NetworkID`，确认三者完整匹配，缺一不可。

---

## 六、一键速查备忘单

将以下保存为常用书签，复制粘贴即可：

- 以太坊主网  
  - RPC: `https://mainnet.infura.io/v3/YOUR_KEY`  
  - chainId: 1

- Polygon 主网  
  - RPC: `https://polygon-rpc.com`  
  - chainId: 137

- Avalanche C-Chain  
  - RPC: `https://api.avax.network/ext/bc/C/rpc`  
  - chainId: 43114

如需更全面数据，可浏览社区维护的查询仓库 [ethereum-lists/chains](https://github.com/ethereum-lists/chains)。