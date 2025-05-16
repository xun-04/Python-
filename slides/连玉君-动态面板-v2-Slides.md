---
marp: true
size: 16:9        # 宽版：4:3
paginate: true  
footer: '连享会 &emsp; [lianxh.cn](https://www.lianxh.cn)'
---

<style>
/*一级标题局中*/
section.lead h1 {
  text-align: center; /*其他参数：left, right*/
}
section {
  font-size: 22px;      /* 正文字号 */
}
h1 {
  color: blackyellow;   /* 标题的颜色 */
  /*font-size: 28px; */ /* 标题的字号, 其它标题也可以这样修改 */
}
h2 {
  color: green;
}
h3 {
  color: darkblue;
}
h4 {
  color: brown;
}
/* 右下角添加页码 */
section::after {
  content: attr(data-marpit-pagination) '/' attr(data-marpit-pagination-total); 
}
header,
footer {
  position: absolute;
  left: 50px;
  right: 50px;
  height: 25px;
}
</style>

<!--顶部文字-->
[lianxh.cn](https://www.lianxh.cn/news/46917f1076104.html) 

<br>

<!--封面图片-->
![bg right:50% w:400 brightness:. sepia:50%](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220722114227.png) 

<!--幻灯片标题-->
# 动态面板数据模型

<br>
<br>

<!--作者信息-->
[连玉君](https://www.lianxh.cn) (中山大学)
arlionn@163.com

<br>



--- ---

### 提纲

- 简介：应用场景
- 模型设定
- 估计方法
  - FD-GMM；SYS-GMM；纠偏 OLS
- 假设检验：序列相关检验；过度识别检验
- Stata 实操 1：模型估计
  - OLS, FE, IV, 2SLS, GMM 对比
  - 简单模拟分析
- 实证分析中的主要陷阱
  - 至少需要几年的数据？
  - 要做哪些检验？一直通不过怎么办？
  - 工具变量太多怎么办？

${\color{white}{a}}$

--- - --


### 简介：我的体重 1

$
(1) \quad y_{t} = \rho y_{t-1} + \varepsilon_{t} 
$

$
(2) \quad y_{t-1} = \rho y_{t-2} + \varepsilon_{t-1}
$

将  (2) 带入 (1):

$
(3) \quad y_{t} = \rho^{2} y_{t-2} + \rho\varepsilon_{t-1} + \varepsilon_{t}
$

$\cdots$

$
(4) \quad y_{t} = \rho^{40} y_{t-40} + \rho^{39}\varepsilon_{it-39} +  \cdots + \rho\varepsilon_{it-1} +\varepsilon_{t}
$

- **Q1：** $\text{Corr}(y_{it},y_{it-2})=0\ ?$, $\text{Corr}(y_{it},y_{it-4})=0\ ?$ 
- **Q2：** $\color{red}{\rho}$ 的含义是什么？

--- - --

### 简介：我的体重 2

$
y_{t} = \rho y_{t-1} + \varepsilon_{t} \qquad \qquad\qquad\qquad\qquad\qquad\qquad\qquad\qquad\quad\ \ (1) \quad
$

<br>

$
y_{t} = \alpha + \rho y_{t-1} + x_{t}\beta + \varepsilon_{t}  \qquad\qquad\qquad\qquad\qquad\qquad\qquad\quad (1a) 
$

<br>

$
y_{it} = \alpha_i + \rho y_{it-1} + x_{it}\beta + \theta_t + \varepsilon_{it}  \quad\qquad\qquad\qquad\qquad\qquad\quad (1b) 
$ 

<br>

$
y_{it} = \alpha_i + \rho y_{it-1} + x_{it-1}\beta_1 + x_{it-2}\beta_2+ \theta_t + \varepsilon_{it}  \qquad\qquad\qquad\ (1c) 
$

<br>
<br>
<br>


--- - --


### 扩展：
- 消费行为的惯性
- 投资行为
  - Carstensen K, Toubal F. Foreign direct investment in Central and Eastern European countries: a dynamic panel analysis. Journal of Comparative Economics, 2004, 32(1): 3-22.
- 能源消费与经济增长 PVAR
  - Huang B N, Hwang M J, Yang C W. Causal relationship between energy consumption and GDP growth revisited: a dynamic panel data approach. Ecological Economics, 2008, 67(1): 41-54.
- 综述
  - Bond S R. Dynamic panel data models: A guide to micro data methods and practice. Portuguese Economic Journal, 2002, 1(2): 141-162. (Cited 3000+)

--- - --

### 简介：部分调整模型 &rarr; 动态面板模型

- 公司资本结构部分调整 (动态权衡理论)
 
$$
y_{it} - y_{it-1} = \lambda(y^*_{it}-y_{it-1}) \quad (5)
$$

- $\lambda$: 调整速度；$y^*_{it}$: 目标负债率；eg. 
  - $\bar{y}_i$ - 公司过去几年的平均负债率; 
  - $\bar{y}_j (j=1,2,\cdots,J)$ 行业均值


文献中通常将 $y_{it}^{*}$ 设定为影响公司负债率的一系列因素的线性组合：

$$
y^*_{it} = x_{it}\phi + \eta_i  + u_{it} \quad (6)
$$

将 (6) 带入 (5)，可得：

$$
y_{it} = \rho y_{it-1} + x_{it}\beta + \alpha_i + \varepsilon_{it}
$$

- $\rho = 1-\lambda$； $\beta=\lambda \phi$； $\alpha_i =\lambda \eta_i$；  $\varepsilon_{it} =\lambda u_{it}$


--- - --

### 资本结构动态调整文献

- Flannery, M. J., K. P. Rangan, 2006, Partial adjustment toward target capital structures, Journal of Financial Economics, 79 (3): 469-506. [[PDF]](https://www.jianguoyun.com/p/DTocOmUQtKiFCBipt9gC)
- 姜付秀, 黄继承. 市场化进程与资本结构动态调整[J]. 管理世界, 2011, 3: 124-134.
- 王朝阳, 张雪兰, 包慧娜..经济政策不确定性与企业资本结构动态调整及稳杠杆, 2018, 经济政策不确定性与企业资本结构动态调整及稳杠杆, 中国工业经济, (12): 134-151.
- 黄俊威, 龚光明. 融资融券制度与公司资本结构动态调整——基于 "准自然实验" 的经验证据[J]. 管理世界, 2019 (10).
- 刘贯春, 刘媛媛, 闵敏. 经济金融化与资本结构动态调整[J]. 管理科学学报, 2019 (03): 71-89.

--- - --

### 估计方法面临的挑战

- 内生性：来源？
- 如何选择工具变量？
  - 外生性 + 相关性
- 如何检验工具变量的合理性？
  - 干扰项序列相关 `estat abond`
  - 过度识别检验 `estat sargan`
- 不同估计方法的优劣
  - POLS, FE, IV-2SLS `regress`, `xtreg`, `xthdfe`, `xtivregress`, `xtivreg2`
  - FD-GMM，SYS-GMM `xtabond`, `xtabond2`, `xtdpd`, `xtdpdsys`
  - 纠偏 OLS: 小样本 `xtbcfe`, `xtlsdvc`
  - ML-SEM: 反向因果 `xtdpdml`
  - 小样本、高粘性 (High persistence)



--- - --


## 模型设定

特征：解释变量中包含了 $y_{i,t-1}$，${\color{red}{\gamma}}$ 是分析重点

$$
  y_{it} = {\color{red}{\gamma}} y_{i,t-1} + x_{it}'\beta + \alpha_i + \varepsilon_{it} \quad (1)
$$

**基本假设：**

- **H1: 无序列相关** $\text{Corr}(\varepsilon_{it}, \varepsilon_{it-1})=0$
- **H2: 外生性** &ensp; &ensp;  $E(\varepsilon_{it}\ |\ y_{it-1}, x_{it}, \alpha_i)=0$

<br>
<br>
<br>
<br>

--- - --


### 递归特征

为了便于说明，我们先不考虑外生变量 $x_{it}$，重点分析如下简化模型：

$$
  y_{it} = \gamma y_{i,t-1} + \alpha_i + \varepsilon_{it}, \quad
   \quad (2)
$$

依据该模型的设定，我们可以将 $y_{i,t-1}$ 表示为：

$$
  y_{i,t-1} = \gamma y_{i,t-2} + \alpha_i + \varepsilon_{i,t-1} \quad (3)
$$

把 (3) 带入 (2)，发现这是一个递归的表达:

$$
y_{it} = \gamma^2 y_{i,t-2} + \underline{\gamma\alpha_i + \alpha_i} + \underline{\gamma\varepsilon_{it-1} + \varepsilon_{it}}
$$


- $\text{Corr}(y_{it-s},\alpha_i) \neq 0 (s\geq 0)$
- $\text{Corr}(y_{it},y_{it-s}) \neq 0 (s \geq 0)$ &emsp; **Q:** 有何好处？
- $\alpha_i$ 和 $\varepsilon_{it-s}$ 都持续对 $y_{it}$ 产生影响，但具有记忆衰减。


--- - --

## 估计 - 1：FD 去掉 $\alpha_i$ 

- 水平方程：$y_{it} = {\gamma} y_{i,t-1} + x_{it}'\beta + {\color{red}{\alpha_i}} + \varepsilon_{it} \qquad\quad (1)$

$\qquad\quad\qquad y_{it-1} = {\gamma} y_{i,t-2} + x_{it-1}'\beta + {\color{red}{\alpha_i}} + \varepsilon_{it-1} \quad (1a)$

- 差分方程：$\Delta y_{it} = {\gamma} \Delta y_{i,t-1} + \Delta x_{it}'\beta + \sout{{\color{red}{\alpha_i}}} + \Delta\varepsilon_{it} \quad (2)$

### 如何估计？
- 有没有内生性问题？ 
  - $\text{Corr}(\Delta x_{it},\Delta\varepsilon_{it})=0$
  - $\text{Corr}(\Delta y_{it-1},\Delta\varepsilon_{it}) \neq 0$， Why?
<br>
<br>



--- - --

## 估计 - 2a：寻找 IV

$$
  \Delta y_{it} = {\gamma} \Delta y_{i,t-1} + \Delta x_{it}'\beta  + \Delta\varepsilon_{it} \quad (2) 
$$

- $\Delta y_{i,t-1} = y_{it-1} - y_{it-2}$
<br>
- $\quad \Delta \varepsilon_{it} = \varepsilon_{it} - \varepsilon_{it-1}$

<br>
<br>
<br>
<br>
<br>
<br>
<br>



--- - --


## 估计 - 2b：寻找 IV

$$
  \Delta y_{it} = {\gamma} {\color{red}{\Delta y_{i,t-1}}} + \Delta x_{it}'\beta  + \Delta\varepsilon_{it} \quad (2) 
$$

### IVs
- $IV_1 = y_{it-2}$ &rarr; $E(y_{it-2}\cdot\Delta\varepsilon_{it})=0$ 
$\cdots$
- $IV_s = y_{it-s}$ &rarr; $E(y_{it-s}\cdot\Delta\varepsilon_{it})=0$
- Q：还有别的 IV 吗？

<br>
<br>
<br>
<br>

(Next Page: GMM)

--- - --

### Notes：IV-2SLS-GMM 

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>



--- - --




--- - --




## Notes 1：何谓 IV ？

$$y_{i} = x_{i}\beta + \varepsilon_{i}$$

**外生性假设：**

- Moment Condition (**MC**): $E(x_{i}\varepsilon_{i})=0$ &emsp; or &emsp; $\text{Corr}(x_{i},\varepsilon_{i})=0$
- Sample MC (**SMC**): 
$$
\frac{1}{N}\sum_{i=1}^{N}\left\{x_{i}(y_{i}-x_{i}\beta)\right\}=0
$$

- **IV** 估计：Stata 实现

```stata
sysuse "nlsw88.dta", clear
gen black = (race==2)
ivregress 2sls wage (tenure = black) hours i.industry
```

--- - --

## Notes 2：何谓 2SLS ？

**如果** $E(x_{i}\varepsilon_{i})\neq 0$，但 $E(z_{1}\varepsilon)=0, E(z_{2}\varepsilon)=0$，

$
\frac{1}{N}\sum_{i=1}^{N}\left[z_{1i}(y_{i}-x_{i}\beta)\right]=0
$

$
\frac{1}{N}\sum_{i=1}^{N}\left[z_{2i}(y_{i}-x_{i}\beta)\right]=0
$

<br>

- 两阶段最小二乘 (**2SLS**)：Stata 实现

```stata
sysuse "nlsw88.dta", clear
gen white = (race==1)
gen black = (race==2)
ivregress 2sls wage (tenure=white black) hours i.industry
```

--- - --

## Notes 3：何谓 GMM ？

**如果** $E(x_{i}\varepsilon_{i})\neq 0$，但 
- **MC：** $E(z_{1}\varepsilon)=0, E(z_{2}\varepsilon)=0$，

- **SMC：** $\qquad \frac{1}{N}\sum_{i=1}^{N}\left[z_{1i}(y_{i}-x_{i}\beta)\right]=g_1$

$\qquad \qquad \qquad \frac{1}{N}\sum_{i=1}^{N}\left[z_{2i}(y_{i}-x_{i}\beta)\right]=g_2
$

- **GMM**：Stata 实现 (also see `help gmm`)

```stata
sysuse "nlsw88.dta", clear
gen white = (race==1)
gen black = (race==2)
ivregress gmm wage (tenure=white black) hours i.industry
```

--- - --

$$
我们又回来了！……
$$


--- - --




## 估计 - 3a - IV-2SLS 估计 `Anderson and Hisao (1981)`

- 水平方程：$\quad y_{it} = {\gamma} y_{i,t-1} + x_{it}'\beta + {\alpha_i} + \varepsilon_{it} \qquad\   (1)$
- 差分方程：$\ \Delta y_{it} = {\gamma} \Delta y_{i,t-1} + \Delta x_{it}'\beta + \Delta\varepsilon_{it} \qquad (2)$
- $H_0: \text{Corr}(\varepsilon_{it}, \varepsilon_{it-1})=0$
- $\text{IVs}$: $\ y_{it-2}, \Delta y_{it-2}$  

$$
\begin{aligned}
E\left(y_{i, t-2} \Delta \varepsilon_{i t}\right) &=0 \\
E\left(\Delta y_{i, t-2} \Delta \varepsilon_{i t}\right) &=0
\end{aligned}
$$

Stata 实操：
```stata
use "xtdyData.dta", clear
ivreg2 D.y (D.L.y = L2.y D.L2.y) x , small cluster(ivar)
```
$\quad\quad  \Delta y_{it} = {\gamma} \Delta y_{i,t-1} + \Delta x_{it}'\beta + \Delta\varepsilon_{it}$


--- - --

### Stata 实操：对比 - codes

```stata
  use "xtdyData.dta", clear
  
  ivreg2 D.y (D.L.y = L2.y),        small cluster(ivar)
  est store IV1
  ivreg2 D.y (D.L.y = L2.y D.L2.y), small cluster(ivar)
  est store IV2
  ivreg2 D.y (D.L.y = L2.y D.L2.y), small cluster(ivar) gmm2s first 
  est store gmm
  
  *-----结果-------
  local m "IV1 IV2 gmm"
  esttab `m' `s', nogap compress replace  ///
         b(%6.3f) s(N r2 ll)               ///
         star(* 0.1 ** 0.05 *** 0.01) 
```

--- - --



### Stata 实操：对比 - results
差分方程：$\ \Delta y_{it} = {\gamma} \Delta y_{i,t-1} + \Delta x_{it}'\beta + \Delta\varepsilon_{it} \qquad (2)$

$\text{IVs}$: $\quad {\color{blue}{(1)}}\ y_{it-2}$; $\quad {\color{blue}{(2)-(3)}}\ y_{it-2}, \Delta y_{it-2}$

```stata
-------------------------------------------------
D.y             (1)          (2)          (3)     
-------------------------------------------------
LD.y           0.580***     0.600***     0.606***
             (46.27)      (39.26)      (50.01)   
D.x            1.292***     1.310***     1.312***
            (129.42)     (111.96)     (121.05)   
_cons         -0.002       -0.008       -0.008   
             (-0.39)      (-1.33)      (-1.31)   
-------------------------------------------------
N           8000.000     7000.000     7000.000   
r2             0.682        0.676        0.675   
ll          -1.4e+04     -1.2e+04     -1.2e+04   
-------------------------------------------------
t statistics in parentheses；* p<0.1, ** p<0.05, *** p<0.01
```

--- - --

### 评价和思考

$$\Delta y_{it} = {\gamma} {\color{red}{\Delta y_{i,t-1}}} + \Delta x_{it}'\beta + \Delta\varepsilon_{it}$$

- 其一，用 $y_{i,t-2}$ 做 IV，至少需要 ? 年的观察值;&emsp;  $\Delta y_{i,t-2}$ 呢？

- 其二，若用 $\Delta y_{i,t-2}$ 作 IV，会导致 $se(\hat{\gamma})$ 异常增大 (Arellano, 1989)。因此，Arellano 建议采用 $y_{i,t-2}$ 做 IV。但这个可能又面临一个弱工具变量问题。

- 自 Hansen (1982) 提出 GMM 后，上述 IV 估计量很少用于估计动态面板模型。例如，Ahn and Schmidt ([1995](https://www.jianguoyun.com/p/DbZS3CIQtKiFCBj-q9gC )) 认为，虽然上述 IV 估计量是一致的，但并非最有效的，因为二者都未充分利用所有可用的矩条件，同时，IV 估计量也未充分考虑差分后的干扰项 ($\Delta \varepsilon_{it}$) 的异方差特征。**Note:** 若假设 $\varepsilon_{it}\sim i.i.d.(0, \sigma_{\varepsilon}^2)$，则 $\Delta\varepsilon_{it}$ 不再服从 $i.i.d.$ 分布。


--- - --

$$
\text{FD-GMM} 
$$

$$
 Arellano\ and\ Bond\ (1991)
$$


--- - --

###  估计 3b-1：FD-GMM - 基本思路 `Arellano and Bond (AB, 1991)`

$$
  \Delta y_{it} = {\gamma} {\color{red}{\Delta y_{i,t-1}}} + \Delta x_{it}'\beta  + \Delta\varepsilon_{it} \quad (2) 
$$

- $IV_1 = y_{it-2}$ &rarr; $E(y_{it-2}\cdot\Delta\varepsilon_{it})=0$ 
- $IV_2 = y_{it-3}$ &rarr; $E(y_{it-3}\cdot\Delta\varepsilon_{it})=0$ 
$\cdots$ (好多！)

<br>
<br>
<br>
<br>
<br>
<br>

--- - --

###  估计 3b-2：FD-GMM - 详解 1 - 矩条件：可变的 IV


- $t=3$ 时，我们能获取的第一期观察值为：
    
$$
  y_{i3}-y_{i2} = \gamma (y_{i2}-y_{i1}) + (\varepsilon_{i3}-\varepsilon_{i2})$$

显然，此时 $y_{i1}$ 是合理的 IV。因为，
- $\text{Corr}(\varepsilon_{it}, \varepsilon_{it-1})=0 \Rightarrow$ $y_{i1}$ 与 $(\varepsilon_{i3}-\varepsilon_{i2})$ 不相关，而 $y_{i1}$ 与 $(y_{i2}-y_{i1})$ 则是高度相关的。我们可以将上述分析归结为如下矩条件：

$$
   E[y_{i1}(\varepsilon_{i3}-\varepsilon_{i2})] = 0
$$

<br>
<br>
<br>


--- - --


###  估计 3b-2：FD-GMM - 详解 1 - 矩条件：可变的 IV


- $t=4$ 时，我们能获取的第一期观察值为：
$$
  y_{i4}-y_{i3} = \gamma (y_{i3}-y_{i2}) + (\varepsilon_{i4}-\varepsilon_{i3})
$$   

此时，$y_{i2}$ 和 $y_{i1}$ 都是 $(y_{i3}-y_{i2})$ 的合理工具变量，可得如下两个矩条件：
$$
   E[y_{i1}(\varepsilon_{i4}-\varepsilon_{i3})] = 0
$$
$$
   E[y_{i2}(\varepsilon_{i4}-\varepsilon_{i3})] = 0
$$

显然，上述分析思路可以一直延续到样本中的所有观察区间，随着 $t$ 的增加，工具变量的数目也会逐渐增加，当 $t=T$ 时，合理的工具变量集为 $(y_{i1}, y_{i2},\cdots, y_{i,T-2})$。


--- - --

###  估计 3b-2：FD-GMM - 详解 2 - 矩条件：工具变量矩阵

相比于上一节介绍的 IV 估计量，GMM 的主要差异在于可以为不同的观察期设定不同的工具变量集。因此，对于个体 $i$ 而言，各期的工具变量集合可汇总为如下矩阵：
    
$$
Z_i = \left[ \begin{array}{cccc}
            [y_{i1}]  & 0               &\cdots & 0          \\
            0         & [y_{i1},y_{i2}] &\cdots & 0          \\
            \vdots    & \vdots          &\ddots & \vdots     \\
            0         & 0               &\cdots & [y_{i1},y_{i2},\cdots,y_{i,T-2}] \\
            \end{array}
     \right]$$

#### 概括一下

> - 对于差分方程 $\Delta y_{it} = {\gamma} {\color{red}{\Delta y_{i,t-1}}} + \Delta x_{it}'\beta  + \Delta\varepsilon_{it}$ 而言，$y_{it}$ 的所有两阶以及两阶以上滞后项都可以作为 ${\color{red}{\Delta y_{it-1}}}$ 的工具变量。**Stata:** `L(2/.).y`
> - **Q:** 能否应用于非平行面板 ？


--- - --

###  估计 3b-2：FD-GMM - 详解 2 - 矩条件：工具变量矩阵

$$
Z_i = \left[ \begin{array}{cccc}
            [y_{i1}]  & 0               &\cdots & 0          \\
            0         & [y_{i1},y_{i2}] &\cdots & 0          \\
            \vdots    & \vdots          &\ddots & \vdots     \\
            0         & 0               &\cdots & [y_{i1},y_{i2},\cdots,y_{i,T-2}] \\
            \end{array}
     \right] \qquad (3a)
$$

设 $p=1$，则 $Z_i$ 行数：$T\!-p\!-1$，列数：$\sum_{m=p}^{T-2}m$ 列 (这也是矩条件的数目)。

样本中所有个体工具变量构成的矩阵为：
     
$$
  \mathbf{Z}=[Z_1',Z_2',\cdots,Z_N']' \qquad (3b)
$$ 

上述矩条件简写如下：

$$
  E(Z_i' \Delta \varepsilon_i) = \mathbf{0}
$$ 


--- - --

###  估计 3b-2：FD-GMM - 详解 3 - 目标函数

为了推导出 GMM 估计量，可将该矩条件重新表述为对应的样本矩条件：
    
$$
  E[Z_i' (\Delta y_{i} - \gamma \Delta y_{i,-1})] = \mathbf{0} \qquad (4)
$$ 

由于工具变量的数目可能远远多于未知参数的个数，我们无法保证上述矩条件严格等于零，此时需要极小化如下目标函数：
    
$$
 \widehat{\beta}_{GMM}=\arg  \underset{\gamma}{\text{min}}
  \left[
      \frac{1}{N} \sum_{i=1}^{N} (Z_i' (\Delta y_{i} - \gamma \Delta y_{i,-1}))
  \right]'
    \mathbf{W}_N
  \left[
      \frac{1}{N} \sum_{i=1}^{N} (Z_i' (\Delta y_{i} - \gamma \Delta y_{i,-1}))
  \right]
$$ 

其中，$\mathbf{W}_{\!N}$ 是一个对称且正定的权重矩阵。需要说明的是，$\mathbf{W}_{\!N}$ 的下标 $N$ 仅表明该权重矩阵依赖于样本数 $N$，并非用于标示矩阵的维度。
<br>



--- - --

###  估计 3b-2：FD-GMM - 详解 3 - 目标函数

$$
 \widehat{\beta}_{GMM}=\underset{\gamma}{\text{min}}
  \left[
      \frac{1}{N} \sum_{i=1}^{N} (Z_i' (\Delta y_{i} - \gamma \Delta y_{i,-1}))
  \right]'
    \mathbf{W}_N
  \left[
      \frac{1}{N} \sum_{i=1}^{N} (Z_i' (\Delta y_{i} - \gamma \Delta y_{i,-1}))
  \right] \quad (5)
$$ 

对 $\gamma$ 求一阶偏导数，可以得到 $\gamma$ 的 GMM 估计量：

$
\begin{aligned}
\hat{\gamma}_{GMM}=&\left[\left(\sum_{i=1}^{N} \Delta \mathbf{y}_{i,-1}^{\prime} Z_{i}\right) \mathbf{W}_{N}\left(\sum_{i=1}^{N} Z_{i}^{\prime} \Delta \mathbf{y}_{i,-1}\right)\right]^{-1} \times \\
&\left[\left(\sum_{i=1}^{N} \Delta \mathbf{y}_{i,-1}^{\prime} Z_{i}\right) \mathbf{W}_{N}\left(\sum_{i=1}^{N} Z_{i}^{\prime} \Delta \mathbf{y}_{i}\right)\right]
\end{aligned} \qquad\qquad (6)
$

> 看看这个简化版：$
\hat{\gamma}_{GMM} = 
[\mathbf{A} \mathbf{W}_{\!N} \mathbf{A}']^{-1} \times [\mathbf{A} \mathbf{W}_{\!N} \mathbf{A}']
$


--- - --


#### 特例：2SLS 估计量 $(\mathbf{W}_N=\mathbf{I})$ 

$\hat{\gamma}_{GMM}$ 的性质决定于 $\mathbf{W}_N$。若 $\mathbf{W}_N=\mathbf{I}$，则 $\hat{\gamma}_{GMM}$ 转化为 **IV/2SLS** 估计量：

$$
\hat{\gamma}_{2SLS}=\left[\Delta \mathbf{y}_{-1}^{\prime} \mathbf{Z}\left(\mathbf{Z}^{\prime} \mathbf{Z}\right)^{-1} \mathbf{Z}^{\prime} \Delta \mathbf{y}_{-1}\right]^{-1}\left[\Delta \mathbf{y}_{-1}^{\prime} \mathbf{Z}\left(\mathbf{Z}^{\prime} \mathbf{Z}\right)^{-1} \mathbf{Z}^{\prime} \Delta \mathbf{y}\right] 
                          \quad (7)
$$

虽然当 $N\rightarrow \infty, T\rightarrow \infty$ 时，$\hat{\gamma}_{IV}$ 是 $\hat{\gamma}$ 的渐进一致性估计量，但该估计量并未考虑干扰项的方差结构，因此并不是 $\hat{\gamma}$ 的有效估计量 (即，估计系数的标准误比较大)。

<br>
<br>
<br>
<br>
<br>


--- - --



###  估计 3b-2：FD-GMM - 详解 4 - 最优权重矩阵 (&#x1F34F;)

小样本下，须慎重选择 $\mathbf{W}_N$ 以便使 $\text{Var} (\hat{\gamma}_{GMM})$ 最小。选择 $\mathbf{W}_N$ 的基本条件：

$$
  \underset{N\rightarrow \infty}{\text{plim}} \mathbf{W}_N
  = \text{Var}(Z_i' \Delta \varepsilon_i)^{-1}
  = E(Z_i' \Delta \varepsilon_i \Delta \varepsilon_i'Z_i)^{-1} \quad (8)
$$ 

这意味着，只要能获得 $\gamma$ 的一致估计量，即可获得最优权重的估计值：

$$
  \widehat{\mathbf{W}}_N^{opt} =
  \left(
    \frac{1}{N} \sum_{i=1}^{N} Z_i' \Delta\hat{\varepsilon}_i
    \Delta\hat{\varepsilon}_i' Z_i
  \right)^{-1} \quad \quad (9)
$$ 


例如，我们可以选择 $\mathbf{W}_N=\mathbf I$，得到 $\hat{\gamma}_{2SLS}$ 后，进一步得到 $\Delta\hat{\varepsilon}_i$，由此计算出 $\widehat{\mathbf{W}}_N^{opt}$ 后重新代入 $\hat{\gamma}_{GMM}$ 估计式，即可得到 $\hat{\gamma}_{GMM}$ 的有效估计量。然而，这一处理方法并未考虑 $\Delta {\varepsilon}_i$ 的方差结构，因此，并非最佳选择。

--- - --

###  估计 3b-2：FD-GMM - 详解 5 - 一步估计法与两步估计法 (&#x1F34E; )

- **一步估计量 $\hat{\gamma}_{1}$**。利用工具变量集 $\mathbf{Z}$，见 $(3)$ 式，并采用 GLS 估计差分模型 (2)，从而得到 $\gamma$ 的估计值，即为 $\hat{\gamma}_{1}$。
- **获取一步估计的残差：** 基于 $\hat{\gamma}_{1}$ 获取残差 $\Delta \hat{\varepsilon}_{i}=\Delta \mathbf{y}_{i}-\hat{\gamma}_{1} \Delta \mathbf{y}_{i,-1}$。

- **两步估计量 $\hat{\gamma}_{2}$**。将 $\Delta \hat{\varepsilon}_{i}$ 代入(9) 式，得到最优权重矩阵 $\widehat{\mathbf{W}}_N^{opt}$，进而设 $\mathbf{W}_N=\widehat{\mathbf{W}}_N^{opt}$，由 (6) 式估计出 $\gamma$ 的两步估计量 $\hat{\gamma}_{2}$。

--- - --


###  估计 3b-2：FD-GMM - 详解 5 - 一步估计法与两步估计法 (&#x1F34E; 白话版 )

- 差分方程：$\ \Delta y_{it} = {\gamma} \Delta y_{i,t-1} + \Delta x_{it}'\beta + \Delta\varepsilon_{it} \qquad (2)$
- 目标函数：$\widehat{\beta}_{GMM}=\underset{\gamma}{\text{min}}
  \left[
      \frac{1}{N} \sum_{i=1}^{N} Z_i' \Delta \varepsilon_{i}
  \right]'
    \mathbf{W}_N
  \left[
      \frac{1}{N} \sum_{i=1}^{N} Z_i' \Delta \varepsilon_{i}
  \right] \quad (5)$
 
- 权重：$\mathbf{W}_N
  = \text{Var}(Z_i' \Delta \varepsilon_i)^{-1}
  = E(Z_i' \Delta \varepsilon_i \Delta \varepsilon_i'Z_i)^{-1} \quad (8)$ 

- **一步法：** 用 $y_{it}$ 的所有两阶以及两阶以上滞后项, `L(2/.).y` 作 ${\color{red}{\Delta y_{it-1}}}$ 的工具变量，使用 GLS 估计 (2)，即可得到 $\hat{\gamma}_{1}$，进而得到对应的残差 $\Delta \hat{\varepsilon}_{it}$。
- **两步法：** 把 $\Delta \hat{\varepsilon}_{it}$ 带入 (8) 式，用 (5) 式得到 GMM 估计量，即 $\hat{\gamma}_{2}$。
- **Q：** 有什么区别？对实操有何启示？
  - 参见 `FD-GMM 应用指南 3 - 过度识别检验` (后文)


--- - --


### 补充：AB91 一步估计量 (One step estimator，略)
此前，我们提到了 Anderson and Hisao ֥(1981) 提出的 IV-2SLS 估计量 $(\mathbf{W}_N=\mathbf{I})$：

$$
\hat{\gamma}_{2SLS}=\left[\Delta \mathbf{y}_{-1}^{\prime} \mathbf{Z}\left(\mathbf{Z}^{\prime} \mathbf{Z}\right)^{-1} \mathbf{Z}^{\prime} \Delta \mathbf{y}_{-1}\right]^{-1}\left[\Delta \mathbf{y}_{-1}^{\prime} \mathbf{Z}\left(\mathbf{Z}^{\prime} \mathbf{Z}\right)^{-1} \mathbf{Z}^{\prime} \Delta \mathbf{y}\right] 
                          \quad (7)
$$

AB 91 在此基础上进行了两个改进，得到了所谓的「**一步估计量**」：
- 使用了更多工具变量。
  - **IVs**: `L(2/.).y`，即 $(3a)$ 和 $(3b)$ 两式对应的 $\mathbf{Z}$ 矩阵。
- 采用 **GLS** 对模型进行估计，因为一阶差分方程 (2) 的干扰项 $\Delta \varepsilon_{it}$ 不再满足同方差假设 ($\text{FD}$ 变换会引入序列相关问题。)
- 这是 Stata 中 `xtabond` 命令的默认估计方法。详情参见 [连玉君-面板数据模型](https://www.jianguoyun.com/p/DQKxR6AQtKiFCBjO2dgC)。


--- - --

## 系统 GMM (SYS-GMM, [AB95](https://www.jianguoyun.com/p/DVpGebsQtKiFCBi-5tgC ), [BB98](https://www.jianguoyun.com/p/DYTwRpYQtKiFCBi45tgC))


 水平方程：$\quad y_{it} = {\gamma} y_{i,t-1} + x_{it}'\beta + {\alpha_i} + \varepsilon_{it} \qquad\   (1)$

 差分方程：$\ \Delta y_{it} = {\gamma} \Delta y_{i,t-1} + \Delta x_{it}'\beta + \Delta\varepsilon_{it} \qquad (2)$

- **基本思想**
  - 当 $\gamma \rightarrow 1$ 时，$\Delta y_{it-1}$ 与 $y_{it-2}$ 的相关性很低，导致弱工具变量问题
  - 此时，可以用 $\Delta y_{it-1}$ 作为 (1) 式水平方程中 $y_{it-1}$ 的工具变量。
  - 把两个方程对应的所有 **矩条件** 写成一个大的 $\mathbf{Z}$ 矩阵，执行 GMM 估计。 
- **评价**
  - **适用情形：** $\gamma \rightarrow 1$
  - **主要局限：** 工具变量太多 (弱)；Sargan test 和序列相关检验经常无法通过
  - **替代方法：** ML-SEM (`xtdpdml`)；`xtdpdgmm`
> System-GMM requires the mean stationarity assumption for consistency. 


--- - --

### 序列相关检验 [[AB91]](https://www.jianguoyun.com/p/Df3hiZYQtKiFCBjb4dgC)

- 水平方程：&emsp; $y_{it} = {\gamma} y_{i,t-1} + x_{it}'\beta + \alpha_i + \varepsilon_{it} \quad (1)$ 
- 差分方程：&emsp; $\Delta y_{it} = {\gamma} \Delta y_{i,t-1} + \Delta x_{it}'\beta  + \Delta\varepsilon_{it} \quad (2)$
- **H1: 无序列相关** $\text{Corr}(\varepsilon_{it}, \varepsilon_{it-1})=0$

$$\Delta \varepsilon_{it}=\varepsilon_{it}-\varepsilon_{it-1}$$

$$\Delta \varepsilon_{it-1}=\varepsilon_{it-1}-\varepsilon_{it-2}$$

$$\Delta \varepsilon_{it-2}=\varepsilon_{it-2}-\varepsilon_{it-3}$$

$\rho_1 = \text{Corr}(\Delta \varepsilon_{it}, \Delta \varepsilon_{it-1}) \rightarrow m_1$ 

$\rho_2 = \text{Corr}(\Delta \varepsilon_{it}, \Delta \varepsilon_{it-2}) \rightarrow m_1$

<br>
<br>


--- - --



## 过度识别检验: Sargan (1958) 

- 水平方程：&emsp; $y_{it} = {\gamma} y_{i,t-1} + x_{it}'\beta + \alpha_i + \varepsilon_{it}$, &emsp; $\text{Corr}(\varepsilon_{it},\varepsilon_{it-1})=0$
- 差分方程：&emsp; $\Delta y_{it} = {\gamma} \Delta y_{i,t-1} + \Delta x_{it}'\beta  + \Delta\varepsilon_{it} \quad (2)$
- **MC:** $E(y_{it-s}\cdot \Delta\varepsilon_{it})=0 , \ \text{for} \ s\geq 2$
- **做个回归：** $\Delta\varepsilon_{it} = \beta_1 + y_{it-2}\beta_2 + y_{it-3}\beta_3 + \cdots + v_{it}$
```stata
 . use "xtdyData.dta", clear
 . xtabond y x dumyr*, robust
 . predict e, e         // residuals
 
 . reg D.e  L(2/6).y  // 只是为了演示原理，实操不能这么做

 . regfit

D.e = -0.00 - 0.00*L2.y + 0.01*L3.y - 0.02*L4.y + 0.01*L5.y + 0.00*L6.y
      (0.03) (0.01)      (0.01)      (0.01)      (0.01)      (0.01)
       N = 4000, R2 = 0.00, adj-R2 = -0.00
```


--- - --

&#x1F34F; 

$$
Stata\ 实操……
$$

--- - --



## Stata 实操

$$
  y_{it} = {\gamma} y_{i,t-1} + x_{it}'\beta + {\alpha_i} + \varepsilon_{it} \quad (1)
$$

- $N=1000, T=10$
- ${\gamma}=0.6, \beta=1.3$
- $\text{Corr}(\alpha_i,\varepsilon_{it})\neq 0$, 

```stata
*-产生一份模拟数据
. xtarsim y x eta, n(1000) t(10) gamma(0.6) beta(1.3) rho(0.2) ///
                   one(corr 3) sn(9) seed(1234)  // DGP  
. tab tvar, gen(dumyr)
. drop dumyr1 dumyr2   
. save "xtdyData.dta", replace  // 保存在当前工作路径下, 随后会用 
```

--- - --

$$
  y_{it} = {\gamma} y_{i,t-1} + x_{it}'\beta + {\alpha_i} + \varepsilon_{it} 
$$

```stata
. use "xtdyData.dta", clear
. xtabond y x

Arellano-Bond  estimation                  N*T = 8,000
Group variable: ivar                       N   = 1,000
Time variable:  tvar
Number of instruments = 38     Wald chi2(2) = 20511.33
                               Prob > chi2  =   0.0000
------------------------------------------------------
           y |      Coef.   Std. Err.      z    P>|z| 
-------------+----------------------------------------
         L1.y|      0.592      0.010    62.04   0.000 
           x |      1.300      0.009   142.84   0.000 
       _cons |      1.207      0.031    39.31   0.000 
------------------------------------------------------
Instruments for differenced equation: 
    GMM-type: L(2/.).y;   Standard: D.x
Instruments for level equation:   
    Standard: _cons
```


--- - --

### Stata 实操: 一切都那么美好！?

```stata
  . use "xtdyData.dta", clear
*-序列相关检验
  . xtabond y x dumyr*, robust
  . estat abond
Arellano-Bond test for zero autocorrelation in first-differenced errors
  +-----------------------+
  |Order |  z     Prob > z|
  |------+----------------|
  |   1  |-21.311  0.0000 |
  |   2  |-.73566  0.4619 |
  +-----------------------+
   H0: no autocorrelation 

*-Sargan 检验
  . xtabond y x dumyr*, twostep 
  . estat sargan 
Sargan test of overidentifying restrictions
        H0: overidentifying restrictions are valid

        chi2(35)     =  29.79257
        Prob > chi2  =    0.7175

```


--- - --

&#x1F34E; 

$$
去 \ Stata \ 家！……
$$

--- - --



## FD-GMM 应用指南 1 - `xtabond` 中加入时间变量

- 无法使用因子变量，如 `i.year`，需要自己手动生成，如 `tab year, gen(dumt)`
```stata
use "xtdyData.dta", clear
tabulate tvar, gen(dumyr)
xtabond y x dumyr*, robust
```


--- - --

### FD-GMM 应用指南 2 - 序列相关检验 [[AB91]](https://www.jianguoyun.com/p/Df3hiZYQtKiFCBjb4dgC)

  - AR1, AR2, 或 m1, m2 (AR2 或 m2 必须报告), see AB91, Table 5; 
  - 命令如下，记得加时间虚拟变量和 `robust` 选项。
```stata
 . xtabond y x dumyr*, robust
 . estat abond
```
  - 检验通过依据：**m2** 对应的 p-value >10%，**m1** 的结果可以忽略
  - 无法通过怎么办？……


--- - --

### FD-GMM 应用指南 3 - 过度识别检验 Sargan-test [[AB91]](https://www.jianguoyun.com/p/Df3hiZYQtKiFCBjb4dgC)

  - 一步估计下的 Sargan 统计量未考虑异方差，存在「过度拒绝问题」, AB91, pp.288
  - 属于模型设定问题。只能在加入 `twostep` 选项的情况下才有用。但此时的 Sargan 统计量存在一定程度的「under reject」问题, AB91, pp.288.
```stata
 . xtabond y x dumyr*, twostep // 这里不能添加 robust 选项！
 . estat sargan
 . xtabond y x dumyr*, robust // 如果 Sargan 检验通过了，再用这个命令估计系数
 . est store m1 // 用这个结果来呈现系数，即 Onestep+RobustSE, AB91, pp.285
```
  - 检验通过依据：p-value >10%
  - 无法通过怎么办？……
  - **Sargan** test: 工具变量较多时仍然没有问题；但对异方差很敏感
  - **Hansen** J test：异方差稳健；工具变量较多时检验力下降  
  

--- - --

> AB 1991, pp.288

On the other hand, the robust $m_2$, statistics and the two-step difference-Sargan test show no serious departures from their nominal size.

The two-step Sargan test tends to under-reject and the two-step Hausman test over-rejects, especially in the last experiment where the variance of $x_{it}$ is much greater. We suspect that the two-step Hausman statistic is very sensitive to the presence of outliers.


--- - --


### 应用指南 4：Sargan 和序列相关检验为何无法通过 ？

Sargan 检验的目的：在于检验工具变量的合理性，简言之，是工具变量与差分后的干扰项是否存在统计上显著的相关性。相当于做如下回归：

$\Delta\varepsilon_{it} = \beta_1 + y_{it-2}\beta_2 + y_{it-3}\beta_3 + \cdots + v_{it}$

然后看 $y_{it-s}\ (s \geq 2)$ 的系数是否联合显著，如果显著，那就悲剧了。

可以看出，我们可以从 $\Delta\varepsilon_{it}$ 和 $y_{it-s}$ 两个角度入手。
- 让 $\Delta\varepsilon_{it}$ 干净一点
- 选择谁做 IVs ？—— $s$ 的选择

- **建议：** ……


--- - --


### 应用指南 5： POLS, FE, GMM 的系数关系

$$
  y_{it} = {\gamma} y_{i,t-1} + x_{it}'\beta + {\alpha_i} + \varepsilon_{it} \quad (1)
$$
- $\hat{\gamma}_{POLS}>\gamma_0$, $T$ 较小时，存在**上偏**偏误，随着 $\gamma_0 \rightarrow 1$, $\hat{\gamma}_{POLS}$ 的偏差程度降低
- $\hat{\gamma}_{FE}<\gamma_0$, $T$ 较小时，存在**下偏**偏误，随着 $\gamma_0 \rightarrow 0$, $\hat{\gamma}_{FE}$ 的偏差程度降低
- $E[\gamma_{GMM}]=\gamma_0$
- $\hat{\gamma}_{FE}\leq \hat{\gamma}_{GMM} \leq \hat{\gamma}_{POLS}$
- See Sun ([2004](https://www.jianguoyun.com/p/DY14a-oQtKiFCBi1uNgC), pp.53;  )


--- - --


![](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20200212144347.png)



--- - --

## 应用指南 6：方法选择依据

Flannery and Hankins ([2013](https://www.jianguoyun.com/p/DVrUk2cQtKiFCBjmttgC), JCF), 基于公司数据的模拟分析表明，
- 影响估计量表现的因素包括：
  - F1：$\alpha_i$
  - F2：${\gamma}\rightarrow 0$, ${\gamma}\rightarrow 1$
  - F3：$x_{it}$ 中是否包含内生变量
  - F4：$\varepsilon_{it}$ 的序列相关程度 (persistence) (Note, $H_0: \text{Corr}(\varepsilon_{it},\varepsilon_{it-1})=0$)。
- 估量选择：
  - 若 F3，则 Bruno (2005) 的「**LSDVC 估计量**, `xtlsdvc`」表现最佳；
  - 若 F2，`xtdpdgmm`, `xtdpdml`
  - 若 F3，适用 `xtabond` 等命令中的 `endog(varlist)` 选项



--- - --



## 方法汇总 (Self-readings)

> Flannery and Hankins ([2013](https://www.jianguoyun.com/p/DVrUk2cQtKiFCBjmttgC), JCF)

- **AB** Arellano and Bond (1991)，`xtabond`, `xtdpd`
- **BB** Blundell Bond (1998)，`xtdpdsys`, `xtabond2`
- **LD** longest differencing (Hahn et al., 2007, Huang and Ritter, 2009),), 
- **LD4** four period differencing (LD4)
- **LSDVC** least squares dummy variable correction (Kiviet, 1995, Bruno, 2005).).
  - SJ 5(4):473--500
- **xtbcfe** SJ 15-4: 986. Everaert and Pozzi (2007)



--- - --

## 新方法1：ML-SEM 估计方法 - `xtdpdml`

- Williams and Allison, SJ 2018(2): 18, 293-326. [[PDF]](https://www.jianguoyun.com/p/DXTrOJ0QtKiFCBjap9gC), [[PPT]](https://www.jianguoyun.com/p/DSzutBUQtKiFCBjHp9gC), [[Replicate]](https://www3.nd.edu/~rwilliam/dynamic/).  优点：
  - 适于短面板 ($T$ 比较小), 优于 `xtabond` (比如，省级面板)。
  - 可以估计 **非时变变量** (如性别，出生地等) 
  - 可以用于克服 **反向因果问题**。
- 相关：**Quasi ML-SEM**，`xtdpdqml`, SJ 2016(4):1013-1038.

<br>
<br>
<br>
<br>
<br>

--- - --


## 新方法2：ML-SEM 的应用 - 克服反向因果问题 - `xtdpdml` 

> Leszczensky, L and T Wolbring. 2019. How to Deal With Reverse Causality Using Panel Data?, WP. [[PDF]](https://www.jianguoyun.com/p/DRPK2cYQtKiFCBjMrdgC), [[Data & Prog]](https://osf.io/4h8nu/).

- 对比了 POLS, FE, AB, BB 等方法，发现 **ML-SEM** 在解决反向因果问题时最优
- 模型：
  $$
  y_{it} = {\gamma} y_{i,t-1} + \sum_{s=1}^p x_{it-s}'\beta_s + \alpha_i + \lambda_t + \varepsilon_{it} 
  $$
- 如果 $\beta_2 \neq 0$，但在模型中却没有加入 $x_{it-2}$,则会导致严重的偏误。
- 用 **ML-SEM** (`help xtdpdml`) 估计 $x$ 对 $y$ 的**当期**和**滞后期**影响.
- 即使存在反向因果，该方法仍然能够同事估计上述两种效应，且当滞后项设定有一定的偏差时，ML-SEM 依旧表现良好，而其它方法则不行。


--- - --

## 新方法3： `xtdpdgmm` 命令

> help xtdpdgmm 

- $y_{it}$ 高度自相关 ($\gamma \rightarrow 1$) 时, 估计更有效更稳健. Ahn and Schmidt ([1995](https://www.jianguoyun.com/p/DbZS3CIQtKiFCBj-q9gC))。这恰恰是 `xtabond` 的软肋。 
- 根据 Windmeijer ([2005](https://www.jianguoyun.com/p/DayMP84QtKiFCBi2rNgC)) 的建议调整小样本下的标准误，SE 具有异方差稳健性。
- 可以做 Hansen J test 和序列相关检验

```stata
*-Setup
  . webuse abdata, clear

*-Arellano-Bond two-step GMM estimator with strictly exogenous covariates
  . xtdpdgmm L(0/1).n w k, gmmiv(L.n, c m(d)) iv(w k, d m(d)) twostep vce(robust)

*-Ahn-Schmidt two-step GMM estimator with nonlinear moment conditions
  . xtdpdgmm L(0/1).n w k, gmmiv(L.n, c m(d)) iv(w k, d m(d)) twostep ///
             vce(robust) noserial
```





--- - --


#### 参考文献 

> Leszczensky and Wolbring ([2019](https://www.jianguoyun.com/p/DRPK2cYQtKiFCBjMrdgC)) Reverse_Causality 列出了所有重要参考文献.

- Anderson, Theodore W. and Cheng Hsiao. 1982. "Formulation and Estimation of Dynamic Models Using Panel Data." Journal of Econometrics 18(1):67–82.
- Ahn, S.C., Schmidt, P., 1995. Efficient estimation of models for dynamic panel data. J. Econometrics 68, 5–27.
- Arellano, M., Bond, S., 1991. Some tests of specification for panel data: Monte Carlo evidence and an application to employment equations. Rev. Econ. Stud. 58, 277–297.
- Arellano, M., Bover, O., 1995. Another look at the instrumental variable estimation of error-components models. J. Econometrics 68, 29–51. 

--- - --


- Bellemare, Marc F., Thomas B. Pepinsky, and Takaaki Masaki. 2017. "Lagged Explanatory Variables and the Estimation of Causal Effects." Journal of Politics 79(3):949–963.
- Blundell, R., Bond, S., 1998. Initial conditions and moment restrictions in dynamic panel data models. J. Econometrics 87, 115–143.
- Bond, S., 2002. Dynamic panel data models: a guide to micro data methods and practice, practice. Port. Econ. J. 1, 141–162.
- Bond, S., Meghir, C., 1994. Dynamic investment models and the firm's financial policy. Rev. Econ. Stud. 61, 197–222.
- Bound, J., Jaeger, D.A., Baker, R.M., 1995. Problems with instrumental variables estimation when the correlation between the instruments and the endogenous explanatory variable is weak. J. Am. Stat. Assoc. 90, 443–450.



--- - --


- Bruno, G.S.F., 2005. Approximating the bias of the LSDV estimator for dynamic unbalanced panel data model. Econ. Lett. 87, 361–366.
- Bun, Maurice J.G. and Frank Windmeijer. 2010. "The Weak Instrument Problem of the System GMM Estimator in Dynamic Panel Data Models." Econometrics Journal 13(1):95–126.
- Faulkender, M., M. J. Flannery, K. Watson Hankins, J. M. Smith, 2012, Cash flows and leverage adjustments, Journal of Financial Economics, 103 (3): 632-646.
- Flannery, M. J., K. W. Hankins, 2013, Estimating dynamic panel models in corporate finance, Journal of Corporate Finance, 19: 1-19. [[PDF]](https://www.jianguoyun.com/p/DVrUk2cQtKiFCBjmttgC)
- Flannery, M. J., K. P. Rangan, 2006, Partial adjustment toward target capital structures, Journal of Financial Economics, 79 (3): 469-506. [[PDF]](https://www.jianguoyun.com/p/DTocOmUQtKiFCBipt9gC )


--- - --


- Hahn, J., Hausman, J., Kuersteiner, G., 2007. Long difference instrumental variables estimation for dynamic panel models with fixed effects. J. Econometrics 140, 574–617.
- Hansen, Larls P. 1982. "Large Sample Properties of Generalized Method of Moments Estimators." Econometrica 50(4):1029–1054.
- Huang, R., Ritter, J., 2009. Testing theories of capital structure and estimating speed of adjustment. J. Financ. Quant. Anal. 44, 237–271.
- Imai, Kosuke and In Song Kim. 2019. "When Should We Use Linear Fixed Effects Regression Models for Causal Inference with Longitudinal Data?" American Journal of Political Science, Early View.
- Kripfganz, S. 2016. "Quasi-maximum likelihood estimation of linear dynamic short-T paneldata models." Stata Journal 16 (4): 1013-1038.

--- - --

- Lee, Myoung-jae. 2016. "Generalized Difference in Differences With Panel Data and Least Squares Estimator." Sociological Methods and Research 45(1):134–157. **G-DID**
- Newey, Whitney K. and Frank Windmeijer. 2009. "Generalized Method of Moments With Many Weak Moment Conditions." Econometrica 77(3):687–719.
- Nickell, Stephen J. 1981. "Biases in Dynamic Models with Fixed Effects." Econometrica 49(6):1417–1426.
- Reed, William Robert. 2015. "On the Practice of Lagging Variables to Avoid Simultaneity." Oxford Bulletin of Economics and Statistics 77(6):897–905.
- Richard Williams, Paul D. Allison, Enrique Moral-Benito, 2018, Linear Dynamic Panel-data Estimation Using Maximum Likelihood and Structural Equation Modeling, Stata Journal, 18(2): 293–326. `xtdpdml`, [[pdf]](https://sci-hub.tw/10.1177/1536867X1801800201)
- Roodman, David. 2012. "How to do xtabond2: An Introduction to Difference and System GMM in Stata." Stata Journal 9(1):86–136.


--- - --


- Windmeijer, F. 2005.  A finite sample correction for the variance of linear efficient two-step GMM estimators.  Journal of Econometrics 126: 25-51.
- Sebastian Kripfganz, 2016, Quasi–maximum Likelihood Estimation of Linear Dynamic Short-T panel-data Models, Stata Journal, 16(4): 1013–1038. [[PDF]](https://sci-hub.tw/10.1177/1536867X1601600411)
- Williams, Richard, Paul D. Allison, and Enrique Moral-Benito. 2018. "Linear Dynamic Panel-Data Estimation using Maximum Likelihood and Structural Equation Modeling." The Stata Journal 18(2): 293-326. `xtdpdml`
- Wooldridge, Jeffrey M. 2010. The Econometrics of Cross-Section and Panel Data, 2nd edition. Cambridge, MA: MIT Press.

--- - --

### FAQs 

> 多数问题都可以在 [连享会课程主页]( https://gitee.com/arlionn/Course ) &rarr; [[FAQs-2019]]( https://gitee.com/arlionn/Course/wikis/2019%E6%9A%91%E6%9C%9F_%E9%AB%98%E7%BA%A7%E7%8F%AD_FAQs.md?sort_id=1551342 ), [[FAQs-2020]]( https://gitee.com/arlionn/Course/wikis/2020%E5%AF%92%E5%81%87_%E9%AB%98%E7%BA%A7%E7%8F%AD_FAQs.md?sort_id=1876432 ) 中得到解答。


--- - --



>### 关于我们
- **Stata连享会** 由中山大学连玉君老师团队创办，定期分享实证分析经验。
- **精品课程：** <https://gitee.com/arlionn/Course>
- **往期精彩推文：**
 [Stata绘图](https://mp.weixin.qq.com/s/xao8knOk0ulGfNc7vasfew) | [面板数据](https://mp.weixin.qq.com/s/8yP1Dijylgreg59QIkqnMg) | [Stata资源](https://mp.weixin.qq.com/s/Kdeoi5uJyNtwwwptdQDQDQ) | [数据+程序](https://mp.weixin.qq.com/s/_3DQacFyy7juRjgFedp9WQ) |  [交乘项-内生性](https://mp.weixin.qq.com/s/61qJNWnL4KRp0fbLxuDGww)

![欢迎加入Stata连享会(公众号: StataChina)](https://file.lianxh.cn/images/20191111/ec83ed2baf9c93494e4f71c9b0f5d766.png)


--- - --

> Agoraki, M.-E. K., M. D. Delis, F. Pasiouras, **2011**, Regulations, competition and bank risk-taking in transition countries, **Journal of Financial Stability**, 7 (1): 38-48. [-Link-](https://doi.org/https://doi.org/10.1016/j.jfs.2009.08.002), [-PDF-](https://sci-hub.ren/https://doi.org/10.1016/j.jfs.2009.08.002)   

$$
r_{i t}=b_{0}^{\prime}+\delta r_{i t-1}+b_{1}^{\prime} L_{t}+b_{2}^{\prime} \text { reg }_{t-1}+b_{3}^{\prime} L_{i t} \times r e g_{t-1}+b_{4}^{\prime} x_{i t}+b_{5}^{\prime} m_{t}+\varepsilon_{i t}^{\prime}
$$
- $r_{i t}$: risk taking level
- $L_{i t}$: bank market power - Lerner index, $L_{i t}={\left(p_{i t}^{u}-m c_{t}\right)}/{p_{i t}^{q}}$
- $reg_{it}$: bank regulation
- 估计方法：SYS-GMM
- A value of $\delta$ between 0 and 1 implies that the dependent variables of the above equations persist, but they will eventually return to their normal (average) level. Values close to 0 mean that the speed of adjustment is high, while values close to 1 imply very slow adjustment.
<!-- 
The choice of the estimation procedure rests on the special features of each empirical model. Estimation of Eq. (1) is carried out using panel data instrumental variables regression. There are two main reasons for this choice. First, it may be possible that after deregulation of the CEE banking systems started, and taking into account the huge transformation of the economy and the society of these countries within a small period, credit risk increased significantly. This led to increased financial instability (note the crises in the CEE banking sectors during the late 1990 s) and in an effort to smooth the turmoil the supervisory authorities reacted by setting new rules and taking new initiatives that are reflected in the regulatory indices. Therefore, it is likely that reverse causality prevails between bank risktaking and each of competition and regulation. ${ }^{10}$ To prevent our model from capturing this adverse causality, we instrument against all risk and macroeconomic variables, their first lags and country dummies, in Eq. (1). ${ }^{11}$
 -->

--- - --

Agoraki, M.-E. K., M. D. Delis, F. Pasiouras, **2011**, Regulations, competition and bank risk-taking in transition countries, **Journal of Financial Stability**, 7 (1): 38-48. [-Link-](https://doi.org/https://doi.org/10.1016/j.jfs.2009.08.002), [-PDF-](https://sci-hub.ren/https://doi.org/10.1016/j.jfs.2009.08.002)