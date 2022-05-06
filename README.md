# to do list:
1. FilecashFAQ (OK)
2. Gas计算 (OK)
3. 惩罚逻辑 (  )
4. 如何搭建集群挖矿
5. 如何设置lotus和miner在不同机器
6. filecash v1.2.2 新功能使用方法


# Filecash FAQ \[Filecash常见问题回答\]

## 一、基础篇

### 1. Filecash主网什么时候上线的？

   Filecash主网在高度58270(北京时间 2020年11月18日15:00)正式启动

### 2. Filecash 主网当前支持哪些大小扇区？

   Filecash主网支持4GB和16GB扇区

### 3. Filecash 当前运行的版本是哪个？

   https://github.com/filecash/filecash_announcement

### 4. Filecash 区块浏览器有哪些？

|  区块链浏览器   | 地址  |
|  ----  | ----  |
| ![tokenview](img/tokenview.png)  | https://fic.tokenview.com/ |
| ![filscout](img/filscout.png)   | https://fic.filscout.io/ |

### 5. Filecash 当前上了哪些交易所？

|  交易所   | 官网  | 交易对传送门  |
|  ----  | ----  | ---- |
| 芝麻开门（gate）|  https://www.gateio.tv |  [FIC/USDT](https://www.gateio.tv/trade/FIC_USDT)|
| MXC| http://www.coinmxc.com/ | [FIC/USDT](https://www.mxc.me/trade/easy#FIC_USDT) |
| QB| https://www.qb.com | |
| MIXPAY| https://mixpay.com/ | |
| Citex|https://trade.citex.me/  | [FIC/USDT](https://trade.citex.me/trade/FIC_USDT) |
| BiKi| http://bikil.com/  |   |
| HOTBIT|https://www.hotbit.pro/  |  [FIC/USDT](https://www.hotbit.pro/exchange?symbol=FILECASH_USDT)|
| HBTC| https://www.bit-e.com/  | |
| HashKeyHub| https://hub.hashkey.com/ | |
| huobi| 
  
### 6. Filecash如何提交提案

   https://github.com/filecash/FICIPs

### 7. Filecash路线图

   https://app.instagantt.com/shared/s/ZOdqrgBwE7zfgrBGkxA7/latest

### 8. 如何参与Filecash社区

   Slack: https://join.slack.com/t/filecashworkspace/shared_invite/zt-kca9ippi-pUniHv0K_zzneG9PxMz2Ow
   Telegram: https://t.me/FilecashGlobal
   
   
### 9. Filecash 挖矿CPU，内存，GPU显存，硬盘资源消耗情况如何？  
**16GB扇区：**  
1个P1需要32G内存，1核心CPU，112G NVME/SSD空间，如果P1P2在一台机器上完成需要165G  
1个P2需要79G内存，可以不占用CPU，6.3G显存，165G NVME/SSD  
1个C2需要69G内存，部分时段全部CPU核心，8.4G显存，不占用硬盘空间。  
注意：如果P1 ，P2 不在一台机器上，P2机器需要GET P1数据到P2机器  
C2不会到P2机器上Get数据。  

NVME硬盘空间占用：1个扇区全流程预留：
~~~
unsealed 16G*1  
sealed   16G*1  
cache/tree-d      32G*1=32G  
cache/data-layer  16G*5=80G  
cache/tree-c      16G*4=64G  
cache/tree-r-last 4.6G*4=18.4G
total:16+16+32+80+64+18.4=226.4G
~~~
bench参考：3800X+128+2080ti 11G+SATA 8T

|  资源名称 | P1 | P2 | C2 |
|  ----  | ----  |----  | ----  |  
| 内存消耗 | 33G | 79G | 69G |
| 显存消耗 | 0G	| 6.3G |	8.4G |
| 用时 | 4h40m | 20m58s| 22m23s|


### 10. 如何参与Filecash挖矿？


   参与Filecash挖矿，参考：
   
   
   https://github.com/filecash/lotus/wiki/1.%E5%A6%82%E4%BD%95%E5%8F%82%E4%B8%8Efilecash%E6%8C%96%E7%9F%BF
   
   
