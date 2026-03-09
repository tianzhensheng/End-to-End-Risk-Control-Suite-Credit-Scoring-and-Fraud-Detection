# End-to-End-Risk-Control-Suite-Credit-Scoring-and-Fraud-Detection

本项目包含两个独立的风控建模实验，分别针对**信用卡审批预测**（评分卡模型）和**信用卡欺诈检测**（异常检测与图分析）。代码基于 Python 实现，旨在复现风控领域的基础流程与进阶算法，并客观展示在公开脱敏数据集上的模型表现。

## 📁 项目结构

```text
.
├── notebooks/
│   ├── 01_Interpretable-Credit-Scoring-Card-Baseline-0309.ipynb       # 信用评分卡基线模型 (Logistic Regression + WOE)
│   └── 02_Advanced-Fraud-Detection-with-Unsupervised-and-Graph-Learning-0309.ipynb      # 反欺诈检测 (Isolation Forest + Graph Analysis)
├── requirements.txt                           # 依赖环境
└── README.md                                  # 项目说明

```

注意：由于数据版权和隐私限制，本仓库不包含原始数据文件 (data/ 文件夹为空)。运行代码前需手动下载数据集。



### 🕵️实验一：信用评分卡基线 (Credit Scoring Baseline)
目标：基于用户基本信息预测信用卡申请是否通过。
数据来源：Credit Card Approval Prediction (Kaggle)
数据许可证：CC0 1.0 Universal (Public Domain)
建模流程
数据预处理：处理缺失值（数值型填中位数，分类型填众数），统一标签编码。
特征工程：
使用卡方分箱 (ChiMerge) 对连续变量离散化。
计算 WOE (Weight of Evidence) 并进行编码。
基于 IV (Information Value) 筛选有效特征，剔除多重共线性。
模型训练：逻辑回归 (Logistic Regression)。
评估与转换：计算 KS/AUC，并将系数转换为标准评分卡格式。
📊 实验结果 (Baseline)
在当前数据集和特征工程策略下，模型表现如下：
AUC: 0.6762
KS Statistic: 0.3415
PSI: < 0.1 (训练集与测试集分布稳定)
结果分析与局限性：
该数据集经过高度脱敏处理，部分关键强特征（如具体收入数值、详细征信记录）缺失或模糊，导致模型区分度受限。AUC 0.67 和 KS 0.34 反映了在信息有限情况下的真实基线水平，而非代码错误。在实际生产环境中，通常需要引入更多外部数据源（如多头借贷、消费行为、设备指纹等）才能将 KS 提升至 0.4+。本项目重点在于展示完整的评分卡构建流程（WOE/IV/PSI）的可解释性优势。

### 🕵️ 实验二：反欺诈检测系统 (Fraud Detection System)
目标：在极度不平衡的交易数据中识别欺诈行为，并尝试挖掘关联关系。
数据来源：Credit Card Fraud Detection (Kaggle)
数据许可证：ODbL (Open Database License)
核心方法
无监督异常检测：
使用 Isolation Forest 和 Local Outlier Factor (LOF) 对全量数据进行初筛，不依赖标签即可发现离群点。
半监督学习 (Self-Training)：
利用少量已知欺诈样本作为种子，迭代标记高置信度的未知样本，缓解样本不平衡问题（正负样本比例约 1:580）。
图网络分析 (Graph Analysis)：
基于交易时间、金额相似度构建 KNN 图。
使用 networkx 计算连通分量，可视化潜在的欺诈团伙聚集区。
📊 实验观察
传统分类器在未经过采样处理时，Recall 低，recal=0.78。
引入半监督迭代后，对少数类（欺诈）的召回率有显著提升，recal=0.82。
图可视化清晰展示了异常点在局部图中的聚集效应，可辅助人工审核进行团伙排查。

## 🚀 如何运行
1. 准备数据
请手动下载以下数据集，并将其放入项目根目录的 data/ 文件夹中（或在 Notebook 中修改读取路径）：
评分卡	
Credit Card Approval Prediction
https://www.kaggle.com/datasets/rikdifos/credit-card-approval-prediction

反欺诈	
Credit Card Fraud Detection
https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud?spm=5176.28103460.0.0.38f97d83OoxTyl

2. 安装依赖
确保已安装 Python 3.8+，然后运行：

pip install -r requirements.txt

3. 运行 Notebook
启动 Jupyter Lab 或 Notebook，依次运行 notebooks/ 下的脚本：

jupyter notebook

注意：首次运行图网络分析部分可能需要几分钟进行计算。

##⚠️ 重要说明
关于模型分数：评分卡模型的 AUC (0.6762) 和 KS (0.3415) 是基於该特定公开数据集的真实结果。请勿将此视为模型能力的上限，实际业务中通过特征增强通常能获得更高收益。
图构建简化：反欺诈部分的图网络是基于相似度构建的模拟图。真实场景中需要基于设备指纹、IP、手机号等强关联字段构建异构图。

环境依赖：代码在 Python 3.8+ 环境下测试通过。

##📄 许可证
本项目代码: MIT License
使用的数据: 遵循各自数据集的开源协议 (CC0 / ODbL)
Last Updated: March 2024


