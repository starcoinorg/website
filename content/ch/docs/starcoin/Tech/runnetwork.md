---
title: 运行本地网络/加入测试网络
weight: 3
---

# 运行本地网络

`starcoin` 命令行工具可以用来启动本地网络。运行本地网络可以方便测试用户合约代码。可以使用 dev 命令编译, 发布,执行智能合约. 

## 使用反方法

`starcoin` [FLAGS] [OPTIONS] [SUBCOMMAND]

FLAGS:
- --disable-file-log 禁止文件日志
- --disable-seed 禁止 seed


OPTIONS:
- --seed 指定 seed
- --dev-period 在 dev 网络情况下，指定出块频率，默认是0，即不出块.
- --net 网络名 ,可以是 dev/halley/proxima/main 其中一个，本地测试网络使用dev
- -s 同步模式, 可选 full 或者 fast

SUBCOMMAND:
- console background 运行节点，节点启动完成后，启动交互式命令行工具
- help  输出帮助信息


使用如下命令即可启动 dev 节点，平均 10s 出一个块:

```
cargo run --bin starcoin -- -n dev --dev-peroid 10 
```

节点启动成功后，可以在日志中找到:

```
Self address is: /ip4/127.0.0.1/tcp/59476/p2p/12D3KooWPePRG6BDdjgtEYmPDxNyJfMWpQ1Rwgefuz9eqksLfxJb
```

接下来设置第二个节点:

```
cargo run --bin starcoin -- -n dev --dev-peroid 10 --seed /ip4/127.0.0.1/tcp/59476/p2p/12D3KooWPePRG6BDdjgtEYmPDxNyJfMWpQ1Rwgefuz9eqksLfxJb

```

当然你也可以使用自带交互式命令行的方式启动:

```
cargo run --bin starcoin -- -n dev --dev-peroid 10 console
```

重复上述步骤，你就可以启动一个本地 dev 网络.

# 加入 Halley/Proxima 网络

可以使用如下命令加入 Halley 网络:
```
cargo run --bin starcoin -- -n halley
```

可以使用如下命令加入 Proxima 网络:
```
cargo run --bin starcoin -- -n proxima
```