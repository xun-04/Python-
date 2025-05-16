

# Stata 中的权重问题

## 1. 背景与问题

在区域经济研究中，通常会基于城市或省级数据来分析**人均 GDP**、**失业率**等经济变量。此类变量都是加总数据，在统计和回归分析中，如果不考虑各区域的规模 (如人口或经济总量)，而直接采用算术平均值，可能会导致偏差。

假设我们有两个地区的经济数据：
- **gdp_pc**：人均 GDP（单位：万元），衡量区域内的平均经济产出水平。
- **pop**：人口数（单位：千万人），反映该地区的规模。
- **GDP**：地区总 GDP，公式为 $\text{GDP} = \text{gdppc} \times \text{pop}$。

示例数据呈现如下：

| City  | gdppc |  pop   | 
| :---: | :----: | :---: | 
|   A   |   6    |  70   | 
|   B   |   3    |  30   | 

如果我们想计算两个地区的平均人均 GDP，最常用的方法便是将两个地区的人均 GDP 做算术平均：

$$
m_a 
= \frac{\text{gdppc}_A + \text{gdppc}_B}{2} 
= \frac{6 + 3}{2} 
= \frac{1}{2} \times 6 + \frac{1}{2} \times 3
= 4.5
$$

然而，这种方法隐含的假设并不合理：两个地区具有相同的权重（即各占 $50\%$）。由于两个地区人口差异显著，这种计算结果并不能真实反映整体的经济水平。

更为合理计算方法应该是：

$$
m_w 
= \frac{\text{GDP}_A + \text{GDP}_B}{\text{Pop}_A + \text{Pop}_B} 
= \frac{6 \times 70 + 3 \times 30}{70 + 30} 
= \frac{70}{100} \times 6 + \frac{30}{100} \times 3
= 5.1
$$


### 统一表述

上述两种计算方法可以统一表述为如下加权平均形式 ($w_1 + w_2 = 1$)：

$$
m = w_1 \cdot \text{gdppc}_A + w_2 \cdot \text{gdppc}_B
$$

- 算术平均值的权重。在 $m_a$ 中，假设两个组的权重是相等的：

$$
w_1 = w_2 = 0.5
$$

- 人口加权平均值的权重。在 $m_w$ 中，权重由各组人口占总人口的比例决定：

$$
w_1 = \frac{\text{Pop}_A}{\text{Pop}_A + \text{Pop}_B} = 0.7, \quad w_2 = \frac{\text{Pop}_B}{\text{Pop}_A + \text{Pop}_B} = 0.3
$$



## 使用 Stata 计算加权平均值

在 Stata 中，我们可以通过 `sum` 命令结合选项 `[aweight=pop]` 来计算加权平均值。以下是相应的 Stata 代码示例：

```stata
* 数据输入
clear
input str1 city gdppc pop
           "A"  6     70
           "B"  3     30
end

* 计算算数人均 GDP (m_a)
sum gdppc

* 计算加权平均人均 GDP (m_w)
sum gdppc [aw=pop] 
```

我们也可以用 `regress` 命令计算 $m_a$ 和 $m_w$：

```stata
reg gdppc             // 算术平均值
reg gdppc [aw = pop]  // 加权平均值
```

精简后的输出结果为：

```stata

. * 计算算数人均 GDP (m_a)
. sum gdppc
     Variable |  Obs   Mean 
     ---------+-------------
        gdppc |    2    4.5 

. * 计算加权平均人均 GDP (m_w)
. sum gdppc [aw=pop] 
     Variable |  Obs  Weight   Mean 
     ---------+---------------------
        gdppc |    2     100    5.1 

. reg gdppc
     ------------------------------
     gdppc | Coefficient  Std. err.
     ------+-----------------------
     _cons |     4.5000     1.5000 
     ------------------------------

. reg gdppc [aw = pop]
    (sum of wgt is 100)
    ------------------------------
    gdppc | Coefficient  Std. err.
    ------+-----------------------
    _cons |     5.1000     1.3748 
    ------------------------------
```


