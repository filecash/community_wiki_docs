# Filecash FAQ\[Filecash常见问题回答\]

## 一、基础篇

1. Filecash主网什么时候上线的？


   Filecash主网在高度58270(北京时间 2020年11月18日15:00)正式启动


2. Filecash 主网当前支持哪些大小扇区？


   Filecash 主网支持4GB和16GB扇区
   
   
   硬件配置： 12核以上CPU 128G 以上内存， 2T 以上NVME ，2080TI，3080,3090显卡


   P1约5小时，P2约6分钟，C2约28分钟。

3. Filecash 当前运行的版本是哪个？


   https://github.com/filecash/filecash_announcement


4. Filecash 区块浏览器有哪些？

|  区块链浏览器   | 地址  |
|  ----  | ----  |
| ![tokenview](img/tokenview.png)  | https://fic.tokenview.com/ |
| ![filscout](img/filscout.png)   | https://fic.filscout.io/ |
| ![ficvue](img/filscout.png)   | https://www.ficvue.com/ |

5. Filecash 当前上了哪些交易所？

|  交易所   | 地址  |
|  ----  | ----  |
| 芝麻开门（gate）|  https://www.gateio.tv | 
| MXC| http://www.coinmxc.com/ | 
| QB| https://www.qb.com | 
| MIXPAY| https://mixpay.com/ |
| Citex|  | 
| BiKi|  | 
| HOTBIT|  | 
| HBTC|  | 
| LBank|  | 
| bitmart|  | 
| HashKeyHub|  |     
  
  
6. Filecash如何提交提案

   https://github.com/filecash/FICIPs

7. Filecash路线图

   https://app.instagantt.com/shared/s/ZOdqrgBwE7zfgrBGkxA7/latest


8. 如何参与Filecash社区

   Slack: https://join.slack.com/t/filecashworkspace/shared_invite/zt-kca9ippi-pUniHv0K_zzneG9PxMz2Ow
   Telegram: https://t.me/FilecashGlobal
   
   
 9. Filecash 挖矿CPU，内存，GPU显存，硬盘资源消耗情况如何？
 
    16GB扇区：
    
    
    1个P1需要32G内存，1核心CPU，112G NVME/SSD空间，如果P1P2在一台机器上完成需要165G
    
    
    1个P2需要79G内存，可以不占用CPU，6.3G显存，165G NVME/SSD
    
    
    1个C2需要69G内存，部分时段全部CPU核心，8.4G显存，不占用硬盘空间。
    
    
    注意：如果P1 ，P2 不在一台机器上，P2机器需要GETP1数据到P2 机器
    
    
          C2不会到P2 机器上get数据。
         
    bench参考：3800X+128+2080ti 11G+SATA 8T
      
      
      |  资源名称   | P1 | P2 | C2 |
      |  ----  | ----  |----  | ----  |  
      |内存消耗	| 33G	| 79G	| 69G |
      |显存消耗	| 0G	| 6.3G |	8.4G |
      | 用时	|4h40m | 20m58s| 	22m23s|
   
   
## 二、部署篇

1. 社区编译方法: https://github.com/filecash/lotus_builder

2. 志愿者提供一键部署脚本 https://gitee.com/imbacloud/filecash-deploy


## 三、运行维护篇 

### 1.节点检查及维护

1.1 如何查看节点运行情况？

`lotus sync wait` 查看state 直到complete状态 

`lotus sync status` 查看同步状态一次

1.2如何查看和本节点互联的Peers
~~~bash
lotus net peers
~~~

1.3如何主动连接其他peer

`lotus net connect <peer>`  

peer格式参考：


/ip4/58.144.223.197/tcp/46121/12D3KooWSud8NV8LDTi2HD32CLVrN8um57YyCSDxYFyZ58XBwu8X



1.4如何查看本地内存池是否拥塞？
~~~bash
lotus mpool pending --local 
~~~

1.5如何查看自己的节点是否分叉？

执行`lotus chain  list` 检查最后几行的消息是否和浏览器，及其他大多数节点一致。


1.6如何导入节点快照


  lotus daemon --import-snapshot=chain-xxx.car 或 lotus daemon --import-chain=指定文件
  
  
  https://docs.file.cash/snapshots/
  
  
