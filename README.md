# Resources
This website is a tutorial repository of meta-analyses and systematic reviews designed specifically for [Cao's group](http://people.ucas.ac.cn/~caoy1) as part of the 2020 CAS Undergraduate Inovation Program.\

*Note:This site is regularly updated by [Ziqian Xia](https://ziqian-xia.github.io/), feel free to reach me by ziqian.research@gmail.com.*

This page will cover resources and tutorials of **meta-analyses** and **systematic reviews** in ecology as well as in environmental research.

## Contents
[1.Meta-Analysis](## Meta-analyses)
- Basics idea behind meta-analysis
- Referred Books and Papers
- R Codes
- New techniques of Meta Analysis

[2.Systematic review](## Systematic review)
- Basics idea behind meta-analysis
- Referred Books and Papers
- R Codes
- New techniques of Meta Analysis

## Meta-analyses
### Basics idea behind meta-analysis

元分析（meta-analysis )统计方法是对众多现有实证文献的再次统计，通过对相关文献中的统计指标利用相应的统计公式，进行再一次的统计分析，从而可以根据获得的统计显著性等来分析两个变量间真实的相关关系。元分析的主要作用就是汇总某一个方面所有符合条件研究的统计结果，评估这个方向研究一个平均效力是什么（假设有很多研究探索喝可乐对男子精子能力的影响，不同的研究可能有不同的结果，元分析就是对这些研究进行一个汇总，并给出判断）。\

1. 元分析的优缺点：\
**优点**\

- 系统(Systematic)
- 透明(Transparent)
- 可复制(Replicable/reproducible)
- 
**缺点**\

- 无法控制的Source bias
- 发表偏差（Publication bias）的存在不可避免地影响结果
- 研究与研究没有可比性
- 研究者下意识的倾向于符合自己假设的显著结果（Agenda-driven bias & researcher allegiance）
- 研究者有意无意的研究失德行为（QRPs）

2. 预注册与元分析\
为了提高研究的透明度，促进研究的规范性、增加公众监督的功能、减少发表偏倚，避免研究机构间不必要的重复试验等，国际上一直在推广进行实验预注册。\

预注册有两种
- 有评审，预注册期刊的一种投稿模式，送到相应的期刊进行同行评审。预注册通过后，不管最后结果如何，均可以发表。

- 没有评审，在OSF等网站上发布预注册信息(可公开，也可保留一段时间)。投稿时将该链接分享给审稿人，增加实验可信度。

这是一篇发表在Naute子刊的预注册元分析：A systematic review and meta-analysis of discrepancies between logged and self-reported digital media use\
预注册的教程：Pre-registration in social psychology—A discussion and suggested template

3. 元分析的流程
请使用[PRISMA流程](http://www.prisma-statement.org/)！
PRISMA 是用于在系统评价和荟萃分析中报告的基于证据的最小项目集。PRISMA 主要侧重于报告评估干预措施效果的评论，但也可用作报告具有评估干预措施以外目标（例如评估病因、流行、诊断或预后）的系统评论的基础。

![Example of PRISMA](https://www.researchgate.net/profile/Rehan-Khan-35/publication/333170634/figure/fig1/AS:776255146307585@1562085057628/PRISMA-model-Preferred-Reporting-Items-for-Systematic-Review-and-Meta-Analysis-PRISMA_W640.jpg)
### Referred Books and Papers
系统的学习元分析，这里有许多很好的教程。主要使用R进行：\

#### Books
- [Doing Meta-Analysis with R: A Hands-On Guide](https://bookdown.org/MathiasHarrer/Doing_Meta_Analysis_in_R/)
- [A Handbook of Statistical Analyses Using R ](https://cran.r-project.org/web/packages/HSAUR2/vignettes/Ch_meta_analysis.pdf)
- [Conducting Meta-Analyses in R with the metafor Package](https://cran.r-project.org/web/packages/metafor/vignettes/metafor.pdf)
#### Papers
##### 指导性的文献
- Meta-analysis in vocational behavior: A systematic review and recommendations for best practices
- A step by step guide for conducting a systematic review and meta-analysis with simulation data
- Method for conducting systematic literature review and meta-analysis for environmental science research
##### 值得学习的，发表在Cell，Nature子刊的元分析文献
- A systematic review and meta-analysis of discrepancies between logged and self-reported digital media use
- Meta-analysis of pro-environmental behaviour spillover
- Alternative meta-analysis of behavioral interventions to promote action on climate change yields different conclusions
- Meta-analyses of factors motivating climate change adaptation behaviour
- An open source machine learning framework for efficient and transparent systematic reviews
- Consequences of recreational hunting for biodiversity conservation and livelihoods

### R Codes
本文的代码修改自Nature中发表过的元分析文献，我会进行持续更新。包含了从分析-检验-作图的一系列代码。重点代码进行了注释。
```r
install.packages("metafor")
install.packages("psych")
install.packages("normtest")

library(metafor)
library(psych)
library(normtest)

setwd("X:/R")

#load datafile
datafile <- read.csv2("X:/R/CCperception.csv")

#Descriptives
min(datafile$r)
max(datafile$r)
sum(datafile$N)
mean(datafile$N)
summary(datafile)

#main analysis
es <- escalc(measure = "ZCOR", ri=datafile$r, ni=datafile$N)
hist(es$yi, breaks = 10, xlab = "effect sizes cc belief", main = "Climate change belief")
set.seed(1);skewness.norm.test(es$yi)
set.seed(1);kurtosis.norm.test(es$yi)

datafile <- cbind(datafile, data.frame(es))
result <- rma(yi, vi, data = datafile)
summary(result)
predict(result, transf=transf.ztor) 
forest(result, slab = paste(datafile$Author))
funnel(result)

#Publication bias and outlier analyses
regtest(result)
(taf <- trimfill(result))
funnel(taf)
inf <- influence(result)
plot(inf, layout=c(1,1))
fsn(yi, vi, data=datafile, type="Rosenthal")
```

## Systematic review
Systematic review