### 小结

通过以上分析，我们可以得出以下结论：
- 算术平均值适用于各组权重相等的情况，如对所有组赋予相同的重要性时。
- 加权平均值则考虑了不同组的权重（如人口、经济总量等），能更准确地反映整体水平，尤其在各组规模差异较大的情况下。

在实际数据分析中，选择合适的平均值计算方法至关重要。正确的加权方法不仅能提高结果的准确性，还能更好地服务于数据驱动的决策过程。

### 应用实例
[Akcigit](https://doi.org/10.1093/qje/qjab022) et al. ([2022](http://sci-hub.ren/10.1093/qje/qjab022), QJE, [-Replication-](https://doi.org/10.7910/DVN/SR410I)) 研究了 20 世纪美国公司税和个人税对创新的影响。他们设定了如下模型来估计州级层面上税收-创新弹性：

$$
Y_{s t} = \beta_{p} \ln T_{s t-3}^p + \beta_{c} \ln T_{s t-3}^c +\gamma \mathbb{X}_{s t}+\delta_{t}+\delta_{s}+\varepsilon_{s t} \quad (3)
$$

其中，

- $Y_{s t}$：$s$ 州在 $t$ 时期的创新产出 (专利申请数、专利引用数、发明人数量等) 的自然对数。
- $T_{s t-3}^p$：滞后 3 年期的个人所得税净税率
- $T_{s t-3}^c$：滞后 3 年期的公司所得税净税率
- $\delta_{t}$ 和 $\delta_{s}$：时间和州固定效应，以捕获不可观测的个体和时间趋势效应
- $X_{s t}$ 代表随时间变化的州层级的变量，包括：
  - 滞后 3 期的人口密度 (城市化程度)
  - 滞后 3 期的人均收入水平 (经济发展水平)
  - 滞后 3 期的研发支出抵免 (税收激励)

考虑到州级数据为加总数据，而美国各州在人口数量、经济规模等方面都存在巨大差异，且模型中还包含了人均收入这一控制变量，在估计过程中，作者使用 1940 年各州的人口数 (**pop1940**) 作为抽样权重。

```stata
use "state_data.dta", clear
gen fiveyear = 5*floor(year/5)
egen statenum_fiveyear = group(statenum fiveyear)

global yx  "lnpat  mtr90_lag3 top_corp_lag3"
global cov "L.real_gdp_pc L.population_density"

reghdfe $yx $cov [aw=pop1940],  ///
        absorb(statenum year)   ///
        vce(cluster statenum_fiveyear year)
```

## fweight：频数权重

> Source: [**[U]** Stata Users' Guide: 20.24 Weighted estimation](https://www.stata.com/manuals/u20.pdf#u20.24Weightedestimation=&page=54.35)

频率权重（fweights）是整数，它用来表示每个观察值的重复次数。从统计角度来看，频率权重并不重要，但从数据处理的角度来看，它具有重要意义。考虑以下数据：

```stata
--------------------- D1.dta : Raw Data ----
type    y    x1   x2
 A     22    1    0
 A     22    1    0
 B     22    1    1
 C     23    0    1
 C     23    0    1
 C     23    0    1
```

虽然这份数据包含 6 个观察值，但只有三种形态 (A, B, C)。因此，很多情况下，此类数据会采用更为精简 (高效) 的方式存储：

```stata
-------------------- D2.dta : Compact Data ----
 type   y    x1   x2   freq
 A     22    1    0     2
 B     22    1    1     1
 C     23    0    1     3
```

我们用变量 **freq** 来记录每种形态的观察值在样本中重复出现的次数，以保证 **D1.dta** 和 **D2.dta** 这两份数据包含的信息相同，可以互相转换。

实证分析中，以下两种估计方法是完全等价的：
```stata
. use "D1.dta", clear   // Raw Data
. regress y x1 x2

. use "D2.dta", clear   // Compact Data
. regress y x1 x2 [fweight=freq]
```

至此，我们可以清晰的了解 `[fweight=freq]` 的作用了：对于 **freq = 2** 的观测值，Stata 将其视为 2 行；同理，对于 **freq = 10** 的观测值，Stata 将其视为 10 行。

你可以将次过程解读为：Stata 会根据 **freq** 变量将 **D2.dta** 的数据恢复成 **D1.dta** 的形态；然后基于 **D1.dta** 执行普通的 OLS 回归。当然，在实际计算过程中，我们只需在 OLS 的估计式中加入权重即可。


### 外部命令如何实现加权估计

有些命令，尤其是外部命令，并不支持加权操作。此时，我们可以使用 `expand` 命令（参见 [**[D]** expand](https://www.stata.com/manuals/dexpand.pdf)）将 **D2.dta** 格式的数据转换成 **D1.dta** 格式，进而执行常规的估计即可。

```stata
. use "D2.dta", clear
. expand freq      // 转换为 D1.dta 格式
. usercmd y x1 x2  // 此时无需加权操作
```

一个重要原则是：使用频率权重运行命令的结果，应与在未加权且扩展后的数据上运行该命令的结果相同。因为未加权的重复数据和频率加权数据本质上只是记录相同信息的两种方式而已。

对于面板数据，可以使用 [**[D]** expandcl](https://www.stata.com/manuals/dexpandcl.pdf) 命令进行数据转换。外部命令 `expgen` 和 `lexpgen` 增加了不少扩展功能。

### 应用实例：PSM-最近邻匹配

在因果推断中，经常使用 PSM 解决样本选择偏误问题。本例以最近邻匹配为例，来说明 **fweight** 的用法。

以下数据源于 Stata 手册 [**[TE]** teffects psmatch](https://www.stata.com/manuals/teteffectspsmatch.pdf)，原始数据及研究背景参见 [Cattaneo](https://doi.org/10.1016/j.jeconom.2009.09.023) ([2010](http://sci-hub.ren/10.1016/j.jeconom.2009.09.023))。

本例中，我们想要分析母亲怀孕期间的吸烟状态（**mbsmoke**）对新生儿出生体重（**bweight**）的影响。由于原始数据中吸烟者的平均年龄大于非吸烟者，为了缓解样本选择偏误产生的影响，我们采用 PSM 中的最近邻匹配法，根据母亲年龄（**mage**）进行 1:1 匹配。

由于本例的主要目的是说明「频数权重」在回归分析中的用法，我们仅从原始数据中抽取了 4 个观察值，进而使用 `psmatch2` 命令进行 PSM 分析。

```stata
webuse "cattaneo2", clear

set seed 1234567
sample 4, count by(mmarried)  # 随机抽取 4 个观察值
sort mbsmoke
rename bweight babywei

psmatch2 mbsmoke mage, n(1) out(babywei) // 1:1 最近邻匹配

-----------------------------------------------------------
Variable     Sample | Treated  Controls  Difference  T-stat
--------------------+--------------------------------------
 babywei  Unmatched |  3203.5   3490.75     -287.25   -0.70
                ATT |  3203.5    3434.0     -230.5    -0.32
--------------------+--------------------------------------
```

执行上述命令后，会自动生成一组以 `_` 开头的变量，如 **_pscore** 表示倾向得分值，**_babywei** 表示结果变量 **babywei** 的反事实估计结果。本例中，我们关注的重点是 **_weight** 变量，它表示某个观测值在匹配过程中被选为匹配对的次数：

- **_weight = 1**：该观测值被使用一次，成功匹配一个其他组的观测值；
- **_weight > 1**：该观测值被多次使用，与多个其他组的观测值匹配；
- **_weight = . (缺失值)**：该观测值未找到合适的匹配对象。

本例中，我们希望从控制组 (**非吸烟者**) 中，为每一个处理组 (**吸烟者**) 中的妇女找到一个与其年龄最接近 (1:1 匹配) 的匹配对象。匹配过程中，控制组的观察值可以被重复使用。

```stata
sort mbsmoke _id 
format _pscore _pdif %4.2f
global vlist "mbsmoke mage _pscore babywei _babywei _id _n1 _pdif _weight"
order $vlist
list  $vlist, sepby(mbsmoke) noobs

--------------------------------------------------------------
  mbsmoke mage _pscore babywei _babywei _id  _n1 _pdif _weight
--------------------------------------------------------------
Nonsmoker   19    0.31    2510        .   1    .     .       1
Nonsmoker   24    0.42    3600        .   2    .     .       .
Nonsmoker   25    0.45    4111        .   3    .     .       .
Nonsmoker   34    0.66    3742        .   4    .     .       3
--------------------------------------------------------------
   Smoker   19    0.31    3260     2510   5    1  0.00       1
   Smoker   31    0.59    2552     3742   6    4  0.07       1
   Smoker   32    0.62    3487     3742   7    4  0.05       1
   Smoker   33    0.64    3515     3742   8    4  0.02       1
--------------------------------------------------------------
```

从上述结果可以看出，**Nonsmoker**（非吸烟者）组的观测值并未被完全使用：
  - 第 1 个观测值（年龄 19）与 **Smoker** 组的第 5 个观测值 (`_id=5`，年龄 19) 匹配，**_weight = 1**。
  - 第 4 个观测值（年龄 34）与 **Smoker** 组的第 6、7 和 8 个观测值匹配 (年龄分别为 31, 32, 33)，**_weight = 3**。
  - 第 2 和 3 个观测值未匹配，**_weight = .**。

完成上述匹配后，我们基本上可以保证处理组和控制组父母在年龄方面大体相近 (样本较为平衡)。进行匹配的直接目的是获取结果变量的反事实估计 **_babywei**。

以 `_id=6` 的妇女为例，如果她不吸烟，则她的孩子的出生体重应为 3742 (反事实结果，记为 $y_6(1)$)，而非 2552 (实际观测结果，记为 $y_6(0)$)。因此，对于该妇女而言，吸烟产生的处理效应为 $\widehat{\text{TE}}_6 = y_6(1) - y_6(0) = 2552 - 3742 = -1190 g$。其它吸烟者的处理效应也都可以相似的方式加以计算。最终，平均处理效应为：$\text{ATE} = (1/4)\sum_{j=5}^{8}\widehat{\text{TE}}_j$。

此时，在执行 OLS 估计时，我们实际使用的数据中并不包含 `_id=2` 和 `_id=3` 的观察值，而 `_id=4` 的观察值则需要复制为三行。因此，回归命令设定如下：

```stata
reg babywei mbsmoke [aw=_weight]

--------------------------------------
babywei |   Coeff   Std. err.      t  
--------+-----------------------------
mbsmoke |  -230.5   466.8267  -0.4938 
  _cons |  3434.0   330.0963  10.4030 
--------------------------------------
```

理解了 **[aw=_weight]** 的含义后，我们也可以直接对如下数据执行 `reg babywei mbsmoke` 或 `ttest babywei, by(mbsmoke)` 命令，并得到相同的结果。

```stata
. expand _weight  // 扩充数据
-------------------------------
  mbsmoke mage babywei  _weight
-------------------------------
Nonsmoker   19    2510        1
Nonsmoker   34    3742        1
Nonsmoker   34    3742        1
Nonsmoker   34    3742        1
-------------------------------
   Smoker   19    3260        1
   Smoker   31    2552        1
   Smoker   32    3487        1
   Smoker   33    3515        1
-------------------------------
```




```stata
// pscore 的计算方法
probit mbsmoke mage
predict ps, pr

list ps _pscore in 1/2, clean

              ps     _pscore  
  1.   .30854283   .30854283  
  2.   .42390537   .42390537  
```


### 何时使用频率权重

频率权重通常用于以下场景：
1. **数据压缩**：当数据中有许多相同的观测值时，可以通过频率权重来表示每个观测值的重复次数，减少数据冗余并提高计算效率。
2. **重复数据**：如果数据中某些样本反复出现，使用频率权重可以更准确地反映这些重复数据对整体估计的贡献，避免重复计算。


### 示例代码

假设我们有以下数据集：

```stata
clear
input str1 type y x1 x2 freq
"A" 22 1 0 2
"B" 22 1 1 1
"C" 23 0 1 3
end
```

进行回归分析时，可以使用如下命令：

```stata
* 使用频率权重进行回归
regress y x1 x2 [fweight=freq]
```

Stata 会根据 `freq` 变量的值来加权每个观测值。每个组的 `type` 和 `freq` 共同决定了权重的分配，从而正确地反映出各组的影响。

### 总结

通过引入 `type` 变量，频率权重的使用更加直观，尤其在分组分析时能够明确表示每个组内观测的重复次数。频率权重允许我们在压缩数据的同时，保持数据的实际意义，并确保加权后的回归估计更加准确。在面对重复数据时，频率权重是一个有效的工具，能够帮助提高计算效率并避免偏差。









## 参考文献
- [Akcigit](http://www.ufukakcigit.com/), U., J. Grigsby, T. Nicholas, S. Stantcheva, **2022**, Taxation and innovation in the twentieth century, **The Quarterly Journal of Economics**, 137 (1): 329-385. [-Link-](https://doi.org/10.1093/qje/qjab022), [-PDF-](https://sci-hub.ren/10.1093/qje/qjab022), [-Appendix-](https://oup.silverchair-cdn.com/oup/backfile/Content_public/Journal/qje/137/1/10.1093_qje_qjab022/1/qjab022_onlineappendix.pdf?Expires=1649965153&Signature=soPrpxU1GGqhdNr58Nc6j-gZhttwWtj2XQG5WxaVp-k7ZqVAJOoYz60biLwCcpgYVpVutAw-uJn59pJkQJOuZlMv6DHHRPiIHE2I7CUNOv5c05r1msRmBbFmzntnyXBov2UkywbuJpob1e59Q5fesu5Z7t6RQFOoh8qgVxjlQcTNgcN6YFuFISMPa2GP8zRbQcNxcFuKbRhPyoUMqFI-MJkwVS7pfl162hJ0ZRa0fH9ho7N3FhBGyoN0jAufE1S3vCSeb2FetG7lhS8JGYMb~FMOcpyRpv1hjSiSL52lSl5W2jT18i1uN-k4atVdt3TN-JFWXMk796qn~BvYnM~85Q__&Key-Pair-Id=APKAIE5G5CRDK6RD3PGA), [-cited-](https://xs2.dailyheadlines.cc/scholar?cites=3691559864871282829&as_sdt=2005&sciodt=0,5&hl=zh-CN&scioq=Social+ties+and+the+selection+of+China%27s+political+elite), [-Replication-](https://doi.org/10.7910/DVN/SR410I), [-WP-version-2018-](https://www.ecb.europa.eu/pub/conferences/shared/pdf/20190905_4th_ARC/Stantcheva_Stefanie.pdf), [-幻灯片-](https://www.docin.com/p-2195999965.html), [-作者提要 2018-](https://voxeu.org/article/taxation-and-innovation-20th-century)
- Cattaneo, M. D. (2010). Efficient semiparametric estimation of multi-valued treatment effects under ignorability. Journal of Econometrics, 155(2), 138–154. [Link](https://doi.org/10.1016/j.jeconom.2009.09.023), [PDF](http://sci-hub.ren/10.1016/j.jeconom.2009.09.023), [Google](<https://scholar.google.com/scholar?q=Efficient semiparametric estimation of multi-valued treatment effects under ignorability>).