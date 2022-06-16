# 数据集

   文件夹中的数据集是已经预处理好的数据集。该数据集中包含了淘宝2010年三月份到七月份的，大概几百万的数据。该数据集主要包含了user表、描述卖家店铺星钻冠等级、开店时长、总交易笔数相关信息；rate表描述了卖家店铺评分信息；trans表描述了卖家店铺开店时长、总交易笔数、价格、数量总的金额、买家ID相关信息；dsr表描述了每个订单中买家对于买家的服务得分、发货得分、物流得分、商品得分。

# Python库

- numpy
- pandas
- matplotlib
- seaborn
- sklearn

# 数据预处理

数据预处理使用EXCEL处理

# 代码

统计每月的物流得分

```
logistics_score = dsr.groupby('月')['物流得分'].mean().plot()
```

统计不同时间段的评分次数

```
time_score = rate.groupby('时')['评分'].count().plot()
```

选出最佳聚类数

```
# 模型创建、训练，选取最优聚类参数K
scores = [ ]
range_values = np.arange(2, 10)
for i in range_values:
    kmeans = KMeans(init='k-means++', n_clusters=i)
    kmeans.fit(df)
    score = silhouette_score(df, kmeans.labels_, metric='euclidean', sample_size=len(df)) #计算得分
    scores.append(score)
    
# 绘制出得分结果
plt.figure()
plt.bar(range_values, scores, width=0.6, color='b', align='center')
plt.title('Silhouette score')
plt.show()
```

聚类

```
#聚类
from sklearn.cluster import KMeans
Kmodol=KMeans(2)
Kmodol.fit(df)
df['类别']=Kmodol.labels_
# data=data.rename(columns={0:'L',1:'R',2:'F',3:'M'})
df
```

特征分析

```
data_X=df.set_index('类别').stack().reset_index().rename(columns={'level_1':'特征'})
g = sns.FacetGrid(data_X, row="类别",col='特征',sharex=False,sharey=False)
g.map(sns.distplot,0)
```

# 小组实际分工情况

孟国庆（组长）：负责实验项目的安排、代码实现、小组汇报。贡献率：24%

黄梓峰：负责数据集汇总，论文的理论基础。贡献率：21%

温志华：负责PPT制作，代码实现。贡献率：19%

胡英俊：负责实验过程中的研究方法。贡献率：12%

贺文韬：负责对实验结果的分析以及结论。贡献率：15%

刘卫春：负责该领域相关知识的搜集。贡献率：9%