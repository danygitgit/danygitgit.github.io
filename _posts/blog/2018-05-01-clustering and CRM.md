---
layout: post
title: 基于Kmean聚类分析航空公司客户价值
categories: Case
description: 利用无监督方法--聚类分析客户价值
keywords: 聚类，客户价值分析

---

利用无监督方法--聚类分析客户价值

###  背景

这次的分析课题是我研究生课程数据分析与决策的小组作业，首先感谢我的小伙伴们。
这一次数据分析完全是以研究问题驱动，也是我们几次小组讨论之后的结果。在拿到数据《某航空公司客户数据》时，我们计划以一个完整的数据挖掘流程去挖掘数据中的商业价值。流程如下：

---
##### 数据预处理->聚类探索->数据可视化->航空公司客户价值分析
---
我们因为我主要负责实现客户聚类以及分析，所有还是打算把它写成一个完整的案例。

### 建模

#### 1.基础模型(RFM)

基于客户价值分析有一个基础模型，即RFM模型(如下图1-1:RFM模型)， RFM是衡量客户价值和客户创利能力的重要工具和手段。在众多的客户关系管理(CRM)的分析模式中，RFM模型是被广泛使用的。该模型通过一个客户的近期购买行为、购买的总体频率以及花了多少钱3项指标来描述该客户的价值状况，从而识别高价值的客户，即：
Recency： 最近消费时间间隔 
Frequency： 消费频率 
Monetary： 消费金额

![图1-1：RFM模型](/images/blog/2018-05-01-1.png  "图1-1：RFM模型")

结合RFM模型我们选取了原始数据中的三个属性值[平均乘机间隔:AVG_INTERVAL,观测窗口内的飞行次数:FLIGHT_COUNT,总票价:SUM_PRICE]三个指标对用户进行聚类，其中总票价是一个新生成的特征：由两年票价总和而来，最终将客户聚为5类。对于RFM模型的聚类结果的类中心（如表1-1：RFM模型聚类的类中心）如下：
表1-1：RFM模型聚类的类中心

| Category |AVG_INTERVAL| FLIGHT_COUNT | SUM_PRICE |
| :-------: | :-----: | :-----: | :-----: |
|C_1|71.59704|9.454219|71189422|
|C_2|12.11986|94|1.29E+11|
|C_3|18.15115|47.36056|2.55E+09|
|C_4|7.213259|108.8333|2.55E+09|
|C_5|19.92913|113.5|2.14E+11|

通过对类中心进行列项归一化（归一化的方法为线性转换：y=（x-MinValue）/(MaxValue-MinValue)），然后基于归一化后的数据画出了雷达图（如图1-2：RFM模型的雷达图）。
![图1-2：RFM模型的雷达图](/images/blog/2018-05-01-2.png  "图1-2：RFM模型的雷达图")

从图中可以看到，每一类人数分别是：C_1：54292人，C_2：6人，C_3：3943人，C_4：32，C_5：2两人，从数据和图中可以看出，单纯依靠RFM三个指标分出的5类用户效果不佳，不仅每一类的人数分布差异很大，且C_2，C_5重叠很大。

---

#### 2. 拓展模型-1（LRFMC）

虽然RFM模型可以衡量客户价值,但是对于这次的航空数据集，在聚类之后对于刻画不同的用户价值的效果并不理想，所以基于RFM模型可能不合适。由于同样的消费金额的不同旅客对航空公司的价值不同，例如买长航线、低等仓的旅客和买短航线、高等仓的旅客消费金额相同，但是价值却是不同的。显然后者更有价值。所以进一步地，选择客户在一定时间内的飞行里程M和乘坐舱位所对应的折扣系数C。同时，因为航空公司会员的加入时间一定程度上可以影响客户价值，所以我们在航空公司客户价值分析模型中添加客户关系长度 L，作为区分客户价值的另一个指标，所以我们构建出LRFMC 模型。

