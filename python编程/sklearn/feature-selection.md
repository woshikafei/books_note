## sklearn.feature_selection

### 单因素特征选择

[GenericUnivariateSelect](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.GenericUnivariateSelect.html#sklearn.feature_selection.GenericUnivariateSelect) ：可以调用其他的feature_score_func并选择最好的score / p-value

**回归**

- [f_regression](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.f_regression.html#sklearn.feature_selection.f_regression) ：每个特征与y进行单变量线性回归查看相关性（稀疏数据效果不佳）
- [mutual_info_regression](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.mutual_info_regression.html#sklearn.feature_selection.mutual_info_regression) ：通过KNN的熵估计找出每个特征与y的互信息
- scipy.stats.pearsonr相关系数
- [minepy.MINE](https://minepy.readthedocs.io/en/latest/install.html#python-users) ：MIC，互信息和最大信息系数 (Mutual information and maximal information coefficient），线性和非线性相关关系，要求数据量较大
- lasso回归通过l1正则稀疏性，并调用[SelectFromModel](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.SelectFromModel.html#sklearn.feature_selection.SelectFromModel) 去除（l1缺点是面对一组互相关特征可能只选择一项，因此使用随机化RandomizedLasso）

**分类**

- [chi2](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.chi2.html#sklearn.feature_selection.chi2) ：卡方检验
- [f_classif](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.f_classif.html#sklearn.feature_selection.f_classif) ：每个特征与y进行ANOVA检验（样本独立且正态分布，否则采用scipy.stats.kruskal）查看相关性（稀疏数据效果不佳）
- [mutual_info_classif](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.mutual_info_classif.html#sklearn.feature_selection.mutual_info_classif) ：通过KNN的熵估计找出每个特征与y的互信息
- SVM或logistic回归通过l1正则稀疏性，并调用[SelectFromModel](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.SelectFromModel.html#sklearn.feature_selection.SelectFromModel) 去除（l1缺点是面对一组互相关特征可能只选择一项，因此使用随机化RandomizedLogisticRegression ）

### 递归特征消除

[RFECV](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.RFECV.html#sklearn.feature_selection.RFECV) ：从初始特征集上，每次训练，获取`feature_importances_`或者`coef_`，然后删除最弱相关的。可选参数：每次删除个数、最少剩余个数

### 其他特征选择

- 方差 [VarianceThreshold](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.VarianceThreshold.html#sklearn.feature_selection.VarianceThreshold)：去除列内方差小于threshold%的特征。默认threshold=0，删除只有唯一值的特征
- 去除相互方差小的那些特征，删除相同的特征
- [SelectFromModel](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.SelectFromModel.html#sklearn.feature_selection.SelectFromModel) ：训练一次模型，删除重要性小于阈值的特征












































