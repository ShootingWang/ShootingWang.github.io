---
title: Machine Learning | 分类模型评估
date: 2020-05-12 10:30:00
tags: [算法,机器学习,分类]
categories: 机器学习
math: true
mathjax: true
index_img: /img/ml.jpg
hide: true
notshow: true
---

<center>Classification</center>
<!--more-->

# 分类方法
从使用的技术上来分，可以把分类方法归结为四种类型：
1. 基于距离的分类方法
 - 最邻近方法
2. 决策树分类方法
 - ID3
 - C4.5
 - VFDT
3. 贝叶斯方法
 - 朴素贝叶斯方法
 - EM算法
4. 规则归纳方法
 - AQ算法
 - CN2算法
 - FOIL算法

# 分类评估
针对一个二分类问题，将实例分为正例（Positive）和负例（Negative）两种。

- **TP**（True Positive）：真正例；是正类且被预测为正类的实例
- **FP**（False Positive）：假正例；是负类但被预测为正类的实例
- **TN**（True Negative）：真负例；是负类且被预测为负类的实例
- **FN**（False Negative）：假负例；是正类但被预测为负类的实例

> 周志华《机器学习》：我们希望得到泛化误差小的学习器。

## 精确率 Precision
**精确率**（Precision）or **查准率**：
$$Precision=\frac{TP}{TP+FP}$$
> 如：警察抓小偷，描述警察抓的人中有多少个是小偷的标准即Precision

计算Precision的加权均值（**AP**，average precision）：
$$AP=\sum_i(R_i-R_{i-1})P_i$$
其中，$R_i$、$P_i$分别是第$i$个阈值对应的Recall和Precision。

```Python
sklearn.metrics.average_precision_score
```

## 召回率 Recall
**召回率**（Recall）or **查全率**：
$$Recall=\frac{TP}{TP+FN}$$
$$召回率=\frac{提取出的正确信息条数}{样本中相关的信息条数}$$
> 如：警察抓小偷，描述有多少比例的小偷被警察抓了的标准即Recall

<meta name="referrer" content="no-referrer" />
{% asset_img pic5.png 维基百科-Precision and Recall %}

## 灵敏度 Sensitivity
**灵敏度**（Sensitivity）or **真正例率**（TPR，True Positive Rate）
$$Sensitivity=TPR=Recall=\frac{TP}{TP+FN}$$
- 召回率的同义词

**1-灵敏度** or **假负例率**（FNR，False Negative Rate）or **漏诊率**
$$FNR=1-Sensitivity=\frac{FN}{TP+FN}$$

## 特异度 Specificity
**特异度**（Specificity）or **真负例率**（TNR，True Negative Rate）
$$Specificity=TNR=\frac{TN}{TN+FP}$$

**1-特异度** or **假正例率**（FPR，False Positive Rate）or **误诊率**
$$FPR=1-Specificity=\frac{FP}{TN+FP}$$

## 错误率 Error
分类错误的样本数占样本总数的比例

$$Error=\frac{FP+FN}{TP+FP+TN+FN}$$


## 准确率/精度 Accuracy
**准确率**（Accuracy，ACC）：分类正确的样本占总样本个数的比例，即
$$ACC=1-\mbox{错误率}=\frac{TP+TN}{TP+FP+TN+FN}$$

准确率存在局限性
{% note default %}
例子：Hulu的奢侈品广告主们希望把广告定向投放给奢侈品用户。Hulu通过第三方的数据管理平台（Data Management Platform，DMP）拿到了一部分奢侈品用户的数据，并以此为训练集和测试集，训练和测试奢侈品用户的分类模型。该模型的分类准确率超过了95%，但在实际广告投放过程中，该模型还是把大部分广告投给了非奢侈品用户。

这是因为：当负样本在训练集中占99%时，分类器把所有样本都预测为负样本也可以获得99%的准确率。所以，当不同类别的样本比例非常不均衡时，占比大的类别往往成为影响准确率的最主要因素。

