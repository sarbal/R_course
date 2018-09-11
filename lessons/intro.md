# Intro
Hi! 

## CouRse aims 
- Introduce you to R 
- Basic concepts in data analysis
- "Cool" visuals 

## What is R? 
- Statistical and graphical language
- Follower of [S](https://en.wikipedia.org/wiki/S_(programming_language))

## What is it good foR?  
- Data mining/analysis 
- Data visualization and graphics
- Statistics! 
- Glorified calculator? 

## How do I...
### Install R
Start off by downloading [R](https://cran.r-project.org/) and then [RStudio](https://www.rstudio.com/).
- [Installing R for Windows](installwindows.md)
- [Installing R for Mac](installmac.md)
- [Installing R for Unix](installunix.md)

## Is it installed? Does it Run? 
### Use R 
- We will get to more fun stuff later, but first things first: 
#### Run RStudio
- Click it! 

### Packages, Repositories, oh my!
- Packages are code (and other!) bundles. 
- Repositories are where packages are located. Most are in [CRAN](https://cran.r-project.org/web/packages/). [Bioconductor](https://www.bioconductor.org/packages/release/BiocViews.html) and also [github](https://github.com/trending/r). 
- More on this [here](http://r-pkgs.had.co.nz/) and [here](https://www.datacamp.com/community/tutorials/r-packages-guide). 
#### Install packages
From CRAN: 
``` 
install.packages("very_important_package")
```
From Bioconductor: 
``` 
source("http://bioconductor.org/biocLite.R")
biocLite("very_important_package")
```
Or (for the next release)
``` 
if (!require("BiocManager"))
    install.packages("BiocManager")
BiocManager::install("my_super_cool_package")
```
From github:
```  
install.packages("devtools")
library(devtools)
devtools::install_github("very_cool_developer/very_important_tool")
```
#### Load libraries 
```
library(very_important_package)
library(very_important_tool)
require(very_important_data)
```


## Before you arrive: 
1. Make sure you've downloaded and installed R and RStudio. 
2. Download the files that are in [data folder](../data). 
3. Download the "helper.R" file. Source it to install all the packages and libraries. 

## On the day:
We will go through two lessons that will teach you some basic R functions, data manipulation, and visualization.  
You can follow along, or just watch. These lessons can be followed fairly independently. 


## WheRe to get help
- https://www.rstudio.com/
- https://www.r-bloggers.com/
- https://www.statmethods.net/index.html

## OtheR useful stuff 
- https://plot.ly/r/
- https://shiny.rstudio.com/
- https://rmarkdown.rstudio.com/
- https://www.rstudio.com/resources/cheatsheets/ 
- http://swirlstats.com/
- https://www.tidyverse.org/
- https://www.listendata.com/p/r-programming-tutorials.html
- http://genomicsclass.github.io/book/
- https://twitter.com/@RLangTip 
- https://twitter.com/hashtag/rstats?src=hash
- http://serialmentor.com/dataviz/
- https://adv-r.hadley.nz/
- https://peerj.com/collections/50-practicaldatascistats/
- http://www-huber.embl.de/csama2018/#home
- https://elifesciences.org/labs/cad57bcf/composing-reproducible-manuscripts-using-r-markdown
- https://masalmon.eu/2018/07/22/wheretogethelp/