---
L：会员入会时间距观测窗口结束的时间（TIME_INTERVAL）
R：客户最近一次乘坐公司分级距观测窗口结束的时间（月数）（LAST_TO_END）
F：客户在观测窗口内乘坐公司飞机的次数（FLIGHT_COUNT）
M：客户在观测窗口内累计的飞行里程（SEG_KM_SUM）
C：客户在观测窗口内乘坐舱位所对应的折扣系数的平均值（avg_discount）

---
在LRFMC模型中，我们选择的特征包括了五类，分别是时间间隔:TIME_INTERVAL, 观测窗口内的飞行次数:FLIGHT_COUNT, 观测窗口总飞行公里数SEG_KM_SUM, 平均折扣率:avg_discount, 最后一次乘机时间至观测窗口结果时长(月)LAST_TO_END，其中时间间隔为会员入会时间距观测窗口结束的时间，聚类结果如下（表2-1 LRFMC聚类模型的类中心）.

表2-1 LRFMC聚类模型的类中心

| Category |TIME_INTERVAL| FLIGHT_COUNT | SEG_KM_SUM |avg_discount|LAST_TO_END|
| :-------: | :-----: | :-----: | :-----: |:-----:|:-----:|
|C_1|1941.1565| 51.8939|81022.5|0.78842|27.4025|
|C_2|1537.4712|15.1758|21362.8|0.72559|97.5557|
|C_3|1730.7709|30.1751|44550.5|0.75299|49.7244|
|C_4|1352.2401|4.97864|6373.01|0.70832|231.587|
|C_5|2130.6303|76.5931|152434.|0.84164|19.4043|

通过对LRFMC聚类结果的类中心进行归一化，我们得到了如下雷达图（图2-1： LRFMC模型雷达图）：
![图2-1 LRFMC模型雷达图](/images/blog/2018-05-01-3.png  "图2-1 LRFMC模型雷达图")

图2-1 LRFMC模型雷达图
通过对上图进行观察和分析，我们知道每一类别人数分别为：C_1:1867,C_2:14887,C_3:5831,C_4:35294,C_5:376, 其中C_5在时间间隔、观测窗口内的飞行次数、观测窗口总飞行公里数、平均折扣率、四个维度都比C_1、C_3、C_2更大，在最后一次乘机时间至观测窗口结果时长(月)这个维度上，C_4>C_2>C_3>C_1>C_5，根据雷达图的性质，面积越大，它的价值越大，所以价值大小排序为：C_5>C_1>C_3>C_2>C_4，依据客户价值排序我们可以将其分为重要保持客户、重要发展客户、重要挽留客户、普通价值客户、低价值客户：
表2-2:LRFMC模型的客户价值关系

| Category |Value Rating| Headcount|客户价值 |
| :-------: | :-----: | :-----: | :-----: |
|C_1|2|1867|重要发展客户|
|C_2|4|14887|普通价值客户|
|C_3|3|5831|重要挽留客户|
|C_4|5|35294|低价值客户|
|C_5|1|376|重要保持客户|

---

#### 3 拓展模型-2（LRFMC+2P模型）

在模型LRFMC模型中我们已经将基于五个指标（LRFMC）将用户划分为五类价值客户分别是重要发展客户、重要保持客户、重要挽留客户、普通价值客户、低价值客户，从雷达图可以比较清晰地观测和分析出客户价值，为了进一步探究是否其他属性会对客户价值聚类有正向作用，我们在LRFMC模型基础上增加了总积分Points_Sum和总票价SUM_PRICE两个属性从而提出了LRFMC+2P模型，以生活检验我们知道，总积分和总票价越高，那么客户在平台的消费力也越大，价值显然更高，因此我们提出的LRFMC+2P模型的参数如下：

---
L：会员入会时间距观测窗口结束的时间（TIME_INTERVAL）
R：客户最近一次乘坐公司分级距观测窗口结束的时间（月数）（LAST_TO_END）
F：客户在观测窗口内乘坐公司飞机的次数（FLIGHT_COUNT）
M：客户在观测窗口内累计的飞行里程（SEG_KM_SUM）
C：客户在观测窗口内乘坐舱位所对应的折扣系数的平均值（avg_discount）
2P: 客户在观测期的总积分和总票价（"Points_Sum", "SUM_PRICE"）

