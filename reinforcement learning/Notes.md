[toc]

# [Contrastive Explanation for Reinforcement Learning via Embedded Self Prediction](./contrastive_explanations_for_reinforcement_learning_via_embedded_self_predictions.pdf)

## Motivation
文章中两个非常显著的亮点，
* 状态特征可解释性，通过利用人可理解的特征展示agent选择偏好动作的原因
* 状态广义值函数(GVF)，用于捕捉有意的具体轨迹特征

agent决策动作时偏好价值函数更大的动作，不同于人类会区分不同状态的性质差异。文章通过将状态映射到设计有物理含义的手工特征上，并用GVF捕捉未来有具体意义的轨迹特征。

## Method
&nbsp;&nbsp;&nbsp;&nbsp;给定一个马尔科夫过程<img src="http://latex.codecogs.com/gif.latex?<S,A,T,R>" />,
其中<img src="http://latex.codecogs.com/gif.latex?S" />为状态空间，
<img src="http://latex.codecogs.com/gif.latex?A" />为动作空间，
<img src="http://latex.codecogs.com/gif.latex?T(s, a, s \prime)" />为转移函数，
<img src="http://latex.codecogs.com/gif.latex?R(s, a)" />为奖励函数。
目标是找到一个最优策略<img src="http://latex.codecogs.com/gif.latex?\pi^*" />满足
<img src="http://latex.codecogs.com/gif.latex?\pi^* = \mathop{\arg\min}\limits_{a}=Q^*(s, a)">。
文章希望通过解释在状态s下的嵌入向量展示为什么agent更倾向于价值更大的动作a，即
<img src="http://latex.codecogs.com/gif.latex?\hat Q(s, a) > \hat Q(s, b)">。

<br/>

&nbsp;&nbsp;&nbsp;&nbsp;并且为刻画未来状态轨迹的变化这里引入广义价值函数（GVF），即，
<img src="http://latex.codecogs.com/gif.latex?F(s,a) = <f_1(s,a), \cdots, f_n(s,a)>" />。
对给定的MDP和策略<img src="http://latex.codecogs.com/gif.latex?\pi">可以通过Bellman算子计算其相应的广义值函数
<img src="http://latex.codecogs.com/gif.latex?Q_F^\pi(s, a) = F(s,a) + \gamma\sum_{s\prime}T(s, a, s \prime)Q_F^\pi(s \prime, \pi(s \prime))">
可以用该GVF将状态s映射成人手工设置的一些状态特征，并用此记录预期的轨迹状态。

<br/>

对当前状态下不同的两个动作a,b，则可以通过比较<img src="http://latex.codecogs.com/gif.latex?\Delta_F^pi(s,a,b)=Q_F^\pi(s, a)-Q_F^\pi(s, b)">
便个凸显出两个不同的动作在未来轨迹上的差异。

<br/>

整个模型的框架可以如下图展现，最后将状态GVF再映射到Q函数上。

![](fig/Contrastive%20Explanation%20for%20RL%20via%20ESP/ESP_frame.jpg)
![](fig/Contrastive%20Explanation%20for%20RL%20via%20ESP/ESP_algo.jpg)

## Conclusion

## Discussion