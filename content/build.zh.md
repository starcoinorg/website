
---
title: 从源码构建
weight: 5
---

从源码构建 Starcoin。

<!--more-->

1. Clone 源代码

     ```shell
    git clone https://github.com/starcoinorg/starcoin.git
    cd starcoin
    ```
2. Setup build environment

    ```shell
    ./scripts/dev_setup.sh
    ```

    如果操作系统是CentOS 6.x ,则需要使用如下命令，单独安装相关开发工具

    ```shell
    yum install centos-release-scl
    yum install devtoolset-7
    . /opt/rh/devtoolset-7/enable
    ```shell

3. Run debug build

    ```shell
   cargo build
    ```
4. Run release build

    ```shell
   cargo build --release
    ```
   
The debug version starcoin at target/debug/starcoin, and release version at target/release/starcoin.