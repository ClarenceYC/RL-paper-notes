# [Contrastive Explanation for Reinforcement Learning via Embedded Self Prediction](./contrastive_explanations_for_reinforcement_learning_via_embedded_self_predictions.pdf)
[arxiv](https://arxiv.org/abs/2010.05180)

## Motivation
文章中两个非常显著的亮点，
* 状态特征可解释性，通过利用人可理解的特征展示agent选择偏好动作的原因
* 状态广义值函数(GVF)，用于捕捉有意的具体轨迹特征（此处隐性建模了环境的状态转移）

agent决策动作时偏好价值函数更大的动作，不同于人类会区分不同状态的性质差异。文章通过将状态映射到设计有物理含义的手工特征上，并用GVF捕捉未来有具体意义的轨迹特征。

## Method
&nbsp;&nbsp;&nbsp;&nbsp;给定一个马尔科夫过程$<S,A,T,R>$,
其中$S$为状态空间，
$A$为动作空间，
$T(s, a, s \prime)$为转移函数，
$R(s, a)$为奖励函数。
目标是找到一个最优策略$\pi^{opt}$
满足
$\pi^{opt} = \mathop{\arg\min}\limits_{a}=Q^{opt}(s, a)$。
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

![](fig/Contrastive_Explanation_for_RL_via_ESP/ESP_frame.jpg)

### 训练
如同上图展示，$\hat C(\cdot;\theta_C )$将状态广义价值函数映射至Q值；
$\hat Q_F^\pi(\cdot; \theta_F)$将状态映射至人工设置状态价值特征。
其对应训练算法如下图所示

![](fig/Contrastive_Explanation_for_RL_via_ESP/ESP_algo.jpg)

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
对非线性的情况采用积分梯度(Integrated Gradient)的方式计算两个不同轨迹特征之间差异在最后Q值上的表现如下
$$\theta_i(s, a, b) = \int_{0}^{1} \frac{\partial \hat C(\hat Q_F^\pi(s, b)-\alpha \cdot (\hat Q_F^\pi(s, a) - \hat Q_F^\pi(s, b)))}{\partial [\hat Q_F^\pi(s, a)]_i} d\alpha$$

便可以得到不同特征在Q函数不同动作上的表现如下
$$\hat Q(s,a) - \hat Q(s,b) = \theta (s, a, b) \cdot \Delta_F(s, a, b)$$
可以得到和线性情况相同的解释。

#### Minimal Sufficient Explanations
在手工特征改变使得最后$Q$函数变大的下标集合为$P=\{i|\Delta_{F, i}(s, a, b) \cdot \theta_i(s, a, b)>0\}$，相对的$N = \bar P$
$MSX$表示使得轨迹变化带来正收益的最小手工特征集合（也即对当前影响最关键的几个特征）。
$$MSX = \mathop{\arg\min}\limits_{E} \{|E|:E \subset P \ and\ \sum_{i \in E}{|\Delta_{F, i}(s, a, b) \cdot \theta_i(s, a, b)|} > \sum_{i \in N}{|\Delta_{F, i}(s, a, b) \cdot \theta_i(s, a, b)|}\}$$

### Experiment
![](fig\Contrastive_Explanation_for_RL_via_ESP\exp.jpg)
上图展示在当前状态下不同动作对未来轨迹特征的GVF在未来的影响以及$MSX$

## Conclusion
文章通过设计手工刻画轨迹特征作为GVF隐式的刻画当前动作对未来轨迹的影响，并可以通过不同动作价值的在GVF的梯度解释agent更偏爱某一个动作的原因。

## Discussion