### 2.钱包维护


2.1 如何创建钱包

bls 格式（推荐使用）：`lotus wallet new bls`  
secp256k1格式（默认）： `lotus wallet new`

2.2.如何备份钱包？
~~~bash
lotus wallet export <钱包地址> > back.key 
~~~
千万保存好自己的key，不要泄露

2.3 如何导入钱包?
~~~bash
lotus wallet import <key文件>
~~~

2.4 如何查看自己的所有钱包地址？
~~~bash
lotus wallet list
~~~

2.5 如何设置钱包默认地址？
~~~bash
lotus wallet set-default <钱包地址>
~~~

2.6 如何转账  

从默认地址发送：`lotus send  <目的钱包地址>  <转账金额>`

2.7 如何从多签地址转账？  
~~~bash
lotus msig propose --from=你的钱包地址  <多签地址> <目的钱包地址>  <金额>
~~~

2.8 如何删除一个钱包地址？
~~~bash
lotus wallet delete <钱包地址> #实际这钱包是一直存在，只是从你的lotus 移除而已
~~~
     
### 3. lotus miner 相关


3.1 如何查看lotus miner 运行状态？
~~~bash
    lotus-miner info
~~~    
    
3.2 如何查看所有扇区（sectors）


    lotus-miner sectors list
    
    
3.3 如何查看某个扇区的log


    lotus-miner sectors status --log <扇区编号>
    
    
3.4 如何查看miner的worker


    lotus-miner sealing workers
    
    
3.5 如何开始抵押扇区


    lotus-miner sectors pledge
    
    
3.6 如何查看当前所有工作


    lotus-miner sealing jobs
    
    
3.7 如何删除一个无效扇区


    lotus-miner sectors remove --really-do-it <扇区编号> P1 阶段失败的扇区可以删除，【！谨慎操作】
    
    
3.8 如何查看window post 失败的扇区


    lotus-miner proving faults
    
    
3.9 如何销毁window post 失败扇区


    lotus-shed sectors terminate --really-do-it <fault 扇区1> <fault 扇区2> <fault 扇区3>   备注：一次可以销毁多个 【！谨慎操作】
    
    
3.10 如何查看miner和worker的存储


    lotus-miner storage list
    
    
3.11 如何查看某个扇区在哪些存储位置


    lotus-miner storage find <扇区编号>
    
3.12 为什么我会掉算力？

   Filecash Filecash网络每天会审核矿工的存储证明，矿工的每个扇区每天需要提交一次window post
   
   
   每个矿工的window post 时间是固定的，在指定时间内（30分钟）必须及时提交到链上，如果失败，就会掉算力。
   
   
   每个partetion的每个deadline里面的扇区一起提交一次。完成后又开始下一个窗口，一直轮巡。
   
   
   
### 4. Filecash运维中哪些坑不能踩

4.1 我可以随便重启服务器、lotus和miner吗？

   
   重启服务器、lotus以及miner必须经过周密计划，如确实需要，请注意以下事项：
   
   
   * lotus-miner window post 期间禁止重启服务器、lotus-miner及lotus daemon
   
   
   * lotus-miner及worker 还有未完成的工作最好不要重启服务器、lotus及lotus daemon
   
   
   * lotus-worker也尽量不要随便重启，集群中miner及各worker相互关联影响。
   
   
   4.2 我可以随便删除一些文件吗？
   
   
   * 禁止删除LOTUS_PATH（默认~/.lotus）中的任何文件，如 token config.toml keystore目录
   
   
   * 禁止删除LOTUS_MINER_PATH(默认~/.lotusminer) 中的任何文件
   
   
   * 禁止删除lotus-miner中任何落盘目录（如删除你可能失去所有算力），具体查看storage.json
   
   
   * miner和worker 中unsealed,sealed ,cache 中的相关扇区文件只有当扇区已经为proving 
   
   
      状态，filnalizedsectors 异常造成垃圾或者扇区已经不存在是方可删除。
   
   
   * 对已经上链的扇区禁止使用lotus-miner sectors remove 删除。
   
   
   * 请不要随意删除修改proof文件，因为这样可能引起window post 失败
   
   
      扇区验证失败，不爆块等问题。
   
   
   


    
    
