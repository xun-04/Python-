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
# header: lianxh.cn
#header: '[连享会](https://www.lianxh.cn)'
#header: '[lianxh.cn](https://www.lianxh.cn/news/46917f1076104.html)'

### ------------------- 页脚 (备选的用 '#' 注释掉)
#footer: 'lianx.cn Marp 模板'
footer: '连享会&nbsp;|&nbsp;[lianxh.cn](https://www.lianxh.cn)&nbsp;|&nbsp;[Bilibili](https://space.bilibili.com/546535876)'
#footer: '![20230202003318](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20230202003318.png)'
---

<!-- 
Notes: 
1. 选中一段文本，按快捷键 'Ctrl+/' 可以将其注释掉；再次操作可以解除 
2. header, footer 后面的文本需要用单引号引起来，否则会有语法错误
3. '#size: 16:9' 不能写为 'size:16:9'，即 'size:' 后要有一个空格
-->



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
  font-size: 24px; 
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
/*  content: 'Page ' attr(data-marpit-pagination) ' / ' attr(data-marpit-pagination-total); */
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
<!-- ![bg right:50% w:400 brightness:. sepia:50%](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220722114227.png)  -->

<!--封面图片-->
![bg right:50% w:400 brightness:. sepia:50%](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220524091346.png)


> `. lianxh ARDL` // 相关资料


<br>

<br>

<!--幻灯片标题-->
# ARDL：自相关分布滞后模型
## 政策长期效应估计

<br>

<!--作者信息-->
#### [连玉君](https://www.lianxh.cn) (中山大学)
arlionn@163.com

<br>


--- - --

### 个人所得税变动对 创新行为的长期影响
- $x$：$t-t_0$
- $y$：
$$\small e=\frac{\Delta ln(Pat)}{\Delta ln(Tax)}$$

![bg left:65% w:800](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/Figure4_mtr90_lnpat.png)