---

基于我们提出的LRFMC+2P模型，结合航空数据的属性，我们对其进行聚类分析，得到了相应的聚类结果，其中每类的类中心（表3-4：LRFMC+2P模型的类中心），同时，对类中心进行归一化处理，然后画出了7个属性的雷达图（如图3-4：LRFMC+2P模型的雷达图）：
表3-1：LRFMC+2P模型的类中心

| Category |TIME_INTERVAL| FLIGHT_COUNT | SEG_KM_SUM |avg_discount|LAST_TO_END|Points_Sum|SUM_PRICE|
| :-------: | :-----: | :-----: | :-----: |:-----:|:-----:|:-----:|:-----:|
|C_1|1424.| 9.5|13534.7|0.71191|181.5091|9259.56|72586528|
|C_2|2215.167|94|257112.|1.11308|20|323501|1.29E+1|
|C_3|1860.25|109|196685.|1.07389|6.583333|249070.|7.68E+1|
|C_4|1971.286|47.6|69385.5|0.83994|29.41255|59803.9|2.57E+1|
|C_5|1894|114|353033|1.09407|17.5|393372|2.14E+1|

![图3-1：LRFMC+2P模型的雷达图](/images/blog/2018-05-01-4.png  "图3-1：LRFMC+2P模型的雷达图")
通过对雷达图进行观察和分析，以及依据雷达图性质，我们可以得到五类客户价值以及人数分布（表3-5：LRFMC+2P模型的客户价值关系），可以明显看出每类价值分布太不均衡，因而并不适合用户刻画用户价值。
表3-2:LRFMC+2P模型的客户价值关系

| Category |Value Rating| Headcount|客户价值 |
| :-------: | :-----: | :-----: | :-----: |
|C_1|5|54326|低价值客户|
|C_2|2|6|重要发展价值|
|C_3|3|12|重要挽留客户|
|C_4|4|3909|普通价值客户|
|C_5|1|2|重要价值客户|

综合以上三个模型，LRFMC的模型更具有实用性，LRFMC模型选择的三个用户属性不仅可以较好刻画用户价值，且每类用户分布较为合理。

### 代码附录

