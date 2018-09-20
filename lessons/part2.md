# Part 2: MoRe 
R is great for data analysis. This next part will go over how to read in data, dp some basic manipulations, and then visualize the results! 

For this part, you will need: 
- [my_dataA.csv](../data/my_dataA.csv)
- [my_dataB.tab](../data/my_dataB.tab)
- [DatasaurusDozen.txt](../data/DatasaurusDozen.txt)
- [primer](../data/primer.Rdata)
- [helper.R](../lessons/helper.R)

## Reading in data
- Depending on the format, there are multiple ways to input data into R 
### From files
- From a comma or tab separated file:
```
dataA <- read.csv(file="my_dataA.csv")
dataB <- read.table(file="my_dataB.tab", header=TRUE)
datasaurus <- read.delim(file="DatasaurusDozen.txt", sep="\t")
```
- There are other functions, like scan()
- If you want to read in data from the prompt: 
```
z <- scan()
1 2 3 

z <- scan(what=" ")
help me
I'm bored
```

- more here: https://stats.idre.ucla.edu/r/modules/reading-in-data-from-an-external-file/

### From other places, types  
- From connections (we haven't gone over this, but more here: https://stat.ethz.ch/R-manual/R-devel/library/base/html/connections.html)
- Excel: using xlsx, rJava e.g., https://www.r-bloggers.com/read-excel-files-from-r/
- PDFs: using pdftools, glue e.g., http://www.brodrigues.co/blog/2018-06-10-scraping_pdfs/
- URLs: using RCurl, e.g., http://rfunction.com/archives/1672 
- SQL databases: using DBI, dblplyr, or plyr. e.g., SRADB https://www.rdocumentation.org/packages/SRAdb/versions/1.30.0/topics/getSRA 
- APIs: using httr, jsonlite, e.g., https://www.r-bloggers.com/accessing-apis-from-r-and-a-little-r-programming/ 
- Twitter: httpuv, twitteR, ROAuth, rtweet, ... e.g., https://cran.r-project.org/web/packages/rtweet/vignettes/intro.html

### Save/export data
- As text 
```
first_line <- c("one", "two", "buckle", "my", "shoe")
second_line <- c("three", "four", "shut", "the", "door")
third_line <- c("five", "six", "pick", "up", "sticks")
fourth_line <- c("seven", "eight", "lay", "them", "straight")
fifth_line <- c("nine", "ten", "that's", "the", "end!")
my_rhyme <- rbind(first_line, second_line, third_line, fourth_line,fifth_line) 
write.table(my_rhyme, file="my_list.txt", sep="\t", quote="", row.names=T)
write.csv(my_rhyme, file="my_list.csv")
```
- As binary file
```
all_my_data <- rnorm(10000) 
my_function <- function(){ 
  print("Hello, world!") 
}
save(all_my_data, my_function, file="my_data.Rdata")
```
### Saving graphics 
- As a pdf or postscript (vector graphics) 
```
my_data <- 1:10
pdf("my_plot.pdf") # or try postscript()  
plot(my_data)
dev.off() 
```
- Or as an image (.png, .jpeg)
```
my_data <- matrix(1:100, ncol=10, nrow=10)
png("my_plot.png") # or try # jpg() 
image(my_data)
dev.off() 
```


### Clean up data
Always good/necessary to do data/sanity checks. 
- Do the data look like how you think they should? 
- Same number of lines imported as the file contains? 
- No weird characters? Some special characters have special properties when being read in. 
- How many empty values? Find/replace empty values with NAs 
- Correct data type? If numbers are stored as characters, there may be something odd in your input. 
- Put data into different variables, into tables or into the tidyverse. 


## Visuals
- R is pretty good for visualization
- We will start off with the famous [Iris](http://stat.ethz.ch/R-manual/R-devel/library/datasets/html/iris.html) dataset. You can read more about it [here](https://en.wikipedia.org/wiki/Iris_flower_data_set).   
``` 
summary(iris)
class(iris)
colnames(iris)
```
- the iris dataset is a dataframe, and the columns are labelled 
- first, we can be a little uncouth, and plot everything 
``` 
plot(iris)
```
- Similar to calling this function, which plots a matrix of scatterplots:
```
pairs(iris, upper.panel = NULL )
```
- Now that we've seen all the bits of the data, we can play with visualizing parts of it differently  
- Let's start with a scatter plot of the sepal length versus the petal length 
``` 
plot(iris$Sepal.Length, iris$Petal.Length, pch=19, col=as.numeric(iris$Species) )
```
- let's play around with this
``` 
## Playing with parameters 
plot(iris$Sepal.Length, iris$Petal.Length, pch=12, cex=3, lwd=4, lty=4, type="b", col=colors()[sample(600,5)][as.numeric(iris$Species)] )
## Plotting smoothed version
plot(lowess(iris$Sepal.Length, iris$Petal.Length), pch=19)
## Plotting using the forumla method 
plot(Petal.Length ~ Sepal.Length, data=iris, pch=19, col=Species)
```

- We can view sepal width by species distributions with other types of plots (note that the functions require the data to be in slightly different formats): 
- Beeswarms
```
boxplot( Sepal.Length ~ Species, data = iris)
beeswarm(Sepal.Length ~ Species, data = iris, corral="gutter", pch=19, add = TRUE)
```
- Stripcharts
```
stripchart(Sepal.Length ~ Species, data=iris, pch=19, vertical=T,  method = 'jitter', jitter = 0.2)
bxplot( Sepal.Length ~ Species, data = iris, add = TRUE)
```
- Beanplots 
```
beanplot( Sepal.Length ~ Species, data = iris, side="f", col="darkgreen")
```
- Violinplots 
``` 
iris.list = lapply( unique(iris$Species), function(si) iris$Sepal.Width[iris$Species==si]) 
vioplot( iris.list[[1]], iris.list[[2]], iris.list[[3]], col="darkgreen")  
```

- We can take a look at the distribution of petal width using a histogram: 
``` 
hist(iris$Petal.Width, col="lightblue")
```
- or build a barplot:
``` 
iris.bar = tapply( iris$Sepal.Length, iris$Species, mean)
barplot(iris.bar, col="black", xlab="Species", ylab="Count", main="Bar plot of mean Sepal Length")
```
- We see biomodality (two modes/peaks) in the petal width data 
- And there is a clear division (<0.75)
- Let's take a look and see what is distinct about the first peak 
- Since we want petal widths less than 0.75, we can slice the data:
``` 
small_petals <- which(iris$Petal.Width <0.75)
count(iris$Species[small_petals])
count(iris$Species)
```
- That was easy enough, and we can show this again by coloring the histogram based on the species
- Plots in R work by layering, so we can start by drawing the whole histogram first
```
hist(iris$Petal.Width, col="lightblue")
``` 
- And then adding each individual species as a layer with the parameter add = T
```
hist(iris$Petal.Width[iris$Species=="setosa"], col="red", add=T)
hist(iris$Petal.Width[iris$Species=="versicolor"], col="blue", add=T)
hist(iris$Petal.Width[iris$Species=="virginica"], col="purple", add=T)
```
- Oops! The histograms are all wonky because we've not specified how to bin the data. R works "intuitively", and picks the best breaks for the data.  
- We can force similar histogram breaks so that we can bin the data equally
```
h <- hist(iris$Petal.Width, col="lightblue")
h
```
- The h variable has the histogram output
- It has useful tags that we can use such as h$counts, h$mids and most importantly, h$breaks.   
```
hist(iris$Petal.Width[iris$Species=="setosa"],  breaks=h$breaks,col="red", add=T)
hist(iris$Petal.Width[iris$Species=="versicolor"], breaks=h$breaks, col="blue", add=T)
hist(iris$Petal.Width[iris$Species=="virginica"],  breaks=h$breaks,col="purple", add=T)
```
- There is still some overlap, so we can play with the opacity of the colors, and force no color for the first histogram
```
h <- hist(iris$Petal.Width, col=0, border=0)
hist(iris$Petal.Width[iris$Species=="setosa"],  breaks=h$breaks,col=makeTransparent("red"), add=T)
hist(iris$Petal.Width[iris$Species=="versicolor"], breaks=h$breaks, col=makeTransparent("blue"), add=T)
hist(iris$Petal.Width[iris$Species=="virginica"],  breaks=h$breaks,col=makeTransparent("purple"), add=T)

```
- How about some density lines? 
```
h <- hist(iris$Petal.Width, freq=F)
d_all <-density( iris$Petal.Width) 
lines(d_all, col="black")
```
- We can keep adding layers to our plots with other functions:
```
points()
polygon()
segments()
abline()
rug()
text()
mtext()
legend()
...
```
- More fun things to play with too 
```
pch
type
lty
lwd
bty
... 
```
- Going back to the matrix scatterplot, let's have a visual that summarizes all the data
```
## Ignore these for now 
panel.hist <- function(x, ...)
{
    usr <- par("usr"); on.exit(par(usr))
    par(usr = c(usr[1:2], 0, 1.5) )
    h <- hist(x, plot = FALSE)
    breaks <- h$breaks; nB <- length(breaks)
    y <- h$counts; y <- y/max(y)
    rect(breaks[-nB], 0, breaks[-1], y, col = "lightgreen", ...)
}
## with size proportional to the correlations.
panel.cor <- function(x, y, digits = 2, prefix = "", cex.cor, ...)
{
    usr <- par("usr"); on.exit(par(usr))
    par(usr = c(0, 1, 0, 1))
    r <- abs(cor(x, y))
    txt <- format(c(r, 0.123456789), digits = digits)[1]
    txt <- paste0(prefix, txt)
    if(missing(cex.cor)) cex.cor <- 0.8/strwidth(txt)
    text(0.5, 0.5, txt, cex = cex.cor * r, col= plasma(100)[round(r,2)*100])
}
pairs(iris, bg=1:3,lower.panel = panel.smooth, pch=19, upper.panel = panel.cor, diag.panel = panel.hist, cex.labels = 2, font.labels = 2)
```

- Great. What if we want to ask how similar are these individual plants to each other within each species. What can we look at? 
- Correlations are fun. Let's use heatmaps to show this. 
```
iris2  = apply(iris[,1:4], 2, as.numeric)
heatmap.2(iris2, RowSideCol=cols7[as.numeric(iris$Species)] , col=viridis(100), trace="none", density="none")
iris.r  = t(apply(iris[,1:4], 1, rank))
heatmap.2(iris.r, RowSideCol=cols7[as.numeric(iris$Species)] , col=viridis(100), trace="none", density="none")
iris.r2  = apply(iris[,1:4], 2, rank)
heatmap.2(iris.r2, RowSideCol=cols7[as.numeric(iris$Species)] , col=viridis(100), trace="none", density="none")
samples.cor = cor( t(iris2) )
heatmap.2(samples.cor, col=plasma(100), ColSideCol=cols7[as.numeric(iris$Species)], trace="none", density="none")
```


Other plots
- Venn diagrams
```
require(venn)
venn( list( 1:10, 5:15, 1:9, 3:9, 5:6, 9:100), zcol=magma(7) , ellipse = T)
require(venneuler)
plot(venneuler( c(A = 10, B =15, "A&B" = 4) ) )
```
- Trees and dendrograms 
```
require(ape)
data(USArrests)
dd <- dist(scale(USArrests), method = "euclidean")
hc <- hclust(dd, method = "ward.D2")
# Dendrogram
plot(hc)
# Tree 
plot(as.phylo(hc), type = "radial")
```

Tutorials for more advanced plotting:
- https://jokergoo.github.io/circlize_book/book/ 
- https://cran.r-project.org/web/packages/phylogram/vignettes/phylogram-vignette.html

## "Tidyr" versions 
We can do most of this with [ggplot2](https://github.com/rstudio/cheatsheets/blob/master/data-visualization-2.1.pdf). To some it is generally more intuitive...
```
g <- ggplot(iris, aes(x = Sepal.Length, y = Petal.Length)) 
g
```
- This does nothing, because we've not specified what we want to draw:
```
g <- g + geom_point() 
```
- Points! Now to color them:
```
g <- g + geom_point(aes(color = Species))
```
- We can keep building onto the "g" variable. 
```
g <- ggplot(iris, aes(x = Sepal.Length, y = Petal.Length, color = Species)) + geom_point()  +  geom_smooth(method = "lm", se = F) 

``` 
- How about boxplots? 
```
g <- ggplot(data=iris, aes(x=Species, y=Sepal.Length))
g + geom_boxplot(aes(fill=Species)) + 
  ylab("Sepal Length") + ggtitle("Iris Boxplot") +
  stat_summary(fun.y=mean, geom="point", shape=5, size=4)  
```
- Histograms:
```
g <- ggplot(data=iris, aes(x=Sepal.Width))
g + geom_histogram(binwidth=0.2, color="black", aes(fill=Species)) +  xlab("Sepal Width") +  ylab("Frequency") + ggtitle("Histogram of Sepal Width") 
```
- Barplots:
```
g <- ggplot(data=iris, aes(x=Species, y=Sepal.Length))
g + geom_bar(stat = "summary", fun.y = "mean") + xlab("Species") +  ylab("Mean") + ggtitle("Bar plot of mean Sepal Length") 
```
More [here](https://www.mailman.columbia.edu/sites/default/files/media/fdawg_ggplot2.html)

## Colors and palettes 
```
colors() 
palette()
```
- Preset colors as strings or as numbers 
- Or based on their RGB 
- e.g.,
```
blacks = c("black", 1, "#000000") 
reds = c("red", 2, "#FF0000") 
allreds = colors()[grep("red", colors())]
```
- Color ramps 
 ```
allredsRamp <- colorRampPalette(allreds)
allredsRamp(100)
grey2blue = colorpanel(100, "lightgrey", "blue", "darkblue")
```
- Predefined palettes:
- default R:
```
rainbow(5)
heat.colors(10)
terrain.colors(100)
topo.colors(10)
cm.colors(5)
```
- R color brewer
```
library(RColorBrewer)
display.brewer.all()
brewer.pal(8, "Set3") 
```
- everyone's new favorite are the viridis palettes (color-blind friendly)
```
library(viridis)
magma()
plasma()
inferno()
viridis()
cividis()
```

#### More here: 
- https://www.nceas.ucsb.edu/~frazier/RSpatialGuides/colorPaletteCheatsheet.pdf
- https://cran.r-project.org/web/packages/viridis/vignettes/intro-to-viridis.html
- https://moderndata.plot.ly/create-colorful-graphs-in-r-with-rcolorbrewer-and-plotly/


## Test yourself! 
1. Load the file "primer.Rdata" into your environment. Plot three plots of the dataset "X". Be as creative as you can in one, as deceptive as you can in the second, and then as clear as you can in the last.
2. Now, using the dataset "Y", plot a heatmap. Aim for clarity!
3. And finally, look at dataset "Z". Plot it the best way you think would show its key feature.
