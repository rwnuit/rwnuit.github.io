# 测试用论文


一种传染病动力学建模方法。 <!--more-->

# <center>Covid-19仓室建模方法概述及应用举例</center>

<center><div style='height:2mm;'></div><div style="font-family:华文楷体;font-size:9pt;"></div></center>
<center><span style="font-family:华文楷体;font-size:9pt;line-height:9mm"></span>
</center>
<div>
<div style="width:52px;float:left; font-family:方正公文黑体;">摘要</div>
<div style="overflow:hidden; font-family:华文楷体;">本文对以SIR为基础的传染病仓室数学建模方法进行了简要概述，随后以Fernández、Charles(2022)提出的SIRD模型为基础，对2020年湖北省疫情数据做了综合分析，对该模型做出基础评估。</div>
</div>
<div>
<div style="width:52px;float:left; font-family:方正公文黑体;">关键词</div> 
<div style="overflow:hidden; font-family:华文楷体;">Compartmental Modeling; SIR; SEIR;</div>
</div>







## Introduction

​		在流行病学研究中，建立传染病传播模型是必要的一环。传染病建模方式多样，在数学建模这一类别中，SIR仓室模型，SEIR模型及其它变种应用较多。SIR将人群分为"Susceptible-Infected-Recovered"三个仓室，总人口随时间在仓室间分配。SEIR模型则对仓室进行了调整——"Susceptible-Exposed-Infected-Recovered"。根据具体的模型设定，仓室人群变动受到一系列参数限制。

​		SIR等仓室模型能够基于有限样本数据，对各期感染、恢复、死亡人数进行预测。模型中的接触率、死亡率能够在一定程度上反映传染病特征，简单的建模手段与及时的预测结果能够评估传染病防控措施的效果与合理性。传染病建模的另一个研究目的在于评估基本再生数的变化。基本再生数为一个具有传染性的个体理论上能够二次传染的人数，其能够反映出社交隔离等疫情防控措施的效果。

​		2019年12月新冠肺炎疫情发生以来，有众多文献对Covid-19传播过程进行了建模分析，其中SIR等仓室模型占比较多。Youssoufa, Aminou(2020)对在Covid-19研究中使用的不同模型进行了总结[^1]， 其中已有的仓室建模方法涵盖了SIR，SEIR，SIQR——"Susceptible-Infectious-Quarantined-Recovered"，SEIQR——"Susceptible-Exposed-Infectious-Quarantined-Recovered"等多类仓室划分。

​		各类仓室模型主要的研究问题包括对各仓室人数，接触率、恢复率等参数的动态估计，以及防疫措施对疾病传播的影响。部分模型进一步考虑了年龄等因素对于疾病传播的影响。

## Compartmental modeling

​		本文使用湖北省在疫情发展初期的数据对仓室模型做一基本应用举例，概述基本再生数$R_0(t)$以及总死亡人数等疫情发展特征。

### Modeling

​		Fernández、Charles提供了一种简易的SIRD建模方式[^2]，以在疫情发展初期对传染病特征、发展趋势进行快速估计和预测。该模型的仓室划分为Susceptible-Infectious-Resolving-Cured-Death。各仓室人数满足：
$$
N=S_t+I_t+R_t+D_t+C_t
$$

| 状态变量 | 定义        |
| -------- | ----------- |
| $S_t$    | Susceptible |
| $I_t$    | Infectious  |
| $R_t$    | Resolving   |
| $D_t$    | Death       |
| $C_t$    | Cured       |

​		该模型基本设定如下，处于易感染状态的人在接触率$\beta_t$下与已感染群体接触后将被感染，模型设定传染性以速率$\gamma$衰减，经过$\frac{1}{\gamma}$时间后患者失去传染性，并被归纳到Resolving仓室中。经过$\frac{1}{\theta}$时间后，患者疾病以两种方式解决，分别归纳到Death和Cured两个仓室中。参数定义如下：

| 参数               | 定义                             | 初始值    |
| ------------------ | -------------------------------- | --------- |
| $\beta_t$          | 接触率                           | $\beta_0$ |
| $\frac{1}{\gamma}$ | 具有传染性的天数                 | 5         |
| $\frac{1}{\theta}$ | 疾病处理天数                     | 10        |
| $\delta$           | 死亡率                           | 0.01      |
| $\alpha$           | $R_0(t)$随每日死亡人数的衰减速率 | 0.05      |



