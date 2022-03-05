[toc]

# [Contrastive Explanation for Reinforcement Learning via Embedded Self Prediction](./contrastive_explanations_for_reinforcement_learning_via_embedded_self_predictions.pdf)
[arxiv](https://arxiv.org/abs/2010.05180)

## Motivation
文章中两个非常显著的亮点，
* 状态特征可解释性，通过利用人可理解的特征展示agent选择偏好动作的原因
* 状态广义值函数(GVF)，用于捕捉有意的具体轨迹特征

agent决策动作时偏好价值函数更大的动作，不同于人类会区分不同状态的性质差异。文章通过将状态映射到设计有物理含义的手工特征上，并用GVF捕捉未来有具体意义的轨迹特征。

## Method
&nbsp;&nbsp;&nbsp;&nbsp;给定一个马尔科夫过程$<S,A,T,R>$,
其中$S$为状态空间，
$A$为动作空间，
$T(s, a, s \prime)$为转移函数，
$R(s, a)$为奖励函数。
目标是找到一个最优策略$\pi^*$
满足
$\pi^* = \mathop{\arg\min}\limits_{a}=Q^*(s, a)$。
文章希望通过解释在状态s下的嵌入向量展示为什么agent更倾向于价值更大的动作a，即$\hat Q(s, a) > \hat Q(s, b)$。

<br/>

&nbsp;&nbsp;&nbsp;&nbsp;并且为刻画未来状态轨迹的变化这里引入广义价值函数（GVF），即，
$F(s,a) = <f_1(s,a), \cdots, f_n(s,a)>$。
对给定的MDP和策略$\pi$可以通过Bellman算子计算其相应的广义值函数
$Q_F^\pi(s, a) = F(s,a) + \gamma\sum_{s\prime}T(s, a, s \prime)Q_F^\pi(s \prime, \pi(s \prime))$
可以用该GVF将状态s映射成人手工设置的一些状态特征，并用此记录预期的轨迹状态。

<br/>

对当前状态下不同的两个动作a,b，则可以通过比较$\Delta_F^\pi(s,a,b)=Q_F^\pi(s, a)-Q_F^\pi(s, b)$
便个凸显出两个不同的动作在未来轨迹上的差异。

<br/>

整个模型的框架可以如下图展现，最后将状态GVF再映射到Q函数上。

![](fig/Contrastive%20Explanation%20for%20RL%20via%20ESP/ESP_frame.jpg)

### 训练
如同上图展示，$\hat C(\cdot;\theta_C )$将状态广义价值函数映射至Q值；
$\hat Q_F^\pi(\cdot; \theta_F)$将状态映射至人工设置状态价值特征。
其对应训练算法如下图所示

![](fig/Contrastive%20Explanation%20for%20RL%20via%20ESP/ESP_algo.jpg)

### 动作差异可解释性分析
对$\hat Q(s,a) - \hat Q(s,b) > 0$有$\Delta_F^\pi(s, a, b)$，进一步

$\hat Q(s,a) - \hat Q(s,b) = \Delta_F^\pi(s, a, b) \cdot W(s,a,b)$

其中$W(s, a, b) \in \mathbb{R}^n$是轨迹状态广义价值函数差对应的权重。

#### 线性
当在简单的线性情况下，当且仅当$\Delta_F^\pi(s, a, b) \cdot W(s,a,b) > 0$
有$\hat Q(s,a) - \hat Q(s,b) > 0$。
这便是控制在人设计的轨迹特征偏好下，选择更有利于reward的轨迹。两条轨迹的差别可以用
$\Delta_F^\pi(s,a,b)$,其对结果的影响则是权重W。

#### 非线性

## Conclusion

## Discussion
