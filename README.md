# Uniswap V2 源码 7 天学习计划

## 学习目标
- **总代码量**：约 600 行核心代码
- **每日学习时间**：2-3 小时
- **产出**：每日笔记 + 最终完整理解文档s
- **学习周期：** 2025年12月10日 —— 2025年12月16日

---

##### Uniswap V2 源码:

```
v2 core仓库地址： https://github.com/Uniswap/v2-core
v2 periphery仓库地址：https://github.com/Uniswap/v2-periphery

```



## 📅 Day 1：基础准备 + ERC20 扩展

| 时间 | 内容                                                         | 文件                                                         |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1h   | 理解 AMM 原理、恒定乘积公式 `x*y=k`                          | 理论学习                                                     |
| 1h   | 逐行阅读 [UniswapV2ERC20.sol](cci:7://file:///d:/code/web3/uniswap/uniswap_v2/src/v2-core/contracts/UniswapV2ERC20.sol:0:0-0:0) | [v2-core/contracts/UniswapV2ERC20.sol](cci:7://file:///d:/code/web3/uniswap/uniswap_v2/src/v2-core/contracts/UniswapV2ERC20.sol:0:0-0:0) |
| 0.5h | 学习 EIP-2612 permit 机制                                    | 同上                                                         |

**学习重点**：

- [ ] ERC20 标准函数
- [ ] `permit()` 无 gas 授权原理
- [ ] `DOMAIN_SEPARATOR` 和 `PERMIT_TYPEHASH` 的作用
- [ ] `ecrecover` 签名验证

**笔记验证问题**：
1. permit 相比 approve 有什么优势？
2. 为什么需要 nonce？
3. DOMAIN_SEPARATOR 如何防止跨链重放攻击？

---

## 📅 Day 2：工厂合约 + 数学库

| 时间 | 内容                                                         | 文件                                                         |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1h   | 逐行阅读 [UniswapV2Factory.sol](cci:7://file:///d:/code/web3/uniswap/uniswap_v2/src/v2-core/contracts/UniswapV2Factory.sol:0:0-0:0) | [v2-core/contracts/UniswapV2Factory.sol](cci:7://file:///d:/code/web3/uniswap/uniswap_v2/src/v2-core/contracts/UniswapV2Factory.sol:0:0-0:0) |
| 0.5h | 理解 CREATE2 确定性部署                                      | 同上                                                         |
| 0.5h | 阅读 [Math.sol](cci:7://file:///d:/code/web3/uniswap/uniswap_v2/src/v2-core/contracts/libraries/Math.sol:0:0-0:0) | [v2-core/contracts/libraries/Math.sol](cci:7://file:///d:/code/web3/uniswap/uniswap_v2/src/v2-core/contracts/libraries/Math.sol:0:0-0:0) |
| 0.5h | 阅读 [UQ112x112.sol](cci:7://file:///d:/code/web3/uniswap/uniswap_v2/src/v2-core/contracts/libraries/UQ112x112.sol:0:0-0:0) | [v2-core/contracts/libraries/UQ112x112.sol](cci:7://file:///d:/code/web3/uniswap/uniswap_v2/src/v2-core/contracts/libraries/UQ112x112.sol:0:0-0:0) |

**学习重点**：
- [ ] `createPair()` 如何用 CREATE2 部署
- [ ] 为什么用 `keccak256(bytecode, salt)` 计算地址
- [ ] 巴比伦法求平方根
- [ ] UQ112.112 定点数表示法

**笔记验证问题**：
1. 为什么 Uniswap 选择 CREATE2 而不是 CREATE？
2. `feeTo` 和 `feeToSetter` 的作用是什么？
3. UQ112x112 为什么用 112 位？

---

## 📅 Day 3：Pair 合约（上）- 状态变量与 mint

| 时间 | 内容                          | 文件                                                         |
| ---- | ----------------------------- | ------------------------------------------------------------ |
| 0.5h | 阅读所有状态变量和修饰符      | [v2-core/contracts/UniswapV2Pair.sol](cci:7://file:///d:/code/web3/uniswap/uniswap_v2/src/v2-core/contracts/UniswapV2Pair.sol:0:0-0:0) |
| 1.5h | 深入理解 `mint()` 函数        | 同上                                                         |
| 0.5h | 理解 `MINIMUM_LIQUIDITY` 设计 | 同上                                                         |

**学习重点**：
- [ ] `reserve0`, `reserve1` 储备量
- [ ] `price0CumulativeLast` 价格累计
- [ ] `lock` 重入锁
- [ ] `mint()` 首次铸造 vs 后续铸造的区别
- [ ] 为什么首次铸造要锁定 1000 wei LP Token

**笔记验证问题**：
1. 手算：如果存入 100 ETH 和 200000 USDC，获得多少 LP Token？
2. `MINIMUM_LIQUIDITY` 如何防止攻击？
3. 为什么 mint 前要先转入代币？

---

## 📅 Day 4：Pair 合约（中）- burn 与 swap

| 时间 | 内容                   | 文件                                                         |
| ---- | ---------------------- | ------------------------------------------------------------ |
| 1h   | 深入理解 `burn()` 函数 | [v2-core/contracts/UniswapV2Pair.sol](cci:7://file:///d:/code/web3/uniswap/uniswap_v2/src/v2-core/contracts/UniswapV2Pair.sol:0:0-0:0) |
| 1.5h | 深入理解 `swap()` 函数 | 同上                                                         |
| 0.5h | 理解 Flash Swap 机制   | 同上                                                         |

**学习重点**：
- [ ] `burn()` 如何按比例返还代币
- [ ] `swap()` 的 k 值校验逻辑
- [ ] 0.3% 手续费如何收取
- [ ] Flash Swap 的回调机制

**笔记验证问题**：
1. 手算：持有 10% LP Token，池子有 100 ETH + 200000 USDC，能取回多少？
2. 手算：用 1 ETH 换 USDC，能得到多少？（假设池子 100:200000）
3. Flash Swap 和 Aave 闪电贷有什么区别？

---

## 📅 Day 5：Pair 合约（下）- 预言机与协议费

| 时间 | 内容                         | 文件                                                         |
| ---- | ---------------------------- | ------------------------------------------------------------ |
| 1h   | 理解 `_update()` 和价格累计  | [v2-core/contracts/UniswapV2Pair.sol](cci:7://file:///d:/code/web3/uniswap/uniswap_v2/src/v2-core/contracts/UniswapV2Pair.sol:0:0-0:0) |
| 1h   | 理解 `_mintFee()` 协议费机制 | 同上                                                         |
| 0.5h | 阅读 `sync()` 和 `skim()`    | 同上                                                         |

**学习重点**：
- [ ] TWAP 时间加权平均价格原理
- [ ] `price0CumulativeLast` 如何累加
- [ ] 协议费 1/6 的数学推导
- [ ] `sync()` vs `skim()` 的使用场景

**笔记验证问题**：
1. 如何用 priceCumulative 计算过去 1 小时的平均价格？
2. 为什么协议费是 1/6 而不是直接收取？
3. 什么情况下需要调用 sync()？

---

## 📅 Day 6：Router 与 Library

| 时间 | 内容                                                         | 文件                                                         |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1h   | 阅读 [UniswapV2Library.sol](cci:7://file:///d:/code/web3/uniswap/uniswap_v2/src/v2-periphery/contracts/libraries/UniswapV2Library.sol:0:0-0:0) | [v2-periphery/contracts/libraries/UniswapV2Library.sol](cci:7://file:///d:/code/web3/uniswap/uniswap_v2/src/v2-periphery/contracts/libraries/UniswapV2Library.sol:0:0-0:0) |
| 1.5h | 阅读 [UniswapV2Router02.sol](cci:7://file:///d:/code/web3/uniswap/uniswap_v2/src/v2-periphery/contracts/UniswapV2Router02.sol:0:0-0:0) 核心函数 | [v2-periphery/contracts/UniswapV2Router02.sol](cci:7://file:///d:/code/web3/uniswap/uniswap_v2/src/v2-periphery/contracts/UniswapV2Router02.sol:0:0-0:0) |
| 0.5h | 理解多跳交换路径                                             | 同上                                                         |

**学习重点**：
- [ ] `pairFor()` 如何不调用链上计算地址
- [ ] `getAmountOut()` / `getAmountIn()` 公式
- [ ] `addLiquidity()` 的滑点保护
- [ ] `swapExactTokensForTokens()` 多跳实现

**笔记验证问题**：
1. 为什么 Library 可以离线计算 Pair 地址？
2. `amountOutMin` 参数如何保护用户？
3. A→B→C 三跳交换的 gas 成本大约是多少？

---

## 📅 Day 7：综合复习 + 实战

| 时间 | 内容                                                         | 文件                                                         |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1h   | 复习所有核心函数，画完整调用图                               | 全部                                                         |
| 1h   | 阅读 [UniswapV2OracleLibrary.sol](cci:7://file:///d:/code/web3/uniswap/uniswap_v2/src/v2-periphery/contracts/libraries/UniswapV2OracleLibrary.sol:0:0-0:0) | [v2-periphery/contracts/libraries/](cci:7://file:///d:/code/web3/uniswap/uniswap_v2/src/v2-periphery/contracts/libraries:0:0-0:0) |
| 1h   | 尝试写一个简单的套利合约（伪代码）                           | 实战练习                                                     |

**最终验证清单**：
- [ ] 能完整描述 addLiquidity 的调用链
- [ ] 能完整描述 swap 的调用链
- [ ] 能手算 LP Token 数量
- [ ] 能手算 swap 输出数量
- [ ] 理解 Flash Swap 攻击向量
- [ ] 理解 TWAP 预言机的优缺点

---

## 📝 每日笔记模板

```markdown
# Day X - [主题]

## 今日学习文件
- 

## 核心概念
1. 
2. 

## 代码笔记
### 函数名
- 作用：
- 关键逻辑：
- 疑问：

## 验证问题答案
1. 
2. 

## 明日预习
- 
```

---

## 🎯 学习建议

1. **不要跳过手算** - 用具体数字验证公式是最好的理解方式
2. **画状态变化图** - 每个函数执行前后状态如何变化
3. **对比思考** - 为什么这样设计？有没有其他方案？
4. **记录疑问** - 随时记录，可以让AI工具先帮你解答，然后远程会议讨论

