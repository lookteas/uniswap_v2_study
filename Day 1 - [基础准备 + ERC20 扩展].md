## Day 1 - [基础准备 + ERC20 扩展]

​																																**日期： 2025年12月10日**

------



### 今日学习文件

- [v2-core/contracts/UniswapV2ERC20.sol](cci:7://file:///d:/code/web3/uniswap/uniswap_v2/src/v2-core/contracts/UniswapV2ERC20.sol:0:0-0:0)



### 核心概念

1. 理解 AMM 原理、恒定乘积公式 `x*y=k`
2. 逐行阅读 [UniswapV2ERC20.sol](cci:7://file:///d:/code/web3/uniswap/uniswap_v2/src/v2-core/contracts/UniswapV2ERC20.sol:0:0-0:0)
3. 学习 EIP-2612 permit 机制



**学习重点**：

- [ ] ERC20 标准函数
- [ ] `permit()` 无 gas 授权原理
- [ ] `DOMAIN_SEPARATOR` 和 `PERMIT_TYPEHASH` 的作用
- [ ] `ecrecover` 签名验证

------



### 代码笔记规范(仅供参考)

> 根据自己认为的重点内容进行标记学习，笔记规范按自己喜好和习惯来

#### **示例：**

##### ***函数名* permit()**

- 作用：允许用户通过**离线签名**的方式授权其代币给某个地址（spender）使用。
- 关键逻辑： 
  - **离线签名：** 用户在本地对一条结构化消息（包含所有者、花费者、金额、截止时间等）进行签名。
  - **提交验证：** 任何人（或中继者）可以将这个签名提交到链上的 `permit()` 函数。

- 疑问：
  - **为什么需要 `nonce` 和 `deadline`？**
  - **`permit()` 和 `approve()` 有什么区别？**




------

## 验证问题答案
1. permit 相比 approve 有什么优势？

2. 为什么需要 nonce？

3. DOMAIN_SEPARATOR 如何防止跨链重放攻击？

   ------

   

## 明日预习内容

> 工厂合约 + 数学库  详情见学习计划文档