​		模型时变系统如下：
$$
\begin{align}
&\Delta S_{t+1}=-\beta_tS_tI_t/N \\
&\Delta I_{t+1}=\beta_tS_tI_t/N-\gamma I_t \\
&\Delta R_{t+1}=\gamma I_t - \theta R_t\\
\end{align}
$$

$$
\begin{align}
&\Delta D_{t+1}=\delta\theta R_t \\
&\Delta C_{t+1}=(1-\delta)\theta R_t \\
\end{align}
$$



### Estimation

​		该模型的一个特性是将经典SIR仓室模型中的接触率$\beta$由常量改为时变参数，且可以从单日新增死亡人数d及其差分$\Delta d$，$\Delta\Delta d$中估计。通过简化上述方程，可以求解$\beta_t$，从而给出对基本再生数$R_0(t)$的估计。
$$
\begin{align}
&\beta_t=\frac{N}{S_t}(\gamma+\frac{\frac{1}{\theta}\Delta\Delta d_{t+3}+\Delta d_{t+2}}{\frac{1}{\theta}\Delta d_{t+2}+d_{t+1}})\\
&S_{t+1}=S_t(1-\frac{\beta_t}{\delta\gamma N}(\frac{1}{\theta}\Delta d_{t+2}+d_{t+1}))\\
\end{align}
$$


#### Estimates of $R_0(t)$

​		对$R_0(t)$进行估计，可以评价病毒的传播能力和一系列社交限制政策的效果。2020年初新冠疫情发生后，湖北省在1月23日采取了封城措施，最大限度减少了人员流动。本文统计1月23日社交隔离开始到4月16日解封后的单日死亡数据，对$R_0(t)$进行估计。在Fernández, Charles(2022)一文中，常量$\gamma$, $\theta$, $\delta$被认定为生物学特征，不随时间与地域变化，本文将沿用其数值。

​		图1为经HP滤波处理前后的单日死亡人数趋势。原始数据因为统计方式差异和报告日期滞后而存在短期波动，例如政府可能在某一天未报告数据，并选择累积至后一天进行报告。为减小短期波动噪声对参数估计的影响，本文对其进行平滑处理(参数$\lambda$=200)。单日死亡人数在一段时间内持续上升，在2月中旬达到峰值，图2中单日死亡人数差分的变化趋势还表明，在隔离政策施行后，单日死亡人数的增速有所放缓，并在2月中旬开始下降。

<img src="/images/d.svg" alt="d" style="zoom:67%;"/>

<img src="/images/dd.svg" alt="dd" style="zoom:67%;"/>

图3描述了基本再生数$R_0(t)$的变化，图4为对应时期接触率$\beta_t=R_0(t)\gamma$的变化。$R_0$相对于1的大小刻画了传染病的传播能力，假设初始状态易感染群体人数为$S_0=N$，如果$R_0$大于1，疫情会进一步传播；如果小于1，该传染病基本不具备发展能力。如图，以政策施行日为起始，$R_0$大于1，此后$R_0(t)$有明显的下降趋势，在2月上旬，$R_0(t)$下降至临界点。

<img src="/images/r.svg" alt="r" style="zoom:38%;" /><img src="/images/beta.svg" alt="beta" style="zoom:38%;" />



#### Estimates of $I_t/N$

​		图5描述了当期感染人数占总人口比的变化。基于死亡人数的历史数据，每期具有传染性的患者人数及其占比均可被估计出。估计的感染高峰期出现在2月9日前后，具有传染性的患者占总人口比达到了0.12%。

<img src="/images/in.svg" alt="in" style="zoom:67%;" />

### Forecasting

​		该模型使用未来3天的数据来估计当期变量，而样本外的状态变量依赖于以新方式估计的$\beta_t$和$R_0(t)$。依照与Fernández, Charles(2022)相同的处理方式，本文假定，$R_0(t)$有一随每日死亡人数变化的恒定衰减速率$\alpha$， 满足：
$$
R_0(t)=a_0e^{-\alpha d(t)}
$$

