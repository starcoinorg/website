---
title: First transaction
weight: 7
---

This article guides you on how to execute your first transaction on the starcoin blockchain.
Before that, I recommend you read tech-related articles to get some idea of the basic concepts of starcoin.

## Prerequisite

Let's say you've run up a starcoin dev node locally.

## A few steps to submit a transaction

- Start the CLI console and connect to the starcoin node.
- Create two accounts: Alice,Bob.
- Mint money into Alice's account.
- Submit transfer transaction: Alice send money to Bob.

### Start the cli console

Let's say the directory of your node is `dev_node` (if the directory of yours is different, replace the `dev_node` in subsequent commands with yours).

Execute the following command to enter the starcoin console.

- Connection via local IPC

```bash
> starcoin -d dev_node console
```

- Connect via websocket
Check the websocket port in your node config file, the config file is at `dev_node/dev/config.yml`

The following is my configuration.

``` toml
[RPC]
ws_address = "127.0.0.1:56982"
```

Then execute the following command. (Please replace `56982` with your port)

```shell
starcoin --connect ws://127.0.0.1:56982 console
```


The console may look like:

```shell
2020-05-28T17:42:07.577269+08:00 INFO starcoin::cmd/starcoin/src/main.rs::27 - Starcoin opts: StarcoinOpt { connect: Some(WebSocket("ws://127.0.0.1:56982")), data_dir: None, net: None, seed: None, dev_period: 0, node_key: None, node_key_file: None, sync_mode: FAST, disable_std_log: false, disable_file_log: false, enable_mine: None, miner_thread: None, disable_seed: false }
2020-05-28T17:42:07.577565+08:00 INFO starcoin::cmd/starcoin/src/main.rs::57 - Try to connect node by websocket: "ws://127.0.0.1:56982"
2020-05-28T17:42:07.583089+08:00 INFO starcoin::cmd/starcoin/src/main.rs::76 - Start console, disable stderr output.
Logger is disabled.
starcoin%.
```

### Create an account

After connecting to the node, let's create two accounts first.


The first account, which is the node's default account, we name it **Alice**.

```bash
starcoin% wallet create -p my-pass
+---------------------------------------------------------------------------------------------------------------
| address | 1d8133a0c1a07366de459fb08d28d2a6 |
+---------------------------------------------------------------------------------------------------------------
| is_default | false | false
+---------------------------------------------------------------------------------------------------------------
| public_key | 7add08c841d0f99f1f90ea2632c72aee483fab882e0d8d6d6defed2f1987345d |
+---------------------------------------------------------------------------------------------------------------

starcoin% wallet show 1d8133a0c1a07366de459fb08d28d2a6

| account.address | 1d8133a0c1a07366de459fb08d28d2a6 |

| account.is_default | false

| account.public_key | 7add08c841d0f99f1f90ea2632c72aee483fab882e0d8d6d6defed2f1987345d |

| auth_key_prefix | 7bc6066656bb248755686d2ab78aef14 |

balance | balance | balance | balance

| sequence_number | sequence_number |

```

- `address` is the account address.
- `public_key` is the public key corresponding to the account address.

> Note that creating an account just creates a pair of keys in the starcoin node and does not update the state of the chain. So balance and sequence_number are still empty at this time.

Then create another one, which we name it **Bob**.

``` bash
starcoin% wallet create -p my-pass
+---------------------------------------------------------------------------------------------------------------
| address | bfbed907d7ba364e1445b971f9182949 |
+---------------------------------------------------------------------------------------------------------------
| is_default | false | false
+---------------------------------------------------------------------------------------------------------------
| public_key | d80234b11619e62a62fac048b2b79a9eec1727b476155e1f8fe19c89c7443076 |
+---------------------------------------------------------------------------------------------------------------

starcoin% wallet show bfbed907d7ba364e1445b971f9182949

| account.address | bfbed907d7ba364e1445b971f9182949 |

| account.is_default | false

| account.public_key | d80234b11619e62a62fac048b2b79a9eec1727b476155e1f8fe19c89c7443076 |

| auth_key_prefix | 7c87272c7fc2f5586a0770d1d718f14f |

| sequence_number | sequence_number |
+- ------
```

The console also provides a command to view all current accounts.

```
starcoin% wallet list
+----------------------------------+------------+------------------------------------------------------------------+
| address                          | is_default | public_key                                                       |
+----------------------------------+------------+------------------------------------------------------------------+
| bfbed907d7ba364e1445b971f9182949 | false      | d80234b11619e62a62fac048b2b79a9eec1727b476155e1f8fe19c89c7443076 |
+----------------------------------+------------+------------------------------------------------------------------+
| 1d8133a0c1a07366de459fb08d28d2a6 | true       | 7add08c841d0f99f1f90ea2632c72aee483fab882e0d8d6d6defed2f1987345d |
+----------------------------------+------------+------------------------------------------------------------------+
```

