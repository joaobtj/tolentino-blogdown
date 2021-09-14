---
title: Criar um pacote do R com o devtools
author: joaobtj
date: '2021-09-12'
slug: pacote
categories:
  - r
tags:
  - r
  - pacote
  - devtools
subtitle: ''
summary: ''
authors: []
lastmod: '2021-09-12T18:27:50-03:00'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---


Para criar um pacote do R, vamos utilizar o pacote [devtools](https://devtools.r-lib.org/).

```r
library(devtools)
```

## Iniciando o projeto

Iniciamos criando um projeto cujo nome é o mesmo nome do pacote:

```r
create_package("C:/devgit/nomedopacote")
```

Isso vai criar um novo projeto no caminho indicado e já ai carregar o novo projeto em uma nova sessão do RStudio.

## Use o *git*

Para usar o *git* no desenvolvimento do pacote, existe a opção `use_git()`

```r
use_git()
```

Aceite para fazer o *commit* dos primeiros arquivos e reiniciar o RStudio para carregar a aba do *git*.

## Escreva sua primeira função

Crie sua primeira função com o comando `use_r()`

```r
use_r("fun")
```

em que `fun`é o nome da função. Esse comando adiciona um arquivo com o mesmo nome da função dentro do subdiretório R.

Sempre faça o commit das suas alterações, conforme seu fluxo de trabalho.

Alternativamente, é possível criar o arquivo diretamente dentro do subdiretório R.

## Documente sua função

Use os comentários do tipo *Roxygen* com o atalho Ctrl+Alt+Shift+R.

Na seção dos comentários, também adicione as funções externas utilizadas, por exemplo:

```r
#' @importFrom stats lm
#' @importFrom magrittr %>%
```
## Use uma licença

Para distribuir seu pacote, é importante escolher uma licença. As mais populares são:

* `use_mit_license()` licença mais permissiva
* `use_gpl3_license()` trabalhos originados devem ser distribuídos sob a mesma condição. 
* `use_cc0_license()` licença de domínio público


## Edite o arquivo `DESCRIPTION`

* Preencha seus dados de autoria do pacote.
* Descreva o pacote nos campos *Title* e *Description*.


## Use GitHub

Após fazer todos os primeiros *commit's*, suba seu pacote para o GitHub com o comando `use_github()`

## Use README

O arquivo README é a página inicial do pacote no GitHub. A função `use_readme_rmd()` cria o arquivo README.Rmd, em que é possível descrever o pacote e mostrar alguns exemplos de uso. também é possível usar código R neste arquivo.

A melhor forma de renderizar o arquivo README.Rmd é com a funçao `build_readme()`.

## Use news

O comando `use_news_md()` cria um arquivo NEWS.md. Neste arquivo, podemos divulgar todas as alterações feitas no pacote a cada lançamento de uma nova versão.

## Numeração da versão

Sempre que uma nova versão for lançada, a numeração deve ser incrementada. O comando `use_version()` incrementa a numeraçao no arquivo *DESCRIPTION*.

As opções são:

1. major --> 1.0.0 - incrementa o primeiro número
2. minor --> 0.1.0 - incrementa o segundo número
3. patch --> 0.0.1 - incrementa o terceiro número
4. dev --> 0.0.0.9001 - incrementa o quarto número (se houver)


Similar, o comando `use_dev_version()` incrementa para uma versão de desenvolvimento, por exemplo, de 1.0.0 para 1.0.0.9000.



## Dicas

1. Sempre faça o *check* do pacote frequentemente quando estiver desenvolvendo. Isso facilita encontrar possíveis erros. Use o comando `check()` ou o atalho Ctrl+Shift+E
1. Use `devtools::load_all(".")` para testar as funções durante o desenvolvimento. Alternativamente, o atalho é Ctrl+Shift+L Esta função disponibiliza as funções simulando o processo de instalar e carregar o pacote.
1. Para instalar seu pacote no diretório local, use o comando `devtools::install()`
1. Muito mais informações em https://r-pkgs.org/index.html

