isomiRs
=======

analyze isomiRs from seqbuster tool

###Installation

Inside R:

```
library(devtools)
library(roxygen2)
devtools::install_github("lpantano/isomiRs", ref="master")
library(isomiRs)
```

There is a R scripts and set of example data at folder `$PATH2SEQBUSTER/R/isomiR_package/test`

Use last version with:

```
devtools::install_github("lpantano/isomiRs", ref="develop")
library(isomiRs)
openVignette(isomiRs)
```

You can go through the vignette to know how to use the main functions.

The rest of the manual is for `master` branch.

###Load project
```
setwd("$PATH2SEQBUSTER/R/isomiR_package/test")
#files coming from miraligner(need to be run with flag -freq)
files<-c("y0d2.hsa.fa.ad.new.mirna",
          "y0d34.hsa.fa.ad.new.mirna",
         "y66d0.hsa.fa.ad.new.mirna",
         "y80d0.hsa.fa.ad.new.mirna"
         )

#data.matrix showing the design of the project.
#columns for the conditions
d<-data.frame(condition=c("p","p","c","c"))
#row.names for the sample names (I faked here)
row.names(d)<-paste(d[,1],1:2,sep="")

#create isomiR S4 object 
obj<-loadIso(files,d,skip=0,header=T)
```

###plot isomiRs distributions among samples
```
obj<-plotIso(obj,type="sub")
```


###Differential expression analysis. 
In this example I defined ref=T that means separate reference miRNA and isomiRs
```
#check diff exp: this will become a DESeq2 obj,
#so any function can be apply to this
dds<-deIso(obj,formula=~condition,ref=T)
#plotMA
library(DESeq2)
plotMA(dds)
```

###access count data from dss object
```
table<-makeCounts(obj)
```