```
#!/usr/bin/env python
# -*- coding: utf-8 -*-
from sklearn.cluster import KMeans
import pandas as pd
import datetime
import numpy as np
import matplotlib.pyplot as plt


def read_data(path):
    return pd.read_csv(path, sep=",")


class Model(object):
    def __init__(self, k):
        self.train_data = None
        self.k_cluster = k
        self.feature = {"feature_3": ["AVG_INTERVAL", "FLIGHT_COUNT", "SUM_PRICE"],
                        "feature_5": ["TIME_INTERVAL", "FLIGHT_COUNT", "SEG_KM_SUM", "avg_discount", "LAST_TO_END"],
                        "feature_7": ["TIME_INTERVAL", "FLIGHT_COUNT", "SEG_KM_SUM", "avg_discount", "LAST_TO_END",
                                      "Points_Sum", "SUM_PRICE"]}

    def new_train_data(self):
        new_train_data = self.train_data.drop(0)
        new_train_data["SUM_PRICE"] = new_train_data["SUM_YR_1"] + new_train_data["SUM_YR_2"]
        new_train_data["LOAD_TIME"] = new_train_data["LOAD_TIME"].apply(lambda x: x.split("/"))
        new_train_data["FFP_DATE"] = new_train_data["FFP_DATE"].apply(lambda x: x.split("/"))
        for i in new_train_data.index:
            year, month, day = int(new_train_data.ix[i, "LOAD_TIME"][0]), int(new_train_data.ix[i, "LOAD_TIME"][1]), \
                               int(new_train_data.ix[i, "LOAD_TIME"][2])
            year_2, month_2, day_2 = int(new_train_data.ix[i, "FFP_DATE"][0]), \
                                     int(new_train_data.ix[i, "FFP_DATE"][1]), int(new_train_data.ix[i, "FFP_DATE"][2])
            new_train_data.ix[i, "TIME_INTERVAL"] = (datetime.datetime(year, month, day) -
                                                     datetime.datetime(year_2, month_2, day_2)).days
        features = []
        for item in self.feature.values():
            for fea in item:
                if fea not in features:
                    features.append(fea)
        new_train_data = new_train_data.dropna()
        new_train_data.to_csv("../data/raw/target_data.csv", sep=",")
        return new_train_data

    def cluster(self, target, label):
        kmean_model = KMeans(self.k_cluster)
        kmean_model.fit(target)
        return [kmean_model.labels_, kmean_model.cluster_centers_]

    def graph(self, cluster_result, label, key):

        # 使用ggplot的绘图风格
        plt.style.use('ggplot')
        r1 = pd.Series(cluster_result[0]).value_counts()  # 统计各个类别的数目
        r2 = pd.DataFrame(cluster_result[1])
        r2 = r2.apply(lambda x: (x - np.min(x)) / (np.max(x) - np.min(x)))  # 找出聚类中心
        # 所有簇中心坐标值中最大值和最小值
        max = r2.values.max()
        min = r2.values.min()
        r = pd.concat([r2, r1], axis=1)  # 横向连接（0是纵向），得到聚类中心对应的类别下的数目
        r.columns = label + [u'类别数目']  # 重命名表头

        # 绘图
        fig = plt.figure(figsize=(10, 8))
        ax = fig.add_subplot(111, polar=True)
        center_num = r.values
        feature = label
        N = len(feature)
        for i, v in enumerate(center_num):
            # 设置雷达图的角度，用于平分切开一个圆面
            angles = np.linspace(0, 2 * np.pi, N, endpoint=False)
            # 为了使雷达图一圈封闭起来，需要下面的步骤
            center = np.concatenate((v[:-1], [v[0]]))
            angles = np.concatenate((angles, [angles[0]]))
            # 绘制折线图
            ax.plot(angles, center, 'o-', linewidth=2, label="category_%d : %d" % (i + 1, v[-1]))
            # 填充颜色
            ax.fill(angles, center, alpha=0.25)
            # 添加每个特征的标签
            ax.set_thetagrids(angles * 180 / np.pi, feature, fontsize=15)
            # 设置雷达图的范围
            ax.set_ylim(min - 0.1, max + 0.1)
            # 添加标题
            plt.title('The ' + key +' of users', fontsize=20)
            # 添加网格线
            ax.grid(True)
            # 设置图例
            plt.legend(loc='upper right', bbox_to_anchor=(1.3, 1.0), ncol=1, fancybox=True, shadow=True)
            # 显示图形
        plt.show()

    @staticmethod
    def combine_results(target_data, cluster_result, filename):
        path = "../data/results/"
        new_data = pd.DataFrame()
        new_data["Cluster_Results"] = pd.Series(cluster_result[0], index=target_data.index)
        new_data = target_data.join(new_data)
        new_data.to_csv(path + filename + "_cluster.csv", sep=",")
        cluster_center = pd.DataFrame(cluster_result[1])
        cluster_center.to_csv(path + filename + "_center.csv", sep=",")
        return new_data

    def run(self):
        self.train_data = read_data("../data/raw/new_data.csv")
        new_train_data = self.new_train_data()
        for key, value in self.feature.items():
            cluster_result = self.cluster(target=new_train_data[value], label=value)
            self.graph(cluster_result, label=value, key=key)
            self.combine_results(target_data=new_train_data, cluster_result=cluster_result, filename=key)
if __name__ == "__main__":
    mo = Model(k=5)
    mo.run()
```
### 总结
本次用户聚类是基于最基础的模型RFM，在RFM模型上进行扩展，选择了第一种扩展模型（LRFMC），实际上，此次建模这是对数据的探索。

如需获取本次案例的数据或者更加详细的分析报告，请下方关注微信公众号

![](/images/blog/2018-05-01-5.png) 

回复：某航空公司数据 | 某航空公司报告 