$$
R_0(t+1)=(1-\alpha\Delta d(t))R_0(t)
$$

从而：
$$
\beta_{t+1}=(1-\alpha \Delta d(t))\beta_t
$$
由此，$D_{t}$等仓室中的人数变化趋势可以基于已有数据被预测。

<img src="/images/cd.svg" alt="cd" style="zoom:67%;" />

​		本文样本包含了湖北省2020年1月23日至4月16日的数据，由蓝色柱状图代表。图6显示了8种不同的预测方式及其结果。底部7条线由上至下，分别表示利用样本内最近七天的死亡数据进行预测的结果。例如，红色线条表示，利用样本最后一日，也就是4月16日实际的死亡数据进行估计。黑色线条表示利用4月10日的实际死亡数据进行估计，顶部的线条代表基于1月23日死亡人数实际数据进行预测的结果，与事实的偏差最大。利用最近的实际数据进行预测将趋势变得相对更平缓。根据2022年12月13日湖北省新冠肺炎疫情情况通报数据，总死亡人数停止在4512人，利用最新样本进行预测的结果与实际值非常接近，例如，使用样本最后一日死亡数据估计出的总死亡人数最终稳定至4393人。

### Disscussion

​		该模型能够以有限的样本数量估计接触率$\beta_t$和基本再生数$R_0(t)$的变化，以此评估政策作用。在疫情发展初期施行控制人员流动的政策有助于降低接触率，阻止疾病的传播，并延缓感染高峰的到来。图4中$\beta_t$的迅速下降表明政策的有效进行，湖北省疫情基本再生数在封城后一个月内迅速下降到了1以下，这意味着单个有传染性的病人无法将受感染者群体扩大开来。该模型对于$I_t/N$的估计能够对感染高峰期进行预测，这有助于缓解政策压力，为医疗资源筹措提供信号。对总死亡人数的预测能够评估疫情带来的损失体量，为疫后生产生活恢复提供建议。

​		同时该模型存在若干不足。死亡率$\delta$等参数被设定为外生给定，但在年龄结构、区域等方面，其存在一定的异质性。使用不同的值会对估计造成一定的影响。另外，$\beta_t$的估计依赖于单日死亡人数及其变化，如果总死亡人数趋平，该模型无法给出合理的估计，例如，接触率$\beta_t$和$I_t$仓室的人数可能为负。对于这一问题，本文选择在估计$\beta_t$时，对其取值做一限制，当$\beta_t>0$时，其允许被前文所述程序估计，当$\beta_t<0$时，即当死亡人数等观测数据变动非常小时，$\beta_t=0.001$。通过这一限制，各仓室人数得以为正。

​		该模型表现受到疫情发展阶段的影响。如图6所示，如果基于早期数据对总死亡人数进行估计，结果会与实际值有很大差异，Fernández, Charles(2022)认为，在疫情未发展到高峰时，也即单日死亡人数在上升的阶段，预测结果受噪声影响较大。在后期，单日死亡人数下降的阶段，该模型则表现较好。

## Conclusion

​		根据传染病特性，仓室模型需要以特殊方式划分人群，这意味着其难以构建普适模型，使用范围较局限。另外，多数仓室模型主要关注变量预测，缺乏对模型的验证与评价。仓室模型还受到数据质量影响，统计方式带来的误差会影响预测结果。例如，诊断能力的改进以及病例的延迟报告会使数据出现异常的波动，进而影响参数的估计。在这一方面，本文缺乏对数据处理方式的评价。





**参考文献:** 

[^1]:  MOHAMADOU, Youssoufa, HALIDOU, Aminou, et KAPEN, Pascalin Tiam. A review of mathematical modeling, artificial intelligence and datasets used in the study, prediction and management of COVID-19. Applied Intelligence, 2020, vol. 50, no 11, p. 3913-3925.
[^2]:  FERNÁNDEZ-VILLAVERDE, Jesús et JONES, Charles I. Estimating and simulating a SIRD model of COVID-19 for many countries, states, and cities. *Journal of Economic Dynamics and Control*, 2022, vol. 140, p. 104318.




