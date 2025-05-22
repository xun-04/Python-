# 房价预测竞赛数据集简介

本项目数据源自 Kaggle 平台的 “房价预测：高级回归技术（House Prices: Advanced Regression Techniques）” 竞赛。该数据集被广泛用于回归建模、特征工程和数据科学教学。下述内容将帮助你快速了解数据结构、变量说明和分析流程。


## 1. 项目背景

本项目的目标是根据爱荷华州埃姆斯市（Ames, Iowa）住宅的各类特征，建立一个回归模型预测房屋的最终售价。你可以使用本数据集开展数据分析、特征工程、机器学习建模等任务。


## 2. 数据简介

数据集包括两部分：

* **训练集（train.csv）**：含有 1460 条带房价的样本记录。
* **测试集（test.csv）**：含有 1459 条无房价的样本记录，需要你对其房价进行预测。

每个样本是一套住宅，包含多达 79 个用于预测房价的特征变量，以及一个目标变量 SalePrice（仅在训练集中给出）。


## 3. 主要变量说明

以下是部分核心变量的简要介绍，完整变量列表及其详细说明见 \[data\_description.txt]。

* **Id**：每套房屋的唯一标识号
* **MSSubClass**：建筑类别（如 20=1 层别墅）
* **MSZoning**：区域划分类型
* **LotFrontage**：临街长度（英尺）
* **LotArea**：土地面积（平方英尺）
* **Street**：街道类型（碎石/沥青）
* **Alley**：巷道类型
* **OverallQual**：整体材质与装修质量评分（1-10）
* **OverallCond**：整体房屋状况评分（1-10）
* **YearBuilt**：房屋建造年份
* **YearRemodAdd**：房屋翻修年份
* **RoofStyle**：屋顶样式
* **ExterQual**：外部材料质量
* **BsmtQual**：地下室高度
* **TotalBsmtSF**：地下室总面积（平方英尺）
* **HeatingQC**：供暖质量和状况
* **CentralAir**：是否中央空调（Y/N）
* **GrLivArea**：地面以上居住面积（平方英尺）
* **FullBath**：完整卫生间数量
* **BedroomAbvGr**：卧室数量（地面以上）
* **KitchenQual**：厨房质量
* **GarageCars**：车库容量（车位数）
* **GarageArea**：车库面积（平方英尺）
* **SalePrice**：房屋最终售价（目标变量，仅训练集）


## 4. 文件结构

* `train.csv`：训练数据，含目标变量
* `test.csv`：测试数据，无目标变量
* `data_description.txt`：每个变量的详细含义
* `sample_submission.csv`：预测提交格式示例


## 5. 分析流程建议

1. **数据读取与初步查看**
   建议使用 pandas 等工具导入并浏览数据结构与缺失值分布。

2. **数据清洗与特征工程**

   * 处理缺失值（填补、删除或标记）
   * 类别变量的编码（如独热编码）
   * 变量的归一化/标准化
   * 新特征构建（如总面积 = 地面以上面积 + 地下室面积）

3. **建模与预测**
   常见模型包括线性回归、岭回归、Lasso、随机森林、梯度提升树等。

4. **模型评估**
   推荐使用 RMSE（均方根误差）等回归性能指标。

5. **结果提交**
   按 `sample_submission.csv` 格式准备预测结果。


## 6. 参考资料

* [竞赛主页（Kaggle）](https://www.kaggle.com/competitions/house-prices-advanced-regression-techniques)
* [数据说明文档（data\_description.txt）](https://www.kaggle.com/competitions/house-prices-advanced-regression-techniques/data)
* 爱荷华州埃姆斯房价原始数据论文：
  De Cock, D. (2011). Ames, Iowa: Alternative to the Boston Housing Data as an End of Semester Regression Project. Journal of Statistics Education, 19(3). [DOI: 10.1080/10691898.2011.11889627](https://doi.org/10.1080/10691898.2011.11889627)


## 7. 版权声明

本数据集仅供教学与学术用途，禁止用于商业目的。原始数据由 Dean De Cock 教授整理并开放用于数据科学竞赛与教学。

