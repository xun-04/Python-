---
### ------------------- 幻灯片还是普通网页
marp: true
#marp: false

### ------------------- 幻灯片尺寸，宽版：4:3
size: 16:9
#size: 4:3

### ------------------- 是否显示页码
paginate: true  
#paginate: false

### ------------------- 页眉 (备选的用 '#' 注释掉)
#header: lianxh.cn
#header: '[连享会](https://www.lianxh.cn)'

### ------------------- 页脚 (备选的用 '#' 注释掉)
footer: '连享会&nbsp;|&nbsp;[lianxh.cn](https://www.lianxh.cn)&nbsp;|&nbsp;[B站](https://space.bilibili.com/546535876)&nbsp;|&nbsp;[课程](https://www.lianxh.cn/KC.html)'
#footer: '![20230202003318](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20230202003318.png)'
---

<!-- Global style: 设置标题字号、颜色 -->
<!-- Global style: 正文字号、颜色 -->
<style>
/*一级标题局中*/
section.lead h1 {
  text-align: center; /*其他参数：left, right*/
  /*使用方法：
  <!-- _class: lead -->
  */
}
section {
  font-size: 20px; 
}
h1 {
  color: blackyellow;
}
h2 {
  color: green;
}
h3 {
  color: darkblue;
}
h4 {
  color: brown;
  /*font-size: 28px; */
}
/* 右下角添加页码 */
section::after {
  /*font-weight: bold; */
  /*text-shadow: 1px 1px 0 #fff; */
  content: attr(data-marpit-pagination) '/' attr(data-marpit-pagination-total); 
}
header,
footer {
  position: absolute;
  left: 50px;
  right: 50px;
  height: 25px;
}
section footer{
    margin-left: 10px !important;
}

/* 这个设置适合于所有的背景图片*/
section figure{
    margin-right: 10px !important;
}

/* 这个设置适合于左侧的背景图片 */
section figure:first-child{
    margin-left: -10px !important;
}
</style>


<!-- ====================================================== -->
<!-- ================= 填入 标题-作者-脚注 等信息 =========== -->
<!-- ====================================================== -->

