# 如何用Tezos客户端委托Tezos(XTZ)及如何运行你自己的节点（betanet）
上一篇委托指南([How to Deligate Zopf Tezzies (Tezos’ XTZ) with Pâtissière Awa from Cryptium Bäckerei](https://medium.com/cryptium/how-to-deligate-zopf-tezzies-tezos-xtz-with-pâtissière-awa-from-cryptium-bäckerei-30db6fded810))中，我编写了一个从源代码安装Tezos的手把手指南。然而由于依赖于 brew 存在许多的问题。今天的指南专门针对所有使用终端或控制台的用户。 包括以下部分：
1.	Installing Tezos from source
2.	Running a Tezos node
3.	Generating a Wallet with Tezos-Client
4.	Delegating XTZ to your favourite baker
5.	从源代码安装Tezos
6.	运行Tezos节点
7.	用Tezos客户端生成一个钱包
8.	把XTZ委托给你喜爱的烘焙师（baker）

由于最近出现了全节点([Reddit Thread](https://www.reddit.com/r/tezos/comments/93qk6p/so_was_there_a_ddos_attack_or_nah/?st=JKBGICK6&sh=a4ebea9e)) 的问题，本文将包括在后台运行您自己的节点，而不是连接到公共的全节点，例如我们的节点。
撸起袖子开干！

## 从源代码安装Tezos（不用 brew）
注意：我用的MacOS，如果使用其他操作系统，确保使用对应的命令。 
1.	安装XCode：
```
$ xcode-select --install
```
2.	安装OPAM的正确版本：
```
$ sh <(curl -sL https://raw.githubusercontent.com/ocaml/opam/master/shell/install.sh)
```
3.	初始化OPAM(需要等一会儿)
```
$ opam init --compiler=4.06.1
$ eval $(opam env)
``` 
4.	克隆Tezos并切换到betanet分支上：
```
$ git clone -b betanet https://gitlab.com/tezos/tezos.git
```
5.	进入tezos文件夹：
```
$ cd tezos
```
6.	我没有装过libev。在这步之前你需要安装它（对不起又用到了brew，不过这是最容易的方式)：
```
$ ruby -e “$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)" < /dev/null 2> /dev/null
$ brew install libev
``` 
7.	你应该在tezos项目的betanet分支上。安装所有的OCaml依赖（需要等一会儿）：
```
$ make build-deps
``` 
8.	编译二进制：
```
$ eval $(opam env)
$ make
```
9.	检查一下我们编译好的二进制，特别是检查一下是否有 tezos-client：
```
$ ls
→ README.md                 tezos-accuser-002-PsYLVpVv
_build                      tezos-admin-client
_opam                       tezos-baker-002-PsYLVpVv
active_protocol_versions    tezos-client
docs                        tezos-endorser-002-PsYLVpVv
dune                        tezos-node
dune-project                tezos-protocol-compiler
dune-workspace              tezos-signer
``` 

## 生成节点标识及运行你的Tezos节点
1.	我们一起用identity generate配置一个节点，难度至少是26：
```
$ ./tezos-node identity generate 26
→ Generating a new identity... (level: 26.00) |...oo|
stored the new identity (30charstring30charstring30cha) into '/Users/<username>/.tezos-node/identity.json'.
``` 
2.	我们运行节点：
```
$ nohup ./tezos-node run --rpc-addr 127.0.0.1:8732 --connections 10 &
→ [1] 56365
appending output to nohup.out
```
3.	节点开始同步，并需要等一会儿，但你可以用如下命令检查进度：
```
$ ./tezos-client bootstrapped
→ Current head: BLB9xxSYJf6M (timestamp: 2018-07-16T04:48:12Z, validation: 2018-08-03T12:51:52Z)
[...]
Current head: BL1pdeP7p1Yd (timestamp: 2018-07-25T09:38:57Z, validation: 2018-08-03T13:10:16Z)
Bootstrapped.
 
Synching…
```
4.	节点在后台运行，即使完成同步后。如果你停掉了进程，在开始之前记得返回节点。

## 用Tezos-client生成一个钱包：创建隐含的账号并充值
我们看一下tezos-client可用的命令：
```
$ ./tezos-client man
→ [...]
Commands for managing the wallet of cryptographic keys:
  list signing schemes
    List supported signing schemes.
  gen keys <new> [-f --force] [-s --sig <ed25519|secp256k1|p256>]
    Generate a pair of keys.
    <new>: new secret_key alias
    -f --force: overwrite existing secret_key
    -s --sig <ed25519|secp256k1|p256>: use custom signature algorithm
[...]
```
按照如下文档创建一个账号：
1.	生成隐含的账号：
```
$ ./tezos-client gen keys <implicit account name>
→ Enter passphrase to encrypt your key:
→ Confirm passphrase:
```
2.	检查账号已经生成成功：
```
$ ./tezos-client list known addresses
→ my_implicit_account: tz136characterstring36characterstrin (encrypted sk known)
```
3.	tz1地址充值，如上所示：`tz136characterstring36characterstrin`
4.	检查账户余额
```
$ ./tezos-client get balance for <tz1>
→ 0 ꜩ
```

## 把你的资金委托给喜爱的烘焙师（baker）
1.	添加你喜爱的烘焙师：
```
$ ./tezos-client add address cryptium_labs_baker tz1eEnQhbwf6trb8Q8mPb2RaPkNk2rN7BKi8
```
记住替换cryptium_labs_baker和tz1地址为你喜爱的烘焙师的内容。
2.	创建一个原始账号并授权：
```
$ ./tezos-client originate account <my_originated_account> for <my_implicit_account> transferring <qty> from <my_implicit_account> --delegate cryptium_labs_baker --fee 0.00
```
记住替换如下内容：
*	<my_originated_account> 原始账号的名字
*	<my_implicit_account> 前面创建的隐含账号名字
*	<qty> 打算授权的XTZ数量
*	cryptium_labs_baker 前面步骤选择的烘焙师名字
*	注意：--fee 0.0 会自动设置费用的最小值，默认是0.05XTZ。
例如，下面是我的例子：
```
$ ./tezos-client originate account my_originated_account for my_implicit_account transferring 0.5 from my_implicit_account --delegate cryptium_labs_baker --fee 0.0
→ Node is bootstrapped, ready for injecting operations.+
[...]
Operation hash: 51characteroperationhash51characteroperationhash51c
Waiting for the operation to be included...
[...]
New contract KT136characterstring36characterstrin originated.
[...]
Contract memorized as my_originated_account.
```
3.	检查创建的合约以及KT1地址
```
$ ./tezos-client list known contracts
→ my_originated_account : KT136characterstring36characterstrin
```
4.	搞定！

资源清单：
*	Tezos Alphanet documentation: http://doc.tzalpha.net
*	Compilation of Tezos from Source: http://doc.tzalpha.net/introduction/howto.html#howto
*	Compiling Betanet by Tezos Community: https://github.com/tezoscommunity/FAQ/blob/master/Compile_Betanet.md

Other Delegation Guides
*	How to Delegate Tezzies (Tezos’ XTZ) with Your Ledger Nano S — With Initial Setup & Screenshots
*	How to Delegate Tezos XTZ with TezBox Online Wallet
*	How to Delegate Tezos XTZ with Galleon Wallet