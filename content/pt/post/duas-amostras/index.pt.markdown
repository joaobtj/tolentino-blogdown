---
title: N maneiras de comparar duas amostras
subtitle: A comparação entre duas amostras é a mais simples e pode ser feita de muitas maneiras diferentes.
summary: A comparação entre duas amostras é a mais simples e pode ser feita de muitas maneiras diferentes.
author: joaobtj
date: '2021-05-10'
slug: duas-amostras
tags:
  - r
  - estatística
lastmod: 
draft: true
featured: no
image:
  caption: ''
  focal_point: smart
  placement: 1
  preview_only: no
projects: []
---

## Comparação entre duas amostras

Existem vários testes descritos na literatura para comparar duas amostras. Este post não tem a intenção de descrever todos estes testes e suas sutilezas, mas sim demonstrar a aplicaçao prática com códigos do R.

Para exemplificar, utilizarei os dados das medidas do comprimento das pétalas de duas espécies de flores: *Iris versicolor* e *Iris virginica*. Estes dados estão no famoso dataset `iris`. 


<a title="C T Johansson, CC BY 3.0 &lt;https://creativecommons.org/licenses/by/3.0&gt;, via Wikimedia Commons" href="https://commons.wikimedia.org/wiki/File:IMG_7911-Iris_virginica.jpg"><img width="200" alt="IMG 7911-Iris virginica" src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/1e/IMG_7911-Iris_virginica.jpg/512px-IMG_7911-Iris_virginica.jpg"></a>

O dataset foi modificado para conter apenas duas espécies (*I. versicolor* e *I. virginica*) e uma coluna (Comprimento das pétalas em centímetros). Além disso, foi traduzido para o português. Para cada espécie, há 50 indivíduos (flores) que compõe a amostra.





```r
library(dplyr)
iris.pt <- iris %>%
  filter(Species == "versicolor" | Species == "virginica") %>%
  mutate(
    especies = factor(Species),
    petalas_comprimento = Petal.Length * 2.54
  ) %>%
  select(especies, petalas_comprimento)
```


<img src="{{< blogdown/postref >}}index.pt_files/figure-html/unnamed-chunk-3-1.png" width="50%" />




Foram selecionados pacotes e funções que aceitam a fórmula do tipo `resposta ~ grupo` em que `resposta`contém os valores observados e `grupo` é um fator (ou vetor) dos grupos correspondentes.


### Teste t de Student

Iniciando pelo mais famoso, o **teste t de Student** é um teste paramétrico usado quando as amostras tem distribuição Normal (ou simétrica). Podemos verificar a proximidade da distribuição normal pelos gráficos densidade e qq-plot:

<img src="{{< blogdown/postref >}}index.pt_files/figure-html/unnamed-chunk-5-1.png" width="100%" />


ou pelo teste de Shapiro-Wilk:


|especies   |         W|   p-valor|
|:----------|---------:|---------:|
|versicolor | 0.9660044| 0.1584778|
|virginica  | 0.9621864| 0.1097754|

O teste t é calculado pela função `t.test`: 


```r
t.test(petalas_comprimento ~ especies, data = iris.pt)
```

```
## 
## 	Welch Two Sample t-test
## 
## data:  petalas_comprimento by especies
## t = -12.604, df = 95.57, p-value < 2.2e-16
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  -3.798545 -2.764815
## sample estimates:
## mean in group versicolor  mean in group virginica 
##                 10.82040                 14.10208
```


### Teste de Wilcoxon-Mann-Whitney

Também chamado de teste de Wilcoxon, é um teste baseado no soma dos postos.


```r
wilcox.test(petalas_comprimento ~ especies, data = iris.pt)
```

```
## 
## 	Wilcoxon rank sum test with continuity correction
## 
## data:  petalas_comprimento by especies
## W = 44.5, p-value < 2.2e-16
## alternative hypothesis: true location shift is not equal to 0
```

```r
coin::wilcox_test(petalas_comprimento ~ especies, data = iris.pt)
```

```
## 
## 	Asymptotic Wilcoxon-Mann-Whitney Test
## 
## data:  petalas_comprimento by especies (versicolor, virginica)
## Z = -8.319, p-value < 2.2e-16
## alternative hypothesis: true mu is not equal to 0
```


### Teste de Van der Waerden

