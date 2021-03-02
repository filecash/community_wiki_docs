# Gas费计算方法

  Gas费 = 小费(MinerFee) + 基本燃烧费(BaseToBurn) + 超额燃烧费(OverEstimateToBurn) 

## 1.小费(MinerFee):

  当 BaseFee + GasPremium > GasFeeCap, MinerFee = GasLimit * (GasFeeCap - BaseFee) 
  当 BaseFee + GasPremium ≤ GasFeeCap, MinerFee = GasLimit * GasPremium 

BaseFee GasPremium 和 GasFeeCap 分别是三种费率，BaseFee针对基本燃烧费，GasPremium针对小费费率，GasFeeCap针对总的支付费率。
官方对小费设定了一个参数，主要是为了让GasFeeCap与两者的关系 BaseFee + GasPremium ，尽可能地支付更少的小费。
目前按市场上的消息小费都是 BaseFee + GasPremium ≤ GasFeeCap, 即是 MinerFee = GasLimit * GasPremium 
```
  小费(MinerFee) = GasLimit * GasPremium 
```

## 2.基本燃烧费(BaseToBurn):

  BaseToBurn = BaseFee * GasUsed
```
  基本燃烧费(BaseToBurn) = BaseFee * GasUsed 
```

## 3.超额燃烧费(OverEstimateToBurn):

对于超额燃烧费主要是为了避免使用过高的Gas费，为gas设定了一个指标Over，其中 Over = GasLimit-11/10*GasUsed

计算 GasLimit/GasUsed 值, 
当 Over < 0 时, GasLimit/GasUsed < 1.1, 计算公式为: OverEstimateToBurn = (GasLimit-GasUsed)*BaseFee
当 Over > GasUsed 时, GasLimit/GasUsed > 2.1, Over=GasUsed, 计算公式为: OverEstimateToBurn = ((GasLimit-GasUsed)*over)/GasUsed*BaseFee = (GasLimit-GasUsed)*BaseFee
当 0 ≤ Over ≦ GasUsed 时, 1.1 ≤ GasLimit/GasUsed ≤ 2.1, 计算公式为: OverEstimateToBurn = ((GasLimit-GasUsed)*over)/GasUsed*BaseFee

由上可知 1.1 ≤ GasLimit/GasUsed ≤ 2.1 较为合理，即是 1.1-2.1 倍较为合理，此时 OverEstimateToBurn = ((GasLimit-GasUsed)*over)/GasUsed*BaseFee 

```
  Over = GasLimit-11/10*GasUsed 
  当 GasLimit/GasUsed < 1.1 时, 超额燃烧费(OverEstimateToBurn) = (GasLimit-GasUsed)*BaseFee
  当 1.1 ≤ GasLimit/GasUsed ≤ 2.1 时, 超额燃烧费(OverEstimateToBurn) = (GasLimit - GasUsed)*Over/GasUsed*BaseFee
  当 GasLimit/GasUsed > 2.1 时, 超额燃烧费(OverEstimateToBurn) = (GasLimit-GasUsed)*BaseFee

  燃烧费(BurnFee) = 基本燃烧费(BaseToBurn) + 超额燃烧费(OverEstimateToBurn)
```


## 案例

Message CID: [bafy2bzacecrui7vx6por454atwr4cdide5bh3qpcfctzbpqpexs574n5kye7u](https://fic.filscout.io/zh/pc/message/bafy2bzacecrui7vx6por454atwr4cdide5bh3qpcfctzbpqpexs574n5kye7u)

可得如下信息：
| 发送方 | 接收方| 数值 | 账户类型 |
| :----: | :----: | :----: | :----: |
| f3vebq...v2zynq | f02266 | 5,294.782856376 NanoFIC | fee |
| f3vebq...v2zynq | f02573 | 0.109369655 FIC | receive |
| f02573 | f099 | 4.3408742337 NanoFIC | burn |

Gas Fee:(GasUsed)       42466015 GasUnit
Gas Limit:              51263316 AttoFIC/GasUnit
Gas Premium:            103286 AttoFIC
Gas Fee Cap:            104340 AttoFIC
Burn Fee:               4.340874233 NanoFIC
Base Fee Burn:          4.2466015 NanoFIC
Over Estimation Burn:   0.094272733 NanoFIC
Miner Tip:           5294.782856376 NanoFIC

```
  BaseFee: 100 AttoFIC
  GasUsed: 42466015 GasUnit
  GasLimit: 51263316 AttoFIC/GasUnit
  GasPremium: 103286 AttoFIC
  
  小费(MinerFee) = GasLimit * GasPremium = 51263316*103286 attoFIC = 5294.782856376 NanoFIC
  
  基本燃烧费(BaseToBurn) = BaseFee * GasUsed = 100*42466015 attoFIC = 4.2466015 NanoFIC
  
  Over = GasLimit-11/10*GasUsed = 51263316-11/10*42466015 = 4550699.5
  GasLimit/GasUsed = 51263316/42466015 = 1.207160973310
  因为 1.1 < 1.207160973310 < 2.1, 
  超额燃烧费(OverEstimateToBurn)=(GasLimit - GasUsed)*Over/GasUsed*BaseFee = (51263316-42466015)*4550699.5/42466015*100 = 0.09427273376616 NanoFIC

  燃烧费(BurnFee) = 基本燃烧费(BaseToBurn) + 超额燃烧费(OverEstimateToBurn) = 4.2466015 + 0.094272733 = 4.34087423376616 NanoFIC = 4.340874233 NanoFIC
```