奢侈品用户只占Hulu全体用户的一小部分，虽然模型的整体分类准确率高，但是不代表对奢侈品用户的分类准确率也高。在线上投放时，仅对模型判定的“奢侈品用户”进行投放，因此，对“奢侈品用户”判定的准确率不够高的问题就很明显了。

可使用更为有效的**平均准确率**（每个类别下的样本准确率的算数平均）作为模型评估的指标。
$$平均准确率=\frac{1}{2}\left(\frac{TP}{TP+FN}+\frac{TN}{TN+FP}\right)$$

但是，有时候，即使分类评估指标选择正确，仍可能会存在模型过拟合或欠拟合、训练集和测试集划分不合理、线下评估与线上测试的样本分布存在差异等问题。
{% endnote %}

## LR+
正例似然比（Positive Likelihood Ratio，LR+）=真正例率 / 假正例率=灵敏度 / （1-特异度）
$$LR+=\frac{TPR}{FPR}$$

## LR-
负例似然比（Negative Likelihood Ratio，LR-）=假负例率 / 真负例率=（1-灵敏度）/ 特异度
$$LR-=\frac{FNR}{TNR}$$

## DOR
DOR（Diagnostic Odds Ratio，诊断优势比）：
$$DOR=\frac{LR+}{LR-}$$

## Youden Index
$$Youden指数=灵敏度+特异度-1=真正例率-假正例率$$

## F1-score
**F1 值**（F1-score）：综合考虑Precision和Recall的度量（metric）
$$F1-score=\frac{2\times Precision\times Recall}{Precision+Recall}$$
- **宏平均F1**（macro-averaging）：先对每个类别单独计算F1值，再取这些F1值的算数平均值作为全局指标；宏平均平等对待每一个类别；值受到稀有类别的影响较大
- **微平均F1**（micro-averaging）：先累加计算各个类别的TP、TN、FP、FN，再由这些值计算F1值；平等考虑样例集中的每一个样例；值受到常见类别的影响较大
- 在multi-class classification（多分类）的情况下，macro-averaging会比micro-averaging好一些，更能体现在small class（稀有类别）上的表现（performance）

## ROC
**ROC曲线**（Receiver Operating Characteristic Curve，接收者操作特征曲线）：是反映灵敏性和特异性连续变量的综合指标。
- 横坐标：FPR（预测为正但实际为负的样本占所有负例样本的比例）；即 1-Specificity
- 纵坐标：TPR（预测为正且实际为正的样本占所有正例样本的比例）；即Sensitivity
- ROC曲线用于绘制采用不同分类阈值时的TPR与FPR
- 阈值最大时，对应坐标点为(0,0)；阈值最小时，对应坐标点为(1,1)
- 降低分类阈值会导致将更多样本归为正例，从而增加假正例和真正例的个数，TPR和FPR会同时增大
- 理想目标：TPR=1，FPR=0，即ROC图中的(0,1)点
- ROC越靠拢(0,1)点、越偏离45°对角线越好；Sensitivity、Specificity越大，效果越好
- 为什么使用ROC曲线？
 > 当测试集中的正负样本的分布变化的时候，ROC曲线能够基本保持原貌


<meta name="referrer" content="no-referrer" />
{% asset_img pic3.png ROC曲线 %}

<meta name="referrer" content="no-referrer" />
{% asset_img pic2.png ROC曲线上点的解读 %}

> **ROC vs F1-score**
When you have a data imbalance between positive and negative samples, you should always use F1-score because of ROC averages over all possible thresholds.

<meta name="referrer" content="no-referrer" />
{% asset_img pic1.png ROC曲线&TP,TN,FP,FN的关系 %}

```Python
## 计算ROC
sklearn.metrics.roc_curve(y_true, y_score)
```

## AUC
**AUC**（Area Under Curve）：ROC曲线下的面积
- 对所有可能的分类阈值的效果进行综合衡量
- 一种解读方式：可把AUC看作模型将某个随机正例样本排列在某个随机负例样本之上的概率（AUC值是一个概率值）
 > AUC代表：随机抽出一个正例、一个负例，用训练得到的分类器对这两个样本进行预测，预测结果为正的概率大于为负的概率的概率
 $$AUC=P\left\{P(Positive)>P(Negative)\right\}$$