## 二、部署篇

1. 社区编译方法: https://github.com/filecash/lotus_builder

2. 志愿者提供一键部署脚本 https://gitee.com/imbacloud/filecash-deploy


# 三、运行维护篇 

## 1.节点检查及维护

### 1.1 如何查看节点运行情况？

`lotus sync wait` 查看state 直到complete状态 

`lotus sync status` 查看同步状态一次

### 1.2 如何查看和本节点互联的Peers
~~~bash
lotus net peers
~~~

### 1.3 如何主动连接其他peer

`lotus net connect <peer>`  

peer格式参考：
/ip4/58.144.223.197/tcp/46121/12D3KooWSud8NV8LDTi2HD32CLVrN8um57YyCSDxYFyZ58XBwu8X

### 1.4 如何查看本地内存池是否拥塞？
~~~bash
lotus mpool pending --local 
~~~

### 1.5 如何查看自己的节点是否分叉？

执行`lotus chain  list` 检查最后几行的消息是否和浏览器，及其他大多数节点一致。


### 1.6 如何导入节点快照快速同步Lotus


  #### 1.6.1备份钱包秘钥

   
      lotus wallet export fxxxx
      
      
  #### 1.6.2关闭lotus进程


      lotus daemon stop (或直接Kill -9 lotus)
      
      
  #### 1.6.3备份lotus目录以备不时之需


      mv ~/.lotus ~/.lotus.bak (这是默认目录，根据情况修改）
      
    
  #### 1.6.4下载最新快照（每天凌晨两点更新）


      wget https://snapshot.file.cash/fic-snapshot-latest.car 
 
      备用地址： 
              
      wget http://118.123.241.59/fic-snapshot-latest.car 
      
      
   #### 1.6.5导入快照


      lotus daemon --import-snapshot fic-snapshot-latest.car
      
      
   #### 1.6.6完成1.6.5后关闭lotus并重启lotus

         pkill -9 lotus 
                  
         lotus daemon

    
### 1.7 节点分叉后如何恢复  

   回到确认未分叉的最近指定高度：
   ~~~bash
   lotus chain sethead --epoch <head>
   ~~~
   再重启lotus ，查看同步情况。
   
   
### 1.8 Filecash 如何增加扇区存储路径？

   
   设置数据存储路径，该路径用来存储最终密封好的数据
   
     
   lotus-miner storage attach --store --init /storage/f01234
   
   
   ！！！带--init参数执行命令后该路径下的数据会被自动清空！！！
   
   
   miner当做worker的时候，增加seal目录
   
   
  lotus-miner storage attach --seal --init /filecash/seal
  
  
  以上两个命令都是在启动了 miner 之后才可以执行，是一种动态添加存储路径的方式，非常灵活。 
  
  
  当然还可以通过--weight设置权重，默认权重是10，数值越大越优先。
  
  
  执行该命令后，可通过以下命令查看存储列表:


   lotus-miner storage list
   

### 1.9 如何修改Filecash存储路径？


   默认的存储目录 ~/.lotusminer 可以移动到其他地方。 
   
   
   先停掉 daemon 和 miner。 
   
   
   移动后新路径为 /storage/f01234，需要手动更改/storage/f01234目录下
   
   
   storage.json 中的 StoragePaths 为新路径：
   
   
      {
       
        "StoragePaths": [ 
        
          { 
          
            "Path": "/storage/f01234" 
            
          } 
          
        ] 
        
      } 
      
      
    注意区分lotusminer目录和密封好扇区存储路径的关系区别。     
    
    lotusminer目录由LOTSU_MINER_PATH环境变量指定。     
    
    而密封好扇区存储路径由lotusminer下storage.json中设置。
    
    
## 2.钱包维护

### 2.1 如何创建钱包

bls 格式（推荐使用）：`lotus wallet new bls`  
secp256k1格式（默认）： `lotus wallet new`

### 2.2 如何备份钱包？
~~~bash
lotus wallet export <钱包地址> > back.key 
~~~
千万保存好自己的key，不要泄露

### 2.3 如何导入钱包?
~~~bash
lotus wallet import <key文件>
~~~

### 2.4 如何查看自己的所有钱包地址？
~~~bash
lotus wallet list
~~~

### 2.5 如何设置钱包默认地址？
~~~bash
lotus wallet set-default <钱包地址>
~~~

### 2.6 如何转账  

从默认地址发送：`lotus send  <目的钱包地址>  <转账金额>`

### 2.7 如何从多签地址转账？  
~~~bash
lotus msig propose --from=你的钱包地址  <多签地址> <目的钱包地址>  <金额>
~~~

### 2.8 如何删除一个钱包地址？
~~~bash
lotus wallet delete <钱包地址> #实际这钱包是一直存在，只是从你的lotus 移除而已
~~~
     
## 3. lotus miner 相关


### 3.1 如何查看lotus miner 运行状态？
~~~bash
    lotus-miner info
~~~    
    
### 3.2 如何查看所有扇区（sectors）


    lotus-miner sectors list
    
    
### 3.3 如何查看某个扇区的log


    lotus-miner sectors status --log <扇区编号>
    
    
### 3.4 如何查看miner的worker


    lotus-miner sealing workers
    
    
### 3.5 如何开始抵押扇区


    lotus-miner sectors pledge
    
    
### 3.6 如何查看当前所有工作


    lotus-miner sealing jobs
    
    
### 3.7 如何删除一个无效扇区


    # 如何正确删除扇区
在你删除扇区之前请确保已经采取了必要的抢救措施，例如遇到存储故障，网络故障，调度故障等等，都要经过一系列的调试，故障诊断 ,最后再考虑删除扇区，谨慎删除扇区，别忘了，该命令有个选项--really-do-it
 
### 3.7.1:　如何删除PreCommitFailed,SealPreCommit1Failed和CommitFailed状态的扇区？


在你删除扇区之前请确保已经采取了必要的抢救措施，例如遇到存储故障，网络故障，调度故障等等，都要经过一系列的调试，故障诊断 ,最后再考虑删除扇区，谨慎删除扇区，别忘了，该命令有个选项--really-do-it
这几种状态因为还没有质押，可通过下面的命令直接删除。

```sh
 $ lotus-miner sectors remove --really-do-it <sectorId>
```
### 3.7.2：如果删除状态为PreCommit1，PreCommit2，并且一直卡顿在这些状态的扇区？
#### 1:首先应尝试如下命令删除
```sh
lotus-miner sealing abort <JobId>
```
```sh
lotus-miner sectors remove <SectorId>
```
如果以上两个命令无法删除，可以执行下面的步骤
#### 2.找一个空闲的Worker（如果所有worker工作满状态，则可以通过执行
```sh
lotus-worker tasks disable [command options] [UNS|C2|PC2|PC1|AP]
```
禁用所有任务，等worker执行完当前任务，就没有任何封装任务了，本操作执行完再通过
注意：执行lotus-worker tasks 命令是，如果报错，需要指定MINER_API_INFO 和 --worker-repo
```sh
lotus-worker tasks enable [command options] [UNS|C2|PC2|PC1|AP]
```
恢复任务），在该worker的LOTUS_WORKER_PATH的unsealed和seald目录下，分别创建对应扇区ID的空扇区文件，假设扇区的ID为100
，那分别在unsealed和seald目录下执行：
```sh
touch s-t0xxxxxx-100# 其中t0xxxxxx是矿工ID
```
#### 3.重启该Worker，注意观察Worker的日志中，该扇区会不会开始封装，如果没有开始封装，可以再重启一下Miner。
 
####4. 待该扇区开始封装，在lotus-miner sealing jobs列表中能看到以后，就可以执行命令先终止掉任务。


```sh 
lotus-miner sealing abort <JobId>
```
 

#### 5：执行


```sh 
lotus-miner sectors remove <SectorId>
```
 

### 3.7.3：如果删除状态为Committing，并且一直卡顿在这些状态的扇区？
#### 1: 同样执行3.7.2 的1，2，3
 
#### 2:创建并重启Worker以后，该扇区并不会出现在lotus-miner sealing jobs列表中，而是直接变为CommitFailed，这个时候，执行以下命令删除扇区即可
```shell 
lotus-miner sectors remove <SectorId>
```
### 3.7.4：如何删除因为存储故障，无法恢复的扇区
删除扇区一定要先链上删除再本地删除，这样能最大程度的减少损失，请记住执行顺序，这个相当重要
 
#### 1：执行

删除扇区一定要先链上删除再本地删除，这样能最大程度的减少损失，请记住执行顺序，这个相当重要
 
```shell 
lotus-miner sectors terminate --really-do-it <sectorNum>
```
这一步的主要作用为清除链上数据，最大限度减少处罚
##### 2: 等到扇区状态变为terminalfinality
 
##### 3：执行

```shell 
lotus-miner sectors remove --really-do-it <sectorNum>
```
这一步的主要作用为清除存储
 

### 3.7.5：删除扇区中最常范的错误


很多人会直接执行：lotus-miner sectors remove --really-do-it <sectorNum>,这是错误的。然后找不到扇区编号，也无法terminate. 这样的情况，可以通过
```shell 
lotus-miner sectors list --fast --states Remomved
```
查看到扇区编号，这个时侯再执行
```shell
lotus-miner sectors terminate --really-do-it <sectorNum> 
```
 
[注：3.7内容来自https://github.com/filecoin-project/community-china/blob/master/documents/tutorial/How_to_delete_sector_correctly/How_to_delete_sector_correctly.md]
 
    
### 3.8 如何查看window post 失败的扇区


    lotus-miner proving faults
    
    
### 3.9 如何销毁window post 失败扇区

    lotus-shed sectors terminate --really-do-it <fault 扇区1> <fault 扇区2> <fault 扇区3>   备注：一次可以销毁多个 【！谨慎操作】
    
### 3.10 如何查看miner和worker的存储

    lotus-miner storage list
    
### 3.11 如何查看某个扇区在哪些存储位置

    lotus-miner storage find <扇区编号>
    
### 3.12 为什么我会掉算力？

   Filecash Filecash网络每天会审核矿工的存储证明，矿工的每个扇区每天需要提交一次window post
   
   
   每个矿工的window post 时间是固定的，在指定时间内（30分钟）必须及时提交到链上，如果失败，就会掉算力。
   
   
   每个partetion的每个deadline里面的扇区一起提交一次。完成后又开始下一个窗口，一直轮巡。
   
   
### 3.13 给矿工添加独立window post账号
 
 
 由于IPFS网络TPS问题，在网络消息拥堵或钱包账户余额不足，容易造成window post失败。
 
 
 为此，我们单独给miner增加一个window post 钱包地址，步骤如下：
 
  新建一个bls钱包地址： lotus wallet new bls
  
  
  往这个钱包里存入 10-100个 FIC： lotus send fxxx 100
  
  
  给Miner设置windowpost专用地址：
  
  
  lotus-miner actor control set --really-do-it 刚才生成钱包地址
 
   
  等待刚才命令执行消息上链成功后，检查结果：
  
  
  lotus-miner actor control list
  
  
      name       ID      key           use    balance                      
      owner      f029xx  f3u5qh65v...  other  3046.247008689520468129 FIC  
      worker     f029xx  f3u5qh65v...  other  3046.247008689520468129 FIC  
      control-0  f030xx  f3rqimboi...  post   99.99122158945087101 FIC     
   
   
  设置成功后window post 将从新设置地址扣费，window post 消息以这个地址发出。 
   
## 4. Filecash运维中哪些坑不能踩

### 4.1 我可以随便重启服务器、lotus和miner吗？

   
   重启服务器、lotus以及miner必须经过周密计划，如确实需要，请注意以下事项：
   
   
   * lotus-miner window post 期间禁止重启服务器、lotus-miner及lotus daemon
   
   
   * lotus-miner及worker 还有未完成的工作最好不要重启服务器、lotus及lotus daemon
   
   
   * lotus-worker也尽量不要随便重启，集群中miner及各worker相互关联影响。
   
   
### 4.2 我可以随便删除一些文件吗？
   
   
   * 禁止删除LOTUS_PATH（默认~/.lotus）中的任何文件，如 token config.toml keystore目录
   
   
   * 禁止删除LOTUS_MINER_PATH(默认~/.lotusminer) 中的任何文件
   
   
   * 禁止删除lotus-miner中任何落盘目录（如删除你可能失去所有算力），具体查看storage.json
   
   
   * miner和worker 中unsealed,sealed ,cache 中的相关扇区文件只有当扇区已经为proving 
   
   
      状态，filnalizedsectors 异常造成垃圾或者扇区已经不存在是方可删除。
   
   
   * 对已经上链的扇区禁止使用lotus-miner sectors remove 删除。
   
   
   * 请不要随意删除修改proof文件，因为这样可能引起window post 失败
   
   
      扇区验证失败，不爆块等问题。
 ### 5. Lotus常用环境变量使用说明
5.1. 通用环境变量

FIL_PROOFS_PARAMETER_CACHE：proof 证明参数路径，

默认在/var/tmp/filecoin-proof-parameters下，建议自己指定。

FFI_BUILD_FROM_SOURCE：从源码编译底层库。

export FFI_BUILD_FROM_SOURCE=1

IPFS_GATEWAY：配置证明参数下载的代理地址。

export IPFS_GATEWAY=export IPFS_GATEWAY="http://proofs.file.cash:8080/ipfs/"

TMPDIR：临时文件夹路径，用于存放显卡锁定文件。

export TMPDIR=/home/user/nvme_disk/tmp

RUST_LOG：配置Rust日志级别，可设置All,Info,Debug等。

export RUST_LOG=Debug 

GOPROXY：配置Golang代理。
export GOPROXY=https://goproxy.cn

5.2. Lotus Deamon环境变量

LOTUS_PATH：lotus daemon 路径，例如：

export LOTUS_PATH=/filecash/lotus

5.3. Lotus Miner环境变量

LOTUS_MINER_PATH：lotus miner 路径，例如：

export LOTUS_MINER_PATH=/filecash/lotusminer

FULLNODE_API_INFO：lotus daemon API 环境变量；

获取API命令（lotus-miner auth api-info --perm admin）

export FULLNODE_API_INFO=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBbGxvdyI6WyJyZWFkIiwid3JpdGUiLCJzaWduIiwiYWRtaW4iXX0.JSdq-OviNQW2dZslvyargJsqgLrlYCjoZCIFkb2u96g:/ip4/192.168.1.10/tcp/1234/http

BELLMAN_CUSTOM_GPU：指定GPU型号;如果已支持不需要手工设置。

export BELLMAN_CUSTOM_GPU="GeForce RTX 2080 Ti:4352"

5.4. Lotus Worker环境变量

LOTUS_WORKER_PATH：Lotus worker 路径；

export LOTUS_WORKER_PATH=/filecash/lotusworker

FIL_PROOFS_MAXIMIZE_CACHING：最大化内存参数；

export FIL_PROOFS_MAXIMIZE_CACHING=1

FIL_PROOFS_USE_MULTICORE_SDR：CPU多核心绑定；

export FIL_PROOFS_USE_MULTICORE_SDR=1

FIL_PROOFS_USE_GPU_TREE_BUILDER：使用GPU计算Precommit2 TREE hash

export FIL_PROOFS_USE_GPU_TREE_BUILDER=1

FIL_PROOFS_USE_GPU_COLUMN_BUILDER：使用GUP计算Precommit2 COLUMN hash；

export FIL_PROOFS_USE_GPU_COLUMN_BUILDER=1

BELLMAN_NO_GPU：不使用GPU计算Commit2；

如果要启用 GPU，则不能让这个环境变量（BELLMAN_NO_GPU）出现在系统的环境变量中（env）;

如果它出现在 env 中，则需要使用unset BELLMAN_NO_GPU命令取消，因为设置 export BELLMAN_NO_GPU=0 无效；

export BELLMAN_NO_GPU=1
