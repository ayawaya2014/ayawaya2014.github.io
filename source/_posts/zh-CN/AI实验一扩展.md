---
title: AI实验一扩展
lang: zh-CN
tags:
  - 教学
  - 编程
  - AI
  - Programming
  - Data
categories:
  - 实验教学 
  - 人工智能基础
abbrlink: fa21b28d
date: 2026-09-29 13:57:00
---

To be continue..
监督学习：决策树——选做实验
<!--more-->
```Python
import pandas as pd
from sklearn.model_selection import train_test_split     #数据分割方法所在的包
from sklearn import tree                                 #决策树算法所在的包
import pickle                                             #将模型文件写入硬盘所需的包
from sklearn.metrics import classification_report        #生成分类报告所需的包

# 加载数据
data =pd.read_csv('finance数据集.csv')
#查看数据的前5行，了解数据概况
print(data.head())

#去除明显没有意义的特征Unnamed: 0
data=data.drop(columns='Unnamed: 0')

#数据预处理
#1.去除包含缺失值的行
data.dropna()

# 2.识别删除包含异常值的行
# 2.1）识别数值类型的特征
numeric_cols = data.select_dtypes(include=['float64', 'int64']).columns
# 2.2）使用IQR识别异常值，
Q1 = data[numeric_cols].quantile(0.25)
Q3 = data[numeric_cols].quantile(0.75)
IQR = Q3 - Q1
# 2.3)移除异常值
data_cleaned = data[~((data[numeric_cols] < (Q1 - 1.5 * IQR)) | (data[numeric_cols] > (Q3 + 1.5 * IQR))).any(axis=1)]

# 3.检查重复值
#3.1)判断一个记录是否是重复记录
duplicates = data_cleaned.duplicated()
#3.2)统计重复记录的数量
num_duplicates = duplicates.sum()
#3.3)删除重复记录
data_cleaned = data_cleaned[~duplicates]
#3.4）打印删除的重复记录数
print(f'删除的重复行数: {num_duplicates}')

# 4.对数值类型的数据做归一化标准化处理：将所有的数据投射到0~1的范围
#4.1）导入要用到的包
from sklearn.preprocessing import MinMaxScaler
#4.2）初始化一个标准化归一化工具
scaler = MinMaxScaler()
#4.3）进行数据标准归一化处理
data_cleaned[numeric_cols] = scaler.fit_transform(data_cleaned[numeric_cols])

#选取目标变量（标签）和自变量（特征），X为特征，y为标签
X = data_cleaned.drop(columns=['SeriousDlqin2yrs'])
y = data_cleaned['SeriousDlqin2yrs']

# 将数据集划分为训练集和测试集（测试集占比20%）
X_train, X_test, y_train, y_test = train_test_split(X,y,test_size=0.2, random_state=42)

# 初始化决策树分类模型（以信息熵为分枝依据）
# criterion="entropy", 按信息熵分枝，  criterion="gini", 按基尼系数分枝
model = tree.DecisionTreeClassifier(criterion="entropy")
#训练 决策树分类 模型
model.fit(X_train, y_train)

# 保存模型
with open('DecisionTree_finance.pkl', 'wb') as file:
    pickle.dump(model, file)

# 使用训练的决策树预测测试集样本的类别
y_pred = model.predict(X_test)
# 将预测结果写入DT_results_finance.txt文件
pd.DataFrame(y_pred, columns=['预测结果']).to_csv('DT_results_finance.txt', index=False)

# 获得测试报告
report = classification_report(y_test, y_pred, zero_division=1)
#计算准确率
accuracy = (y_test == y_pred).mean()
#将测试报告及其他评估指标写入DT_report_finance.txt文件
with open('DT_report_finance.txt', 'w') as file:
    file.write(report)
    file.write(f'\n\n模型准确率: {accuracy:.2f}\n')
    file.write(f'训练集得分: {model.score(X_train, y_train)}\n')
    file.write(f'测试集得分: {model.score(X_test, y_test)}\n')

```


