---
layout: post
title: Solr打分计算公式
date: 2017-10-09 22:38
tags: [solr]
---



solr 6.3 版本的打分公式

<!-- more -->

---

**计算公式：**
> 
$$score(q,d) = coord(q,d) \cdot queryNorm(q) \cdot \sum_{t\;in\;q} {(\;tf(t\;in\;d) \cdot idf(t)^2 \cdot t.getBoost() \cdot  norm(t,d)\;)}$$

1. `coord(q,d)：` coordination factor
> how many of the query terms are found in the specified document
> 关键词命中率

2. `queryNorm(q)：` query normalization
> serves as a normalization factor to attempt to make cores between queries comparable  
>
> $$queryNorm(q) = queryNorm(sumOfSquaredWeights) = \frac{1}{\sqrt{sumOfSquaredWeights}}$$  
>
> $sumOfSquaredWeights = q.getBoost()^2 \cdot (idf(t) \cdot t.getBoost())^2$

`sumOfSquaredWeights` 等于每个 `term` 相加。默认 `boost = 1`，则为每个 `term` 的 `idf(t)` 的平方相加

3. `tf(t in d):`  term frequency
> it is a measure of how often a particular term appears in a matching document  
>
$tf(t\;in\;d) = frequency^{1/2}$

4. `idf(t)：` inverse document frequency
> rarer terms give higher contribution to the total score
>
$idf(t) = 1 + \log \left(\frac{docCount\;+\;1}{docFreq\;+\;1} \right)$
* $\log$ 是以 e 为底数的对数；对应 JAVA 中的 $\log$ 方法

5. `norm(t,d)：` normalization factor
> combine the matching document’s boost, the matching field’s boost, and a length-normalization factor that penalizes longer documents  
>
$norm(t,d) = d.getBoost() \cdot lengthNorm(f) \cdot f.getBoost()$
>
> $lengthNorm = 1 \; / \; Math.sqrt(numTerms) $

6. `t.getBoost：` 即每个词的权重，没给则默认为 1







