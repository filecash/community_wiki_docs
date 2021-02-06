# Filecash FAQ\[Filecash常见问题回答\]

## 一、基础篇

1. Filecash主网什么时候上线的？

2. Filecash 主网当前支持哪些大小扇区？推荐的硬件配制有那些？封装效率如何？

3. Filecash 当前运行的版本是哪个？

4. Filecash 区块浏览器有哪些？

|  区块链浏览器   | 地址  |
|  ----  | ----  |
| ![tokenview浏览器](img/tokenview.png)  | https://fic.tokenview.com/ |
| ![filscout浏览器](img/filscout.png)   | https://fic.filscout.io/ |

5. Filecash 当前上了哪些交易所？

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

`lotus net connect <peer>`  (peer格式参考：/ip4/58.144.223.197/tcp/46121/12D3KooWSud8NV8LDTi2HD32CLVrN8um57YyCSDxYFyZ58XBwu8X)

1.4如何查看本地内存池是否拥塞？
~~~bash
lotus mpool pending --local 
~~~

1.5如何查看自己的节点是否分叉？

执行`lotus chain  list` 检查最后几行的消息是否和浏览器，及其他大多数节点一致。


1.6如何导入节点快照


  lotus daemon --import-snapshot=chain-xxx.car 或 lotus daemon --import-chain=指定文件
  
  
2.钱包维护


2.1 如何创建钱包

bls 格式（推荐使用）：`lotus wallet new bsl`  
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
     
3. lotus miner 相关


3.1 如何查看lotus miner 运行状态？


    lotus-miner info
    
    
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
    
    
