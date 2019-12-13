## LR

## 参考

<https://blog.csdn.net/guoziqing506/article/details/81328402>





## 公式推导

$$
z_i = x_iW+b \text{ , where }W:m^0*m^1 \text{ and } x_i:1*m^0
$$

sigmoid 函数：
$$
\phi(z) = \frac{1}{1+e^{-z}}
$$
loss function:
$$
J(w) = \sum_{i=1}^n \frac{(\phi(z_i)-y_i)^2}{2}
$$
$y=0,y=1$的概率为：
$$
p(y=0|W,x) = \phi(z)  \\
p(y=1|W,x) = 1-\phi(z) 
$$
合并，即：
$$
p(y|X;W) = \phi(z)^y(1-\phi(z))^{1-y}
$$
反向传播，极大似然，需要最大化：
$$
\prod_{i=1}^n \phi(z)^{y_i}(1-\phi(z))^{1-y_i}
$$


对数化
$$
L(W) = \sum_{i=1}^n y_i \ln(\phi(z_i)) + (1-y_i) \ln(1-\phi(z_i))
$$
因为
$$
\phi(z)' = \phi(z)(1-\phi(z))
$$


求导可得：
$$
\frac{\partial{L}}{\partial{Z}} = y-\phi(Z)
$$

$$
\frac{\partial{L}}{\partial{W}} = \frac{1}{m} \cdot X^T \cdot \frac{\partial{L}}{\partial{Z}} \text{ , 其中m为样本数量，因为它的权重梯度是所有样本的和}
$$

$$
\frac{\partial{L}}{\partial{b}} = np.mean(\frac{\partial{L}}{\partial{Z}},axis=0,keepdims=True) \text{  , 即最后的维度为}1*m^1
$$





















