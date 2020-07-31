--- 
title: "R as GIS for Economists"
author: "Taro Mieno"
date: "2020-05-14"
site: bookdown::bookdown_site
documentclass: book
bibliography: [RGIS.bib]
biblio-style: apalike
link-citations: yes
description: "This is a minimal example of using the bookdown package to write a book. The output format for this example is bookdown::gitbook."
---

# Welcome {-}

This book is being developed as part of my effort to put together course materials for my data science course targeted at upper-level undergraduate and graduate students at the University of Nebraska Lincoln. This books aims particularly at **spatial data processing for an econometric project**, where spatial variables become part of an econometric analysis. Over the years, I have seen so many students and researchers who spend so much time just processing spatial data (often involving clicking the ArcGIS (or QGIS) user interface to death), which is a waste of time from the perspective of academic productivity. My hope is that this book will help researchers become more proficient in spatial data processing and enhance the overall productivity of the fields of economics for which spatial data are essential.  

**About me**

I am an Assistant Professor at the Department of Agricultural Economics at University of Nebraska Lincoln, where I also teach Econometrics for Master's students. My research interests lie in precision agriculture, water economics, and agricultural policy. My personal website is [here](http://taromieno.netlify.com/). 

**Comments and Suggestions?**

Any constructive comments and suggestions about how I can improve the book are all welcome. Please send me an email at tmieno2@unl.edu or create an issue on [the github page](https://github.com/tmieno2/R-as-GIS-for-Economists)  of this book.

<hr>
<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License</a>.

# Preface {-}

## Why R as GIS for Economists? {-}

R has extensive capabilities as GIS software. In my opinion, $99\%$ of your spatial data processing needs as an economist will be satisfied by R. But, there are several popular options for GIS tasks other than R:

+ Python
+ ArcGIS
+ QGIS

Here I compare them briefly and discuss why R is a good option.

### R vs Python {-}

Both R and Python are actually heavily dependent on open source software GDAL and GEOS for their core GIS operations (GDAL for reading spatial data, and GEOS for geometrical operations like intersecting two spatial layers).^[For example, see the very first sentence of [this page](https://cran.r-project.org/web/packages/sf/index.html)] So, when you run GIS tasks on R or Python you basically tell R or Python what you want to do and they talk to the software, let it do the job, and return the results to you. This means that R and Python are much different in their capability at GIS tasks as they are dependent on the common open source software for many GIS tasks. When GDAL and GEOS get better, R and Python get better (with a short lag). Both of them have good spatial visualization tools as well. Moreover, both R and Python can communicate with QGIS and ArcGIS (as long you as have them installed of course) and use their functionalities from within R and Python via the bridging packages: `RQGIS` and `PyQGIS` for QGIS, and `R-ArcGIS` and `ArcPy`.^[We do not learn them in this lecture note because I do not see the benefits of using them.] So, if you are more familiar with Python than R, go ahead and go with Python. From now on, my discussions assume that you are going for the R option, as otherwise, you would not be reading the rest of the book anyway.

### R vs ArcGIS or QGIS {-}

ArcGIS is commercial software and it is quite expensive (you are likely to be able to get a significant discount if you are a student at or work for a University). On the other hand, QGIS is open source and free. It has seen significant development over the decade, and I would say it is just as competitive as ArcGIS. QGIS also uses open source geospatial software GDAL, GEOS, and others (SAGA, GRASS GIS). Both of them have a graphical interface that helps you implement various GIS tasks unlike R which requires programming. 

Now, since R can use ArcGIS and QGIS through the bridging packages, a more precise question we should be asking is whether you should program GIS tasks using R (possibly using the bridging packages) or manually implement GIS tasks using the graphical interface of ArcGIS or QGIS. The answer is programming GIS tasks using R. First, manual GIS operations are hard to repeat. It is often the case that in the course of a project you need to redo the same GIS task except that the underlying datasets have changed. If you have programmed the process with R, you just run the same code and that's it. You get the desired results. If you did not program it, you need to go through many clicks on the graphical interface all over again, potentially trying to remember how you actually did it the last time.^[You could take a step-by-step note of what you did though.] Second and more important, manual operations are not scalable. It has become much more common that we need to process many large spatial datasets. Imagine you are doing the same operations on $1,000$ files using a graphical interface, or even $50$ files. Do you know what is good at doing the same tasks over and over again without complaining? A computer. Just let it do what it likes to do. You have better things do. 

Finally, should you learn ArcGIS or QGIS in addition to (or before) R? I am doubtful. As economists, the GIS tasks we need to do are not super convoluted most of the time. Suppose $\Omega_R$ and $\Omega_{AQ}$ represent the set of GIS tasks R and $ArcGIS/QGIS$ can implement, respectively. Further, let $\Omega_E$ represent the set of skills economists need to implement. Then, $\Omega_E \in \Omega_R$ $99\%$ (or maybe $95\%$ to be safe) of the time and $\Omega_E \not\subset \Omega_{AQ}\setminus\Omega_R$ $99\%$ of the time. Personally, I have never had to rely on either ArcGIS or QGIS for my research projects after I learned how to use R as GIS. 

One of the things ArcGIS and QGIS can do but R cannot do ($\Omega_{AQ}\setminus\Omega_R$) is create spatial objects by hand using a graphical user interface, like drawing polygons and lines. Another thing that R lags behind ArcGIS and QGIS is 3D data visualization. But, I must say neither of them is essential for economists at the moment. Finally, sometime it is easier and faster to make a map using ArcGIS and QGIS especially for a complicated map.^[Let me know if you know something that is essential for economists that only ArcGIS or QGIS can do. I will add that to the list here.] 

### Summary {-}

+ **You have never used any GIS software?**

Learn R first. If you find out you really cannot complete the tasks you would like to do using R, then turn to other options. 

+ **You have used ArcGIS or QGIS and do not like them because they crash often?**

Why don't you try R?^[I am not saying R does not crash. R does crash. But, often times, the fault is yours, rather than the software's.] You may realize you actually do not need them.

+ **You have used ArcGIS or QGIS before and are very comfortable with them, but you need to program repetitive GIS tasks?**

Learn R and maybe take advantage of `R-ArcGIS` or `RQGIS`, which this book does not cover.

+ **You know for sure that you need to run only a simple GIS task once and never have to do any GIS tasks ever again?**

Stop reading and ask one of your friends to do the job. Pay him/her $\$20$ per hour, which is way below the opportunity cost of setting up either ArcGIS or QGI and learning to do that simple task on them.


## How is this book different from other online books and resources? {-}

We are seeing an explosion of online (and free) resources that teach how to use R for spatial data processing.^[This phenomenon is largely thanks to packages like `bookdown` [@Rbookdown], `blogdown` [@Rblogdown], and `pkgdown` [@Rpkgdown] that has lowered the cost of professional contents creation much much lower than before. Indeed, this book was built taking advantage of the `bookdown` package.]  Here is an incomplete list of such resources:

+ [Geocomputation with R](https://geocompr.robinlovelace.net/)
+ [Spatial Data Science](https://keen-swartz-3146c4.netlify.app/)
+ [Spatial Data Science with R](https://www.rspatial.org/index.html)
+ [Introduction to GIS using R](https://www.jessesadler.com/post/gis-with-r-intro/)
+ [Code for An Introduction to Spatial Analysis and Mapping in R](https://bookdown.org/lexcomber/brunsdoncomber2e/)
+ [Introduction to GIS in R](https://annakrystalli.me/intro-r-gis/index.html)
+ [Intro to GIS and Spatial Analysis](https://mgimond.github.io/Spatial/index.html)
+ [Introduction to Spatial Data Programming with R](http://132.72.155.230:3838/r/)
+ [Reproducible GIS analysis with R](http://staff.washington.edu/phurvitz/r_gis/)
+ [R for Earth-System Science](http://geog.uoregon.edu/bartlein/courses/geog490/index.html)
+ [Rspatial](http://rspatial.org/index.html)
+ [NEON Data Skills](https://www.neonscience.org/resources/data-skills)
+ [Simple Features for R](https://r-spatial.github.io/sf/)
<!-- + [Nick Eubank](https://www.nickeubank.com/gis-in-r/) -->

Thanks to all these resources, it has become much easier to self-teach R for GIS work than six or seven years ago when I first started using R for GIS. Even though I have not read through all these resources carefully, I am pretty sure every topic found in this book can also be found _somewhere_ in these resources (except the demonstrations). So, you may wonder why on earth you can benefit from reading this book. It all boils down to search costs. Researchers in different disciplines require different sets of spatial data skills. The available resources are either very general covering so many topics that economists are very unlikely to use. It is particularly hard for those who do not have much experience in GIS to identify whether particular skills are essential or not. So, they could spend so much time learning something that is not really useful. The value of this book lies in its deliberate incomprehensiveness. It only packages materials that satisfy the need of most economists, cutting out many topics that are likely to be of limited use for economists. 

For those who are looking for more comprehensive treatments of spatial data handling and processing in one book, I personally like [Geocomputation with R](https://geocompr.robinlovelace.net/) a lot. Increasingly, the developer of R packages created a website dedicated to their R packages, where you can often find vignettes (tutorials), like [Simple Features for R](https://r-spatial.github.io/sf/). 

## What is going to be covered in this book? {-}

The book starts with the very basics of spatial data handling (e.g., importing and exporting spatial datasets) and moves on to more practical spatial data operations (e.g., spatial data join) that are useful for research projects. This books is still under development. Right now, only Chapter 1 is available. I will work on the rest of the book over the summer. The "coming soon" chapters are close to be done. I just need to add finishing touches to those chapters. The "wait a bit" chapters need some more work, adding contents, etc.  

+ Chapter 1: Demonstrations of R as GIS (available)
	* groundwater pumping and groundwater level
	* precision agriculture
	* land use and weather
	* corn planted acreage and railroads
	* groundwater pumping and weather
+ Chapter 2: The basics of vector data handling using `sf` package (coming soon)
	* spatial data structure in `sf`
	* import and export vector data
	* (re)projection of spatial datasets
	* single-layer geometrical operations (e.g., create buffers, find centroids)
	* other miscellaneous basic operations
+ Chapter 3: Spatial interactions of vector datasets (coming soon)
	* spatially subsetting one layer based on another layer
	* extracting values from one layer to another layer^[`over` function in `sp` language]
+ Chapter 4: The basics of raster data handling using `raster` and `terra` packages (coming soon)
	* import and export raster data
	* stacking raster data
+ Chapter 5: Spatial interactions of vector and raster datasets (wait a bit)
	* extracting values from a raster layer to a vector layer
+ Chapter 6: Efficient spatial data processing (wait a bit)  
	* parallelization 
+ Chapter 7: Downloading publicly available spatial datasets (wait a bit)
	* Sentinel 2 (`sen2r`)
	* USDA NASS QuickStat (`tidyUSDA`)
	* PRISM (`prism`)
	* Daymet (`daymetr`)
	* USGS (`dataRetrieval`)
+ Chapter 8: Parallel computation (wait a bit)

As you can see above, this book does not spend any time on the very basics of GIS concepts. Before you start reading the book, you should know the followings at least (it's not much): 

+ What Geographic Coordinate System (GCS), Coordinate Reference System (CRS), and projection are ([this](https://annakrystalli.me/intro-r-gis/gis.html) is a good resource)
+ Distinctions between vector and raster data ([this](https://gis.stackexchange.com/questions/57142/what-is-the-difference-between-vector-and-raster-data-models) is a simple summary of the difference)

Finally, this book does not cover spatial statistics or spatial econometrics at all. This book is about spatial data _processing_. Spatial analysis is something you do _after_ you have processed spatial data.


## Conventions of the book and some notes {-}

Here are some notes of the conventions of this book and notes for R beginners and those who are not used to reading `rmarkdown`-generated html documents.

### Texts in gray boxes {-}

They are one of the following:

+ objects defined on R during demonstrations
+ R functions
+ R packages

When it is a function, I always put parentheses at the end like this: `st_read()`.^[This is a function that draws values randomly from the uniform distribution.] Sometimes, I combine a package and function in one like this: `sf::st_read()`. This means it is a function called `st_read()` from the `sf` package. 

### Colored Boxes {-}

Codes are in blue boxes, and outcomes are in red boxes.

Codes:


```r
runif(5)
```

Outcomes:


```
## [1] 0.5463091 0.0733040 0.4261112 0.1165969 0.1574750
```

### Parentheses around codes {-}

Sometimes you will see codes enclosed by parenthesis like this:


```r
(
  a <- runif(5)
)
```

```
## [1] 0.12203153 0.01051125 0.37176512 0.70698067 0.52884845
```

The parentheses prints what's inside of a newly created object (here `a`) without explicitly evaluating the object. So, basically I am signaling that we will be looking inside of the object that was just created. 

This one prints nothing.


```r
a <- runif(5)
```

### Footnotes {-}

Footnotes appear at the bottom of the page. You can easily get to a footnote by clicking on the footnote number. You can also go back to the main narrative where the footnote number is by clicking on the curved arrow at the end of the footnote. So, don't worry about having to scroll all the way up to where you were after reading footnotes.

## Session Information {-}

Here is the session information when compiling the book:


```r
sessionInfo()	
```

```
## R version 4.0.0 (2020-04-24)
## Platform: x86_64-apple-darwin17.0 (64-bit)
## Running under: macOS Mojave 10.14.1
## 
## Matrix products: default
## BLAS:   /System/Library/Frameworks/Accelerate.framework/Versions/A/Frameworks/vecLib.framework/Versions/A/libBLAS.dylib
## LAPACK: /Library/Frameworks/R.framework/Versions/4.0/Resources/lib/libRlapack.dylib
## 
## locale:
## [1] en_US.UTF-8/en_US.UTF-8/en_US.UTF-8/C/en_US.UTF-8/en_US.UTF-8
## 
## attached base packages:
## [1] stats     graphics  grDevices utils     datasets  methods   base     
## 
## loaded via a namespace (and not attached):
##  [1] compiler_4.0.0  magrittr_1.5    bookdown_0.18   htmltools_0.4.0
##  [5] tools_4.0.0     yaml_2.2.1      Rcpp_1.0.4.6    stringi_1.4.6  
##  [9] rmarkdown_2.1   knitr_1.28      stringr_1.4.0   digest_0.6.25  
## [13] xfun_0.13       rlang_0.4.6     evaluate_0.14
```