<!--顶部文字-->
[lianxh.cn](https://www.lianxh.cn/news/46917f1076104.html) 

<br>

<!--封面图片-->
![bg right:40% brightness:. sepia:50%](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220524091346.png)

<!--幻灯片标题-->
# 线性回归分析 (3)
- 多元回归系数的含义
- FWL 定理 (理论基础)
- 虚拟变量 
- FE, TWFE, MultiFE, IntFE
- DID, RDD, RKD

<br>

<!--作者信息-->
[连玉君](https://www.lianxh.cn) (中山大学)
arlionn@163.com

<br>

<!-- 
Notes: 
1. 选中一段文本，按快捷键 'Ctrl+/' 可以将其注释掉；再次操作可以解除 
2. header, footer 后面的文本需要用单引号引起来，否则会有语法错误
3. '#size: 16:9' 不能写为 'size:16:9'，即 'size:' 后要有一个空格
-->



--- - --
<!-- backgroundColor: #C1CDCD -->

# FWL 定理

## (Frisch-Waugh-Lovell therom)

--- - --

<!-- backgroundColor: #FFFFF9 -->

- FWL 定理由 Frisch and Waugh (1933) 和 Lovell (1963) 提出
  - 它阐释了 OLS 回归的一个重要性质，为理解多元回归的系数含义，估计高维固定效应提供了重要的理论基础。
  - Davidson and MacKinnon ([1993](http://qed.econ.queensu.ca/pub/dm-book/EIE-davidson-mackinnon-2021.pdf), 19-24) 以及 Davidson and MacKinnon ([2004](http://qed.econ.queensu.ca/pub/faculty/mackinnon/econ850/), [PDF](http://qed.econ.queensu.ca/ETM/ETM-davidson-mackinnon-2021.pdf), pp. 62–75, [Slides](http://qed.econ.queensu.ca/pub/faculty/mackinnon/econ850/slides/econ850-slides-h03.pdf)) 对此进行非常细致的介绍。
- **应用：**
  - 在 Stata 中，`reghdfe` 等处理高维固定效应的命令基本原理便是 FWL 定理
  - 在 `ivreg2`, `lasso2` 等命令中经常出现的 `partial()` 选项也基于 FWL 定理
  - 用于可视化展示多元回归结果的部分回归图命令 `avplot`, `reganat`, `avciplot`, `avciplots`, `binscatter` 等也都基于 FWL 定理。

- **Stata 实操**
  - Filoso, V., **2013**, Regression Anatomy, **Stata Journal**, 13(1): 92–106. [-PDF-](https://journals.sagepub.com/doi/pdf/10.1177/1536867X1301300107)
  - [图示线性回归系数：Frisch-Waugh定理与部分回归图](https://www.lianxh.cn/news/e346db1a68211.html)
  - [多元回归系数：我们都解释错了？](https://www.lianxh.cn/news/22f1f266f5868.html)


--- - --
<!-- backgroundColor: white -->

### 如何控制 x2 的影响？

![bg left:40% w:400](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/OLS-venn-01.png)

![w:400](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/OLS-venn-02.png)

--- - --

$Y=X_1 {\color{red}{\beta_1}} + X_2 \beta_2 + u$ $\ \ \Leftrightarrow$ $\ \ \tilde{Y}= \tilde{X}_1 {\color{red}{\beta_1}} + v$

![](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/Lianxh_装饰黄线.png)

`reg Y X2` 
`predict eY, res` &rarr; $\small\ \ \ \tilde{Y} = A + {\color{blue}{B}}$

`reg X1 X2` 
`predict eX1, res` &rarr; $\tilde{X}_1 = F + {\color{blue}{B}}$
![](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/Lianxh_装饰黄线.png)

`reg eY eX1` &rarr; `dis _b[eX1]` = $\small{\color{red}{\widehat{\beta}_1}}$ &rarr; ${\color{blue}{B}}$

![bg left:40% w:400](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/OLS-venn-01.png)



--- - --

#### 如果只需要估计系数

$$Y=X_1 {\color{red}{\beta_1}} + X_2 \beta_2 + u \ \ {\color{blue}{\Leftrightarrow}} \ \ Y= \tilde{X}_1 {\color{red}{\beta_1}} + v$$

- 事实上，只需从 $X_1$ 中去除  (partial out) $X_2$ 的影响，得到 $\tilde{X}_1$，进而用 $Y$ 对 $\tilde{X}_1$ 进行回归即可。即，如下回归都是等价的：
  
  - `reg` $\tilde{Y}$ on $\small\tilde{X}_1$
  - `reg` ${Y}$ on $\small\tilde{X}_1$
  - `reg` ${Y}$ on $\small{X}_1, {X}_2$

![bg left:40% w:400](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/OLS-venn-01.png)

--- - --

![bg left:35% w:350](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/OLS-venn-01.png)

### 遗漏变量偏误
- **真实模型：** $Y=X_1 {\color{red}{\beta_1}} + X_2 \beta_2 + u$
- &#x2753; 如果不控制 $x_2$，即 
  - `reg Y X1` $\small\iff$ $Y = X_1\theta_1 + \underbrace{{\color{red}{\varepsilon}}}_{X_2\beta_2+u}$
  - $\widehat{\theta}_1 \neq \beta_1$
--- - --




<!-- backgroundColor: #C1CDCD -->

## FWL 应用 1: 

### 常数项的作用

--- - --
<!-- backgroundColor: #FFFFF9 -->

![bg left:50% w:600](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/FWL_NoConstant.png)

> **包含常数项：**
- $E(Y_{i})= m_i = \boldsymbol{1} {\color{red}{\alpha}} + X_{i} {\color{blue}{\beta}}$
- $E(Y_{i}|X=0)= {\color{red}{\alpha}}$
  - $E(Y_{i}|X=0.5)= {\color{red}{\alpha}} + 0.5\beta$
  
> 不**包含常数项：**
- $E(Y_{i})= m_i = X_{i} {\color{blue}{\beta}}$
- $E(Y_{i}|X=0)= 0$

--- - --

### FWL 应用 1：常数项的作用 (1) - FWL 视角


<br>

$Y_{i}=\boldsymbol{1} {\color{red}{\alpha}} + X_{i} {\color{blue}{\beta}} + u_{i}$

$\bar{Y} = \boldsymbol{1} {\color{red}{\alpha}} + \bar{X} {\color{blue}{\beta}} + \bar{u}$

$\Longrightarrow$

$Y_{i} - \bar{Y} =  (X_{i}-\bar{X}) {\color{blue}{\beta}} + (u_{i}-\bar{u})$

#### Stata 实操

- `reg Y` &rarr; `predict eY, res` &emsp; $\widehat{e}_{Yi} = Y_i - \bar{Y}$ &emsp; (Note: $\bar{Y} = (1/N)\sum_i^N Y_i$)
- `reg X` &rarr; `predict eX, res` &emsp; $\widehat{e}_{Xi} = X_i - \bar{X}$

- &#x1F34E;  `reg eY eX` $\iff$ `reg y X`

![bg left:40% w:750](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/FWL_DeConstant.png)

<br>

> [Codes](https://gitee.com/arlionn/graph/wikis/dofiles/OLS_FWL_constant?sort_id=13200673)

--- - --
### 常数项的作用
- $x$ 和 $y$ 的量纲不一致问题
- 吸收 $x$ 或 $y$ 的缩放 $(x \to x/c)$ 或平移 $(x \to x-c)$ 产生的影响，以保证统计推断 ($t$ 值不受影响)
- 保证模型的 $R^2 \in [0,1]$，否则可能出现 $R^2<0$ 或 $R^2>1$ 的情况。
- 在 DID 和 RDD 分析中，常数项本身就有明确的经济含义

![](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/Lianxh_装饰黄线.png)

Note: 平移 $(x \to x-c)$ 在 RDD 和交互项设定中非常必要，参见 
 - [交乘项的中心化问题](https://www.lianxh.cn/news/454644a5b7e3b.html) 
 - [Stata+R：一文读懂精确断点回归-RDD](https://www.lianxh.cn/news/96fb6b7e847e1.html)
 - [Stata：断点回归RDD简明教程](https://www.lianxh.cn/news/789f031b0c110.html)


<!-- 

## 附：FWL 的进一步说明 

> Self-reading

--- - --

## The Frisch-Waugh-Lovell Theorem
Let's say we want to estimate the following model using OLS ([-Link-](https://scholar.princeton.edu/sites/default/files/bstewart/files/chern.handout.pdf)):
$$Y=\beta_{0}+\beta_{1} D+\beta_{2} X+U$$

The Frisch-Waugh-Lovell Theorem shows us that we can recover the OLS estimate of $\beta_{1}$ using a residuals-on-residuals OLS regression:
- Regress $Y$ on $X$ using OLS &rarr;  ${\color{blue}{\widehat{e}_{YX}}}=Y-\widehat{Y}$ 
- Regress $D$ on $X$ using OLS &rarr;  ${\color{red}{\widehat{e}_{DX}}}=D-\widehat{D}$ 
- Regress ${\color{blue}{\widehat{e}_{YX}}}$ on ${\color{red}{\widehat{e}_{DX}}}$ using OLS
  - The estimated coefficient on $\widehat{e}_{DX}$ will be the same as the estimated coefficient $\hat{\beta}_{1}$ from regressing $Y$ on $D$ and $X$ using OLS!



--- - --

## Frisch-Waugh-Lovell: Regression Anatomy
- In the simple bivariate case: [-Link-](http://sekhon.berkeley.edu/causalinf/fa2015/slides_section/Slides_OLS.pdf)
$$\small
\beta_{1}=\frac{\operatorname{Cov}\left(Y_{i}, X_{i}\right)}{\operatorname{Var}\left(X_{i}\right)}
$$
- In the multivariate case, $\beta_{j}$ is:
  $$\small
  \beta_{j}=\frac{\operatorname{Cov}\left(Y_{i}, \tilde{X}_{i j}\right)}{\operatorname{Var}\left(\tilde{X}_{i j}\right)}=\frac{\operatorname{Cov}\left(\tilde{Y}_{i}, \tilde{X}_{i j}\right)}{\operatorname{Var}\left(\tilde{X}_{i j}\right)}
  $$
- $\small\tilde{A}_{i j}\left(\tilde{Y}_{i}\right.$ or $\small\left.\tilde{X}_{j}\right)$ are the residuals from the regression of $A_{i j}$ on all other covariates.
- The multiple regression coefficient $\hat{\beta}_{j}$ represents the additional contribution of $x_{j}$ on $y$, after $x_{j}$ has been adjusted for $1, x_{1}, \ldots, x_{j-1}, x_{j+1}, \ldots, x_{p}$
- What happens when $x_{j}$ is highly correlated with some of the other $x_{k}$ 's?


--- - --

## Frisch-Waugh-Lovell: Regression Anatomy
- Claim: $\beta_{j}=\frac{\operatorname{Cov}\left(\tilde{Y}_{i}, \tilde{X}_{i j}\right)}{\operatorname{Var}\left(\tilde{X}_{i j}\right)}$, i.e $\small\operatorname{Cov}\left(Y_{i}, \tilde{X}_{i j}\right)=\operatorname{Cov}\left(\tilde{Y}_{i}, \tilde{X}_{i j}\right)$
- Proof:
Let $\small\tilde{Y}_{i}$ be the residuals of a regression of all the covariates except $X_{j i}$ on $Y_{i}$, i.e
$$\small
\begin{aligned}
&X_{j i}=\beta_{0}+\beta_{1} X_{1 i}+\beta_{2} X_{2}+\cdots+\beta_{P} X_{P i}+f_{i} \\
&Y_{i}=\alpha_{0}+\alpha_{1} X_{1 i}+\alpha_{2} X_{2}+\cdots+\alpha_{P} X_{P i}+e_{i}
\end{aligned}
$$
Then, $\small\hat{e}_{i}=\tilde{Y}_{i}$, and $\small\hat{f}_{i}=\tilde{X}_{j i}$
- It follows from the OLS algorithm that $\small\operatorname{Cov}\left(x_{k i}, \tilde{X}_{j i}\right)=0, \forall_{k \neq j}$. As the residuals of a regression are not correlated with any of the covariates
$$\small
\begin{aligned}
\operatorname{Cov}\left(\tilde{Y}_{i}, \tilde{X}_{i j}\right)=\operatorname{Cov}\left(Y_{i}-\right.&\left.\hat{\alpha}_{0}-\hat{\alpha}_{1} X_{1 i}-\hat{\alpha}_{2} X_{2}-\cdots+\hat{\alpha}_{P} X_{P i}, \tilde{X}_{i j}\right) \\
&=\operatorname{Cov}\left(Y_{i}, \tilde{X}_{i j}\right)
\end{aligned}
$$
 -->

--- - --

## FWL 应用 2: 虚拟变量

<br>

![bg left:45% w:720](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/FWL_Dummy_a01.png)


```stata
      y     x   M    F   cons  
   ---------------------------   
    5.7   1.6   0    1      1  
    6.4   1.4   0    1      1  
    9.3   2.0   1    0      1  
    9.0   1.4   1    0      1  
    9.3   1.7   1    0      1 
```

### 包含常数项
$y_{i} = \alpha_1 + {\color{blue}{\gamma}}\,M_{i} + \beta x_{i} + u_{i}$

$E[y_{i}\,|\,M=0] = \alpha_1 \quad\ \  +  \beta x_{i} + u_{i}$
$E[y_{i}\,|\,M=1] = \underbrace{\alpha_1 + {\color{blue}{\gamma}}}_{\alpha_2} + \beta x_{i} + u_{i}$

--- - --
<!-- backgroundColor: #FFFFF9 -->

### 不包含常数项
![bg left:45% w:720](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/FWL_Dummy_a01.png)

```stata
      y     x   M    F   cons  
   ---------------------------   
    5.7   1.6   0    1      1  
    6.4   1.4   0    1      1  
    9.3   2.0   1    0      1  
    9.0   1.4   1    0      1  
    9.3   1.7   1    0      1 
```
$y_{i} = \alpha_1 M_{i} + \alpha_2 F_{i} + \beta x_{i} + u_{i}$

$E[y_{i}\,|\,M=1] =  \alpha_1 + \beta x_{i} + u_{i}$
$E[y_{i}\,|\,F=1]\ = \alpha_2 + \beta x_{i} + u_{i}$

&#x1F34F; $y_{gi} = \alpha_g + \beta x_{gi} + u_{gi}\ (g=1,2)$

--- - --
<!-- backgroundColor: #FFFFF9 -->

![bg left:40% w:680](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/FWL_Dummy_a01.png)


```stata
. eststo m1: reg y M x
. eststo m2: reg y M F x, noconstant
. esttab m1 m2, nogap compress 

------------------------------------
                 (1)          (2)   
------------------------------------
x              0.458***     0.458***
              (5.71)       (5.71)   
M              3.105***     8.212***
             (33.58)      (72.51)   
F                           5.107***
                          (47.03)   
_cons          5.107***             
             (47.03)                
------------------------------------
N                 40           40   
------------------------------------
```

--- - --
<!-- backgroundColor: #e6e6fa -->
#### &#x1F34F; codes
```stata
*------------------
*- FWL: 虚拟变量 
*------------------

*-产生模拟数据
  set scheme tufte  //lean1 绘图模板
  clear
  set obs 40
  set seed 1359
  
  gen x = 2*uniform() 
  gen e = 0.3*rnormal()
  gen M = (_n>20)
  
  gen     y = 5 + 0.6*x + e 
  replace y = 3 + y if M 

```

--- - --
#### &#x1F34F; codes
```stata
*-图示：变截距
  
  #delimit ;
    tw (scatter y x if M==0, ms(+))
       (lfit    y x if M==0, lp(solid))
       (scatter y x if M==1, ms(oh))
       (lfit    y x if M==1, lp(solid)),
       ylabel(4(2)10) xtitle("")
       legend(off) aspect(1.2);
  #delimit cr
  graph export "FWL_Dummy_a01.png", replace width(1200)

*-呈现数据片段
  gen F = 1-M
  label var F "1-M"
  gen cons = 1
  format y x %3.1f
  list y x M F cons in 19/23, clean noobs
  
*-回归结果
  eststo m1: reg y M x
  eststo m2: reg y M F x, noconstant
  esttab m1 m2, nogap compress order(x M F) nomtitle
```

--- - --

<!-- backgroundColor: #FFFFF9 -->
### FWL 视角：动图展示
![bg left:50% w:640](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/OLS_Animation_of_Control_01.gif)

$Y_{i} = \alpha + \beta X_{i} + {\color{blue}{\gamma}}\,W_{i} + u_{i}$

$W_i=\{0,1\}$

- $W=0$: 
  - $Y_{i} = \alpha + \beta X_{i} + u_{i}$
- $W=1$: 
  - $Y_{i} = \alpha+{\color{blue}{\gamma}} + \beta X_{i} + u_{i}$

> Source: [NickCH-K](https://github.com/NickCH-K) &rarr; [causalgraphs](https://nickchk.com/causalgraphs.html) &rarr; [github](https://github.com/NickCH-K/causalgraphs)

--- - --

<!-- backgroundColor: #FFFFF9 -->
### FWL 视角
```stata
   g    i     y     x     D0   D1  
 ---------------------------------   
   1    1    5.7   1.6     0    1   
   1    2    6.4   1.4     0    1   
 ---------------------------------  
   2    1    9.3   2.0     1    0   
   2    2    9.0   1.4     1    0   
   2    3    9.3   1.7     1    0   
```

$(1)\quad Y_{gi} = {\color{blue}{\gamma_g}}D_g + \beta X_{gi} + u_{gi}, \quad g=1,2$ 

$(2)\quad Y_{gi}-\bar{Y}_g = \beta (X_{gi}-\bar{X}_g) + (u_{gi}-\bar{u}_g), \quad \text{de-mean within group}$

#### Stata 实例
```stata
sysuse "auto.dta", clear

reg mpg weight foreign                         // (1)

bysort foreign: center mpg weight, prefix(c_)  
reg c_mpg  c_weight                            // (2) 手动

reghdfe mpg weight, absorb(foreign)            // (2) 更简洁
```

--- - --
<!-- backgroundColor: white -->

## FWL 应用 3: 固定效应模型 (FE)


**原始数据:** 

$$
\begin{aligned}
y_{it} &= \alpha_i + x_{it}\beta + \varepsilon_{it} \\
& = \sum_i^N \alpha_i D_i + x_{it}\beta + \varepsilon_{it}
\end{aligned}
$$

**组内去心 (De-meaned):** 

$$
y_{it} -\bar{y}_i \\
= (x_{it}-\bar{x}_i)\beta + (\varepsilon_{it}-\bar{\varepsilon}_i)
$$

![bg right:55% w:680](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/Fig_OLS_FE_01.png)

<br>

#### FE：de-meaned 正式表述
$$
\begin{aligned}
y_{i t} &=x_{i t} \beta+ {\color{red}{\alpha_{i}}}+\varepsilon_{i t} \qquad (1) \\
\bar{y}_{i} &=\bar{x}_{i} \beta+{\color{red}{\alpha_{i}}}+\bar{\varepsilon}_{i} \\
\left(y_{i t}-\bar{y}_{i}\right) &=\left(x_{i t}-\bar{x}\right) \beta+\left({\color{red}{\alpha_{i}}}-{\color{red}{\alpha_{i}}}\right)+\left(\varepsilon_{i t}-\bar{\varepsilon}_{i}\right) \\
\ddot{y}_{i t} &=\ddot{x}_{i t} \beta+\ddot{\varepsilon}_{i t} \qquad\qquad\ (2)
\end{aligned}
$$

> [Stata Codes](https://gitee.com/arlionn/graph/wikis/dofiles/FWL_FE_scatter)


<!-- 
--- - --
#### 数据生成过程 (DGP)
```stata
clear
set obs 60
set seed 13599

egen id= seq(), from(1) to(3) block(20)
bysort id : gen t = _n + 1990

gen x1 = 3*rnormal() 
gen  e = 1*rnormal()
gen y = .

gen     x = x1 + 0
replace x = x1 + 4 if 1.id
replace x = x1 - 5 if 3.id

replace y = 6  + 0.4*x + e if id==1
replace y = 10 + 0.4*x + e if id==2
replace y = 15 + 0.4*x + e if id==3

bysort id: center y x, prefix(c_)  // De-mean

save "FWL_sim_data_FE", replace 
```

--- - --
#### 绘图1：手动
```stata
use "FWL_sim_data_FE", clear
#delimit ;
  twoway (scatter y x     if id==1, mcolor(green) msymbol(+)) 
         (scatter y x     if id==2, mc(red)  ms(oh))
         (scatter y x     if id==3, mc(blue) ms(dh))
         (lfit    y x,    lcolor(purple) lw(*1.5))    
         (lfit    y x     if id==1, lc(gray) lw(*1))
         (lfit    y x     if id==2, lc(gray) lw(*1))
         (lfit    y x     if id==3, lc(gray) lw(*1))
         (scatter c_y c_x if id==1, mcolor(green) msymbol(+)) 
         (scatter c_y c_x if id==2, mc(red)  ms(oh))
         (scatter c_y c_x if id==3, mc(blue) ms(dh))
         (lfit    c_y c_x,lcolor(purple) lw(*1.5))    
         (lfit    c_y c_x if id==1, lc(gray) lw(*1))
         (lfit    c_y c_x if id==2, lc(gray) lw(*1))
         (lfit    c_y c_x if id==3, lc(gray) lw(*1))
         , 
         yline(5, lp(longdash) lc(blue*1.5%30))
         ylabel(,angle(0))
         text(15.5 9.4 "Raw") text( 4 8.8   "De-mean")
         legend(off)  ;
#delimit cr
``` -->

--- - --

#### 绘图 2：使用 `binscatter2` 命令

```stata
  use "FWL_sim_data_FE", clear

  global legend "legend(ring(0) pos(2) col(1))"
  global scheme "scheme(white_tableau)"
  global scheme "scheme(tufte)"
  
  binscatter2 y x, n(60) by(id)            /// // Raw
              $legend $scheme  

  binscatter2 y x, n(60) by(id) absorb(id) /// // De-mean
              $legend $scheme
```

--- - --

- $\tilde{y}_{it} = y_{it} - \bar{y}_{i.} + {\color{red}{\bar{\!{\bar{y}}}_{..}}}$ &emsp; 命令： `binscatter2 ...`
- $\ddot{y}_{it} = y_{it} - \bar{y}_{i.}$ &emsp; &emsp; &emsp; 命令：`binscatter2 ..., noaddmean`
  
![h:500](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220703002127.png)

--- - --
<!-- backgroundColor: #FFFFF9 -->
#### 系数可变

![w:620](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/FWL_demean_het.png)

```stata
replace y = 10 + 0.6*x + e if id==2 // NEW 0.4-->0.6
replace y = 15 + 0.8*x + e if id==3 // NEW 0.4-->0.8
```

--- - --
<!-- backgroundColor: #C1CDCD -->

## FWL 应用 4: 

### 倍分法 (DID)

--- - --
<!-- backgroundColor: white -->
![bg left:50% w:600](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/Animation-of-DID-01.gif)

### DID
$\small Y_{it} = \alpha + \beta_1 Treat_{it} + \beta_2 Post_{it} + {\color{blue}{\gamma}}\,Treat_{it}\times Post_{it} + u_{it}$

> Source:   
> [NickCH-K](https://github.com/NickCH-K) &rarr; [causalgraphs](https://nickchk.com/causalgraphs.html) &rarr; [github](https://github.com/NickCH-K/causalgraphs)

采用动图呈现了常用的因果推断模型的核心思想，包括 IV, DID, FE, RDD, PSM 等

--- - --

### DID：模型设定

$$
y_{it} = \alpha + \theta_1 Treat_i+ \theta_2 Post_t  + \gamma\ Treat_i \times Post_t + {x}_{it}'\beta + \varepsilon_{it}
$$


其中，
- $Treat_i$ 为处理组虚拟变量，用以标记实验组和控制组。若第 $i$ 组属于实验组，则 $Treat_i=1$，否则 $Treat_i=0$。
- $Post_t$ 是标记时间时点的虚拟变量，设 $t_0$ 为政策发生的时点，则对于 $t>t_0$ 的观察值 $Post_t=1$，否则 $Post_t=0$。


### 系数含义
为了便于表述，假设 $\beta=0$，即先不考虑其他控制变量 $x_{it}$ 的影响。


- $E(y_{it}|Treat=0, Post=0) = \hat{\alpha} = C_0$ 
- $E(y_{it}|Treat=1, Post=0) = \hat{\alpha} + \hat{\theta}_1 = Y_0$ 
- $E(y_{it}|Treat=0, Post=1) = \hat{\alpha} + \hat{\theta}_2 = C_1$
- $E(y_{it}|Treat=1, Post=1) = \hat{\alpha} + \hat{\theta}_1 + \hat{\theta}_2 + \hat{\gamma} = Y_1$




---

我们可以先分析一下 $\theta_1$ 和 $\theta_2$ 的含义：

$E(y_{it}|Treat=0, Post=0) = \hat{\alpha} = C_0$ 

$E(y_{it}|Treat=1, Post=0) = \hat{\alpha} + \hat{\theta}_1 = Y_0 \quad {\color{red}{\Rightarrow}}\quad \hat{\theta}_1 = Y_0 - C_0$ 

$E(y_{it}|Treat=0, Post=1) = \hat{\alpha} + \hat{\theta}_2 = C_1 \quad {\color{red}{\Rightarrow}}\quad \hat{\theta}_2 = C_1 - C_0$

- $\hat{\theta}_1 = Y_0 - C_0$，个体固定效应，这其实就是「处理组」和「控制组」在政策实施之前的「结果变量」差异，
- $\hat{\theta}_2 = C_1 - C_0$，时间趋势效应。由于控制组并未收到政策的影响，因此其结果变量在事前与事后的变化完全归因为时间趋势。


---

由此可以得到：

- $E(y_{it}|Treat=0, Post=0) = \hat{\alpha} = C_0$ 
- $E(y_{it}|Treat=1, Post=0) = \hat{\alpha} + \hat{\theta}_1 = Y_0$
- $E(y_{it}|Treat=0, Post=1) = \hat{\alpha} + \hat{\theta}_2 = C_1$
- $E(y_{it}|Treat=1, Post=1) = \hat{\alpha} + \hat{\theta}_1 + \hat{\theta}_2 + \hat{\gamma} = Y_1$
$$
\begin{aligned}
\hat{\gamma} &=\left(Y_{1}-C_{1}\right)-\left(Y_{0}-C_{0}\right) \\
&=\left(Y_{1}-Y_{0}\right)-\left(C_{1}-C_{0}\right)
\end{aligned}
$$
- $(Y_1-C_1) = \hat{\theta}_1 + \hat{\gamma} = \color{blue}{个体效应} + 政策效应$
- $(Y_1-Y_0) = \hat{\theta}_2 + \hat{\gamma} = \color{green}{时间趋势} + 政策效应$


--- - --

### 标准 DID (两期)
$$
Y_{\text {it }}=\gamma_{0}+\gamma_{1} {\color{red}{D_{i}}}+\gamma_{2} {\color{blue}{T_{t}}}+\beta {\color{red}{D_{i}}} \times {\color{blue}{T_{t}}}+\theta W_{i t}+\varepsilon_{i l}
$$
- 结果变量：$Y_{i t}$
- 政策分组变量：$D_{i}$ &emsp;  处理组取值为 1 ，控制组取值为 0
- 政策时间变量：$T_{t}$ &emsp;  政策时点后取值为 1, 政策时点之前取值为 0
- 交互项的系数 $\beta$ : 反映的是经过两次差分后得到的「纯净」因果效应

$$\small
\begin{array}{|l|l|l|l|}
\hline \mathbb{E}\left(Y_{i t} \mid D_{i}, T_{t}\right) & T_{t}=0 & T_{t}=1 & \Delta_{i t} \\
\hline D_{i}=0 & \gamma_{0} & \gamma_{0}+\gamma_{2} & \gamma_{2} \\
\hline D_{i}=1 & \gamma_{0}+\gamma_{1} & \gamma_{0}+\gamma_{1}+\gamma_{2}+\beta & \gamma_{2}+\beta \\
\hline \Delta_{i t} & \gamma_{1} & \gamma_{1}+\beta & \beta \\
\hline
\end{array}
$$

--- - --
有关 DID 的详细介绍和实现，参见：
- [Qu Zhaopeng, 2020, Lecture 7B: Difference in Differences](https://byelenin.github.io/MicroEconometrics/Slides/GradMetrics_2020_Lec7B.pdf)
- [Scott Cunningham, DID - Slides and Codes](https://github.com/scunning1975/codechella)
- 中文：
  - 在 Stata 命令窗口中输入如下命令： [倍分法专题](https://www.lianxh.cn/blogs/39)
  - `. lianxh DID 差分`

--- - --


### Regression DD - 
### Card and Krueger - 1 

Card, D., A. B. Krueger, **1994**, Minimum wages and employment: A case study of the fast-food industry in new jersey and pennsylvania, **AER**, 84 (4): 772-793. [-PDF-](https://uh.edu/~adkugler/Card%26Krueger.pdf) 

![bg left:60% w:600](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220706110428.png)


--- - --

### Regression DD - Card and Krueger - 2

- The equivalent regression includes time and group fixed effects:
$$
Y_{i t s}=\alpha+\gamma N\!J_{s}+\lambda d_{t}+\delta(N\!J \times d)_{s t}+\varepsilon_{i t s}
$$
- $N\!J$ is a dummy equal to 1 if the observation is from NJ
- $d$ is a dummy equal to 1 if the observation is from November (the post period)
- This equation takes the following values
  - PA Pre: $\alpha$
  - PA Post: $\alpha+\lambda$
  - NJ Pre: $\alpha+\gamma$
  - NJ Post: $\alpha+\gamma+\lambda+\delta$
- DD estimate: (NJ Post - NJ Pre) - (PA Post - PA Pre $)=\delta$

--- - --

## 面板数据模型

- FE
- TWFE
- IntFE

参见「**PX_A_2022b\A6_Panel\Lecture**」文件夹下如下讲义：
- **连玉君_Panel_01_FE.pdf**
- **连玉君_Panel_02_HDFE.pdf**
- **连玉君_Panel_03_IntFE.pdf**




--- - --
<!-- backgroundColor: #C1CDCD -->
## FWL 定理：小结

--- - --
<!-- backgroundColor: #FFFFF9 -->
### 几种典型设定 - 1
$$Y=X_1 {\color{red}{\beta_1}} + X_2 \beta_2 + u \quad{\color{blue}{\Leftrightarrow}} \quad \tilde{Y}= \tilde{X}_1 {\color{red}{\beta_1}} + v$$

- $\small\bf{X_2} = \bf{1}$，去除样本均值 &emsp; `reg y x`
  
- $\small{\bf{X_2}} = Trend_t$，去除时间趋势
  - `reg y x  c.year`
- $\small{\bf{X}}_{{\bf{2}}it} = \alpha_i = \sum_1^N \alpha_i D_i$，去除个体效应
  -  `reg y x i.id` or `xtreg y x, fe`  or  `reghdfe y x, absorb(id)`
- $\small{\bf{X}}_{{\bf{2}}it} = \lambda_t= \sum_1^T \lambda_t D_t$，去除时间效应 
  - `reg y x i.year` or `areg y x, ab(year)`



--- - --
<!-- backgroundColor: #FFFFF9 -->
### 几种典型设定 - 2
$$Y=X_1 {\color{red}{\beta_1}} + X_2 \beta_2 + u \quad{\color{blue}{\Leftrightarrow}} \quad \tilde{Y}= \tilde{X}_1 {\color{red}{\beta_1}} + v$$

- $\small{\bf{X}}_{{\bf{2}}it} = \alpha_i + \lambda_t$，双向固定效应 &rarr; DID
  - `. reghdfe y x, absorb(id year)` or `xtreg y x i.year, fe`
- $\small{\bf{X}}_{{\bf{2}}sjit} = \alpha_s + \alpha_j + \alpha_i + \lambda_t$，多维固定效应
  - `. reghdfe y x1 x2, absorb(province industry firm year)`

- $\small{\bf{X}}_{{\bf{2}}jt} = Ind_j + \lambda_t + Ind_j\times \lambda_t$，交互固定效应 
  - `. reghdfe y x, absorb(industry year industry#year)`




--- - --

<center>

## 连享会  &#x1F34E; 

### [lianxh.cn](https://www.lianxh.cn)



