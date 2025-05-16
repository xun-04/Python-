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
![bg right:45% brightness:. sepia:50% w:500](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220804094545.png)

<!--幻灯片标题-->
# 线性回归分析 (2)
## 变量转换 / 弹性系数 / 因子变量

<br>

<!--作者信息-->
[连玉君](https://www.lianxh.cn) (中山大学)
arlionn@163.com

<br>

--- - --

<!-- backgroundColor: white -->

# 1. 变量的缩放和平移

### Why ？
- 美观：便于呈现：
  - Bad：$0.000000456$ 或 $1,234,534,903,875 .$
  - $x_1 \ (\hat{\beta}_1=0.0005)$ &emsp; $\to$ &emsp;  $x_2 = x/1000 \ (\hat{\beta}_2 = 0.5)$   
  - 系数和 SE 会按比例变化，但 **不会** 影响 t-stats 或统计推断
- 可比：便于解释
  - 对 $y$, $x$ 进行标准化。$\tilde{x}=[x-\bar{x}]/\operatorname{sd}(x)$：减去样本平均值，除以样本 SD。
    - `reg y x, beta` &emsp; 
    - `center y x, prefix(c_)` &rarr; `ivregress c_y c_x`
- 亦可用原始数据估计模型，然后将每个系数乘以相应的 SD。
  - Marginal effect $\Longrightarrow \Delta$ in $y$ units for a 1 SD $\Delta$ in $x$


--- - --

## $y \to cy$: Scaling the Dependent Variable

- 将被解释变量乘以常数 $c \Longrightarrow$ OLS 估计系数估计值会放大 $c$ 倍 (同比例变化)
$$
\begin{gathered}
y=\alpha+\beta x+u \\
\Longleftrightarrow \quad c y=(c \alpha)+(c \beta) x + cu
\end{gathered}
$$
- 干扰项的方差会受到影响 ($RSS_c =  c^2 RSS$)

## $x \to cx$: Scaling the Independent Variable
- 将解释变量 $x$ 放大 $c$ 倍 $\Longrightarrow$ 常数项不受影响；斜率变为原来的 $1/c$ 倍
$$
\begin{aligned}
y &=\alpha+\beta x+u \\
\Longleftrightarrow \quad &=\alpha+(\beta / c) c x+u
\end{aligned}
$$

### 共同点
- $R^{2}$ 和 $t$ 统计量不受影响


--- - --

```stata
. sysuse "nlsw88.dta", clear
. gen lnwage      = ln(wage)
. gen lnwage_x_10 = lnwage*10  // y*10
. gen age_sq      = age*age 
. gen age_sq_100  = age_sq/100  // x/100

. eststo m1: reg lnwage       hours age age_sq 
. eststo m2: reg lnwage       hours age age_sq_100
. eststo m3: reg lnwage_x_10  hours age age_sq 
 
. esttab m1 m2 m3, nogap scalar(r2 rss) 
------------------------------------------------------------

N = 2242         (1) lnwage       (2) lnwage   (3) lnwage_x_10
------------------------------------------------------------
hours              0.0112***       0.0112***        0.112***
                   (9.88)          (9.88)          (9.88)   
age                 0.141           0.141           1.412   
                   (1.33)          (1.33)          (1.33)   
age_sq           -0.00183                         -0.0183   
                  (-1.36)                         (-1.36)   
age_sq_100                         -0.183                   
                                  (-1.36)                   
_cons              -1.256          -1.256          -12.56   
                  (-0.60)         (-0.60)         (-0.60)   
------------------------------------------------------------
r2                 0.0430          0.0430          0.0430   
rss                 706.6           706.6         70655.7   
------------------------------------------------------------
```

--- - --

<!-- backgroundColor: #FFFFF9 -->

### 应用：系数报告

- Hansen, B. E., **1999**, Threshold effects in non-dynamic panels: Estimation, testing, and inference, **Journal of Econometrics**, 93 (2): 345-368. [-Link-](https://doi.org/10.1016/S0304-4076(99)00025-1), [-PDF-](https://sci-hub.ren/10.1016/S0304-4076(99)00025-1)

$$
\begin{aligned}
I_{i t}=& \mu_{i}+\theta_{1} Q_{i t-1}+\theta_{2} Q_{i t-1}^{2}+\theta_{3} Q_{i t-1}^{3}+\theta_{4} D_{i t-1} \\
&+\theta_{5} Q_{i t-1} D_{i t-1}+\beta_{1} C F_{i t-1} I\left(D_{i t-1} \leqslant \gamma_{1}\right) \\
&+\beta_{2} C F_{i t-1} I\left(\gamma_{1}<D_{i t-1} \leqslant \gamma_{2}\right)+\beta_{3} C F_{i t-1} I\left(\gamma_{2}<D_{i t-1}\right)+e_{i t} \quad (24)
\end{aligned}
$$

![h:370](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220629145919.png)

--- - --
> Paravisini, D., V. Rappoport, P. Schnabl, D. Wolfenzon, **2015**, Dissecting the effect of credit supply on trade: Evidence from matched credit-export data, **The Review of Economic Studies**, 82 (1): 333-359. [-Link-](https://doi.org/10.1093/restud/rdu028), [-PDF-](https://sci-hub.ren/10.1093/restud/rdu028), [Appendix](http://econ.mathematik.uni-ulm.de/ejd/readme_files/restud/2015/restud_82_1_10/MS16482OnLineAppendix.pdf), [Replication](https://academic.oup.com/restud/article/82/1/333/1545669)


![](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220726103726.png)

--- - --
<!-- backgroundColor: white -->

## $y \to cy$, $x\to kx$: Changing Units of Both $y$ and $x$
- 模型:
$$
y=\alpha+\beta x+u
$$
- $y \to cy$，$x\to mx$
$$
\begin{aligned}
c y &=c \alpha+c \beta x+c u \\
c y &=(c \alpha)+(c \beta / m) m x+c u
\end{aligned}
$$
- **intercept** scaled by $c$, **slope** scaled by $c / m$

--- - --

## $x \to x+k$：Shifting $x$ &emsp;  &#x1F34E; 
- 模型:
$$
y=\alpha+\beta x+u
$$
- $x \to x+k$
$$
\begin{aligned}
y&=\alpha+\beta(x+k)-\beta k+u \\
& =(\alpha-\beta k)+\beta(x+k)+u
\end{aligned}
$$
- 截距项变动 $-\beta k$，斜率不变
- 应用：RDD，RKD，参见下页的例子，以及
- 应用：交乘项设定中系数易于解释，参见 [交乘项-交叉项的中心化问题](https://www.lianxh.cn/news/454644a5b7e3b.html)

--- - --
### MC 实例
- DGP: &emsp;  $y = a + \theta D + x\beta + u$, &emsp; $D = I(x>5)$
- RDD 设定：
  - $x_c = x - c \ \Longrightarrow \ x = x_c+c$
  - $y = (a+ {\color{red}{c\beta}}) + \theta D + x_c \beta + u = \gamma + \theta D + x_c \beta + u$

- Keypoint: $E(y\,|\,x_c=0) = \gamma$ &ensp;<font color=green>等价于</font>&ensp; $E(y\,|\,x=c) = \gamma$

```stata
  clear
  set obs 60
  set seed 12345
  gen x = 10*runiform() // force variable
  gen     D=0
  replace D=1 if x>5    // Treat variable
  
  gen e = rnormal()/2   // noise
  gen y  = 3 + 2*D + 0.6*x  + e
  gen xc = x-5   
  label var xc "xc=x-5"
```

--- - --
<!-- backgroundColor: white -->

![h:700](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/OLS_shift_x_RDD_03.png)


--- - --
<!-- backgroundColor: #e6e6fa -->
```stata
*-Graphing 1: raw data  
#d ;
  tw  (scatter y x if x<=5, ms(oh) mc(blue))
      (function y=3+0.6*x if x<=5,range(0 5))
      (scatter y x if x> 5, ms(+) mc(red))
      (function y=5+0.6*x if x<=5,range(5 10) lp(solid) lc(gray))
      (function y=5+0.6*x if x<=5,range(0  5) lp(dash)  lc(gray)),
      xline(5, lp(solid) lc(gray*0.3) lw(*1.5)) 
      ylabel(1(2)11) xlabel(0(2)4 5 6(2)10)
      legend(off) aspect(1)
      note("DGP: y = 3 + 2*I(x>5) + 0.6x + u");
#d cr  
```

```stata
*-Graphing 2: shifted data       xc = x-c
#d ;
  tw  (scatter y xc if xc<=0, ms(oh) mc(blue))
      (function y=6+0.6*x if xc<=0, range(-5 0))
      (scatter y xc if xc>0, ms(+) mc(red))
      (function y=8+0.6*x if xc >0, range( 0 5) lp(solid) lc(gray))
      (function y=8+0.6*x if xc<=0, range(-5 0) lp(dash)  lc(gray)),
      xline(0, lp(solid) lc(gray*0.3) lw(*1.5)) 
      xtitle(xc) ylabel(1(2)11) xlabel(-5 0 5)
      legend(off) aspect(1)
      note("DGP: y = 6 + 2*I(xc>0) + 0.6xc + u (Note: xc = x-5)") ;
#d cr 
```

--- - --

```stata
  *-更简洁的做法
    binscatter2 y x , rd(5) n(60) aspect(1)
    binscatter2 y xc, rd(0) n(60) aspect(1)
    
  *-regression 
    eststo m0: reg y x  D
    eststo m1: reg y xc D
    esttab m0 m1, nogap r2 order(D) nomti label

                                  (1)             (2)   
    ----------------------------------------------------
    D                           2.133***        2.133***
                               (9.74)          (9.74)   
    x                           0.588***                
                              (16.23)                   
    xc=x-5                                      0.588***
                                              (16.23)   
    Constant                    3.010***        5.950***
                              (25.16)         (51.00)   
    ----------------------------------------------------
    Observations                   60              60   
    R-squared                   0.974           0.974   
```

--- - --
<!-- backgroundColor: white -->
## Shifting Both $y$ and $x$

- 模型:
$$
y=\alpha+\beta x+u
$$
- $y \to y+c$，$x \to x+k$
$$
\begin{aligned}
&c+y=c+\alpha+\beta x+u \\
&c+y=c+\alpha+\beta(x+k)-\beta k+u \\
&c+y=(c+\alpha-\beta k)+\beta(x+k)+u
\end{aligned}
$$
- 截距项变动 $(c-\beta k)$，斜率不变



--- - --

<!-- backgroundColor: #C1CDCD -->

# 2. 对数转换


--- - --
<!-- backgroundColor: white -->
## 教育回报问题
- 先看一下基本的 **wage-education** 回归方程：
  $$
  \text{wage}=\alpha+\beta \text{edu} +u
  $$ 
  - 含义：$\small \Delta \text{wage}/\Delta \text{edu} = \beta$，即所有教育水平上的工资水平的变化都相同。
  
  - 例如，若 $\small\widehat{\beta}=0.6$，则教育年限从 5 年增加到 6 年 (5&rarr;6) 所导致的工资水平的变化为 0.6 万元，
    - 而教育年限从 11&rarr;12 年，或 15&rarr;16 年，导致的工资变化也都是 0.6 万元。

  - &#x2753; 假设 $\small \Delta \text{wage}/\Delta \text{edu} = \beta$ 合理吗 ？

<br>
<br>

--- - --

## 变量的对数转换

- 更为合理的假设是，每多一年的教育都会导致工资以恒定的比例（即百分比）增长
$$\small
\log (\text{wage})=\alpha+\beta \text{edu}+u
$$

- 教育增加一单位引起的 **工资变化百分比** (Percent change) 为：
$$\small
\% \Delta \text{wage} \approx(100 \beta) \Delta \text{edu}
$$
- $\small\log (\text{wage})$ 与 $\small\text{edu}$ 之间是线性关系 (对数线性)；$\small\text{wage}$ 与 $\small\text{edu}$ 是非线性关系 (指数)
$$\small
\begin{aligned}
& \log (\text{wage})=\alpha+\beta \text{edu}+u \\
\Longrightarrow & \text{wage}=\exp (\alpha+\beta \text{edu}+u)
\end{aligned}
$$

- 参见 [知乎-伍德里奇导论笔记 I](https://zhuanlan.zhihu.com/p/384591374)
- [Stata codes](https://gitee.com/arlionn/graph/wikis/dofiles/log_transfer)

![bg right:40% w:350](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/log_transfer_03_combine_narrow.png)

--- - --

## lny ~ x


$$
\log (\text{wage})=0.584+0.083 \text{edu}, \quad R^{2}=0.186
$$
- 每增加一年的教育，工资增加 $8.3\%$（注意，这里是工资的百分比变化，而不是工资对数的变化！）。
- 对于没有受教育的人，他们的工资为 $\exp (0.584)$。若样本中 edu 的最小值为 6，则该结果没有意义。

<br>

## lny ~ lnx

$$
\log (\text{ salary }) = 4.822 + 0.257 \log (\text{ sales }), \quad R^{2} = 0.211
$$

- 解释：销售额每增加 1% ，薪水增加 0.257%  
- 截距项没有意义……因为没有公司销售额为 0 。

--- - --
<!-- 
## Changing Units in Log-Level Model
- What happens to intercept and slope if we $\Delta$ units of dependent variable when it's in log form?
$$
\begin{aligned}
& \log (y)=\alpha+\beta x+u \\
\Longleftrightarrow & \log (c)+\log (y)=\log (c)+\alpha+\beta x+u \\
\Longleftrightarrow & \log (c y)=(\log (c)+\alpha)+\beta x+u
\end{aligned}
$$
- Intercept shifted by $\log (c)$, slope unaffected because slope measures proportionate change in log-log model

--- - --

## Changing Units in Level-Log Model
- What happens to intercept and slope if we $\Delta$ units of independent variable when it's in log form?
$$
\begin{aligned}
& y=\alpha+\beta \log (x)+u \\
\Longleftrightarrow & \beta \log (c)+y=\alpha+\beta \log (x)+\beta \log (c)+u \\
\Longleftrightarrow & y=(\alpha-\beta \log (c))+\beta \log (c x)+u
\end{aligned}
$$
- Slope measures proportionate change

--- - -- -->

## 对数转换后的系数含义

| Model | Dependent Variable | Independent Variable | Interpretation of beta |
| :--- | :---: | :---: | :---: |
| Level-level | $y$      | $x$      | $\Delta y=\beta \Delta x$ |
| Level-log   | $y$      | $log(x)$ | $\Delta y=(\beta/100)%\Delta x$ |
| Log-level   | $log(y)$ | $x$      | $\%\Delta y=(100 \beta)\Delta x$ |
| Log-log     | $log(y)$ | $log(x)$ | $\%\Delta y=\beta \%\Delta x$ |

- E.g., In Log-level model, $100 \times \beta=\%$ change in $y$ for a 1 unit increase in $\times(100 \beta=$ semi-elasticity $)$
- E.g., In Log-log model, $\beta=\%$ change in $y$ for a $1 \%$ change in $\times(\beta$ = elasticity)



<!-- 

## 范例：估计劳动力供给弹性系数
> Goldberg, J., **2016**, Kwacha gonna do? Experimental evidence about labor supply in rural malawi, **AEJ: AE**, 8 (1): 129-149. [Link](https://doi.org/10.1257/app.20130369), [PDF](https://sci-hub.ren/10.1257/app.20130369), [-Codes-](http://doi.org/10.3886/E116331V1), [2011-wp](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.661.4360&rep=rep1&type=pdf) 

- **Goal:** 估计马拉维 (Malawi) 农村「日工市场」的 **劳动力供给-工资弹性**。
- **方法：**
  - OLS: 计算弹性
  - Fixed effects
  - SE: robust, clustered SE, wild Bootstrap SE
--- - --

- **实验过程:** 
  - 每周一次，连续 12 周，向 529 名成年人提供工作福利类计划的工作机会。
  - 日工资从工资分配的百分之十到百分之九十不等，个人有权每周最多工作一天。 在这种情况下（农业淡季），74% 的个人以最低工资工作，因此无论可观察到的特征如何，估计的劳动力供应弹性都很低（0.15）。
  - 连续12周，每周一次，我为529名成年人提供工作福利类项目的工作机会。日工资从工资分配的第10%到第90%不等，个人每周最多工作一天。
  - 在这种情况下 (农业淡季)，74%的个人以最低工资工作，因此，无论观察到的特征如何，估计的劳动力供应弹性都很低 (0.15)。

--- - --

### 弹性计算
I estimate the elasticity of employment from data aggregated to 120 village-week observations. I run ordinary least squares regressions of the form
$$\small
\text{ labor }_{t v}=\alpha+\beta \ln \left(\text{wage}_{t v}\right)+\nu_{t v}
$$
- $\beta$ is the **marginal effect** of a one log-point, or approximately one percent, change in wages on the fraction of individuals in village $v$ working in a given week.
- The marginal effect is **not an elasticity**. Transform formula,
$$\small
\epsilon_{e}=\frac{\partial Q}{\partial P} \times \frac{P}{Q} .
$$
- Because the independent variable is **log-wages**, I compute 
  
$$\small\epsilon_{e}=\frac{\beta}{\text{mean}(\text{labor})}$$

--- - --

![](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220610164114.png)

--- - --

**Notes:** 
- This table reports results from OLS estimates. 
- Standard errors clustered at the village level reported in parentheses. 
- Unit of observation is village $\times$ week, sample is all individuals. 
- $p$-value from 999 **wild bootstrap** iterations calculated against a null hypothesis of $\beta=0$. 
- $p$-value from RI calculated from 10 ! $-1$ permutations of the village wage schedule. The RI $p$-value is the fraction of permutations in which the true coefficient falls within the $\alpha$ tail of the distribution.
- `***`, `**`, `*` Significant at the 1%, 5%, and 10% level.


--- - --
## Statistical vs. Economic Significance
- These are not the same thing
- We can have a statistically insignificant coefficient but it may be economically large.
  - Maybe we just have a power problem due to a small sample size, or little variation in the covariate
- We can have a statistically significant coefficient but it may be economically irrelevant.
  - Maybe we have a very large sample size, or we have a lot of variation in the covariate (outliers)
- You need to think about both statistical and economic significance when discussing your results.
- &#x2753; [回归分析](https://www.lianxh.cn/blogs/32.html) &rarr; [抛弃p值？经济显著性与统计显著性](https://www.lianxh.cn/news/e6b0322debd5c.html)

--- - --

## Reporting Regression Results 
> A table of OLS regression output should show the following: [-Source-](https://finance.wharton.upenn.edu/~mrrobert/resources/Teaching/CorpFinPhD/Linear-Regression-Slides.pdf)

1. the dependent variable,
1. the independent variables (or a subsample and description of the other variables),
1. the corresponding estimated coefficients, and standard errors (or t-stats),
1. stars by the coefficient to indicate the level of statistical significance, if any ( 1 star for $5 \%, 2$ stars for $1 \%)$,
1. the adjusted $R^{2}$, and the number of observations used in the regression.

In the body of paper, focus discussion on variable(s) of interest: sign, magnitude, statistical \& economic significance, economic interpretation.  

Discuss "other" coefficients if they are "strange" (e.g., wrong sign, huge magnitude, etc.)  -->


--- - --
<!-- backgroundColor: #F1CDCD -->

# 3. 变量的简洁表示

- **因子变量** `reg wage hours i.race i.industry i.industry#c.year`
  - 详情：[Stata：因子变量全攻略](https://www.lianxh.cn/news/314564eb6d725.html) &emsp; 
  - 帮助：[`help fvvarlist`](https://www.stata.com/manuals/u11.pdf#u11.4.3Factorvariables)；[`help varlist`](https://www.stata.com/manuals/u11.pdf#u11.4varnameandvarlists)
  
- **时序变量** `reg D.wage D.hours D.L.(z1 z2 z3)`
  - 帮助：[`help varlist`](https://www.stata.com/manuals/u11.pdf#u11.4varnameandvarlists)；[`help tsvarlist`](https://www.stata.com/manuals/u11.pdf#u11.4.4Time-seriesvarlists)
--- - --

<!-- backgroundColor: white -->

## 3.1 因子变量

<font color=black size=5>

> 详情：[Stata：因子变量全攻略](https://www.lianxh.cn/news/314564eb6d725.html) &emsp; 帮助：[`help fvvarlist`](https://www.stata.com/manuals/u11.pdf#u11.4.3Factorvariables)；[`help varlist`](https://www.stata.com/manuals/u11.pdf#u11.4varnameandvarlists)

| 符号 | 含义 | 实例 |
| :--- | :--- | :--- | 
| `i.` | 标示为类别变量 | `reg y x i.id i.year` &rarr; $y_{it}=a_i+x_{it}\beta + \lambda_t + u_{it}$|
| `c.` | 标示为连续变量 | | 
| `o.` | 略去某个类别或变量 | | 
| `#` | 交乘项 | `reg y D x i.D#c.x` &rarr; <br>$\quad  y_i = a + D_i\beta_1 + x_i\beta_2 + D_i x_i \gamma + u_i$ <br> `reg y x c.x#i.year` &rarr; $y_{it}=a+x_{it}\beta_{\color{red}{t}} + u_{it}$ |
| `##` | 两个变量及其交乘项  | `reg y x##z` &rarr; $y_i=a+x_i\beta_1+z_i\beta_2 + x_iz_i\beta_3 + u_i$|

</font> 

--- - --

```stata
clear
input group   x
        1     30
        1     50
        2     40
        2     60
        2     80
        3     70
end 
```
```stata
. list group i.group i.group#c.x, clean  

                   1.      2.      3.   1.group#   2.group#   3.group# 
       group   group   group   group        c.x        c.x        c.x  
  1.       1       1       0       0         30          0          0  
  2.       1       1       0       0         50          0          0  
  3.       2       0       1       0          0         40          0  
  4.       2       0       1       0          0         60          0  
  5.       2       0       1       0          0         80          0  
  6.       3       0       0       1          0          0         70
```

--- - --

![bg left:25% w:260](https://fig-lianxh.oss-cn-shenzhen.aliyuncs.com/20220701091100.png)

```stata
. reg x i.group, noheader 

--------------------------------------------------
     x | Coefficient  Std. err.      t    P>|t|   
-------+------------------------------------------
 group |
    2  |     20.000     16.667     1.20   0.316   
    3  |     30.000     22.361     1.34   0.272   
       |
 _cons |     40.000     12.910     3.10   0.053   
--------------------------------------------------


. regfit

x =  40.00 + 0.00*1b.group + 20.00*2.group + 30.00*3.group
    (12.91) (0.00)          (16.67)         (22.36)
     N = 6, R2 = 0.43, adj-R2 = 0.05
```

--- - --

## 因子变量：设定基准组的运算符及含义
假设类别变量 $x$ 的取值为 $x\in \small \{2,5,7,9\}$，每种取值的频数分别为 $\small 20, 50, 70, 10$。


<font color=black size=5>

| 基准组运算符 | 含义 |
| :--- | :--- |
| `ib2.x` | 使用 $x=2$ 作为基准组 |
| `ib(#2).x` | 使用 $x$ 的第二种取值 ($x=5$) 所对应的类别作为基准组，等价于 `ib5.x` |
| `ib(first).x` | 使用变量的最小值所对应的类别作为基准组 (该项为 Stata 默认选项)，等价于 `ib2.x` |
| `ib(last).x` | 使用变量的最大值所对应的类别作为基准组，等价于 `ib9.x` |
| `ib(freq).x` | 使用变量值的频数最大的类别作为基准组，等价于 `ib7.x` |
| `ibn.x` | 不设基准组 |

</font> 

--- - --

### 例 1：种族与工资

模型设定：$ Wage_i = \alpha + black_i\beta_1 + hours_i\beta_2 + black_i \times hours_i {\color{red}{\beta_3}} + u_i$

```stata
. sysuse "nlsw88.dta", clear

*-传统方法
. gen black=1
. replace black=0 if race!=2
. gen black_x_hours = black*hours
. reg wage black hours black_x_hours

*-因子变量法
. reg wage 2.race c.hours 2.race#c.hours
. reg wage 2.race##c.hours //与上一行等价

------------------------------------------------------------------------------
        wage | Coefficient  Std. err.      t    P>|t|     [95% conf. interval]
-------------+----------------------------------------------------------------
        race |
      Black  |     -1.619      1.266    -1.28   0.201       -4.101       0.864
       hours |      0.089      0.012     7.24   0.000        0.065       0.113
race#c.hours |
      Black  |      0.007      0.033     0.22   0.827       -0.057       0.071
       _cons |      4.810      0.474    10.14   0.000        3.880       5.741
------------------------------------------------------------------------------
```

--- - --

模型设定：$ Wage_i = \alpha + B_i\beta_1 + h_i\beta_2 + B_i \times h_i {\color{red}{\beta_3}} + {\color{blue}{h_i^2\beta_4 + B_i\times h_i^2\beta_5}} + u_i$

```stata
. sysuse "nlsw88.dta", clear

. reg wage 2.race##c.hours##c.hours

-------------------------------------------------------------------
                wage |          Coeff       SE        t     P>|t|  
---------------------+---------------------------------------------
                race |
              Black  |         -3.461      2.274    -1.52   0.128  
               hours |          0.133      0.044     3.03   0.002  
                     |
        race#c.hours |
              Black  |          0.123      0.127     0.97   0.331  
                     |
     c.hours#c.hours |         -0.001      0.001    -1.05   0.296  
                     |
race#c.hours#c.hours |
              Black  |         -0.002      0.002    -0.95   0.344  
                     |
               _cons |          4.165      0.778     5.35   0.000  
-------------------------------------------------------------------
```

--- - --
<!-- backgroundColor: #FFFFF9 -->

## 3.2 时序变量的表示
帮助：[`help varlist`](https://www.stata.com/manuals/u11.pdf#u11.4varnameandvarlists)；[`help tsvarlist`](https://www.stata.com/manuals/u11.pdf#u11.4.4Time-seriesvarlists)
&#x1F34F; `reg y L.y L(0/2).x D.L.z` ${\color{red}{\rightarrow\ \ }}$  $\small y_t = a + \rho y_{t-1} + \sum_{s=0}^3 \theta_s x_{t-s} + \Delta z_{t-1} + u_t$


$$
\begin{array}{lll}
\text { Operator } & \text {Meaning } & \\
\hline 
\texttt { L.x } &  x_{t-1} & \text{1-period lag} \\
\texttt { L2.x } &  x_{t-2} & \text{2-period lag} \\
\ldots & \\
\texttt { F.x } &  x_{t+1} & \text{1-period lead} \\
\texttt { F2. } &  x_{t+2} & \text{ 2-period lead } \\
\ldots & \\
\texttt { D. } & \Delta x_t \ \ = x_{t}-x_{t-1} & \text {first difference } \\
\texttt { D2. } & \Delta^2 x_t = (x_{t}-x_{t-1})-\left(x_{t-1}-x_{t-2}\right) & \text{difference of difference} \\
\ldots & \\
\texttt { L(0/2).x } & x_{t}, x_{t-1}, x_{t-2} & \text {sevaral variables} \\
\texttt { L.D.x } & \Delta x_{t-1} & \text {can be nested} \\
\end{array}
$$

--- - --
<!-- backgroundColor: #e6e6fa -->
```stata
. use "https://www.stata-press.com/data/r17/gxmpl1", clear
. format gnp cpi %5.1f

. list year  L(1/3).(gnp cpi)  D.cpi, clean

                   L.      L2.      L3.      L.     L2.     L3.    D. 
       year      gnp      gnp      gnp     cpi     cpi     cpi   cpi  
 ---------------------------------------------------------------------
  1.   1989        .        .        .       .       .       .     .  
  2.   1990   5837.9        .        .   124.0       .       .   6.7  
  3.   1991   6026.3   5837.9        .   130.7   124.0       .   5.5  
  4.   1992   6367.4   6026.3   5837.9   136.2   130.7   124.0   4.1  
  5.   1993   6689.3   6367.4   6026.3   140.3   136.2   130.7   4.2  
  6.   1994   7098.4   6689.3   6367.4   144.5   140.3   136.2   3.7  
  7.   1995   7433.4   7098.4   6689.3   148.2   144.5   140.3   4.2  
  8.   1996   7851.9   7433.4   7098.4   152.4   148.2   144.5   4.5 
```

--- - --

```stata
. use "https://www.stata-press.com/data/r17/invest2", clear
. keep if time<=5&company<=2
. format invest market %4.1f
. list company time invest L(1/2).invest market D1.market

     +--------------------------------------------------------------+
     |                                L.      L2.                 D.|
     | company   time   invest   invest   invest   market    market |
     |--------------------------------------------------------------|
  1. |       1      1    317.6        .        .   3078.5         . |
  2. |       1      2    391.8    317.6        .   4661.7    1583.2 |
  3. |       1      3    410.6    391.8    317.6   5387.1     725.4 |
  4. |       1      4    257.7    410.6    391.8   2792.2   -2594.9 |
  5. |       1      5    330.8    257.7    410.6   4313.2    1521.0 |
     |--------------------------------------------------------------|
  6. |       2      1     40.3        .        .    417.5         . |
  7. |       2      2     72.8     40.3        .    837.8     420.3 |
  8. |       2      3     66.3     72.8     40.3    883.9      46.1 |
  9. |       2      4     51.6     66.3     72.8    437.9    -446.0 |
 1.  |       2      5     52.4     51.6     66.3    679.7     241.8 |
     +--------------------------------------------------------------+
```