<!-- The Van der Waerden test is a non-parametric test for testing the hypothesis that k sample distribution functions are equal. Van der Waerden's test is similar to the Kruskal-Wallis one-way analysis of variance test in that it converts the data to ranks and then to standard normal distribution quantiles. The ranked data is known as the 'normal scores'. Hence, the Van der Waerden test is sometimes referred to as a 'normal scores test'. -->

<!-- The benefit of Van der Waerden's test is that it performs well compared to ANOVA (analysis of variance) when the group population samples are normally distributed and the Kruskal-Wallis test when the samples are not normally distributed. -->

<!-- The null and alternative hypotheses of the Van der Waerden test can be generally stated as follows: -->

<!-- H0: All of the k population distribution functions are equal. -->
<!-- HA: At least one of the k population distribution functions are not equal and tend to yield larger observations to the other distribution functions. -->



```r
coin::normal_test(petalas_comprimento ~ especies, data = iris.pt)
```

```
## 
## 	Asymptotic Two-Sample van der Waerden (Normal Quantile) Test
## 
## data:  petalas_comprimento by especies (versicolor, virginica)
## Z = -7.8184, p-value = 5.351e-15
## alternative hypothesis: true mu is not equal to 0
```

```r
PMCMR::vanWaerden.test(petalas_comprimento ~ especies, data = iris.pt)
```

```
## 
## 	Van der Waerden normal scores test
## 
## data:  petalas_comprimento by especies
## Van der Waerden chi-squared = 61.127, df = 1, p-value = 5.351e-15
```


### Teste de Kolmogoroc-Smirnov


### Teste de Wald e Wolfowitz (Runs)

### Teste de Mediana



### Monte Carlo


### Teste de permutação de Fisher-Pitman



```r
perm::permTS(petalas_comprimento ~ especies, data = iris.pt)
```

```
## 
## 	Permutation Test using Asymptotic Approximation
## 
## data:  petalas_comprimento by especies
## Z = -7.8248, p-value = 5.084e-15
## alternative hypothesis: true mean especies=versicolor - mean especies=virginica is not equal to 0
## sample estimates:
## mean especies=versicolor - mean especies=virginica 
##                                           -3.28168
```

```r
coin::oneway_test(petalas_comprimento ~ especies, data = iris.pt)
```

```
## 
## 	Asymptotic Two-Sample Fisher-Pitman Permutation Test
## 
## data:  petalas_comprimento by especies (versicolor, virginica)
## Z = -7.8248, p-value = 5.084e-15
## alternative hypothesis: true mu is not equal to 0
```


### Bootstrap








### Teste de Kruskal Wallis




```r
kruskal.test(petalas_comprimento ~ especies, data = iris.pt)
```

```
## 
## 	Kruskal-Wallis rank sum test
## 
## data:  petalas_comprimento by especies
## Kruskal-Wallis chi-squared = 69.206, df = 1, p-value < 2.2e-16
```

```r
kSamples::qn.test(petalas_comprimento ~ especies, data = iris.pt, test="KW")
```

```
## 
## 
##  Kruskal-Wallis k-sample test.
## 
## Number of samples:  2
## Sample sizes:  50, 50
## Number of ties: 66
## 
## Null Hypothesis: All samples come from a common population.
## 
##   test statistic  asympt. P-value 
##        6.921e+01        1.110e-16
```

```r
coin::kruskal_test(petalas_comprimento ~ especies, data = iris.pt)
```

```
## 
## 	Asymptotic Kruskal-Wallis Test
## 
## data:  petalas_comprimento by especies (versicolor, virginica)
## chi-squared = 69.206, df = 1, p-value < 2.2e-16
```

### Teste de mediana de Brown-Mood


```r
coin::median_test(petalas_comprimento ~ especies, data = iris.pt)
```

```
## 
## 	Asymptotic Two-Sample Brown-Mood Median Test
## 
## data:  petalas_comprimento by especies (versicolor, virginica)
## Z = -8.3848, p-value < 2.2e-16
## alternative hypothesis: true mu is not equal to 0
```


### Teste de Savage


```r
coin::savage_test(petalas_comprimento ~ especies, data = iris.pt)
```

```
## 
## 	Asymptotic Two-Sample Savage Test
## 
## data:  petalas_comprimento by especies (versicolor, virginica)
## Z = -6.9659, p-value = 3.263e-12
## alternative hypothesis: true mu is not equal to 0
```




