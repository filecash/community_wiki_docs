# 关于WindowPost惩罚公式

## 1.计算PowerDelta

在windowpost过程中，会计算出一个名为PowerDelta的数值，如果windowpost验证成功以后，就会发送一条UpdateClaimedPower消息，下面我们首先来看这个数值如何计算。

### 1.1.故障排除

在windowpost证明阶段，首先会去查找有故障的sector，这里面有几个关键的变量，分别是：

FaultyPower：已经故障的算力

lostUnprovenPower：未被证明的算力，且已经丢弃的

newPowerDelta：算力的变化

这三者之间的关系如下：
$$
\mathbf{newPowerDelta}=\mathbf{lostUnprovenPower}-\mathbf{FaultyPower}
$$


retractedRecoverPower：已恢复的算力

hasNewFaults：新申报的故障扇区个数

### 1.2.生成新的算力变化

计算PowerDelta的公式如下：
$$
\mathbf{PowerDelta}=
\sum_{\mathbf{k}=1}^\mathbf{n}
\mathbf{newPowerDelta[k]}
+
\mathbf{ActivateUnproven[k]}
+
\mathbf{recoveredPower[k]}
$$
其中n表示windowpost提交的所有分区，每一个分区都有自己的newPowerDelta，ActivateUnproven和recoveredPower。

## 2.UpdateClaimedPower阶段

计算TotalCommitedPower：
$$
\mathbf{TotalCommitedPower}=\mathbf{TotalCommitedPower}+\mathbf{PowerDelta}
$$
生成newClaimed：
$$
\mathbf{newClaimed}=\mathbf{oldClaimed}+\mathbf{PowerDalta}
$$
计算出以上内容之后，会拿当前矿工全网共识的最小算力(minPower)同oldClaimed与newClaimed进行对比，对不结果如下所示：

1.oldClaimed小于minPower，newClaimed 大于等于minPower：
$$
\mathbf{MinerAboveMinPowerCount}=\mathbf{MinerAboveMinPowerCount}+1
$$

$$
\mathbf{TotalPower}=\mathbf{TotalPower}+\mathbf{newClaimed}
$$

2.oldClaimed大于等于minPower，newClaimed 小于minPower：
$$
\mathbf{MinerAboveMinPowerCount}=\mathbf{MinerAboveMinPowerCount}-1
$$

$$
\mathbf{TotalPower}=\mathbf{TotalPower}-\mathbf{oldClaimed}
$$

3.oldClaimed大于等于minPower，newClaimed 大于等于minPower：
$$
\mathbf{TotalPower}=\mathbf{TotalPower}+\mathbf{PowerDelta}
$$

## 3.UpdateNetWorkKPI

这是一个定时任务，会每隔一段时间，会根据当前矿工的算力做一次KPI，UpdateNetWorkKPI消息，输入的参数是TotalPower。

### 3.1.更新下一次收益的参数

计算当前的thisEpochBaselinePower：
$$
\mathbf{BaselineExponent}=340282591298641078465964189926313473653
$$

$$
\mathbf{thisEpochBaselinePower}
=
Rsh(
\mathbf{BaselineExponent}
\times
\mathbf{TotalPower}
,
128
)
$$

$$
\mathbf{cappedRealizedPower}
=
min(
\mathbf{thisEpochBaselinePower}
,
\mathbf{TotalPower}
)
$$

$$
\mathbf{CumsumRealized}
=
\mathbf{CumsumRealized}
+
\mathbf{cappedRealizedPower}
$$

如果CumsumRealized大于CumsumBaseline的前提下，会不断循环执行以下计算流程：
$$
\mathbf{EffectiveNetworkTime}=\mathbf{EffectiveNetworkTime}+1
$$

$$
\mathbf{EffectiveBaselinePower}
=
Rsh(
\mathbf{BaselineExponent}
\times
\mathbf{EffectiveBaselinePower}
,
128
)
$$

$$
\mathbf{CumsumBaseline}=\mathbf{CumsumBaseline}+\mathbf{EffectiveBaselinePower}
$$

### 3.2.计算出下一次收益状况

$$
\mathbf{rewardTheta}=Lsh(\mathbf{effectiveNetworkTime},128)
$$

$$
\mathbf{diff}
=
Lsh(
\mathbf{cumsumBaseline}
-
\mathbf{cumsumRealized}
,
128
)
$$

$$
\mathbf{diff}=\mathbf{diff} \mod \mathbf{baselinePowerAtEffectiveNetworkTime}
$$

$$
\mathbf{rewardTheta}=\mathbf{rewardTheta}-\mathbf{diff}
$$

以上的步骤会被执行两次，第一次执行后的结果保存再prevRewardTheta，之后再执行3.1的步骤；执行完3.1的步骤以后，再执行一次后，讲值保存再currRewardTheta。

当前的收益是由以下公式得到：
$$
\mathbf{SimpleTotal}=330\times10^{24}
$$

$$
\lambda=37396271439864487274534522888786
$$

$$
\mathbf{ExpLamSubOne}=37396273494747879394193016954629
$$

$$
\mathbf{simpleReward}=\mathbf{SimpleTotal}\times\mathbf{ExpLamSubOne}
$$

$$
\mathbf{epochLam}=\mathbf{epoch}\times\lambda
$$

$$
\mathbf{simpleReward}=
Rsh(
\mathbf{simpleReward}
\times
(e^{-\mathbf{epohLam}})
,
128
)
$$

$$
\mathbf{currEtl}=e^{-Rsh(\mathbf{currRewardTheta}\times\lambda,128)}
$$

$$
\mathbf{currReward}=
\mathbf{baselineTotal}
\times
(1\times10^{18}-\mathbf{currEtl})
$$

$$
\mathbf{prevEtl}=e^{-Rsh(\mathbf{prevRewardThrta}\times\lambda,128)}
$$

$$
\mathbf{prevReward}=
\mathbf{baselineTotal}
\times
(1\times10^{18}-\mathbf{prevEtl})
$$

$$
\mathbf{baselineReward}=\mathbf{currReward}-\mathbf{prevReward}
$$

$$
\mathbf{reward}=Rsh(\mathbf{simpleReward}-\mathbf{baselineReward},128)
$$

如此，reward是最终的矿工收益。

## 4.关于惩罚公式的后续工作

从以上步骤来看，lotus再每次进行windowpost的时候，都会做一个累加计算，累加的值算进TotalPower变量中。

矿工最终的收益跟TotalPower累加的总算力有关，例如矿工10G的算力，总共要进行100次Windowpost的话，那么最终的TotalPower值为1000G，如果中间掉过算力，那么肯定加不满1000G。

假设矿工自己申报某个扇区，那么故障块不会算入算力，其他扇区依然提供正常的算力输出。

假设矿工直接掉扇区，且自己不申报的话，还会被倒扣算力，惩罚更严重。

从以上步骤可以看出，惩罚公式需要反向的推导和归纳才能得出，推导出这样的公式需要额外的工作时间支出。
