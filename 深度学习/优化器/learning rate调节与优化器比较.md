# learning rate调节与优化器比较

### 普通SGD

$$
\theta = \theta - \alpha \cdot \bigtriangledown_\theta J(\theta_{t-1})
$$

其中$\alpha$为学习率，$\bigtriangledown_\theta J$为梯度

### AdaGrad

$$
g_t = \bigtriangledown_\theta J(\theta_{t-1}) \\
\theta_{t} = \theta_{t-1} - \alpha \cdot \frac{g_t}{\sqrt{\sum_{i=1}^t g_t^2}}
$$

其中$g_t$代表第t时间步的梯度。

根据第二条公式的后半部分，因此能累计之前所有的梯度之和。因此得出如下趋势：

- 由于累积的梯度和一直变大，更新的梯度大致是越来越慢的，直到慢慢收敛，比传统SGD可能达到更好的收敛（$g_t$成大比例上升的特殊情况例外，此时必定是$alpha$选取有问题）。
- 缺点：往往对于训练时间步较长的情况来说，由于分母项越来越大，学习率会收缩到无法继续更新。

### RMSProp

$$
g_t = \bigtriangledown_\theta J(\theta_{t-1}) \\
v_t = \gamma \cdot v_{t-1} + (1-\gamma) \cdot g_t^2 \\
\theta_t = \theta_{t-1}-\alpha \cdot \frac{g_t}{\sqrt{v_t}+\epsilon}
$$

其中$\gamma$是指数衰减率（超参数，往往设为0.9），$\epsilon$是为了防止分母为0（超参，往往设为$10^8$）。由于$v_t$遵循之前的规律，那么代入$v_{t-1},v_{t-2}$，可以得到如下公式：
$$
v_t = \gamma^3 \cdot v_{t-3} +(1-\gamma)\cdot g_t^2 + \gamma(1-\gamma)g_{t-1}^2 + \gamma^2(1-\gamma)g_{t-2}^2
$$
假设代入$\gamma = 0.9$，得
$$
v_t = 0.729 \cdot v_{t-3} +0.1\cdot g_t^2 + 0.09g_{t-1}^2 + 0.081g_{t-2}^2
$$

- 大致可以看到，当前$v_t$受之前的影响只会越来越小。
- 另外此种情况比较稳定，可以克服AdaGrad梯度变小消失的问题。
- 但是假设$g_t$在某一段始终相等且很大，$\theta$的下降始终为$1*\alpha$固定值，可能会导致下降较慢。

### Adam优化器

结合AdaGrad（不完全是，更像动量优化）和RMSProp的优势。
$$
g_t = \bigtriangledown_\theta J(\theta_{t-1}) \\
m_t = \beta \cdot m_{t-1} + (1-\beta) \cdot g_t \\
\hat{m_t} = \frac{m_t}{1-\beta^t} \\
v_t = \gamma \cdot v_{t-1} + (1-\gamma) \cdot g_t^2 \\
\hat{v_t} = \frac{v_t}{1-\gamma^t} \\
\theta_t = \theta_{t-1}-\alpha \cdot \frac{\hat{m_t}}{\sqrt{\hat{v_t}}+\epsilon}
$$
其中$\beta$为超参，默认0.9。$\gamma$为超参，默认0.999。$\epsilon$为了避免除数为0，默认为$10^8$。

其中$\hat{m_t},\hat{v_t}$为修正项，由于初始化其为0，会导致它在训练初期偏向于0，而$1-\beta^t$将在t足够大时约等于1，即不再修正。

由$\theta$更新公式可以看到，分子为梯度均值，分母为梯度平方根。

在使用过程中，貌似Adam前期更快收敛，但收敛到几乎局部最优时，跟SGD时间差不多。且有些实验表明，Adam更需要调参，对正则化要求更高，否则最后在测试集上效果比SGD差。

### 寻找各local minimum进行ensemble(改变学习率)

<https://jsideas.net/python/2018/03/14/snapshot_ensemble.html>

运用深度模型时很好用的方法：模拟退火在找到local minimum时修改学习率，找其他local minimum，最后选取好的进行ensemble。

但是lightGMB等GBDT模型不能热重启，看到过有kaggle第一名的解决方案中有调节随机种子的，应该也是一种找其他local minimum的方法



