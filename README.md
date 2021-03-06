<a href="https://www.python.org/downloads/"><img  src="https://img.shields.io/badge/python-3.6%2B-brightgreen"></a>
<a href="https://github.com/pandas-dev/pandas"><img src="https://img.shields.io/badge/pandas-1.0.1-yellow"></a>
<a href="https://github.com/scipy/scipy"><img src="https://img.shields.io/badge/scipy-1.4.1-brightgreen"></a>
<a href="https://github.com/numpy/numpy"><img src="https://img.shields.io/badge/numpy-1.18.1-blue"></a>
<a href="https://github.com/statsmodels/statsmodels"><img src="https://img.shields.io/badge/statsmodels-0.11.0-red"></a>
<a href="https://pypi.org/project/PyQt5/"><img src="https://img.shields.io/badge/pyqt5-5.10-orange"></a>

## Introduction

### 功能介绍

- [x] 分位数Granger因果检验：计算各分位区间Sup-Wald统计量。

- [ ] 分位数VAR模型估计：自回归分布滞后模型
- [ ] 脉冲响应函数计算
- [ ] 各分位点脉冲图绘制

### 代码实现原理

* 使用`pyqt5`生成GUI界面

* 使用`statsmodels`进行分位数回归

* 使用`pandas`将结果保存在excel文件

## Display

### 主窗口界面

<div align=center><img src= "https://raw.githubusercontent.com/lei940324/picture/master/typora202004/03/224113-33109.png" width="450"></div>


主要包括：

- **工具栏**：

  * **导入数据**：点击该按钮，选择路径导入数据，如没有导入数据，默认导入测试数据        
  * **开始运行**：点击后进行分位数Granger因果检验，计算Sup-wald统计量   
    
  * **终止运行**：点击后终止程序          
  
  * **查看数据**：点击查看导入数据     
    
  * **初始化**：点击后初始化各参数设定    
  
  * **QVAR估计**：点击后进行QVAR模型参数设定    

- **区间设定**：区间**起点、中点、个数**设定以及**生成区间**按钮     

  > **注意**：个数是点的个数，比如设定17个，则会生成16个分位区间

  **例子：**

  起点=0.1，终点=0.9，个数=2，则会生成区间：[0.1, 0.9]

  起点=0.1，终点=0.9，个数=3，则会生成区间：[0.1, 0.5]、[0.5, 0.9]

- **参数设定**

  * **日期**：勾选表示第一列为日期序列，在进行计算wald统计量时会将其删除；若取消勾选，则不会删除第一列数据

  * **模式设定**：代表循环模式，默认模式为**单因素对各市场**

    >  **注意：数据要根据模式进行相应的排序**

    假定p=1，q=2，估计方程形式则为：Y=c<sub>1</sub>+c<sub>2</sub>Y<sub>-1</sub>+c<sub>3</sub>X<sub>-1</sub>+c<sub>4</sub>X<sub>-2</sub>

    |    模式选择    |                           内容说明                           |                      数据排序                      |                         计算规则描述                         |
    | :------------: | :----------------------------------------------------------: | :------------------------------------------------: | :----------------------------------------------------------: |
    | 单因素对各市场 | 研究单一因素对各市场的因果关系，比如房价对股票，汇率市场的因果关系 | data=[X,Y<sub>1</sub>,Y<sub>2</sub>,Y<sub>3</sub>] | X对Y<sub>1</sub>回归；X对Y<sub>2</sub>回归；X对Y<sub>3</sub>回归 |
    |    相互影响    | 研究两因素之间的因果关系，比如房价与股票，汇率市场的相互因果关系 | data=[X,Y<sub>1</sub>,Y<sub>2</sub>,Y<sub>3</sub>] | X对Y<sub>1</sub>回归；X对Y<sub>2</sub>回归；X对Y<sub>3</sub>回归；Y<sub>1</sub>对X回归；Y<sub>1</sub>对Y<sub>2</sub>回归;Y<sub>1</sub>对Y<sub>3</sub>回归;......... |
    | 多因素对单市场 | 研究多因素对单个市场的因果关系，比如各情绪对汇率市场的因果关系 | data=[X<sub>1</sub>,X<sub>2</sub>,X<sub>3</sub>,Y] | X<sub>1</sub>对Y回归，X<sub>2</sub>对Y回归；X<sub>3</sub>对Y回归 |

  * **信息准则**：表示确定最优滞后阶数所选用的信息准则，包括AIC或BIC准则

    AIC(p, q) = lnS(θ) + (p+q+1)/T

    BIC(p, q) = lnS(θ) + (p+q+1)×lnT/(2T)

    其中S(θ)表示分位数非对称绝对值残差和，T为样本容量，p,q均为滞后阶数。

  * **滞后估计数**：计算最优滞后阶数时，需要分区间计算AIC/BIC值，选取该区间最小AIC/BIC值所对应的滞后阶数，则为最优滞后阶数。该参数是设定在整个分位区间选择计算的分位点个数，默认选取30个点

  * **最大阶数**：表示选取的最大滞后阶数，默认选取5阶

  - **wald估计数**：计算wald统计量时，在分位区间选择计算的分位点个数，默认选取1000个点      
  - **有效数字**：表示保留的小数点位数，默认保留三位小数   
    
  - **输出日志**：勾选后输出**运行细节.txt**文件，默认勾选       

