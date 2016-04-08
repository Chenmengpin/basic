# ExpressionSet 对象简单讲解

> 这个对象其实是对表达矩阵加上样本分组信息的一个封装，由biobase这个包引入。它是eSet这个对象的继承。

## 一个现成例子

> 下面是一个具体的例子，来源于CLL这个包，是用hgu95av2芯片测了22个样本

```R
    > library(CLL)
    > data(sCLLex)
    > sCLLex
    ExpressionSet (storageMode: lockedEnvironment)
    assayData: 12625 features, 22 samples  ##表达矩阵
      element names: exprs 
    protocolData: none
    phenoData
      sampleNames: CLL11.CEL CLL12.CEL ... CLL9.CEL (22 total)
      varLabels: SampleID Disease   ## 样本分组信息
      varMetadata: labelDescription
    featureData: none
    experimentData: use 'experimentData(object)'
    Annotation: hgu95av2 
    > exprMatrix=exprs(sCLLex)
    > dim(exprMatrix)
    [1] 12625    22
    > meta=pData(sCLLex)
    > table(meta$Disease)
    
    progres.   stable 
          14        8 
    > 
```
    根据上面的信息可以看出该芯片共12625个探针，这22个样本根据疾病状态分成两组，14vs8
    这个数据对象就可以打包做很多包的分析输入数据。
    对这个包的分析，重点就是exprs函数提取表达矩阵，pData函数看看该对象的样本分组信息。

## 自己构造ExpressionSet对象

> 根据上面的讲解，我们知道了在这个对象其实很简单，就是对表达矩阵加上样本分组信息的一个封装。
> 所以我们就用上面得到的exprMatrix和meta来构建一个ExpressionSet对象，biobase包里面提供了详细的说明

```R
    metadata <- data.frame(labelDescription=c('SampleID', 'Disease'),
                       row.names=c('SampleID', 'Disease'))
    phenoData <- new("AnnotatedDataFrame",data=meta,varMetadata=metadata)
    myExpressionSet <- ExpressionSet(assayData=exprMatrix,
                                     phenoData=phenoData,
                                     annotation="hgu95av2")
    > myExpressionSet
    ExpressionSet (storageMode: lockedEnvironment)
    assayData: 12625 features, 22 samples 
      element names: exprs 
    protocolData: none
    phenoData
      sampleNames: CLL11.CEL CLL12.CEL ... CLL9.CEL (22 total)
      varLabels: SampleID Disease
      varMetadata: labelDescription
    featureData: none
    experimentData: use 'experimentData(object)'
    Annotation: hgu95av2 
    > 
```
> 从上面的构造过程可以看出，重点就是表达矩阵加上样本分组信息

## 其它例子

### ALL包的数据
```R
    library(ALL)
    data(ALL)
    ALL
    
    ExpressionSet (storageMode: lockedEnvironment)
    assayData: 12625 features, 128 samples
        element names: exprs
    protocolData: none
    phenoData
        sampleNames: 01005 01010 … LAL4 (128 total)
        varLabels: cod diagnosis … date last seen (21 total)
        varMetadata: labelDescription
    featureData: none
    experimentData: use ‘experimentData(object)’
    pubMedIds: 14684422 16243790 
    Annotation: hgu95av2
```
这个数据非常出名，很多其它算法包都会拿这个数据来举例子，只有真正理解了ExpressionSet对象才能学会bioconductor系列包 

## 用GEOquery包来下载数据
```R
    gse1009=GEOquery::getGEO("GSE1009")
    gse1009[[1]] ## 这就是ExpressionSet对象
```
