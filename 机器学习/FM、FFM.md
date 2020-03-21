FM：https://blog.csdn.net/weixin_38664232/article/details/89524374（物品库小于千万级别，可以尝试FM一路召回）

FM：可以理解为每一个特征（onehot之后k维）的embedding累加求内积





## FM（Factorization Machines）

$$
y = \sum_{i=1}^{n-1} \sum_{j=i+1}^{n} w_{ij} x_i x_j \\

\text{其中,} W \text{矩阵可以分解为}VV^T \\

\text{原式}=\sum_{i=1}^{n-1} \sum_{j=i+1}^{n} <v_i,v_j> x_i x_j \\

= \frac{1}{2}\sum_{i=1}^{n} \sum_{j=1}^{n} <v_i,v_j> x_i x_j - \frac{1}{2}\sum_{i=1}^{n} <v_i,v_i> x_i x_i \\

= \frac{1}{2}(\sum_{i=1}^{n} \sum_{j=1}^{n} \sum_{f=1}^{k} v_{if} v_{jf} x_i x_j - \sum_{i=1}^{n} \sum_{f=1}^{k} v_{if} v_{if} x_i x_i) \\

= \frac{1}{2}\sum_{f=1}^{k}((\sum_{i=1}^{n} v_{if}x_i)(\sum_{j=1}^n v_{jf}x_j) - \sum_{i=1}^n v_{if}^2 x_i^2) \\

= \frac{1}{2} \sum_{f=1}^k ((\sum_{i=1}^n v_{if}x_i)^2 - \sum_{i=1}^n v_{if}^2 x_i^2)
$$

其中，$v_{if}x_i$其实就是第f个特征的embedding。



## FFM（Field-aware Factorization Machine）

假设样本的n个特征属于f个field，那么FFM的二次项有nf个隐向量，每个特征对其他的每个field学一个隐向量。

如果隐向量的长度为k，那么FFM的二次参数有nfk个，远多于FM模型的nk个。
$$
y = w_0 + \sum_{i=1}^n w_ix_i + \sum_{i=1}^{n-1} \sum_{j=i+1}^{n} <V_{i,f_j},V_{j,f_i}> x_i x_j \\
$$
使用logistic loss，反向传播即可。