> Source: Akcigit, et al. ([2022](https://doi.org/10.1093/qje/qjab022), [PDF](https://sci-hub.ren/10.1093/qje/qjab022)) **QJE**

--- - --

### 公司所得税
![bg left:65% w:800](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/Figure4_top_corp_ln_inv.png)

--- - --
<!-- backgroundColor: #C1CDCD -->

## 提纲

- ARDL 模型简介
- 长期效应 v.s. 短期效应
- ARDL 的理论基础
  - 部分调整模型
  - 理性预期模型
- 考虑共同相关因素的 ARDL 模型
- 应用实例

--- - --
<!-- backgroundColor: #FFFFF9 -->
## 简介

- 微观
  - 消费：持有收入假说
  - 投资：R&D, M&A
  - 资本结构：权衡理论 + 调整成本
- 宏观
  - 货币政策、房地产刺激政策
  - 目标通胀率 / 目标失业率 &rarr; 部分调整 + 粘性
- 估计长期效应
- 挑战：
  - 模型设定的理论基础 ？
  - 空间相关、共同因素 (common factor)


--- - --

### ARDL 的应用
  
![](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220611230155.png)


--- - --
<!-- backgroundColor: #C1CDCD -->

## 模型设定

- DL 模型
  
- ARDL 模型
- Panel ARDL 模型
- 异质性共同相关Panel ARDL 模型


--- - --
<!-- backgroundColor: #FFFFF9 -->
### DL 模型：分布滞后模型 (distributed lag model)

<br>

$$
y_{t}=\alpha+x_{t}^{\prime} \beta_{0}+x_{t-1}^{\prime} \beta_{1}+x_{t-2}^{\prime} \beta_{2}+\cdots+x_{t-q}^{\prime} \beta_{q}+e_{t} .
$$

<br>

- 假设：某些解释变量的多期滞后项都对被解释变量有影响。
- 例如，投资行为
  - $x_{t}$: 第 $t$ 期的新增投资, $y_{t}$: 公司价值。
  - $\small DL$ 原因：有些投资项目需要 3-5 年甚至更长的时间才能完成 (如并购后整合、新药研发) &rarr; $x$ 对 $y$ 的影响具有**滞后性和累积性**。 

--- - --

### DL 模型：分布滞后模型 (distributed lag model)

<br>

$$
y_{t}=\alpha+x_{t}^{\prime} \beta_{0}+x_{t-1}^{\prime} \beta_{1}+x_{t-2}^{\prime} \beta_{2}+\cdots+x_{t-q}^{\prime} \beta_{q}+e_{t}  \quad (1)
$$

<br>

**系数的含义**有两种解释: 
- 短期影响：如 $\beta_{1}$ 反应的是 $x_{t-1}$ 对 $y_{t}$ 的影响 (控制其他因素) 
- 长期影响 (长期乘数)：$\small \text{LM} = \beta_{0}+\cdots+\beta_{q}$
  - 反映了 $x$ 对 $y$ 的**累积影响**

<br>
<br>


--- - --

### ARDL 模型  (AutoRegressive Distributed Lag model)

<br>

$$
y_{t}=\alpha+ {\color{red}{\lambda_{1} y_{t-1}}} +\cdots+\lambda_{p} y_{t-p}+x_{t-1}^{\prime} \beta_{1}+\cdots+x_{t-q}^{\prime} \beta_{q}+e_{t} \quad (2)
$$

- 文献中应用更为普遍。`findit ardl` 
- 在模型 (1) 中加人 $y_{t-s}$, 以反映 $y_{t}$ 的自回归特征 (即, $y_{t}$ 具有一定的延续性, 会受到其滞后项的影响



### $\operatorname{ARDL}(p, q, \ldots, q)$

<br>

$$
y_{t}=c_{0} + \sum_{i=1}^{p} \phi_{i} y_{t-i}+\sum_{i=0}^{q} \beta_{i}^{\prime} \mathbf{x}_{t-i}+u_{t},
$$

<br>

- $p \geq 1, q \geq 0$
- 此处假设所有解释变量都具有相同的滞后期数
- 更一般化的设定中，每个解释变量可以有不同的滞后期数

--- - --

### Panel ARDL

<br>

在面板数据中, 可以进一步加人固定效应。例如, Panel-ARDL $(1,1)$ 模型设定为：

$$
y_{i, t}= {\color{red}{\alpha_{i}}}+\lambda y_{i, t-1}+\beta_{0} x_{i, t}+\beta_{1} x_{i, t-1}+u_{i, t}
$$

也可以加人：
- 时间固定效应 $\lambda_{t}$，以及 $\lambda_{t}$ 与其他变量的交乘项
- 时间趋势项 $Trend_t$，以及 $Trend_t$ 与其他变量的交乘项
- 其他控制变量 $\left(\mathbf{w}_{i t}\right)$，以及它们高阶滞后项。

--- - --

### Panel ARDL：实例 1

<br>

Dell, Jones, and Olken ( [2012](https://doi.org/10.1257/mac.4.3.66), [PDF](http://sci-hub.ren/10.1257/mac.4.3.66), AEJ ) 在研究气候变化与经济增长关系时，设定了如下模型 ( 参见 [Appendix II](https://www.aeaweb.org/aej/mac/app/2010-0092_app.pdf), A1.5 式 )：

$$
\Delta y_{i t}=a_{i}+\sum_{\ell=1}^{p} \lambda_{\ell} \Delta y_{i, t-\ell}+\sum_{\ell=0}^{p+1} \beta_{\ell} T_{i t-\ell}+\varepsilon_{i t} .
$$

- $\Delta y_{it}$ 是国家 $i$ 在 $t$  时点的实际人均 GDP 的对数，
- $a_{i}$ 是国家层面的固定效应，
- $T_{it}$ 是国家 $i$ 在 $t$  时点的人口加权平均气温。

--- - --

### Panel ARDL：实例 2

<br>

- Burke et al. ([2015](https://doi.org/10.1038/nature15725), [PDF](http://sci-hub.ren/10.1038/nature15725)), Kalkuhl and Wenz (2020) 等都对此模型进行了扩展。
- Kahn et al. ([2021](https://doi.org/10.1016/j.eneco.2021.105624), [PDF](https://www.nber.org/system/files/working_papers/w26167/w26167.pdf)) 的设定如下：

  $$
  \Delta y_{i t}=a_{i}+\sum_{\ell=1}^{p} \lambda_{\ell} \Delta y_{i, t-\ell}+\sum_{\ell=0}^{p} \boldsymbol{\beta}_{\ell}^{\prime} \Delta \mathbf{x}_{i, t-\ell}+\sum_{\ell=0}^{p} \boldsymbol{\theta}_{\ell}^{\prime} \Delta \mathbf{x}_{i, t-\ell} \times {\color{red}{\mathbb{I}(\cdot)}} + \varepsilon_{i t}
  $$
  - $\Delta \mathbf{x}_{i, t}$ 表示气候变化，
  - ${\color{red}{\mathbb{I}(\cdot)}}$ 是各类反应国家特征的虚拟变量，如低收入、处于热带地区等。

--- - --

### 异质性共同相关Panel ARDL 模型

- 变化 1：允许异质性系数：$\lambda \rightarrow \lambda_{i}, \beta \rightarrow \beta_{i}$
- 变化 2：引人共同因子 (common factors)：$\mathbf{f}_t = [f_{1t}, f_{2t}, \cdots, f_{mt}]$

例如, Stata 中的 `xtdcce2` 命令，见 Ditzen ([2021](https://doi.org/10.1177/1536867x211045560), [PDF](http://pro1.unibz.it/projects/economics/repec/bemps81.pdf))，对应的模型设定为：
$$
\begin{aligned}
y_{i, t} &=\alpha_{i}+\lambda_{i} y_{i, t-1}+\beta_{0, i} x_{i, t}+\beta_{1, i} x_{i, t-1}+u_{i, t} \\
u_{i, t} &=\sum_{l=1}^{m} \rho_{y, i, l} f_{t, l}+e_{i, t} \\
x_{i, t} &=\sum_{l=1}^{m} \rho_{x, i, l} f_{t, l}+\xi_{i, t}
\end{aligned}
$$
其中, $i=1, \ldots, N, t=1, \ldots, T_{i}$ 。


--- - --
<!-- backgroundColor: #C1CDCD -->

## 长期效应与短期效应

<br>

- 在 ARDL 模型中, 变量之间存在很强的动态关系：
  - $x$ 的当期值和滞后项，以及 $y$ 的滞后项都会对 $y_{t}$ 产生影响。

- 这些影响可以归结为「**短期效应**」 和「**长期效应**」两类
  - **长期效应** 反映了 $x$ 和 $y$ 之间的长期均衡关系。

<br>
<br>

--- - --
<!-- backgroundColor: #FFFFF9 -->
### 短期效应
这里, 先以最简单的 ARDL $(1,1)$ 模型为例进行说明:

$$
y_{t}=\alpha+\lambda y_{t-1}+\beta_{0} x_{t}+\beta_{1} x_{t-1}+u_{t}
$$

**短期效应** 定义为：
$$
\frac{\partial y_{t}}{\partial x_{t}}=\beta_{0}, \quad \frac{\partial y_{t}}{\partial x_{t-1}}=\beta_{1}
$$

- 以 $\beta_{1}$ 为例, 短期关系反映的是在控制其他因素 (如与 $x_{t-1}$ 有较强相关性 的 $x_{t}$, 以及 $y_{t-1}$ ) 的情况下, $x_{t-1}$ 对 $y_{t}$ 的条件边际影响。

--- - --

### 长期效应：$\operatorname{ARDL}(1, 1)$ 模型
长期效应反映的是 $x$ 和 $y$ 的**长期均衡值之间的关系**。

$$\small
y_{t}=\alpha+\lambda y_{t-1}+\beta_{0} x_{t}+\beta_{1} x_{t-1}+u_{t}
$$

把式 中 $x$ 和 $y$ 的当期值和滞后项统一替换为 $\tilde{x}$ 和 $\tilde{y}$ (二者的长期均衡值), 即

$$\small
\tilde{y}(1-\lambda)=\alpha+\left(\beta_{0}+\beta_{1}\right) \tilde{x}
$$

求解 $\tilde{y}$ 可得:

$$\small
\tilde{y}=\frac{\alpha}{1-\lambda}+\frac{\beta_{0}+\beta_{1}}{1-\lambda} \tilde{x}
$$

因此, $x$ 单位变化对 $y$ 的**长期影响**由下式给出

$$\small
\frac{\partial \tilde{y}}{\partial \tilde{x}}=\frac{\beta_{0}+\beta_{1}}{1-\lambda}
$$

--- - --
### 长期效应：$\operatorname{ARDL}(p, q)$ 模型

$$\small
y_{t}=\alpha+\sum_{\ell=1}^{p} \lambda_{\ell} y_{t-\ell}+\sum_{\ell=0}^{q} \beta_{\ell} x_{t-\ell}+\varepsilon_{t} .
$$
**长期乘数** long-run multiplier (LM) 定义为:
$$\small
\mathrm{LM}=\frac{\beta_{0}+\cdots+\beta_{q}}{1-\lambda_{1}-\cdots-\lambda_{p}}
$$
- LM 是模型参数的非线性函数
- 对于面板数据模型而言, LM 的定义和计算方法并没有本质差别
- 如果选择的滞后阶 $p$ 和 $q$ 足够大, ARDL 模型的误差项可以近似为白噪声, 此时, 模型可以解释变量之间的动态均衡关系, 亦可用传统方法计算标准误。



--- - --
<!-- backgroundColor: #C1CDCD -->

##  时间趋势

--- - --
<!-- backgroundColor: #FFFFF9 -->
### 时间趋势：简介
许多经济时间序列的均值都是随时间变化的。我们可以使用如下模型 刻画这一特征：
$$
y_{t}=\operatorname{Trend}_{t}+u_{t}
$$
这里, $y_{t}$ 包含两个部分：时间趋势项 $\operatorname{Trend}_{t}$ 和随机扰动项 $u_{t}$ 。后者可以设定为线性过程或自回归过程:
$$
\alpha(\mathrm{L}) u_{t}=e_{t}
$$
时间趋势项则常被设定为时间变量 $(t)$ 的线性模型:
$$
\operatorname{Trend}_{t}=\beta_{0}+\beta_{1} t
$$

或二次函数形式 (以反映时间趋势的非线性特征):
$$
\text { Trend }_{t}=\beta_{0}+\beta_{1} t+\beta_{2} t^{2} .
$$

--- - --
<!-- backgroundColor: #FFFFF9 -->
### 时间趋势项
在 ARDL ($p, q$) 模型中加入时间趋势项是文献中惯用的做法：
$$
y_{t}=\alpha_{0}+\alpha_{1} y_{t-1}+\cdots+\alpha_{p} y_{t-p}+x_{t-1}^{\prime} \beta_{1}+\cdots+x_{t-q}^{\prime} \beta_{q}+ {\color{red}{\gamma t}}+e_{t} 
$$

- 无论把时间趋势项设定成何种形式，本质上都是一种近似。

- 可以通过各种灵活的设定，让其尽可能反映数据本身的特征。比如，
  - 加入高阶项 $t^3, t^4, \cdots$，
  - 或允许 $\beta_{1}$ 和 $\beta_{2}$ 具有异质性，如随个体发生变化，即 $\beta_{1i}$, $\beta_{2i}$。


--- - --

### 时间趋势项：实例 1
- Burke et al. ([2015](https://doi.org/10.1038/nature15725), [PDF](http://sci-hub.ren/10.1038/nature15725))，气候变化 ($T_{it}$) 与经济增长 ($\Delta y_{i t}$)：

$$
\Delta y_{i t}=\alpha_{i}+\delta_{t}+\alpha T_{i t}+ {\color{blue}{\beta T_{i t}^{2}}} + {\color{red}{\gamma_{i} t+\phi_{i} t^{2}}}+\varepsilon_{i t}
$$

- $y_{i t}$ 表示国家 $i$ 在第 $t$ 年的人均 GDP，
- $T_{it}$ 表示气温，
- $\alpha_{i}$ 为国家层面的固定效应，
- $\delta_{t}$ 为时间效应。
- $\gamma_{i} t$ 和 $\phi_{i} t^{2}$ 分别是线性时间趋势一次和二次项。

> 注意，这里采用了非常灵活的设定，允许每个国家有不同的时间趋势，因为参数 $\gamma_{i}$ 和 $\phi_{i}$ 都可以随国家而变化。

--- - --

### 时间趋势项：实例 2

<br>

- Kalkuhl and Wenz ([2020](https://doi.org/10.1016/j.jeem.2020.102360), [PDF](http://sci-hub.ren/10.1016/j.jeem.2020.102360)) 在 Burke et al. ([2015](https://doi.org/10.1038/nature15725), [PDF](http://sci-hub.ren/10.1038/nature15725)) 的设定中进一步增加了两项：$\Delta T_{i t}$ 及交乘项 $T_{i t} \times \Delta T_{i t}$，以便捕捉短期气温变化产生的影响:

<br>

$$
\Delta y_{i t}=a_{i}+\delta_{t}+\lambda {\color{blue}{\Delta T_{i t}}}+ \psi {\color{red}{T_{i t}}} \times {\color{blue}{\Delta T_{i t}}} + \alpha T_{i t}+\beta T_{i t}^{2}+\gamma_{i} t+\phi_{i} t^{2}+\varepsilon_{i t}
$$

<br>

- Note：时间趋势项可以作为 ARDL 模型设定中的控制变量，有些时候，它本身就是研究的重点。 


--- - --
<!-- backgroundColor: #C1CDCD -->

## ARDL 模型的理论基础

<br>

$\mathrm{ARDL}$ 模型是「简约式」而非「结构式」模型设定, 但其背后有很强的经济含义。

比如, 可以从经济学中由来已久的两个重要模型推导出 ARDL 的设定形式：


- 部分调整模型
  
- 理性预期模型

<br>
<br>

--- - --
<!-- backgroundColor: #EAFEF9 -->
### 部分调整模型 (Partial adjustment model) 

令 $y_{t}^{*}$ 为决策变量 $y_{t}$ 的预期值 (如, 目标体重、目标负债率、目标汇 率、目标通胀率等), 并假设 $y_{t}^{*}$ 与 $x_{t}$ 存在如下关系:
$$
y_{t}^{*}=\alpha+\beta x_{t}+u_{t}
$$
假设 $y_{t}$ 基于如下一阶「**部分调整过程**」向其预期水平调整：
$$
y_{t}-y_{t-1}=\lambda\left(y_{t}^{*}-y_{t-1}\right),
$$
其中, $\lambda$ 为调整系数：
- 如果 $\lambda=0$, 则不会进行调整
- 如果 $\lambda=1$, 则调整 可以瞬时完成
- 通常而言, $0<\lambda<1$ 
  
--- - --
<!-- backgroundColor: #FFFFF9 -->
### 部分调整模型 $\longrightarrow$ ARDL 

<br>

$$
y_{t}^{*}=\alpha+\beta x_{t}+u_{t} \ \ \ \ \qquad (1)
$$

假设 $y_{t}$ 基于如下一阶「**部分调整过程**」向其预期水平调整：

$$
y_{t}-y_{t-1}=\lambda\left(y_{t}^{*}-y_{t-1}\right)  \quad (2)
$$

用 (1) 代替 $y_{t}^{*}$, 可得

$$
\begin{aligned}
y_{t} &=\lambda \alpha+(1-\lambda) y_{t-1}+\lambda \beta x_{t}+\lambda u_{t} \\
&=\alpha_{0}+\theta y_{t-1}+\gamma x_{t}+v_{t}
\end{aligned} \quad (3)
$$

显然, 这是一个 $A R D L(1,0)$ 模型。

--- - --

### 部分调整模型：扩展

目标值的设定：
$$
y_{t}^{*}=\alpha+\beta x_{t}+u_{t} \qquad\qquad (1)
$$

- **隐含假设：** 公司基于当期的 $x_{t}$ 信息来确定 $y_{t}^{*}$ 

**扩展 1：** 考虑信息获取的滞后性，则将 $y_{t}^{*}$ 设定成如下形式或许更为合理：
$$
y_{t}^{*}=\alpha+\beta_{1} x_{t-1}
$$
此时, (3) 式将转变为一个 $\operatorname{ARDL}(1,1)$ 模型。

**扩展 2：** 当然, 也可以假设公司会同时结合第 $t$ 期和 $t-1$ 期的信息确定 $y_{t}^{*}$ :
$$
y_{t}^{*}=\alpha+\beta_{0} x_{t}+\beta_{1} x_{t-1}
$$
此时, (3) 将转变为如下形式:
$$
y_{t}=\alpha_{0}+\theta y_{t-1}+\gamma_{0} x_{t}+\gamma_{1} x_{t-1}+v_{t}
$$


--- - --

### 部分调整模型：应用

部分调整模型经常应用于
- 资本结构调整、现金持有行为 (Venkiteshwaran, V. ([2011](https://doi.org/https://doi.org/10.1016/j.rfe.2011.06.002), [PDF](http://sci-hub.ren/https://doi.org/10.1016/j.rfe.2011.06.002)))，
  - Flannery and Rangan ([2006](https://doi.org/10.1016/j.jfineco.2005.03.004), [PDF](http://sci-hub.ren/10.1016/j.jfineco.2005.03.004)) 采用了 (1) 的设定方式来研究公司的资本结构调整速度。
- 银行资本充足率 (Baik et al. ([2022](https://doi.org/https://doi.org/10.1016/j.jbankfin.2022.106548), [PDF](http://sci-hub.ren/https://doi.org/10.1016/j.jbankfin.2022.106548)))，汇率调整等问题的研究。

- Flannery and Hankins ([2013](https://doi.org/10.1016/j.jcorpfin.2012.09.004), [PDF](http://sci-hub.ren/10.1016/j.jcorpfin.2012.09.004)) 对此类模型及其估计方法进行了系统的评述。

此外，我们也可以把部分调整模型与理性预期模型结合起来。


--- - --
<!-- backgroundColor: #EAFEF9 -->

### 理性预期模型

$$
y_{t}=\alpha+\beta\left({ }_{t} x_{t+1}^{e}\right)+u_{t} \quad (1)
$$
根据理性预期假设 (见 Pesaran (1987c)), $x_{t+1}^{e}$ 定义如下:
$$
{ }_{t} x_{t+1}^{e}=E\left(x_{t+1} \mid \Omega_{t}\right)  \qquad (2)
$$
其中, $\Omega_{t}$ 表示在 $t$ 时点上可以获得的所有信息的集合, 简称「信息集」。

${ }_{t} x_{t+1}^{e}$ 的含义： 基于第 $t$ 时点上的信息集 $\Omega_{t}$ 形成的对变量 $x$ 在第$t+1$ 时点的预期值。

--- - --
<!-- backgroundColor: #FFFFF9 -->
假设: $\Omega_{t}=\left\{x_{t}, x_{t-1}, \ldots, y_{t}, y_{t-1}, \ldots\right\}$ 。同时, 假设 $x_{t}$ 服从 $A R(2)$ 过程:
$$
x_{t}=\mu_{1} x_{t-1}+\mu_{2} x_{t-2}+\varepsilon_{t}
$$
则
$$
{ }_{t} x_{t+1}^{e}=\mu_{1} x_{t}+\mu_{2} x_{t-1}  \quad (3)
$$
将 (3) 代人 (1), 可得:
$$
y_{t}=\alpha+\beta\left(\mu_{1} x_{t}+\mu_{2} x_{t-1}\right)+u_{t}
$$
或
$$
y_{t}=\alpha+\theta_{1} x_{t}+\theta_{2} x_{t-1}+u_{t}
$$
其中, $\theta_{1}=\beta \mu_{1}, \theta_{2}=\beta \mu_{2}$ 。


--- - --

### 理性预期模型：扩展 1
$$
y_{t}=\alpha+\beta\left({ }_{t} x_{t+1}^{e}\right)+u_{t} \quad (1)
$$

其一, 可以在 (1) 式进一步加人其他变量的预期值, 如 ${ }_{t} z_{t+1}^{e}$ 
  - 例如，若 $y_{t}$ 表示工资, $x_{t}$ 和 $z_{t}$ 可以分别表示失业率 $\left(u e_{t}\right)$ 和通胀率 $\left(\pi_{t}\right)$, 则 (1-21) 式可表示为:
$$
y_{t}=\alpha+\beta_{1}\left({ }_{t} u e_{t+1}^{e}\right)+\beta_{2}\left({ }_{t} \pi_{t+1}^{e}\right)+u_{t}
$$
- 又如, 设 $z_{t+1}^{e}={ }_{t} x_{t+2}^{e}$, 则
$$
y_{t}=\alpha+\beta_{1}\left({ }_{t} x_{t+1}^{e}\right)+\beta_{2}\left({ }_{t} x_{t+2}^{e}\right)+u_{t}
$$
在利率期限结构理论中, 预期理论便认为当前的利率水平决定于投资者对 末来不同期限的债券的收益率的预期。

--- - --
### 理性预期模型：扩展 2

<br>

$$
y_{t}=\alpha+\beta\left({ }_{t} x_{t+1}^{e}\right)+u_{t} \ \quad (1)
$$

$$
{ }_{t} x_{t+1}^{e}=E\left(x_{t+1} \mid \Omega_{t}\right) \ \qquad (2)
$$

$$
{ }_{t} x_{t+1}^{e}=\mu_{1} x_{t}+\mu_{2} x_{t-1}  \qquad (3)
$$

其二, 我们可以将 (3) 式设定为更一般化的 $A R(p)$ 形式。

- 例如, 对 于季度数据, 可以设定 $p=4$; 
  
- 或对于序列相关较为强烈的变量 (如财政支 出、研发支出等), 即便是年度数据, 我们依然可以将 $p$ 设定为 3 或更大的 数值。


--- - --
### 理性预期模型：扩展 3 - 包含内生变量当期预期值的模型

有些情况下, $\small y_{t}$ 的预期值是影响 $\small y_{t}$ 的一个重要因素,

$$\small
\begin{aligned}
y_{t} &=\alpha+\beta\left({}_{t-1} y_{t}^{e}\right)+\gamma x_{t}+u_{t}, \quad \beta \neq 1, \\
&= \alpha+\beta E\left(y_{t} \mid \Omega_{t-1}\right)+\gamma x_{t}+u_{t}.
\end{aligned}
$$

经过一些简单推导，可得：

$$\small
\begin{aligned}
y_{t} & =\frac{\alpha}{1-\beta}+\frac{\gamma \beta}{1-\beta}\left(\mu_{1} x_{t-1}+\mu_{2} x_{t-2}\right)+\gamma x_{t}+u_{t} \\
      & = \theta_0 + \theta_1 x_{t-1} + \theta_2 x_{t-2} +\gamma x_{t}+u_{t}
\end{aligned}
$$

显然，这是一个典型的 ARDL(1,2) 模型。

> More: Pesaran, M. H. Time series and panel data econometrics[M]. Oxford University Press, 2015. [Link](https://xs2.dailyheadlines.cc/books?hl=zh-CN&lr=&id=5jITDAAAQBAJ&oi=fnd&pg=PP1&ots=GoClMYVT_f&sig=m4JhzfS2AKDisbWZTuCnuKcR_FE), Chp 6.





<!-- 理性预期 + 部分调整 随后加入 -->




--- - --
<!-- backgroundColor: #C1CDCD -->

## 面板 ARDL 模型

<br>

- Ditzen, J. **2021**. Estimating long-run effects and the exponent of cross-sectional dependence: An update to `xtdcce2`. **Stata Journal**, 21 (3): 687-707.
 [Link](https://doi.org/10.1177/1536867x211045560), [PDF1](http://sci-hub.ren/10.1177/1536867x211045560). [PDF2](http://pro1.unibz.it/projects/economics/repec/bemps81.pdf), [-Slides-](https://www.stata.com/meeting/nordic-and-baltic19/slides/nordic19_ditzen.pdf)
  - 有关面板 ARDL 的多数文献都在这篇文章中列出了
- 中文解读：[面板数据模型-xtdcce2：动态共同相关和截面相关](https://www.lianxh.cn/news/2296811af7839.html) 

--- - --
<!-- backgroundColor: #FFFFF9 -->

### 共同相关估计量 (CCE)

考虑如下变系数模型 (Pesaran 2006),
$$
\begin{aligned}
&y_{i t}=\alpha_{i}+\boldsymbol{\beta}_{i}^{\prime} \mathbf{x}_{i t}+u_{i t} \\
&u_{i t}=\boldsymbol{\gamma}_{i}^{\prime} \mathbf{f}_{t}+e_{i t}
\end{aligned} \quad (1)
$$
- $\mathbf{f}_{t}$ 是不可观测的共同因子 (common factor), 
- $\gamma_{i}$ 为异质性因子载荷 (factor loading), $\alpha_{i}$ 是个体固定效应。
- $e_{i t}$ 随机扰动项, 满足独立同分布 (IID) 假设。

进一步假设异质性系数围绕共同均值随机波动, 设定如下:
$$
\boldsymbol{\beta}_{i}=\boldsymbol{\beta}+\mathbf{v}_{i}, \quad \mathbf{v}_{i} \sim \operatorname{iid}\left(\mathbf{0}, \boldsymbol{\Omega}_{v}\right)
$$

--- - --
<!-- backgroundColor: #FFFFF9 -->

### 共同相关估计量(CCE)：MG 估计 (Mean Group)

$$
\begin{aligned}
&y_{i t}=\alpha_{i}+\boldsymbol{\beta}_{i}^{\prime} \mathbf{x}_{i t}+u_{i t} \\
&u_{i t}=\boldsymbol{\gamma}_{i}^{\prime} \mathbf{f}_{t}+e_{i t}
\end{aligned} \quad (1)
$$

- **CCE 估计量 (Common Correlated Estimator)**：
  - 假设 $\mathbf{x}_{i t}$ 严格外生，用其截面均值 $\overline{\mathbf{x}}_{t}$ 近似表示不可观测的共同因子 $\mathbf{f}_{t}$
   $$y_{i t}=\alpha_{i}+\boldsymbol{\beta}_{i}^{\prime} \mathbf{x}_{i t}+ {\color{red}{\boldsymbol{\theta}_{i}^{\prime} \mathbf{\bar{x}}_{i}}} + u_{i t}  \quad (2)$$
  - 则 (2) OLS 估计是一致的 (仅适用于静态模型)

- **基本思想：** 当横截面维度接近无穷大时, 可以用横截面 平均值逐渐消除不可观测的共同因子产生的影响 (Pesaran 2006, p.969)。

- **Stata 实现**：Eberhardt (2012) 编写的 `xtmg`, Ditzen (2018, 2021) 编写的 `xtdcce2` 命令。

--- - --

### 动态 CC 模型

$$\small
\begin{aligned}
&y_{i t}=\alpha_{i}+ {\color{blue}{\lambda_{i} y_{i, t-1}}}+\boldsymbol{\beta}_{i}^{\prime} \mathbf{x}_{i t}+u_{i t} \\
&u_{i t}=\gamma_{i}^{\prime} \mathbf{f}_{t}+e_{i t}
\end{aligned}
$$
- 其中, 干扰项 $u_{i t}$ 存在截面弱相关, $\small E\left(\lambda_{i}\right)=\lambda$。
- 此时, 被解释变量的一阶滞后项 $y_{i, t-1}$ 不再是外生的。
- Chudik and Pesaran (2015b) 提出, 如果把滞后因变量和外生变量的截面均值的 $p_{T}$ 阶滞后项加人模型, 则可以获得一致估计量。其中, 滞后阶数 $\small p_{T}=\lfloor\sqrt[3]{T}\rfloor$, 表示对 $\small\sqrt[3]{T}$ 取整后的数值, 如 $\small\lfloor\sqrt[3]{100}\rfloor \simeq\lfloor 4.64\rfloor=4$ 。待估方程为 
$$\small
y_{i t}=\alpha_{i}+\lambda_{i} y_{i, t-1}+\boldsymbol{\beta}_{i}^{\prime} \mathbf{x}_{i t}+\sum_{l=0}^{p_{T}} \boldsymbol{\delta}_{i l}^{\prime} \overline{\mathbf{z}}_{t-l}+e_{i t}
$$
其中，$\overline{\mathbf{z}}_{t}=\left(\bar{y}_{t-1}, \overline{\mathbf{x}}_{t}\right)$，它可以视为不可观测的共同因子 $\mathbf{f}_{t}$ 的代理变量。

--- - --
### 动态 CC 模型：MG 估计量

<br>

$$
\boldsymbol{\beta}_{i}=\boldsymbol{\beta}+\mathbf{v}_{i}, \quad \mathbf{v}_{i} \sim \operatorname{iid}\left(\mathbf{0}, \boldsymbol{\Omega}_{v}\right)
$$

若把 $\lambda_{i}$ 和 $\boldsymbol{\beta}_{i}$ 堆叠放置为 $\boldsymbol{\pi}_{i}=\left(\lambda_{i}, \boldsymbol{\beta}_{i}\right)$，则 MG 估计量为：
$$
\widehat{\boldsymbol{\pi}}_{\mathrm{MG}}=\frac{1}{N} \sum_{i=1}^{N} \widehat{\boldsymbol{\pi}}_{i}
$$

- Stata 实现命令：Ditzen (2018, [2021](http://pro1.unibz.it/projects/economics/repec/bemps81.pdf)) 编写的 `xtdcce2` 命令。
- 中文解读：[面板数据模型-xtdcce2：动态共同相关和截面相关](https://www.lianxh.cn/news/2296811af7839.html)
  
--- - --
### 动态 CC 模型：ECM 表示

$$
\begin{aligned}
&y_{i t}=\alpha_{i}+ {\color{blue}{\lambda_{i} y_{i, t-1}}}+\boldsymbol{\beta}_{i}^{\prime} \mathbf{x}_{i t}+u_{i t} \\
&u_{i t}=\gamma_{i}^{\prime} \mathbf{f}_{t}+e_{i t}
\end{aligned}
$$

上式可以表示为 **误差修正模型 (ECM)** 的形式:
$$
\Delta y_{i t}=\phi_{i}\left(y_{i t-1}-\boldsymbol{\theta}_{i}^{\prime} \mathbf{x}_{i t}\right)+\alpha_{i}+\boldsymbol{\beta}_{i}^{\prime} \Delta \mathbf{x}_{i t}+u_{i t}
$$

- $\phi_{i}=\left(1-{\color{blue}{\lambda_{i}}}\right)$：误差修正的**调整速度**，预期为负值
- $\left(y_{i, t-1}-\boldsymbol{\theta}_{i}^{\prime} \mathbf{x}_{i t}\right)$：误差修正项 (error-correction term) 
- $\boldsymbol{\theta}_{i}=\boldsymbol{\beta}_{i} / \phi_{i}$：**长期系数** (long-run coefficient)，此处假设具有同质性
- $\boldsymbol{\beta}_{i}$：短期动态调整关系，异质

--- - --

### CS-ARDL：扩展 $\operatorname{ARDL}\left(p_{y}, p_{x}\right)$ 模型

回顾：$\small \operatorname{ARDL}\left(1,1\right)$ 模型
$$
\begin{aligned}
&y_{i t}=\alpha_{i}+ {\color{blue}{\lambda_{i} y_{i, t-1}}}+\boldsymbol{\beta}_{i}^{\prime} \mathbf{x}_{i t}+u_{i t} \\
&u_{i t}=\gamma_{i}^{\prime} \mathbf{f}_{t}+e_{i t}
\end{aligned} \quad (1)
$$

模型 (1) 可以扩展为 $\operatorname{ARDL}\left(p_{y}, p_{x}\right)$ 模型:
$$
y_{i, t}=\mu_{i}+\sum_{l=1}^{p_{y}} \lambda_{l, i} y_{i, t-l}+\sum_{l=0}^{p_{x}} \beta_{l, i} x_{i, t-l}+\sum_{l=0}^{p} \gamma_{i, l}^{\prime} \overline{\mathbf{z}}_{t-l}+e_{i, t}
$$
个体的 **长期系数** 为：
$$
\widehat{\theta}_{\mathrm{CS}-\mathrm{ARDL}, i}=\frac{\sum_{l=0}^{p_{x}} \widehat{\beta}_{l, i}}{1-\sum_{l=1}^{p_{y}} \widehat{\lambda}_{l, i}}
$$


--- - --

### 动态异质性模型

$\operatorname{ARDL}(p, \underbrace{q, q, \ldots, q})$ model
$$
y_{i t}=\alpha_{i}+\sum_{j=1}^{p} \lambda_{i j} y_{i, t-j}+\sum_{j=0}^{q} \delta_{i j}^{\prime} \mathbf{x}_{i, t-j}+u_{i t},
$$

表示为 **误差修正模型**：
$$
\Delta y_{i t}=\alpha_{i}+ {\color{blue}{\phi_{i}}} y_{i, t-1}+ {\color{red}{\boldsymbol{\beta}_{i}^{\prime}}} \mathbf{x}_{i t}+\sum_{j=1}^{p-1} \lambda_{i j}^{*} \Delta y_{i, t-j}+\sum_{j=0}^{q-1} \delta_{i j}^{* \prime} \Delta \mathbf{x}_{i, t-j}+u_{i t},
$$


--- - --
$$
\Delta y_{i t}=\alpha_{i}+ {\color{blue}{\phi_{i}}} y_{i, t-1}+ {\color{red}{\boldsymbol{\beta}_{i}^{\prime}}} \mathbf{x}_{i t}+\sum_{j=1}^{p-1} \lambda_{i j}^{*} \Delta y_{i, t-j}+\sum_{j=0}^{q-1} \delta_{i j}^{* \prime} \Delta \mathbf{x}_{i, t-j}+u_{i t},
$$
其中，
$$
\begin{aligned}
\phi_{i} &=-\left(1-\sum_{j=1}^{p} \lambda_{i j}\right), \quad \boldsymbol{\beta}_{i}=\sum_{j=0}^{q} \delta_{i j}, \\
\lambda_{i j}^{*} &=-\sum_{m=j+1}^{p} \lambda_{i m}, j=1,2, \ldots, p-1, \\
\delta_{i j}^{*} &=-\sum_{m=j+1}^{q} \delta_{i m}, j=1,2, \ldots, q-1 .
\end{aligned}
$$


--- - --
<!-- backgroundColor: #D1CDCD -->

## 应用：文献解读

--- - --
<!-- backgroundColor: #C1CDCD -->

### 应用文献 - 1：税收对创新行为的长期影响

<br>

[Akcigit](http://www.ufukakcigit.com/), U., J. Grigsby, T. Nicholas, S. Stantcheva, **2022**,    
Taxation and innovation in the twentieth century,   
**Quarterly Journal of Economics**, 137 (1): 329-385.  
[-Link-](https://doi.org/10.1093/qje/qjab022), [-PDF-](https://file-lianxh.oss-cn-shenzhen.aliyuncs.com/Refs/Paper2022/Akcigit-2022-QJE.pdf), [-Appendix-](https://oup.silverchair-cdn.com/oup/backfile/Content_public/Journal/qje/137/1/10.1093_qje_qjab022/1/qjab022_onlineappendix.pdf?Expires=1649965153&Signature=soPrpxU1GGqhdNr58Nc6j-gZhttwWtj2XQG5WxaVp-k7ZqVAJOoYz60biLwCcpgYVpVutAw-uJn59pJkQJOuZlMv6DHHRPiIHE2I7CUNOv5c05r1msRmBbFmzntnyXBov2UkywbuJpob1e59Q5fesu5Z7t6RQFOoh8qgVxjlQcTNgcN6YFuFISMPa2GP8zRbQcNxcFuKbRhPyoUMqFI-MJkwVS7pfl162hJ0ZRa0fH9ho7N3FhBGyoN0jAufE1S3vCSeb2FetG7lhS8JGYMb~FMOcpyRpv1hjSiSL52lSl5W2jT18i1uN-k4atVdt3TN-JFWXMk796qn~BvYnM~85Q__&Key-Pair-Id=APKAIE5G5CRDK6RD3PGA), [-cited-](https://xs2.dailyheadlines.cc/scholar?cites=3691559864871282829&as_sdt=2005&sciodt=0,5&hl=zh-CN&scioq=Social+ties+and+the+selection+of+China%27s+political+elite), [-Replication-](https://doi.org/10.7910/DVN/SR410I)


--- - --
<!-- backgroundColor: #FFFFF9 -->
### 问题背景

> :cat: 模型设定 (TWFE)

$$\small
\begin{aligned}
Y_{s t}=& \alpha+\beta_{p} \ln \left(1-M T R 90_{s t-3}\right)+\beta_{c} \ln \left(1-\operatorname{Corp} . \mathrm{MTR}_{s t-3}\right) \\
&+\gamma \mathbb{X}_{s t}+\delta_{t}+\delta_{s}+\varepsilon_{s t} \quad \text{(3)}
\end{aligned}
$$

- $\small Y_{s t}$：$s$ 州在 $t$ 时期的创新产出：专利数、引用数、发明人人数等
- $\small MTR90_{s t-3}$：滞后 3 年期的个人所得税率
- $\small Corp.MTR_{s t-3}$：滞后 3 年期作为公司所得税
- $\small \delta_{t}$ 和 $\delta_{s}$：时间和州固定效应，以捕获不可观测的个体和时间趋势效应
- $\small X_{s t}$ 代表随时间变化的州层级的变量，包括：(滞后 3 期) 的人口密度 (城市化程度)、人均收入 (经济发展)、研发支出抵免 (税收激励)


--- - --

为了估计税收对创新的长期影响，作者设定了如下分布滞后 (**DL**) 模型：
$$\small
\begin{aligned}
Y_{s t}-Y_{s t-1}=\delta_{t} +\sum_{l=-5}^{20} \beta_{l}\left[\ln \left(1-T_{s t-l}\right)-\ln \left(1-T_{s t-l-1}\right)\right] + \Delta X_{s t-1}^{\prime} v+\epsilon_{s t}
\end{aligned} \quad (10)
$$
- $\small Y_{s t}$：$s$ 州在 $t$ 时期的创新产出：专利数等；$\small T_{s t}$ 为个人 和/或 公司所得税税率
- $\delta_{t}$ 是时间固定效应，$X$ 是控制变量。

作者使用公式 (11) 估计 **Figure 4** 中的 **累积处理效应**，并且将累积效应在 $t-1$ 时期进行了中心化处理，目的是使得在 $t=0$ 时的累积效应等于 0。
$$\small
\mathcal{B}_{l}=\underbrace{\left[\sum_{\tau=-5}^{l} \beta_{l}\right]}_{\begin{array}{c}
\text { Effect from } t-5 \\
\text { through } t+l
\end{array}}-\underbrace{\left[\sum_{\tau=-5}^{-1} \beta_{l}\right]}_{\begin{array}{c}
\text { Renormalizing to be } \\
\text { relative to year } t-1
\end{array}} \quad \text{(11)}
$$

--- - --

$$\small
\begin{aligned}
Y_{s t}-Y_{s t-1}=\delta_{t} &+\sum_{l=-5}^{20} \beta_{l}\left[\ln \left(1-T_{s t-l}\right)-\ln \left(1-T_{s t-l-1}\right)\right] + \Delta X_{s t-1}^{\prime} v+\epsilon_{s t}
\end{aligned} \quad (10)
$$
```stata
use "$D/Akcigit2022_state.dta", clear 

global controls "LD.(real_gdp_pc population_density rd_credit)"
gen x = D.mtr90   // 将解释变量简记为 x

  reghdfe D.lnpat                ///
          F( 1/5).x  L(0/20).x   ///
          LD.top_corp  $controls ///
          [aw=pop1940],          ///
          absorb(year)           ///
          vce(cluster statenum)  

*-滞后 0-3 期的累积效应
lincom L3.x + L2.x + L1.x + L0.x           
```

--- - --

### 短期效应: 全都不显著
```stata
. reghdfe D.lnpat F( 1/5).x  L(0/20).x LD.top_corp  $controls ///
          [aw=pop1940], absorb(year) vce(cluster statenum)

. esttab, nogap r2 ar2 stat(F)
. coefplot, keep(*.x) xline(0,lp(dash) lc(gray%60))          
```

![w:600](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/gr_Akcigit2022_Fig4_SR.png)

--- - --

### 长期累积效应：
> 以 $t \in [-3, 0]$ 为例

```stata
. reghdfe D.lnpat F( 1/5).x  L(0/20).x LD.top_corp  $controls ///
          [aw=pop1940], absorb(year) vce(cluster statenum)

. lincom L3.x + L2.x + L1.x + L0.x  

        (1)  x + L.x + L2.x + L3.x = 0
        
        ---------------------------------------------------------
         D.lnpat |  Coeff      SE   t     P>|t|       [95% CI]
        ---------+-----------------------------------------------
             (1) |  0.467   0.257  1.82   0.076    -0.050   0.984
        ---------------------------------------------------------
```
> 其它各点：可以使用循环语句计算 $\widehat{\theta}$ 和 $se(\widehat{\theta})$，继而计算 $90\%$ 置信区间，绘图。

--- - --

### Figures 4 (a)
![w:850](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/gr_Akcigit2022_Figure4_a.png)

--- - --

**结果解读：**
- 整体来看，在 $(-5, -1)$ 时期不存在显著的趋势效应 (预期效应)，说明不存在所得税以外的其他因素对创新产生影响，因而我们所得到的显著效果是由所得税变动引致的结果。
- **个税：** 如果在 $t=0$ 时期将个人所得税净税率 $(1-MTR90)$ 提高 $1\%$，则 20 年的累积效应使得 **专利申请数** (Panel A) 和 **定居该州的发明人数量** (Panel B) 显著增加大约 $2\%$：弹性系数约为 2。
- **公司税：** 如果在 $t=0$ 将公司所得税净税率 $(1-Corp.TAX)$ 提高 $1\%$，则 20 年的累计效应使得 **专利申请数** (Panel C) 和**定居该州的发明人数量** (Panel D) 显著增加大约 $3\%$-$4\%$：弹性系数约为 $3$-$4$。

--- - --

> Source: Akcigit, et al. ([2022](https://doi.org/10.1093/qje/qjab022), [PDF](http://sci-hub.ren/10.1093/qje/qjab022)), Tab 5. 虚线表示 90% CI   
> **个税：** 个人所得税净税率 $(1-MTR90)$ 提高 $1\%$，则 20 年的累积效应会使得 **专利申请数** (Panel A) 和 **定居该州的发明人数量** (Panel B) 显著增加大约 $2\%$。

![](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220714111224.png)

--- - --

> Source: Akcigit, et al. ([2022](https://doi.org/10.1093/qje/qjab022), [PDF](http://sci-hub.ren/10.1093/qje/qjab022)), Tab 5. 虚线表示 90% CI  
> **公司税：** 公司所得税净税率 $\small (1-Corp.TAX)$ 提高 $1\%$，则 20 年的累计效应使得 **专利申请数** (Panel C) 和**定居该州的发明人数量** (Panel D) 显著增加约 $3\%$-$4\%$。

![](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220714111345.png)


--- - --
<!-- backgroundColor: #DFFFF9 -->
### 扩展 ：动态模型 Panel-ARDL(1, p)
考虑到创新行为具有时序相关性，可以基于部分调整模型或理性预期模型，将 (10) 式设定为动态 Panel ARDL 模型：

$$\small
\begin{aligned}
y_{st}=\delta_{t} + {\color{red}{\rho y_{st-1}}} + \sum_{{\color{blue}{l=0}}}^{p} \beta_{l}Tax_{it-l}  + \Delta X_{s t-1}^{\prime} v+\epsilon_{s t}
\end{aligned} \quad (10a)
$$

其中，$\small y_{st}=Y_{s t}-Y_{s t-1}$，$\small Tax_{it} = \ln \left(1-T_{s t-l}\right)-\ln \left(1-T_{s t-l-1}\right)$

长期效应定义为：
$$
\widehat{\theta}_l = \frac{\sum_{l=0}^{p} \widehat{\beta}_{l}}{1-\rho}
$$

--- - --
<!-- backgroundColor: #FFFFF9 -->
##### 简化代码
```stata
 use "$D/Akcigit2022_state_temp.dta", clear 	
 global controls "LD.(real_gdp_pc population_density rd_credit)"
 gen y = D.lnpat   
 global maxlags = 20 
 #d ;
 gen beta =. ;  gen se =. ;  gen upper_CI95 =. ;  gen lower_CI95 =. ;
 gen time = _n in 1/$maxlags;  
 #d cr
 
 forvalues lag=0/$maxlags{;
   reghdfe y  L.y  L(0/${maxlags}).x LD.top_corp  $controls ///
           [aw=pop1940], absorb(year) vce(cluster statenum) 
    local betax "_b[L0.x]"
    forvalues p=1/`lag'{
        local betax  `betax' + _b[L`p'.x]
    }
    nlcom (`betax') / (1-_b[L.y]), post  // Long-run Effects ooooooooooooooo
    qui replace beta = r(b)[1,1] if time== `lag' 
    qui replace se  = sqrt(r(V)[1,1])  if time== `lag'
    qui replace upper_CI95 = beta + 1.96*se if time== `lag'
    qui replace lower_CI95 = beta - 1.96*se if time== `lag'
 }
```

--- - --

```stata
tw  (connect beta time)     ///
    (line upper_CI95 time)  ///
    (line lower_CI95 time)
```
![bg left:55% w:660](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/gr_Akcigit2022_Figure4_a_dynamic.png)


--- - --

### 其它扩展

- 使用 FD-GMM / SYS-GMM (`xtabond` / `xtdpdsys`) 估计 Panel-ARDL (1, $\small p$) 模型
- 考虑 Common Factors 导致的空间相关性，采用 MG 估计量，或 CS-DL 估计量进行估计。
  - `xtmg`, `xtpmg`, `xtdcce2`
- 参见：
  - Eberhardt, M., **2022**, Democracy, growth, heterogeneity, and robustness, 
**European Economic Review**, 147: 104173. [-Link-](https://doi.org/https://doi.org/10.1016/j.euroecorev.2022.104173), [-PDF-](https://sci-hub.ren/https://doi.org/10.1016/j.euroecorev.2022.104173), [Replication](https://github.com/lezme/markuseberhardt/tree/gh-pages/EER%20Replication%20files) 
  - Ditzen, J. **2021**. Estimating long-run effects and the exponent of cross-sectional dependence: An update to `xtdcce2`. **Stata Journal**, 21 (3): 687-707. [Link](https://doi.org/10.1177/1536867x211045560), [PDF1](http://sci-hub.ren/10.1177/1536867x211045560). [PDF2](http://pro1.unibz.it/projects/economics/repec/bemps81.pdf), [-Slides-](https://www.stata.com/meeting/nordic-and-baltic19/slides/nordic19_ditzen.pdf).

--- - --
<!-- backgroundColor: #C1CDCD -->

## 应用文献 - 2：民主与长期经济增长

<br>

Eberhardt, M., **2022**,   
Democracy, growth, heterogeneity, and robustness,   
**European Economic Review**, 147: 104173. 
[-Link-](https://doi.org/10.1016/j.euroecorev.2022.104173), [-PDF1-](https://sci-hub.ren/10.1016/j.euroecorev.2022.104173), [-PDF2-](https://lezme.github.io/markuseberhardt/ANRR_new.pdf), [Replication](https://github.com/lezme/markuseberhardt/tree/gh-pages/EER%20Replication%20files)


--- - --
<!-- backgroundColor: #FFFFF9 -->
### 概要

- 实证研究了各国民主化 (**Dem**) 对长期增长 ( **y** ) 的 **异质性** 影响。
- 虽然现有文献认识可能存在异质性，但现有研究都是在同质性假设下进行的。
- 本文基于 Panel ARDL 模型，在动态异质性设定下估计了 Dem 和 y 的长期关系
- 结果表明，长期来看，民主对人均收入有正向显著影响，约为 10\% (约为近期文献估计值的一半)。
- 作者还分析了各国异质性 “**民主红利**” 的模式。

--- - --

### 主要方法

- `xtmg` (MG) + `xtabond` + `ivreg2` + `regife`
- MG + DID + Interactive Fixed Effects
- MG + quantile regression 
- MG + IV
- 稳健性检验：删样本、改变估计方法
  - 4.1. Sample reduction by minimum observation count
  - 4.2. Sample reduction by sample end year

--- - --

### Why MG estimator ? 忽略异质性政策效应会导致 2SLS 失效

- An important aside on pooled 2SLS regressions.
- **heterogeneity misspecification**: modelling a heterogeneous relationship with a pooled (homogeneous) model, **violates** the basic assumptions of 2SLS estimators (Pesaran and Smith, 1995): 
- If the true coefficient on the variable of interest $x_{i t}$ is $\beta_{i}$, yet the implementation imposes $\beta$, the error term $\varepsilon$ by construction contains $\left(\beta_{i}-\beta\right) x_{i t}$. It is now easy to see that due to the presence of $\left(\beta_{i}-\beta\right) x_{i t}$ in the error no potential instrument $z$ can both be relevant, $E\left[z_{i t} x_{i t}\right] \neq 0$, and valid, $E\left[z_{i t} \varepsilon_{i t}\right]=0$. 
- This econometric argument highlights the serious implications for any claims of '**causal inference**' when heterogeneity is ignored.

--- - --

### 模型设定：静态 - _**MG**

对于经历过政权更迭的国家 ( 从民主&larr;&rarr;专制 )，本文设定了如下静态回归模型

$$y_{i t}=\alpha_{i}+\theta_{i} \operatorname{Dem}_{i t}+\boldsymbol{\beta}_{i}^{\prime} \boldsymbol{X}_{i t}+\delta_{i}^{y} \bar{y}_{t}+\boldsymbol{\delta}_{i}^{X^{\prime}} \overline{\boldsymbol{X}}_{t}+\varepsilon_{i t} \quad (4)$$

- $y$ is per capita GDP (in logs and multiplied by 100), 
- $\small Dem$ is the democracy dummy, and 
- $\boldsymbol{X}$ is a set of controls (gross investment share of GDP and trade openness). 
- $\bar{y}$ and $\overline{\boldsymbol{X}}$ 是控制组 (从未经历过政权更迭的国家) 的截面均值 (年度均值)
- **Pesaran (2006)**, **Westerlund and Urbain (2015)**: the use of cross-section averages is very simple yet powerful in capturing a **common factor structure**.

--- - --

### Stata 实操：
> 文中 Table 1 - Panel (a) - Col (1) &emsp; `16.624`
$$y_{i t}=\alpha_{i}+\theta_{i} \operatorname{Dem}_{i t}+\boldsymbol{\beta}_{i}^{\prime} \boldsymbol{X}_{i t}+\delta_{i}^{y} \bar{y}_{t}+\boldsymbol{\delta}_{i}^{X^{\prime}} \overline{\boldsymbol{X}}_{t}+\varepsilon_{i t} \quad (4)$$
```stata
xtmg y dem  l0ddem l1ddem l2ddem ///
      if dem_sample==1 & c==1, robust
```
--- - --

### 模型设定：动态 (CS-DL) - _**C&K MG**_
The dynamic variant of Eq. (4) is: [Chan and Kwok (2022)](https://doi.org/10.1080/07350015.2021.1914636)
$$
\begin{aligned}
y_{i t}=& \alpha_{i}+ {\color{red}{\theta_{i}^{*}}} \operatorname{Dem}_{i t}+ {\color{red}{\boldsymbol{\beta}_{i}^{*^{\prime}}}} \boldsymbol{X}_{i t} + \sum_{\ell=0}^{p-1} \omega_{i \ell}^{D} {\color{blue}{\Delta \mathrm{Dem}_{i, t-\ell}}} + \sum_{\ell=0}^{p-1} \omega_{i \ell}^{X^{\prime}} {\color{blue}{\Delta \boldsymbol{X}_{i, t-\ell}}} \\
&+\sum_{\ell=0}^{p_{\bar{y}}} \delta_{i \ell}^{* y} \bar{y}_{t-\ell}+\sum_{\ell=0}^{p_{\bar{X}}} \delta_{i \ell}^{* X^{\prime}} \overline{\boldsymbol{X}}_{t-\ell}+\varepsilon_{i t},
\end{aligned}
$$
- **短期效应：** the two terms involving sums in ${\color{blue}{\Delta \mathrm{Dem}_{i, t-\ell}}}$ and ${\color{blue}{\Delta \boldsymbol{X}_{i, t-\ell}}}$ capture the **short-run** effects
- **长期效应**：${\color{red}{\theta_{i}^{*}}}$ and ${\color{red}{\boldsymbol{\beta}_{i}^{*^{\prime}}}}$ represent the **long-run** coefficients for the effects of democracy and additional controls on income per capita, respectively - I use stars to indicate that the interpretation

--- - --

### 模型设定及解释

- The use of this '**CS-DL**' version of the [Chan and Kwok (2022)](https://doi.org/10.1080/07350015.2021.1914636) approach is convenient since the long-run democracy coefficient, $\theta_{i}^{*}$, can be estimated in a single step rather than two as in an error-correction specification or the ANRR ARDL implementations.
- Following suggestions in Chudik et al. (2016) I adopt $\small p_{\bar{y}}=0$ and $\small p=p_{\bar{X}}= \mathrm{int}\left(T^{1 / 3}\right)=3$, where $\small T$ is the time dimension of the panel. My presentation below

--- - --

![](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220713212747.png)

1. [该表](https://www.sciencedirect.com/science/article/pii/S0014292122000976#bb21) 另外 [三栏](https://ars.els-cdn.com/content/image/1-s2.0-S0014292122000976-fx1001.jpg) 采用其它方式式定义「民主」，结果没有发生实质性变化。The four alternative democracy dummies are by [Acemoglu et al. (2019)](https://www.sciencedirect.com/science/article/pii/S0014292122000976#b3) – ANRR, [Boix et al. (2013)](https://www.sciencedirect.com/science/article/pii/S0014292122000976#b15) – BMR, [Cheibub et al. (2010)](https://www.sciencedirect.com/science/article/pii/S0014292122000976#b20) – CGV, and [Papaioannou and Siourounis (2008)](https://www.sciencedirect.com/science/article/pii/S0014292122000976#b42).   
2. (1) and (3) simple **Mean Group** estimator, (2) and (4) Chan and Kwok (C&K) **DID Mean Group** estimator


--- - --

### 稳健性检验 - 1 
###### Eberhardt, M. ([2022](https://doi.org/https://doi.org/10.1016/j.euroecorev.2022.104173), [PDF](http://sci-hub.ren/https://doi.org/10.1016/j.euroecorev.2022.104173)) Fig. 2 (a)

<br>

![bg left:50% w:600](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220713214605.png)

使用最小二乘法 (`reg`), 中位数回归 (`qreg`) 和稳健回归 (`rreg`) 比较了「大众」民主化和「偏向精英」民主化两组样本的 LR 系数。

> [Stata Codes](https://github.com/lezme/markuseberhardt/blob/gh-pages/EER%20Replication%20files/Figure%202%20-%20Analysing%20Heterogeneity.do)


--- - --
<!-- backgroundColor: #FFFFF9 -->
### 应用文献 - 3
- Aslan, A., E. Dogan, B. Altinoz. 2019, Chapter 4 - single-country versus multiple-country studies[C], in B. Özcan,I. Öztürk eds, Environmental kuznets curve (ekc), Academic Press,  25-36. [-Link-](https://doi.org/https://doi.org/10.1016/B978-0-12-816797-7.00004-7), [-PDF1-](https://sci-hub.ren/https://doi.org/10.1016/B978-0-12-816797-7.00004-7)
  - 介绍了环境经济学领域应用 ARDL 模型的文献
- ardl 命令
  - Kripfganz, S., and D. C. Schneider (2022). `ardl`: Estimating autoregressive distributed lag and equilibrium correction models. Manuscript under review by the Stata Journal. [-PDF-](http://www.kripfganz.de/research/Kripfganz_Schneider_ardl.pdf), [-Slides-](http://repec.org/usug2018/uk18_Kripfganz.pdf), [Discussion at Statalist](https://www.statalist.org/forums/forum/general-stata-discussion/general/1434232-ardl-updated-stata-command-for-the-estimation-of-autoregressive-distributed-lag-and-error-correction-models)
  - `net install ardl, from(http://www.kripfganz.de/stata/)`

--- - --

### 应用文献 - 4
- Eberhardt, M., C. Helmers, H. Strauss, **2013**, Do spillovers matter when estimating private returns to r&d?, **Review of Economics and Statistics**, 95 (2): 436-448. [-Link-](https://doi.org/10.1162/REST_a_00272), [-PDF-](https://sci-hub.ren/10.1162/REST_a_00272), ，[WP version](https://www.google.com/url?q=https%3A%2F%2Flezme.github.io%2Fmarkuseberhardt%2Fr%26d.pdf&sa=D&sntz=1&usg=AOvVaw3EeMaUde0pYKaqwgDj-mTV), [Appendix](https://github.com/lezme/markuseberhardt/blob/gh-pages/REStatTA.pdf), [Replication](https://github.com/lezme/markuseberhardt/tree/gh-pages/Data%20and%20Do-files%20MS14022), [-cited-](https://xs2.dailyheadlines.cc/scholar?cites=4181591381375402166&as_sdt=2005&sciodt=0,5&hl=zh-CN)
- Goldberg, J. 2016. "Kwacha gonna do? Experimental evidence about labor supply in rural malawi". American Economic Journal: Applied Economics, 8 (1): 129-149. [Link](https://doi.org/10.1257/app.20130369), [Link](https://www.aeaweb.org/articles?id=10.1257/app.20130369), [PDF](http://sci-hub.ren/10.1257/app.20130369), [Replication](http://doi.org/10.3886/E116331V1)
  - OLS 估计工资弹性，简单使用了 ARDL 模型，主要是解释变量的滞后项

- Ahmed, W. M. A., **2020**, Stock market reactions to domestic sentiment: Panel cs-ardl evidence, **Research in International Business and Finance**, 54: 101240. [-Link-](https://doi.org/https://doi.org/10.1016/j.ribaf.2020.101240), [-PDF-](https://sci-hub.ren/https://doi.org/10.1016/j.ribaf.2020.101240), [Replication](https://data.mendeley.com/datasets/kppptn9vtg/1)



--- - --

### 
<!-- backgroundColor: #C1CDCD -->
## 参考文献

--- - --
<!-- backgroundColor: #FFFFF9 -->

### 参考文献-1

- Hansen B E . 2021. Econometrics. Princeton University Press. [Data and Contents](https://www.ssc.wisc.edu/~bhansen/econometrics/), [PDF](https://www.ssc.wisc.edu/~bhansen/econometrics/Econometrics.pdf), Sec 14.41-43. 
  - 介绍了 ARDL 的基本设定和长期效应的估算公式。
- Pesaran, M. H. Time series and panel data econometrics[M]. Oxford University Press, 2015. [Link](https://xs2.dailyheadlines.cc/books?hl=zh-CN&lr=&id=5jITDAAAQBAJ&oi=fnd&pg=PP1&ots=GoClMYVT_f&sig=m4JhzfS2AKDisbWZTuCnuKcR_FE). 
  - 该书第 6 章提供了此类模型的理论基础，也讲解了部分调整模型、各类理性预期模型与 ARDL 之间的关系。 
- Ghysels, E., M. Marcellino, 2018, Applied economic forecasting using time series methods, Oxford University Press. [-Link-](https://global.oup.com/academic/product/applied-economic-forecasting-using-time-series-methods-9780190622015), [PDF](https://file-lianxh.oss-cn-shenzhen.aliyuncs.com/Refs/Books/Ghysels-2018-Applied-Economic-Forecasting-using-Time-Series-Methods.pdf), [-Codes-R-Eviews](https://www.dropbox.com/sh/sillgeh8x4grldp/AADas6dbmREz_YRzyDrXtuv-a?dl=0), [-Replication-](https://github.com/arlionn/Ghysels-2018-Applied_Economic-R)

--- - --

### 参考文献-2
- Levendis, J. D. Time series econometrics: Learning through replication[M]. Springer, 2019. [-Link-](https://doi.org/10.1007/978-3-319-98282-3), [-PDF1-](https://sci-hub.ren/10.1007/978-3-319-98282-3), [PDF2](https://file-lianxh.oss-cn-shenzhen.aliyuncs.com/Refs/Books/Levendis_2019_Time-Series-Econometrics-Learning-Through-Replication.pdf)
- Levendis, J.D. (2018). Cointegration and VECMs. In: Time Series Econometrics. Springer Texts in Business and Economics. Springer, Cham. [Link](https://doi.org/10.1007/978-3-319-98282-3_12),[PDF](https://link.springer.com/content/pdf/10.1007/978-3-319-98282-3_12.pdf). 
  - 本章介绍了误差修正模型的经济含义和推导过程，是理解 ARDL 模型中长期和短期关系的基础。  
- Kahn, M. E., K. Mohaddes, R. N. C. Ng, M. H. Pesaran, M. Raissi,J.-C. Yang, 2021, Long-term macroeconomic effects of climate change: A cross-country analysis, Energy Economics, 104: 105624. [-Link-](https://doi.org/10.1016/j.eneco.2021.105624), [-PDF1-](https://sci-hub.ren/10.1016/j.eneco.2021.105624), [-PDF2-](https://www.nber.org/system/files/working_papers/w26167/w26167.pdf), [-Replication-](http://dx.doi.org/10.17632/hytzz8wftw), [Cited](https://xs2.dailyheadlines.cc/scholar?cites=6240333152230290872&as_sdt=2005&sciodt=0,5&hl=zh-CN).
  - 这是 Panel-ARDL 模型目前最主流的用法。
- Stata 命令：`ardl`, `dynardl`, `xtdcce2`, `reghdfe`  

--- - --

### 参考文献-3
- Ditzen, J. **2021**. Estimating long-run effects and the exponent of cross-sectional dependence: An update to `xtdcce2`. **Stata Journal**, 21 (3): 687-707. [Link](https://doi.org/10.1177/1536867x211045560), [PDF1](http://sci-hub.ren/10.1177/1536867x211045560). [PDF2](http://pro1.unibz.it/projects/economics/repec/bemps81.pdf), [-Slides-](https://www.stata.com/meeting/nordic-and-baltic19/slides/nordic19_ditzen.pdf). PPT 中主要模型的 Stata 实现
- **共同相关效应**：Pesaran M H. Estimation and Inference in Large Heterogeneous Panels with a Multifactor Error Structure[J]. Econometrica, 2006, 74(4):967-1012. [-PDF-](https://www.sci-hub.ren/10.2307/3805914)
- **动态共同相关效应**：Chudik A, Pesaran M H. Common correlated effects estimation of heterogeneous dynamic panel data models with weakly exogenous regressors[J]. Journal of Econometrics, 2015, 188(2): 393-420. [-PDF-](https://www.sci-hub.ren/10.1016/j.jeconom.2015.03.007)
- **截面相关检验 — CD 检验**：Pesaran M H. Testing Weak Cross-Sectional Dependence in Large Panels[J]. Econometric Reviews, 2015, 34(6-10): 1089–1117. [-PDF-](https://www.sci-hub.ren/10.1080/07474938.2014.956623)

--- - --
### 参考文献-4
- **截面相关检验 — BPK 检验**：Bailey N G, Kapetanios M H, Pesaran M H. Exponent of cross-sectional dependence: estimation and inference[J]. Journal of Applied Econometrics, 2016, 31: 929-960. [-PDF-](https://www.sci-hub.ren/10.1002/jae.2476) 
- **Stata 命令 `xtdcce2`**：Jan Ditzen. Estimating Dynamic Common-Correlated Effects in Stata[J]. Stata Journal, 2018, 18(3): 585–617. [-PDF-](https://www.sci-hub.ren/10.1177/1536867x1801800306)
- **学习指南**：如果想要更加深入学习动态共同相关效应的具体用法，一方面可以阅读上述所列的论文，另一方面可以查看 `xtdcce2` 应用 [文献 1](https://xs2.dailyheadlines.cc/scholar?cites=15567514879509263685&as_sdt=2005&sciodt=0,5&hl=zh-CN) 和 [文献 2](https://xs2.dailyheadlines.cc/scholar?hl=zh-CN&as_sdt=0%2C5&q=xtdcce++stata&btnG=)，及 中文解读：[面板数据模型-xtdcce2：动态共同相关和截面相关](https://www.lianxh.cn/news/2296811af7839.html)。
- 此外，`xtdcce2` 命令语法和功能处于持续更新状态中，如果要使用 `xtdcce2` , 请认真查阅最新版的 help 文档。

--- - --


- Jordan, S. and A.Q. Philips. 2018. "Cointegration testing and dynamic simulations of autoregressive distributed lag models." Stata Journal 18(4): 902-923. [-PDF-](https://journals.sagepub.com/doi/pdf/10.1177/1536867X1801800409), [-Appli-](https://andyphilips.github.io/dynamac/), [Stata](https://andyphilips.github.io/dynamac/index-Stata.html), [-github-](https://github.com/andyphilips/dynamac), `dynardl`
  - Khan, M.K., J.Z. Teng and M.I. Khan. 2019. "Effect of energy consumption and economic growth on carbon dioxide emissions in Pakistan with dynamic ARDL simulations approach." Environmental Science and Pollution Research: 1-11.
   


--- - --


<center>

# Thanks

<br>
<br>

### [lianxh.cn](https://www.lianxh.cn)