---
title: RStudio Server no Ubuntu dentro do Windows via WSL
author: joaobtj
date: '2021-10-24'
slug: rstudio-server-wsl
categories: []
tags:
  - rstudio
  - server
  - ubuntu
  - linux
  - wsl
subtitle: ''
summary: ''
authors: []
lastmod: '2021-10-24T14:03:22-03:00'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---


# Instalar e executar o RStudio Server no Ubuntu via WSL do Windows

É possível instalar e executar o RStudio no Ubuntu dentro do Windows, via WSL (Windows Subsystem for Linux).

No momento em que este tutorial está sendo escrito, a versão mais recente do R é a 4.1 e do Ubuntu LTS é a 20.4 (focal)


## Instalar o Ubuntu no Windows via WSL

Seguir os passos no link abaixo:

https://ubuntu.com/wsl


## Instalar o R (versão 4.1)

No terminal do Ubuntu, execute os seguintes códigos:

```linux
# update indices
sudo apt update -qq
# install two helper packages we need
sudo apt install --no-install-recommends software-properties-common dirmngr
# add the signing key (by Michael Rutter) for these repos
# To verify key, run gpg --show-keys /etc/apt/trusted.gpg.d/cran_ubuntu_key.asc 
# Fingerprint: 298A3A825C0D65DFD57CBB651716619E084DAB9
wget -qO- https://cloud.r-project.org/bin/linux/ubuntu/marutter_pubkey.asc | sudo tee -a /etc/apt/trusted.gpg.d/cran_ubuntu_key.asc
# add the R 4.0 repo from CRAN -- adjust 'focal' to 'groovy' or 'bionic' as needed
sudo add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran40/"
# install R and others
sudo apt install r-base r-base-dev libcurl4-openssl-dev libssl-dev libxml2-dev

```

Acessar os pacotes do Cran:

```
sudo add-apt-repository ppa:c2d4u.team/c2d4u4.0+
sudo apt update
```
Adaptado de https://cran.r-project.org/bin/linux/ubuntu


## Instalar o RStudio Server

No terminal do Ubuntu, execute os seguintes códigos:

```
sudo apt-get install gdebi-core
wget https://download2.rstudio.org/server/bionic/amd64/rstudio-server-2021.09.2-382-amd64.deb
sudo gdebi rstudio-server-2021.09.2-382-amd64.deb
```

Versão:  2021.09.2+382 de 2022-01-10

Verifique a versão mais recente neste link:  https://www.rstudio.com/products/rstudio/download-server/debian-ubuntu/


## Acessar o RStudio Server

Para iniciar o RStudio Server, no terminal, execute:

```
sudo rstudio-server start
```

No navegador, acesse: 

[http://localhost:8787/](http://localhost:8787/)

O usuário e senha são os mesmos do Ubuntu.


Para interromper o RStudio Server, no terminal, execute:

```
sudo rstudio-server stop
```