### Use Faucet to top up your account

 In dev environment, faucet can be used to mint accounts. faucet only exists in dev and test net to make it easier for developers developing and testing dapps.
 
 Let's do it!.

 ``` bash
starcoin% dev get_coin -v 5000000
+-----------------+------------------------------------------------------------------+
| gas_unit_price  | 1                                                                |
+-----------------+------------------------------------------------------------------+
| id              | 65610116a010de657c9faeca94c2b91b9e4fd36f62bc8d7ccbdbb6fdd2e64769 |
+-----------------+------------------------------------------------------------------+
| max_gas_amount  | 2000000                                                          |
+-----------------+------------------------------------------------------------------+
| sender          | 0000000000000000000000000a550c18                                 |
+-----------------+------------------------------------------------------------------+
| sequence_number | 1                                                                |
+-----------------+------------------------------------------------------------------+
```

`dev get_coin` will mint some coins the default account, and if the account does not exist on the chain, it will creates the account first and then transfers a specified (with `-v`) number of coins to the account.
The output of the command is the transaction data  issued by the FAUCET account (address `0000000000000000000000000A550C18`).

Wait a few seconds and then check your account information again.

```
starcoin% wallet show 1d8133a0c1a07366de459fb08d28d2a6
+--------------------+------------------------------------------------------------------+
| account.address    | 1d8133a0c1a07366de459fb08d28d2a6                                 |
+--------------------+------------------------------------------------------------------+
| account.is_default | false                                                            |
+--------------------+------------------------------------------------------------------+
| account.public_key | 7add08c841d0f99f1f90ea2632c72aee483fab882e0d8d6d6defed2f1987345d |
+--------------------+------------------------------------------------------------------+
| auth_key_prefix    | 7bc6066656bb248755686d2ab78aef14                                 |
+--------------------+------------------------------------------------------------------+
| balances.STC       | 5000000                                                          |
+--------------------+------------------------------------------------------------------+
| sequence_number    | 0                                                                |
+--------------------+------------------------------------------------------------------+
```

Now, `balances` and `sequence_number` is filled.



### Submit Transaction

First you need to unlock Alice's account and authorize node to sign the transaction using Alice's private key.

```` bash
wallet unlock -p my-pass 1d8133a0c1a07366de459fb08d28d2a6
````

where `-p my-pass` is the password that was needed when creating the account.

Once the account is unlocked, execute the following command.

```
starcoin% txn transfer -f 1d8133a0c1a07366de459fb08d28d2a6 -t bfbed907d7ba364e1445b971f9182949 -k 7add08c841d0f99f1f90ea2632c72aee483fab882e0d8d6d6defed2f1987345d -v 10000
+-----------------+------------------------------------------------------------------+
| gas_unit_price  | 1                                                                |
+-----------------+------------------------------------------------------------------+
| id              | 44433463c38aacd31731fba1a38d3a38f9a14c0281a264634e470c8f25bd557d |
+-----------------+------------------------------------------------------------------+
| max_gas_amount  | 1000000                                                          |
+-----------------+------------------------------------------------------------------+
| sender          | 1d8133a0c1a07366de459fb08d28d2a6                                 |
+-----------------+------------------------------------------------------------------+
| sequence_number | 0                                                                |
+-----------------+------------------------------------------------------------------+
```

- `-F 1d8133a0c1a07366de459fb08d28d2a6`: is Alice's account address.
- `-T bfbed907d7ba364e1445b971f9182949`: is Bob's account address.
- `-k 7add08c841d0f99f1f90ea2632c72aee483fab882e0d8d6d6defed2f1987345d`: is the public key of Bob.

> If, Bob's account does not yet exist on the chain, then his public key should be provided, the transfer transaction will automatically create Bob's account on the chain.


At this point, the transaction has been submitted to the chain. 
You still need to wait a few seconds (in the dev environment, maybe longer in test env) to let the transaction included the chain. 
Then check Bob's account information again:.


``` bash
starcoin% wallet show bfbed907d7ba364e1445b971f9182949
+--------------------+------------------------------------------------------------------+
| account.address    | bfbed907d7ba364e1445b971f9182949                                 |
+--------------------+------------------------------------------------------------------+
| account.is_default | false                                                            |
+--------------------+------------------------------------------------------------------+
| account.public_key | d80234b11619e62a62fac048b2b79a9eec1727b476155e1f8fe19c89c7443076 |
+--------------------+------------------------------------------------------------------+
| auth_key_prefix    | 7c87272c7fc2f5586a0770d1d718f14f                                 |
+--------------------+------------------------------------------------------------------+
| balances.STC       | 10000                                                            |
+--------------------+------------------------------------------------------------------+
| sequence_number    | 0                                                                |
+--------------------+------------------------------------------------------------------+
```

Bob has the money now!