> The **AUC value** is equivalent to the probability that a randomly chosen positive example is ranked higher than a randomly chosen negative example.
—— Fawcett, 2006

- AUC的取值范围为[0,1]；但AUC一般都处于直线y=x的上方，所以AUC的取值范围一般在0.5和1之间
 - 预测结果100%错误的模型的AUC=0
 - 预测结果100%正确的模型的AUC=1
 - 对应AUC更大的分类器，分类效果更好
- AUC的尺度不变：测量预测的排名情况，而不是测量其绝对值
- AUC的分类阈值不变：测量模型预测的质量，而不考虑所选的分类阈值
- 在假负例与假正例的代价存在较大差异的情况下，尽量减少一种类型的分类错误可能至关重要
 > 如：进行垃圾邮件检测时，可能希望优先考虑尽量减少假正例
- AUC=1，是完美分类器；采用这个预测模型时，存在至少一个阈值能得到完美预测；绝大多数预测的场合，不存在完美分类器
- 0.5 < AUC <1 ，优于随机猜测；该模型妥善设定阈值的话，能有预测价值
- AUC = 0.5，等同于随机猜测；该模型没有预测价值
- AUC  < 0.5，比随机猜测还差

```Python
## 计算AUC
sklearn.metrics.roc_auc_score(y_true, y_score)
```

## PR
**PR**（Precision-Recall）曲线
- 横轴：Recall
- 纵轴：Precision
- 不同阈值下的Precision、Recall值
- 精确率越高，召回率越高，模型和算法就越高效；即，PR曲线越靠近右上越好

<meta name="referrer" content="no-referrer" />
{% asset_img pic4.png An Introduction to ROC Analysis %}

# 其他
## 误差
- **训练误差/经验误差**（training error / empirical error）：分类器（学习器）在训练集上的误差
- **泛化误差**（generalization error）：分类器（学习器）在新样本上的误差


# 参考资料
- [精确率与召回率，RoC曲线与PR曲线](https://www.cnblogs.com/pinard/p/5993450.html)
- [AUC的计算方法](https://blog.csdn.net/qq_22238533/article/details/78666436)
- [机器学习之模型评估详解](https://www.lagou.com/lgeduarticle/109119.html)
- [分类方法归结](https://blog.csdn.net/weixin_41791402/article/details/102557277)
- [分类效果评价](https://blog.csdn.net/xiaoyu714543065/article/details/8559741)
- [F1 Score vs ROC AUC](https://intellipaat.com/community/14889/f1-score-vs-roc-auc)
- [维基百科-Precision and recall](https://en.wikipedia.org/wiki/Precision_and_recall)
- [数据挖掘习题](https://wenku.baidu.com/view/a5fa7ae7cd22bcd126fff705cc17552707225e29.html?from=search)
- [分类（Classification）：ROC和曲线下面积](https://developers.google.com/machine-learning/crash-course/classification/roc-and-auc)
- [精确率、召回率、F1 值、ROC、AUC 各自的优缺点是什么？](https://www.zhihu.com/question/30643044)
- [机器学习之分类性能度量指标 : ROC曲线、AUC值、正确率、召回率](https://www.jianshu.com/p/c61ae11cc5f6)
- [An Introduction to ROC Analysis](https://ccrma.stanford.edu/workshops/mir2009/references/ROCintro.pdf)
- [sklearn.metrics.roc_auc_score](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html)
- [sklearn.metrics.average_precision_score](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html#sklearn.metrics.average_precision_score)
- [sklearn.metrics.roc_curve](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_curve.html#sklearn.metrics.roc_curve)
- [《百面机器学习》](https://book.douban.com/subject/30285146/)

# 推荐阅读
- {% post_link Machine-Learning-聚类模型评估 %}
- {% post_link Machine-Learning-汇总 %}