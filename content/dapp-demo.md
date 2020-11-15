---
id: dapp-tutorial
title: DAPP 开发指南
---

本文以一个 defi 挖矿的例子介绍如何在 starcoin 上开发 DApp。（完整的代码链接在 https://github.com/starcoinorg/rewarding-dapp）

## 准备工作

- 本地启动一个 dev 节点。可参考 [运行网络](./runnetwork.zh.md) 章节。
- 到 [starcoin wallet](https://github.com/starcoinorg/starcoin_wallet_flutter/releases) 页面下载 Starcoin 钱包。
- clone https://github.com/starcoinorg/rewarding-dapp  到本地。

## 钱包初始化

将 dev 节点的私钥导入到钱包中。
查看私钥的命令如下：
``` bash
starcoin% account export 0xf6832e44f94c95606d2ab895b719e6d2811047115a84d87646abb4ee7393bf29
account 0x2291c747b396303a6475db8468a910d0, private key: 0xf6832e44f94c95606d2ab895b719e6d2811047115a84d87646abb4ee7393bf29
```



## 合约编译

依次执行以下命令进行合约编译。（需要将文件路径替换成你自己的文件路径）
``` bash
# 编译 module
starcoin% dev compile /Volumes/SATA2000DM008-2FR102/projects/starcoinorg/rewarding-pool-dapp/contracts/modules/RewardPool.move -o /Volumes/SATA2000DM008-2FR102/projects/starcoinorg/rewarding-pool-dapp/contracts/target/modules
starcoin% dev compile /Volumes/SATA2000DM008-2FR102/projects/starcoinorg/rewarding-pool-dapp/contracts/modules/CoCo.move -o /Volumes/SATA2000DM008-2FR102/projects/starcoinorg/rewarding-pool-dapp/contracts/target/modules

# 编译 scripts
starcoin% dev compile -d /Volumes/SATA2000DM008-2FR102/projects/starcoinorg/rewarding-pool-dapp/contracts/modules -o /Volumes/SATA2000DM008-2FR102/projects/starcoinorg/rewarding-pool-dapp/contracts/target /Volumes/SATA2000DM008-2FR102/projects/starcoinorg/rewarding-pool-dapp/contracts/scripts/create_coco_and_pool.move
starcoin% dev compile -d /Volumes/SATA2000DM008-2FR102/projects/starcoinorg/rewarding-pool-dapp/contracts/modules -o /Volumes/SATA2000DM008-2FR102/projects/starcoinorg/rewarding-pool-dapp/contracts/target /Volumes/SATA2000DM008-2FR102/projects/starcoinorg/rewarding-pool-dapp/contracts/scripts/create_coco.move
starcoin% dev compile -d /Volumes/SATA2000DM008-2FR102/projects/starcoinorg/rewarding-pool-dapp/contracts/modules -o /Volumes/SATA2000DM008-2FR102/projects/starcoinorg/rewarding-pool-dapp/contracts/target /Volumes/SATA2000DM008-2FR102/projects/starcoinorg/rewarding-pool-dapp/contracts/scripts/create_pool.move
starcoin% dev compile -d /Volumes/SATA2000DM008-2FR102/projects/starcoinorg/rewarding-pool-dapp/contracts/modules -o /Volumes/SATA2000DM008-2FR102/projects/starcoinorg/rewarding-pool-dapp/contracts/target /Volumes/SATA2000DM008-2FR102/projects/starcoinorg/rewarding-pool-dapp/contracts/scripts/exit_pool.move
starcoin% dev compile -d /Volumes/SATA2000DM008-2FR102/projects/starcoinorg/rewarding-pool-dapp/contracts/modules -o /Volumes/SATA2000DM008-2FR102/projects/starcoinorg/rewarding-pool-dapp/contracts/target /Volumes/SATA2000DM008-2FR102/projects/starcoinorg/rewarding-pool-dapp/contracts/scripts/stake.move
starcoin% dev compile -d /Volumes/SATA2000DM008-2FR102/projects/starcoinorg/rewarding-pool-dapp/contracts/modules -o /Volumes/SATA2000DM008-2FR102/projects/starcoinorg/rewarding-pool-dapp/contracts/target /Volumes/SATA2000DM008-2FR102/projects/starcoinorg/rewarding-pool-dapp/contracts/scripts/withdraw_rewards.move
```

合约编译后的字节码会生成到 contracts/targget 路径下，这些字节码会被 dapp 用到，请确保路径正确。

## 在链上初始化奖励池

合约编译好之后，需要在链上初始化出挖矿所用到的奖励池。使用以下命令提交交易。


```bash
dev execute -b /Volumes/SATA2000DM008-2FR102/projects/starcoinorg/rewarding-pool-dapp/contracts/target/create_coco_and_pool.mv --type_tag 0x1::STC::STC --arg 10000000u128 --arg 3600
```

## 配置 DAPP

修改  `/Volumes/SATA2000DM008-2FR102/projects/starcoinorg/rewarding-pool-dapp/src/config/index.ts` 文件中 `Deployer` 修改成你的节点默认账号的地址。

``` javascript
// replace this with your deployer address.
export const Deployer = "2291c747b396303a6475db8468a910d0";
```

## 启动 DAPP

``` bash
> yarn serve
```

DApp 启动成功后，打开 `http://localhost:8080/#/` 页面。
然后就可以探索它的功能了。