- **估计信息显示**：展示运行信息

### QVAR估计界面

<div align=center><img src= "https://raw.githubusercontent.com/lei940324/picture/master/typora202004/04/165304-871745.png" width="300"></div>


- **工具栏**：

  * **开始运行**：点击后进行QVAR模型估计        
  
  * **绘制脉冲图**：点击计算各分位点脉冲响应示意图   
  
  * **导入数据**：如需增加控制变量，则需点击该按钮导入控制变量数据集       

- **滞后阶数**

  * p：被解释变量y的滞后阶数

  * q：解释变量x的滞后阶数

- **分位点**：默认选取5个分位点[0.1, 0.25, 0.5, 0.75, 0.9]

  * √：点击增加分位点

  * ×：点击减少分位点

  > 注意：最多计算10个分位点

- **控制变量**：加入控制变量的回归公式

- **信息显示**：显示提示信息

## Usage

**第一步**：在当前路径下的命令行输入：

```shell
python main.py
```

> 提示：在当前文件夹中，右键点击cmd或者shell打开命令行

**第二步**：点击**导入数据**按钮

输入成功的话，会有导入成功的提示

**第三步**：设定各参数

**第四步**：点击**开始运行**按钮，等待程序运行结束，结果保存在**运行结果**文件夹下的**Granger.xlsx**文件内

## 项目目录

```
|-- Quantile
    |-- beauty_UI.py             // 美化GUI界面代码    
    |-- func.py                  // 主函数代码，定义分位数Granger因果检验计算    
    |-- main.py                  // 主程序       
    |-- README.md                // 说明文件        
    |-- data                     // 数据保存文件夹
    |   |-- output.xlsx          // 测试产生的结果文件   
    |   |-- Sup_wald_lag.xlsx    // 检验Sup_wald显著性文件   
    |   |-- 测试数据.xlsx         // 可以使用该文件进行测试，查看结果  
    |   |-- 运行细节.txt          // 测试日志  
    |-- pyqt5 界面               // 使用Qt Creator 建立窗口产生的文件
    |   |-- GUI                 // 主窗口
    |   |-- child_GUI           // 子窗口
    |-- 运行结果                 // 运行结果存储文件夹
```

> 代码主体为**main.py**与**func.py**文件

## 估计原理

### 使用函数介绍

使用`statsmodels`库进行分位数回归命令：

1)  使用R型公式来拟合模型

| **formula** | **说明**                               | **示例**             |
| ----------- | -------------------------------------- | -------------------- |
| ~           | 分隔符，左边为响应变量，右边为解释变量 | y ~ x                |
| +           | 添加变量                               | y ~ x1 + x2          |
| -           | 移除变量                               | y ~  x - 1(移除截距) |

2)  参数命令

| **属性**   | **说明**       | **方法**             | **说明**       |
| ---------- | -------------- | -------------------- | -------------- |
| res.params | 获取估计参数值 | res.summary()        | 展示估计结果   |
| res.bse    | 获取标准差     | res.cov_params()     | 获取协方差矩阵 |
| res.resid  | 获取残差       | res.f_test("x2 = 0") | Wald检验       |



### 分位数Granger因果检验实现原理

由于github很难展示latex格式的公式，可以[click这里](https://blog.csdn.net/u013289615/article/details/105450785)进行查看。



## Development Tool

|       工具名       |     功能      |                           图标icon                           |                           官网下载                           |
| :----------------: | :-----------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|     Qt Creator     | GUI界面可视化 | <img src= "https://raw.githubusercontent.com/lei940324/picture/master/typora202003/31/182029-164220.png" width="50" align="absmiddle"> | [click](http://download.qt.io/official_releases/qtcreator/)  |
|      PyCharm       |  代码编辑器   | <img src= "https://raw.githubusercontent.com/lei940324/picture/master/typora202003/31/182340-937174.png" width="50" align="absmiddle"> | [click](https://www.jetbrains.com/pycharm/download/#section=windows) |
| Visual Studio Code |  代码阅读器   | <img src= "https://raw.githubusercontent.com/lei940324/picture/master/typora202004/14/193013-466582.png" width="50" align="absmiddle"> |           [click](https://code.visualstudio.com/)            |



## reference

* **书籍：**
  * 《Python Qt GUI与数据可视化编程》

  * 《陈强高级计量经济学》

* **文献：**
  * Koenker & Machado1999 Inference QuantileReg

  * Asymmetric Least Squares Estimation and Testing

  * Tests-for-Parameter-Instability-and-Structural-Change-With-Unknown-Change-Point

  * 房地产价格与汇率的联动关系研究———基于分位数 Granger 因果检验法

  * 基于分位数Granger因果的网络情绪与股市收益关系研究

* **其他:**

  * Eviews 8帮助文件

  * 张晓峒分位数回归讲义

## License

[MIT](https://github.com/lei940324/Quantile/blob/master/LICENSE) © 热心市民石头