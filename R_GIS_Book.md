--- 
title: "R as GIS for Economists"
author: "Taro Mieno"
date: "2020-06-04"
site: bookdown::bookdown_site
documentclass: book
bibliography: [RGIS.bib]
biblio-style: apalike
link-citations: yes
description: "This is a minimal example of using the bookdown package to write a book. The output format for this example is bookdown::gitbook."
---

# Welcome {-}

This book is being developed as part of my effort to put together course materials for my data science course targeted at upper-level undergraduate and graduate students at the University of Nebraska Lincoln. This book aims particularly at **spatial data processing for econometric projects**, where spatial variables become part of an econometric analysis. Over the years, I have seen many students and researchers who spend so much time just processing spatial data (often involving clicking the ArcGIS (or QGIS) user interface to death), which is a waste of time from the perspective of academic productivity. My hope is that this book will help researchers become more proficient in spatial data processing and enhance the overall productivity of the fields of economics for which spatial data are essential. 



**About me**

I am an Assistant Professor at the Department of Agricultural Economics at the University of Nebraska Lincoln, where I also teach Econometrics for Master's students. My research interests lie in precision agriculture, water economics, and agricultural policy. My personal website is [here](http://taromieno.netlify.com/). 

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

Both R and Python are actually heavily dependent on open source software GDAL and GEOS for their core GIS operations (GDAL for reading spatial data, and GEOS for geometrical operations like intersecting two spatial layers).^[For example, see the very first sentence of [this page](https://cran.r-project.org/web/packages/sf/index.html)] So, when you run GIS tasks on R or Python you basically tell R or Python what you want to do and they talk to the software, let it do the job, and return the results to you. This means that R and Python are not much different in their capability at GIS tasks as they are dependent on the common open source software for many GIS tasks. When GDAL and GEOS get better, R and Python get better (with a short lag). Both of them have good spatial visualization tools as well. Moreover, both R and Python can communicate with QGIS and ArcGIS (as long you as have them installed of course) and use their functionalities from within R and Python via the bridging packages: `RQGIS` and `PyQGIS` for QGIS, and `R-ArcGIS` and `ArcPy`.^[We do not learn them in this lecture note because I do not see the benefits of using them.] So, if you are more familiar with Python than R, go ahead and go with Python. From now on, my discussions assume that you are going for the R option, as otherwise, you would not be reading the rest of the book anyway.

### R vs ArcGIS or QGIS {-}

ArcGIS is commercial software and it is quite expensive (you are likely to be able to get a significant discount if you are a student at or work for a University). On the other hand, QGIS is open source and free. It has seen significant development over the decade, and I would say it is just as competitive as ArcGIS. QGIS also uses open source geospatial software GDAL, GEOS, and others (SAGA, GRASS GIS). Both of them have a graphical interface that helps you implement various GIS tasks unlike R which requires programming. 

Now, since R can use ArcGIS and QGIS through the bridging packages, a more precise question we should be asking is whether you should program GIS tasks using R (possibly using the bridging packages) or manually implement GIS tasks using the graphical interface of ArcGIS or QGIS. The answer is programming GIS tasks using R. First, manual GIS operations are hard to repeat. It is often the case that in the course of a project you need to redo the same GIS task except that the underlying datasets have changed. If you have programmed the process with R, you just run the same code and that's it. You get the desired results. If you did not program it, you need to go through many clicks on the graphical interface all over again, potentially trying to remember how you actually did it the last time.^[You could take a step-by-step note of what you did though.] Second and more important, manual operations are not scalable. It has become much more common that we need to process many large spatial datasets. Imagine you are doing the same operations on $1,000$ files using a graphical interface, or even $50$ files. Do you know what is good at doing the same tasks over and over again without complaining? A computer. Just let it do what it likes to do. You have better things do. 

Finally, should you learn ArcGIS or QGIS in addition to (or before) R? I am doubtful. As economists, the GIS tasks we need to do are not super convoluted most of the time. Suppose $\Omega_R$ and $\Omega_{AQ}$ represent the set of GIS tasks R and $ArcGIS/QGIS$ can implement, respectively. Further, let $\Omega_E$ represent the set of skills economists need to implement. Then, $\Omega_E \in \Omega_R$ $99\%$ (or maybe $95\%$ to be safe) of the time and $\Omega_E \not\subset \Omega_{AQ}\setminus\Omega_R$ $99\%$ of the time. Personally, I have never had to rely on either ArcGIS or QGIS for my research projects after I learned how to use R as GIS. 

One of the things ArcGIS and QGIS can do but R cannot do ($\Omega_{AQ}\setminus\Omega_R$) is create spatial objects by hand using a graphical user interface, like drawing polygons and lines. Another thing that R lags behind ArcGIS and QGIS is 3D data visualization. But, I must say neither of them is essential for economists at the moment. Finally, sometime it is easier and faster to make a map using ArcGIS and QGIS especially for a complicated map.^[Let me know if you know something that is essential for economists that only ArcGIS or QGIS can do. I will add that to the list here.] 

Using R as GIS, however, comes with a learning curve for those who have never used R because basic knowledge of R and general programming is required. On the other hand, the GUI-based use of ArcGIS and QGIS has a very low start-up cost. For those who have used R for other purposes like data wrangling and regression analysis, you have already (or almost) climbed up the hill and are ready to learn how to use R as GIS.

### Summary {-}

+ **You have never used any GIS software, but are very comfortable with R?**

Learn how to use R as GIS first. If you find out you really cannot complete the GIS tasks you would like to do using R, then turn to other options.

+ **You have never used any GIS software and R?**

This is tough. If you expect significant amount of GIS work, learning R basics and how to use R as GIS is a good investment of your time.  

+ **You have used ArcGIS or QGIS and do not like them because they crash often?**

Why don't you try R?^[I am not saying R does not crash. R does crash. But, often times, the fault is yours, rather than the software's.] You may realize you actually do not need them.

+ **You have used ArcGIS or QGIS before and are very comfortable with them, but you need to program repetitive GIS tasks?**

Learn R and maybe take advantage of `R-ArcGIS` or `RQGIS`, which this book does not cover.

+ **You know for sure that you need to run only a simple GIS task once and never have to do any GIS tasks ever again?**

Stop reading and ask one of your friends to do the job. Pay him/her $\$20$ per hour, which is way below the opportunity cost of setting up either ArcGIS or QGI and learning to do that simple task.


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

Thanks to all these resources, it has become much easier to self-teach R for GIS work than six or seven years ago when I first started using R for GIS. Even though I have not read through all these resources carefully, I am pretty sure every topic found in this book can also be found _somewhere_ in these resources (except the demonstrations). So, you may wonder why on earth you can benefit from reading this book. It all boils down to search costs. Researchers in different disciplines require different sets of spatial data skills. The available resources are typically very general covering so many topics that economists are unlikely to use. It is particularly hard for those who do not have much experience in GIS to identify whether particular skills are essential or not. So, they could spend so much time learning something that is not really useful. The value of this book lies in its deliberate incomprehensiveness. It only packages materials that satisfy the need of most economists, cutting out many topics that are likely to be of limited use for economists. 

For those who are looking for more comprehensive treatments of spatial data handling and processing in one book, I personally like [Geocomputation with R](https://geocompr.robinlovelace.net/) a lot. Increasingly, the developer of R packages created a website dedicated to their R packages, where you can often find vignettes (tutorials), like [Simple Features for R](https://r-spatial.github.io/sf/). 



## What is going to be covered in this book? {-}

The book starts with the very basics of spatial data handling (e.g., importing and exporting spatial datasets) and moves on to more practical spatial data operations (e.g., spatial data join) that are useful for research projects. This books is still under development. Right now, only Chapters 1 through 5 and Appendix A are available. I will work on the rest of the book over the summer. The "coming soon" chapters are close to be done. I just need to add finishing touches to those chapters. The "wait a bit" chapters need some more work, adding contents, etc.  

+ Chapter 1: Demonstrations of R as GIS (available)
	* groundwater pumping and groundwater level
	* precision agriculture
	* land use and weather
	* corn planted acreage and railroads
	* groundwater pumping and weather
+ Chapter 2: The basics of vector data handling using `sf` package (available)
	* spatial data structure in `sf`
	* import and export vector data
	* (re)projection of spatial datasets
	* single-layer geometrical operations (e.g., create buffers, find centroids)
	* other miscellaneous basic operations
+ Chapter 3: Spatial interactions of vector datasets (available)
	* understand topological relations of multiple `sf` objects
	* spatially subsetting a layer based on another layer
	* extracting values from one layer to another layer
+ Chapter 4: The basics of raster data handling using `terra` and `raster` packages (available)
	* understand object classes by the `terra` and `raster` packages
	* import and export raster data
	* stack raster data
	* quick plotting
+ Chapter 5: Spatial interactions of vector and raster datasets (available)
	* cropping a raster layer to the geographic extent of a vector layer 
	* extracting values from a raster layer to a vector layer
+ Chapter 6: Efficient spatial data processing (wait a bit)  
	* parallelization 
+ Chapter 7: Downloading publicly available spatial datasets (wait a bit)
	* USDA NASS QuickStat (`tidyUSDA`)
	* PRISM (`prism`)
	* Daymet (`daymetr`)
	* USGS (`dataRetrieval`)
	* Sentinel 2 (`sen2r`)
+ Appendix A: Loop and parallel computation (available)
+ Appendix B: Cheatsheet (wait a bit)

As you can see above, this book does not spend any time on the very basics of GIS concepts. Before you start reading the book, you should know the followings at least (it's not much): 

+ What Geographic Coordinate System (GCS), Coordinate Reference System (CRS), and projection are ([this](https://annakrystalli.me/intro-r-gis/gis.html) is a good resource)
+ Distinctions between vector and raster data ([this](https://gis.stackexchange.com/questions/57142/what-is-the-difference-between-vector-and-raster-data-models) is a simple summary of the difference)

This book is about spatial data processing and does not provide detailed explanations on non-spatial R operations, assuming some basic knowledge of R. In particular, the `dplyr` and `data.table` packages are extensively used for data wrangling. For data wrangling using `tidyverse` (a collection of packages including `dplyr`), see [R for Data Science](https://r4ds.had.co.nz/). For `data.table`, [this](https://cran.r-project.org/web/packages/data.table/vignettes/datatable-intro.html) is a good resource.

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
## [1] 0.17448088 0.88812459 0.53977916 0.48794622 0.09249116
```

### Parentheses around codes {-}

Sometimes you will see codes enclosed by parenthesis like this:


```r
(
  a <- runif(5)
)
```

```
## [1] 0.7322736 0.8454720 0.4828391 0.2133137 0.1774909
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
## [13] xfun_0.14       rlang_0.4.6     evaluate_0.14
```





<!--chapter:end:index.Rmd-->

# R as GIS: Demonstrations {#demo} 







## Before you start {-}

The primary objective of this chapter is to showcase the power of R as GIS through demonstrations using mock-up econometric research projects^[Note that this lecture does not deal with spatial econometrics at all. This lecture is about spatial data processing, not spatial econometrics. [This](http://www.econ.uiuc.edu/~lab/workshop/Spatial_in_R.html) is a great resource for spatial econometrics in R.]. Each project consists of a project overview (objective, datasets used, econometric model, and GIS tasks involved) and demonstration. This is really **NOT** a place you learn the nuts and bolts of how R does spatial operations. Indeed, we intentionally do not explain all the details of how the R codes work. We reiterate that the main purpose of the demonstrations is to get you a better idea of how R can be used to process spatial data to help your research projects involving spatial datasets. Finally, note that these *mock-up* projects use extremely simple econometric models that completely lacks careful thoughts you would need in real research projects. So, don't waste your time judging the econometric models, and just focus on GIS tasks. If you are not familiar with html documents generated by `rmarkdown`, you might benefit from reading the conventions of the book in the Preface. Finally, for those who are interested in replicating the demonstrations, directions for replication are provided below. However, I would suggest focusing on the narratives for the first time around, learn the nuts and bolts of spatial operations from Chapters 2 through 5, and then come back to replicate them. 

### Target Audience {-}

The target audience of this chapter is those who are not very familiar with R as GIS. Knowledge of R certainly helps. But, I tried to write in a way that R beginners can still understand the power of R as GIS^[I welcome any suggestions to improve the reading experience of unexperienced R users.]. Do not get bogged down by all the complex-looking R codes. Just focus on the narratives and figures to get a sense of what R can do.

### Direction for replication {-}

---

**Datasets**

Running the codes in this chapter involves reading datasets from a disk. All the datasets that will be imported are available [here](https://www.dropbox.com/sh/cyx9clgmshwc8eo/AAApv03Qpx84IGKCyF5v2rJ6a?dl=0). In this chapter, the path to files is set relative to my own working directory (which is hidden). To run the codes without having to mess with paths to the files, follow these steps:^[I thought about using the `here` package, but I found it a bit confusing for unexperienced R users.]

+ set a folder (any folder) as the working directory using `setwd()`  
+ create a folder called "Data" inside the folder designated as the working directory  
+ download the pertinent datasets from [here](https://www.dropbox.com/sh/cyx9clgmshwc8eo/AAApv03Qpx84IGKCyF5v2rJ6a?dl=0) and put them in the "Data" folder

**Notes**

+ `stargazer()` function creates regression tables using regression results. To generate readable tables on your console, change `type = "html"` to `type = "text"`.

## Demonstration 1: The impact of groundwater pumping on depth to water table {#Demo1}

<!-- this is for making stargazer table nicer -->

<style>
.book .book-body .page-wrapper .page-inner section.normal table
{
  width:auto;
}
.book .book-body .page-wrapper .page-inner section.normal table td,
.book .book-body .page-wrapper .page-inner section.normal table th,
.book .book-body .page-wrapper .page-inner section.normal table tr
{
  padding:0;
  border:0;
  background-color:#fff;
}
</style>

### Project Overview

---

**Objective:**

* Understand the impact of groundwater pumping on groundwater level. 

---

**Datasets**

* Groundwater pumping by irrigation wells in Chase, Dundy, and Perkins Counties in the southwest corner of Nebraska 
* Groundwater levels observed at USGS monitoring wells located in the three counties and retrieved from the National Water Information System (NWIS) maintained by USGS using the `dataRetrieval` package.

---

**Econometric Model**

In order to achieve the project objective, we will estimate the following model:

$$
 y_{i,t} - y_{i,t-1} = \alpha + \beta gw_{i,t-1} + v
$$

where $y_{i,t}$ is the depth to groundwater table^[the distance from the surface to the top of the aquifer] in March^[For our geographic focus of southwest Nebraska, corn is the dominant crop type. Irrigation for corn happens typically between April through September. For example, this means that changes in groundwater level ($y_{i,2012} - y_{i,2011}$) captures the impact of groundwater pumping that occurred April through September in 2011.] in year $t$ at USGS monitoring well $i$, and $gw_{i,t-1}$ is the total amount of groundwater pumping that happened within the 2-mile radius of the monitoring well $i$. 

---

**GIS tasks**

* read an ESRI shape file as an `sf` (spatial) object 
  - use `sf::st_read()`
* download depth to water table data using the `dataRetrieval` package developed by USGS 
  - use `dataRetrieval::readNWISdata()` and `dataRetrieval::readNWISsite()`
* create a buffer around USGS monitoring wells
  - use `sf::st_buffer()`
* convert a regular `data.frame` (non-spatial) with geographic coordinates into an `sf` (spatial) objects
  - use `sf::st_as_sf()`  and `sf::st_set_crs()`
* reproject an `sf` object to another CRS
  - use `sf::st_transform()`
* identify irrigation wells located inside the buffers and calculate total pumping
  - use `sf::st_join()`

---

**Preparation for replication**

Run the following code to install or load (if already installed) the `pacman` package, and then install or load (if already installed) the listed package inside the `pacman::p_load()` function.


```r
if (!require("pacman")) install.packages("pacman")
pacman::p_load(
  sf, # vector data operations
  dplyr, # data wrangling
  dataRetrieval, # download USGS NWIS data
  lubridate, # Date object handling
  stargazer, # regression table generation
  lfe # fast regression with many fixed effects 
)  
```

### Project Demonstration

The geographic focus of the project is the southwest corner of Nebraska consisting of Chase, Dundy, and Perkins County (see Figure \@ref(fig:NE-county) for their locations within Nebraska). Let's read a shape file of the three counties represented as polygons. We will use it later to spatially filter groundwater level data downloaded from NWIS.




```r
three_counties <- st_read(dsn = "./Data", layer = "urnrd") %>% 
  #--- project to WGS84/UTM 14N ---#
  st_transform(32614)
```

```
Reading layer `urnrd' from data source `/Users/tmieno2/Dropbox/TeachingUNL/RGIS_Econ/Data' using driver `ESRI Shapefile'
Simple feature collection with 3 features and 1 field
geometry type:  POLYGON
dimension:      XY
bbox:           xmin: -102.0518 ymin: 40.00257 xmax: -101.248 ymax: 41.00395
CRS:            4269
```

<div class="figure">
<img src="Demonstration_files/figure-html/NE-county-1.png" alt="The location of Chase, Dundy, and Perkins County in Nebraska" width="672" />
<p class="caption">(\#fig:NE-county)The location of Chase, Dundy, and Perkins County in Nebraska</p>
</div>
---

We have already collected groundwater pumping data, so let's import it. 


```r
#--- groundwater pumping data ---#
(
urnrd_gw <- readRDS("./Data/urnrd_gw_pumping.rds")
)
```

```
       well_id year  vol_af      lon     lat
    1:    1706 2007 182.566 245322.3 4542717
    2:    2116 2007  46.328 245620.9 4541125
    3:    2583 2007  38.380 245660.9 4542523
    4:    2597 2007  70.133 244816.2 4541143
    5:    3143 2007 135.870 243614.0 4541579
   ---                                      
18668:    2006 2012 148.713 284782.5 4432317
18669:    2538 2012 115.567 284462.6 4432331
18670:    2834 2012  15.766 283338.0 4431341
18671:    2834 2012 381.622 283740.4 4431329
18672:    4983 2012      NA 284636.0 4432725
```

`well_id` is the unique irrigation well identifier, and `vol_af` is the amount of groundwater pumped in acre-feet. This dataset is just a regular `data.frame` with coordinates. We need to convert this dataset into a object of class `sf` so that we can later identify irrigation wells located within a 2-mile radius of USGS monitoring wells (see Figure \@ref(fig:sp-dist-wells) for the spatial distribution of the irrigation wells).


```r
urnrd_gw_sf <- urnrd_gw %>% 
  #--- convert to sf ---#
  st_as_sf(coords = c("lon", "lat")) %>% 
  #--- set CRS WGS UTM 14 (you need to know the CRS of the coordinates to do this) ---# 
  st_set_crs(32614) 

#--- now sf ---#
urnrd_gw_sf
```

```
Simple feature collection with 18672 features and 3 fields
geometry type:  POINT
dimension:      XY
bbox:           xmin: 239959 ymin: 4431329 xmax: 310414.4 ymax: 4543146
CRS:            EPSG:32614
First 10 features:
   well_id year  vol_af                 geometry
1     1706 2007 182.566 POINT (245322.3 4542717)
2     2116 2007  46.328 POINT (245620.9 4541125)
3     2583 2007  38.380 POINT (245660.9 4542523)
4     2597 2007  70.133 POINT (244816.2 4541143)
5     3143 2007 135.870   POINT (243614 4541579)
6     5017 2007 196.799 POINT (243539.9 4543146)
7     1706 2008 171.250 POINT (245322.3 4542717)
8     2116 2008 171.650 POINT (245620.9 4541125)
9     2583 2008  46.100 POINT (245660.9 4542523)
10    2597 2008 124.830 POINT (244816.2 4541143)
```

<div class="figure">
<img src="Demonstration_files/figure-html/sp-dist-wells-1.png" alt="Spatial distribution of irrigation wells" width="672" />
<p class="caption">(\#fig:sp-dist-wells)Spatial distribution of irrigation wells</p>
</div>
---

Here are the rest of the steps we will take to obtain a regression-ready dataset for our analysis.

1. download groundwater level data observed at USGS monitoring wells from National Water Information System (NWIS) using the `dataRetrieval` package 
2. identify the irrigation wells located within the 2-mile radius of the USGS wells and calculate the total groundwater pumping that occurred around each of the USGS wells by year 
3. merge the groundwater pumping data to the groundwater level data

---

Let's download groundwater level data from NWIS first. The following code downloads groundwater level data for Nebraska from Jan 1, 1990, through Jan 1, 2016.


```r
#--- download groundwater level data ---#
NE_gwl <- readNWISdata(
    stateCd="Nebraska", 
    startDate = "1990-01-01", 
    endDate = "2016-01-01", 
    service = "gwlevels"
  ) %>% 
  dplyr::select(site_no, lev_dt, lev_va) %>% 
  rename(date = lev_dt, dwt = lev_va) 

#--- take a look ---#
head(NE_gwl, 10)
```


```
           site_no       date   dwt
1  400008097545301 2000-11-08 17.40
2  400008097545301 2008-10-09 13.99
3  400008097545301 2009-04-09 11.32
4  400008097545301 2009-10-06 15.54
5  400008097545301 2010-04-12 11.15
6  400008100050501 1990-03-15 24.80
7  400008100050501 1990-10-04 27.20
8  400008100050501 1991-03-08 24.20
9  400008100050501 1991-10-07 26.90
10 400008100050501 1992-03-02 24.70
```

`site_no` is the unique monitoring well identifier, `date` is the date of groundwater level monitoring, and `dwt` is depth to water table. 

We calculate the average groundwater level in March by USGS monitoring well (right before the irrigation season starts):^[`month()` and `year()` are from the `lubridate` package. They extract month and year from a `Date` object.]


```r
#--- Average depth to water table in March ---#
NE_gwl_march <- NE_gwl %>% 
  mutate(
    date = as.Date(date),
    month = month(date),
    year = year(date),
  ) %>% 
  #--- select observation in March ---#
  filter(year >= 2007, month == 3) %>% 
  #--- gwl average in March ---#
  group_by(site_no, year) %>% 
  summarize(dwt  = mean(dwt))

#--- take a look ---#
head(NE_gwl_march, 10)
```

```
# A tibble: 10 x 3
# Groups:   site_no [2]
   site_no          year   dwt
   <chr>           <dbl> <dbl>
 1 400032101022901  2008 118. 
 2 400032101022901  2009 117. 
 3 400032101022901  2010 118. 
 4 400032101022901  2011 118. 
 5 400032101022901  2012 118. 
 6 400032101022901  2013 118. 
 7 400032101022901  2014 116. 
 8 400032101022901  2015 117. 
 9 400038099244601  2007  24.3
10 400038099244601  2008  21.7
```

Since `NE_gwl` is missing geographic coordinates for the monitoring wells, we will download them using the `readNWISsite()` function and select only the monitoring wells that are inside the three counties.  


```r
#--- get the list of site ids ---#  
NE_site_ls <- NE_gwl$site_no %>% unique()

#--- get the locations of the site ids ---#  
sites_info <- readNWISsite(siteNumbers = NE_site_ls) %>% 
  dplyr::select(site_no, dec_lat_va, dec_long_va) %>% 
  #--- turn the data into an sf object ---#
  st_as_sf(coords = c("dec_long_va", "dec_lat_va")) %>% 
  #--- NAD 83 ---#
  st_set_crs(4269) %>% 
  #--- project to WGS UTM 14 ---#
  st_transform(32614) %>% 
  #--- keep only those located inside the three counties ---#
  .[three_counties, ]
```

---

We now identify irrigation wells that are located within the 2-mile radius of the monitoring wells^[This can alternatively be done using the `st_is_within_distance()` function.]. We first create polygons of 2-mile radius circles around the monitoring wells (see Figure \@ref(fig:buffer-map)).


```r
buffers <- st_buffer(sites_info, dist = 2*1609.34) # in meter
```

<div class="figure">
<img src="Demonstration_files/figure-html/buffer-map-1.png" alt="2-mile buffers around USGS monitoring wells" width="672" />
<p class="caption">(\#fig:buffer-map)2-mile buffers around USGS monitoring wells</p>
</div>

We now identify which irrigation wells are inside each of the buffers and get the associated groundwater pumping values. The `st_join()` function from the `sf` package will do the trick.


```r
#--- find irrigation wells inside the buffer and calculate total pumping  ---#
pumping_neaby <- st_join(buffers, urnrd_gw_sf)
```

Let's take a look at a USGS monitoring well (`site_no` = $400012101323401$).


```r
filter(pumping_neaby, site_no == 400012101323401, year == 2010)
```

```
Simple feature collection with 7 features and 4 fields
geometry type:  POLYGON
dimension:      XY
bbox:           xmin: 279690.7 ymin: 4428006 xmax: 286128 ymax: 4434444
CRS:            EPSG:32614
          site_no well_id year  vol_af                       geometry
1 400012101323401    6331 2010      NA POLYGON ((286128 4431225, 2...
2 400012101323401    1883 2010 180.189 POLYGON ((286128 4431225, 2...
3 400012101323401    2006 2010  79.201 POLYGON ((286128 4431225, 2...
4 400012101323401    2538 2010  68.205 POLYGON ((286128 4431225, 2...
5 400012101323401    2834 2010      NA POLYGON ((286128 4431225, 2...
6 400012101323401    2834 2010 122.981 POLYGON ((286128 4431225, 2...
7 400012101323401    4983 2010      NA POLYGON ((286128 4431225, 2...
```

As you can see, this well has seven irrigation wells within its 2-mile radius in 2010.   

Now, we will get total nearby pumping by monitoring well and year. 


```r
(
total_pumping_nearby <- pumping_neaby %>% 
  #--- calculate total pumping by monitoring well ---#
  group_by(site_no, year) %>% 
  summarize(nearby_pumping = sum(vol_af, na.rm = TRUE)) %>% 
  #--- NA means 0 pumping ---#  
  mutate(
    nearby_pumping = ifelse(is.na(nearby_pumping), 0, nearby_pumping)
  )
)
```

```
Simple feature collection with 2396 features and 3 fields
geometry type:  POLYGON
dimension:      XY
bbox:           xmin: 237904.5 ymin: 4428006 xmax: 313476.5 ymax: 4545687
CRS:            EPSG:32614
# A tibble: 2,396 x 4
# Groups:   site_no [401]
   site_no      year nearby_pumping                                     geometry
 * <chr>       <int>          <dbl>                                <POLYGON [m]>
 1 4000121013…  2007           571. ((286128 4431225, 286123.6 4431057, 286110.…
 2 4000121013…  2008           772. ((286128 4431225, 286123.6 4431057, 286110.…
 3 4000121013…  2009           500. ((286128 4431225, 286123.6 4431057, 286110.…
 4 4000121013…  2010           451. ((286128 4431225, 286123.6 4431057, 286110.…
 5 4000121013…  2011           545. ((286128 4431225, 286123.6 4431057, 286110.…
 6 4000121013…  2012          1028. ((286128 4431225, 286123.6 4431057, 286110.…
 7 4001301013…  2007           485. ((278847.4 4433844, 278843 4433675, 278829.…
 8 4001301013…  2008           515. ((278847.4 4433844, 278843 4433675, 278829.…
 9 4001301013…  2009           351. ((278847.4 4433844, 278843 4433675, 278829.…
10 4001301013…  2010           374. ((278847.4 4433844, 278843 4433675, 278829.…
# … with 2,386 more rows
```

---

We now merge nearby pumping data to the groundwater level data, and transform the data to obtain the dataset ready for regression analysis.


```r
#--- regression-ready data ---#
reg_data <- NE_gwl_march %>% 
  #--- pick monitoring wells that are inside the three counties ---#
  filter(site_no %in% unique(sites_info$site_no)) %>% 
  #--- merge with the nearby pumping data ---#
  left_join(., total_pumping_nearby, by = c("site_no", "year")) %>% 
  #--- lead depth to water table ---#
  arrange(site_no, year) %>% 
  group_by(site_no) %>% 
  mutate(
    #--- lead depth ---#
    dwt_lead1 = dplyr::lead(dwt, n = 1, default = NA, order_by = year),
    #--- first order difference in dwt  ---#
    dwt_dif  = dwt_lead1 - dwt
  )  

#--- take a look ---#
dplyr::select(reg_data, site_no, year, dwt_dif, nearby_pumping)
```

```
# A tibble: 2,022 x 4
# Groups:   site_no [230]
   site_no          year dwt_dif nearby_pumping
   <chr>           <dbl>   <dbl>          <dbl>
 1 400130101374401  2011   NA              358.
 2 400134101483501  2007    2.87          2038.
 3 400134101483501  2008    0.78          2320.
 4 400134101483501  2009   -2.45          2096.
 5 400134101483501  2010    3.97          2432.
 6 400134101483501  2011    1.84          2634.
 7 400134101483501  2012   -1.35           985.
 8 400134101483501  2013   44.8             NA 
 9 400134101483501  2014  -26.7             NA 
10 400134101483501  2015   NA               NA 
# … with 2,012 more rows
```

---

Finally, we estimate the model using `felm()` from the `lfe` package (see [@gaure2013lfe] for how it works).


```r
#--- OLS with site_no and year FEs (error clustered by site_no) ---#
reg_dwt <- felm(dwt_dif ~ nearby_pumping | site_no + year | 0 | site_no, data = reg_data)
```

Here is the regression result.


```r
stargazer(reg_dwt, type = "html")
```


<table style="text-align:center"><tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"></td><td><em>Dependent variable:</em></td></tr>
<tr><td></td><td colspan="1" style="border-bottom: 1px solid black"></td></tr>
<tr><td style="text-align:left"></td><td>dwt_dif</td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">nearby_pumping</td><td>0.001<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.0001)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Observations</td><td>1,342</td></tr>
<tr><td style="text-align:left">R<sup>2</sup></td><td>0.409</td></tr>
<tr><td style="text-align:left">Adjusted R<sup>2</sup></td><td>0.286</td></tr>
<tr><td style="text-align:left">Residual Std. Error</td><td>1.493 (df = 1111)</td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"><em>Note:</em></td><td style="text-align:right"><sup>*</sup>p<0.1; <sup>**</sup>p<0.05; <sup>***</sup>p<0.01</td></tr>
</table>

---

## Demonstration 2: Precision Agriculture


<style>
.book .book-body .page-wrapper .page-inner section.normal table
{
  width:auto;
}
.book .book-body .page-wrapper .page-inner section.normal table td,
.book .book-body .page-wrapper .page-inner section.normal table th,
.book .book-body .page-wrapper .page-inner section.normal table tr
{
  padding:0;
  border:0;
  background-color:#fff;
}
</style>

### Project Overview

---

**Objectives:**

+ Understand the impact of nitrogen on corn yield 
+ Understand how electric conductivity (EC) affects the marginal impact of nitrogen on corn 

---

**Datasets:**

+ The experimental design of an on-farm randomized nitrogen trail on an 80-acre field 
+ Data generated by the experiment
  * As-applied nitrogen rate
  * Yield measures 
+ Electric conductivity 

---

**Econometric Model:**

Here is the econometric model, we would like to estimate:

$$
yield_i = \beta_0 + \beta_1 N_i + \beta_2 N_i^2 + \beta_3 N_i \cdot EC_i + \beta_4 N_i^2 \cdot EC_i + v_i
$$

where $yield_i$, $N_i$, $EC_i$, and $v_i$ are corn yield, nitrogen rate, EC, and error term at subplot $i$. Subplots which are obtained by dividing experimental plots into six of equal-area compartments.  

---

**GIS tasks**

* read spatial data in various formats: R data set (rds), shape file, and GeoPackage file
  - use `sf::st_read()` 
* create maps using the `ggplot2` package
  - use `ggplot2::geom_sf()`
* create subplots within experimental plots
  - user-defined function that makes use of `st_geometry()` 
* identify corn yield, as-applied nitrogen, and electric conductivity (EC) data points within each of the experimental plots and find their averages
  - use `sf::st_join()` and   `sf::aggregate()`

---

**Preparation for replication**

+ Run the following code to install or load (if already installed) the `pacman` package, and then install or load (if already installed) the listed package inside the `pacman::p_load()` function.


```r
if (!require("pacman")) install.packages("pacman")
pacman::p_load(
  sf, # vector data operations 
  dplyr, # data wrangling 
  ggplot2, # for map creation 
  stargazer, # regression table generation 
  patchwork # arrange multiple plots 
)  
```

+ Run the following code to define the theme for map:


```r
theme_for_map <- theme(
  axis.ticks = element_blank(),
  axis.text= element_blank(), 
  axis.line = element_blank(),
  panel.border = element_blank(),
  panel.grid.major = element_line(color='transparent'),
  panel.grid.minor = element_line(color='transparent'),
  panel.background = element_blank(),
  plot.background = element_rect(fill = "transparent",color='transparent')
)  
```

### Project Demonstration

We have already run a whole-field randomized nitrogen experiment on a 80-acre field. Let's import the trial design data


```r
#--- read the trial design data ---#
trial_design_16 <- readRDS("./Data/trial_design.rds")
```

Figure \@ref(fig:trial-fig) is the map of the trial design generated using `ggplot2` package.


```r
#--- map of trial design ---#
ggplot(data = trial_design_16) +
  geom_sf(aes(fill = factor(NRATE))) +
  scale_fill_brewer(name = "N", palette = "OrRd", direction = 1) +
  theme_for_map
```

<div class="figure">
<img src="Demonstration_files/figure-html/trial-fig-1.png" alt="The Experimental Design of the Randomize Nitrogen Trial" width="672" />
<p class="caption">(\#fig:trial-fig)The Experimental Design of the Randomize Nitrogen Trial</p>
</div>

---

We have collected yield, as-applied NH3, and EC data. Let's read in these datasets:^[Here we are demonstrating that R can read spatial data in different formats. R can read spatial data of many other formats. Here, we are reading a shapefile (.shp) and GeoPackage file (.gpkg).]


```r
#--- read yield data (sf data saved as rds) ---#
yield <- readRDS("./Data/yield.rds")

#--- read NH3 data (GeoPackage data) ---#
NH3_data <- st_read("Data/NH3.gpkg")

#--- read ec data (shape file) ---#
ec <- st_read(dsn="Data", "ec")
```

Figure \@ref(fig:Demo2-show-the-map) shows the spatial distribution of the three variables. A map of each variable was made first, and then they are combined into one figure using the `patchwork` package^[Here is its [github page](https://github.com/thomasp85/patchwork). See the bottom of the page to find vignettes.].


```r
#--- yield map ---#
g_yield <- ggplot() +
  geom_sf(data = trial_design_16) +
  geom_sf(data = yield, aes(color = yield), size = 0.5) +
  scale_color_distiller(name = "Yield", palette = "OrRd", direction = 1) +
  theme_for_map

#--- NH3 map ---#
g_NH3 <- ggplot() +
  geom_sf(data = trial_design_16) +
  geom_sf(data = NH3_data, aes(color = aa_NH3), size = 0.5) +
  scale_color_distiller(name = "NH3", palette = "OrRd", direction = 1) +
  theme_for_map

#--- NH3 map ---#
g_ec <- ggplot() +
  geom_sf(data = trial_design_16) +
  geom_sf(data = ec, aes(color = ec), size = 0.5) +
  scale_color_distiller(name = "EC", palette = "OrRd", direction = 1) +
  theme_for_map

#--- stack the figures vertically and display (enabled by the patchwork package) ---#
g_yield/g_NH3/g_ec
```

<div class="figure">
<img src="Demonstration_files/figure-html/Demo2-show-the-map-1.png" alt="Spatial distribution of yield, NH3, and EC" width="672" />
<p class="caption">(\#fig:Demo2-show-the-map)Spatial distribution of yield, NH3, and EC</p>
</div>

---

Instead of using plot as the observation unit, we would like to create subplots inside each of the plots and make them the unit of analysis because it would avoid masking the within-plot spatial heterogeneity of EC. Here, we divide each plot into six subplots.

The following function generate subplots by supplying a trial design and the number of subplots you would like to create within each plot:


```r
gen_subplots <- function(plot, num_sub) {

  #--- extract geometry information ---#
  geom_mat <- st_geometry(plot)[[1]][[1]]

  #--- upper left ---#
  top_start<- (geom_mat[2,])

  #--- upper right ---#
  top_end<- (geom_mat[3,])

  #--- lower right ---#
  bot_start<- (geom_mat[1,])

  #--- lower left ---#
  bot_end<- (geom_mat[4,])

  top_step_vec <- (top_end-top_start)/num_sub
  bot_step_vec <- (bot_end-bot_start)/num_sub

  # create a list for the sub-grid

  subplots_ls <- list()

  for (j in 1:num_sub){

    rec_pt1 <- top_start + (j-1)*top_step_vec
    rec_pt2 <- top_start + j*top_step_vec
    rec_pt3 <- bot_start + j*bot_step_vec
    rec_pt4 <- bot_start + (j-1)*bot_step_vec

    rec_j <- rbind(rec_pt1,rec_pt2,rec_pt3,rec_pt4,rec_pt1)

    temp_quater_sf <- list(st_polygon(list(rec_j))) %>%
      st_sfc(.) %>%
      st_sf(., crs = 26914)

    subplots_ls[[j]] <- temp_quater_sf

  }

  return(do.call('rbind',subplots_ls))

}
```

Let's run the function to create six subplots within each of the experimental plots.


```r
#--- generate subplots ---#
subplots <- lapply(
  1:nrow(trial_design_16), 
  function(x) gen_subplots(trial_design_16[x, ], 6)
  ) %>% 
  do.call('rbind', .) 
```

Figure \@ref(fig:map-subgrid) is a map of the subplots generated.


```r
#--- here is what subplots look like ---#
ggplot(subplots) +
  geom_sf() +
  theme_for_map
```

<div class="figure">
<img src="Demonstration_files/figure-html/map-subgrid-1.png" alt="Map of the subplots" width="672" />
<p class="caption">(\#fig:map-subgrid)Map of the subplots</p>
</div>

---

We now identify the mean value of corn yield, nitrogen rate, and EC for each of the subplots using `sf::aggregate()` and `sf::st_join()`.


```r
(
reg_data <- subplots %>% 
  #--- yield ---#
  st_join(., aggregate(yield, ., mean), join = st_equals) %>%
  #--- nitrogen ---#
  st_join(., aggregate(NH3_data, ., mean), join = st_equals) %>%
  #--- EC ---#
  st_join(., aggregate(ec, ., mean), join = st_equals)
)
```

```
Simple feature collection with 816 features and 3 fields
geometry type:  POLYGON
dimension:      XY
bbox:           xmin: 560121.3 ymin: 4533410 xmax: 560758.9 ymax: 4533734
CRS:            EPSG:26914
First 10 features:
      yield   aa_NH3       ec                       geometry
1  220.1789 194.5155 28.33750 POLYGON ((560121.3 4533428,...
2  218.9671 194.4291 29.37667 POLYGON ((560134.5 4533428,...
3  220.3286 195.2903 30.73600 POLYGON ((560147.7 4533428,...
4  215.3121 196.7649 32.24000 POLYGON ((560160.9 4533429,...
5  216.9709 195.2199 36.27000 POLYGON ((560174.1 4533429,...
6  227.8761 184.6362 31.21000 POLYGON ((560187.3 4533429,...
7  226.0991 179.2143 31.99250 POLYGON ((560200.5 4533430,...
8  225.3973 179.0916 31.56500 POLYGON ((560213.7 4533430,...
9  221.1820 178.9585 33.01000 POLYGON ((560227 4533430, 5...
10 219.4659 179.0057 41.89750 POLYGON ((560240.2 4533430,...
```

Here are the visualization of the subplot-level data (Figure \@ref(fig:Demo2-subplot-fig)):  


```r
(ggplot() +
  geom_sf(data = reg_data, aes(fill = yield), color = NA) +
  scale_fill_distiller(name = "Yield", palette = "OrRd", direction = 1) +
  theme_for_map)/
(ggplot() +
  geom_sf(data = reg_data, aes(fill = aa_NH3), color = NA) +
  scale_fill_distiller(name = "NH3", palette = "OrRd", direction = 1) +
  theme_for_map)/
(ggplot() +
  geom_sf(data = reg_data, aes(fill = ec), color = NA) +
  scale_fill_distiller(name = "EC", palette = "OrRd", direction = 1) +
  theme_for_map)
```

<div class="figure">
<img src="Demonstration_files/figure-html/Demo2-subplot-fig-1.png" alt="Spatial distribution of subplot-level yield, NH3, and EC" width="672" />
<p class="caption">(\#fig:Demo2-subplot-fig)Spatial distribution of subplot-level yield, NH3, and EC</p>
</div>

---

Let's estimate the model and see the results:


```r
lm(yield ~ aa_NH3 + I(aa_NH3^2) + I(aa_NH3*ec) + I(aa_NH3^2*ec), data = reg_data) %>% 
  stargazer(type = "html")
```


<table style="text-align:center"><tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"></td><td><em>Dependent variable:</em></td></tr>
<tr><td></td><td colspan="1" style="border-bottom: 1px solid black"></td></tr>
<tr><td style="text-align:left"></td><td>yield</td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">aa_NH3</td><td>-1.223</td></tr>
<tr><td style="text-align:left"></td><td>(1.308)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">I(aa_NH32)</td><td>0.004</td></tr>
<tr><td style="text-align:left"></td><td>(0.003)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">I(aa_NH3 * ec)</td><td>0.002</td></tr>
<tr><td style="text-align:left"></td><td>(0.003)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">I(aa_NH32 * ec)</td><td>-0.00001</td></tr>
<tr><td style="text-align:left"></td><td>(0.00002)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">Constant</td><td>327.993<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(125.638)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Observations</td><td>784</td></tr>
<tr><td style="text-align:left">R<sup>2</sup></td><td>0.010</td></tr>
<tr><td style="text-align:left">Adjusted R<sup>2</sup></td><td>0.005</td></tr>
<tr><td style="text-align:left">Residual Std. Error</td><td>5.712 (df = 779)</td></tr>
<tr><td style="text-align:left">F Statistic</td><td>2.023<sup>*</sup> (df = 4; 779)</td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"><em>Note:</em></td><td style="text-align:right"><sup>*</sup>p<0.1; <sup>**</sup>p<0.05; <sup>***</sup>p<0.01</td></tr>
</table>

---

## Demonstration 3: Land Use and Weather


<style>
.book .book-body .page-wrapper .page-inner section.normal table
{
  width:auto;
}
.book .book-body .page-wrapper .page-inner section.normal table td,
.book .book-body .page-wrapper .page-inner section.normal table th,
.book .book-body .page-wrapper .page-inner section.normal table tr
{
  padding:0;
  border:0;
  background-color:#fff;
}
</style>

### Project Overview

---

**Objective**

+ Understand the impact of past precipitation on crop choice in Iowa (IA). 

---

**Datasets**

+ IA county boundary 
+ Regular grids over IA, created using `sf::st_make_grid()` 
+ PRISM daily precipitation data downloaded using `prism` package
+ Land use data from the Cropland Data Layer (CDL) for IA in 2015, downloaded using `cdlTools` package

---

**Econometric Model**

The econometric model we would like to estimate is:

$$
 CS_i = \alpha + \beta_1 PrN_{i} + \beta_2 PrC_{i} + v_i
$$
where $CS_i$ is the area share of corn divided by that of soy in 2015 for grid $i$ (we will generate regularly-sized grids in the Demo section), $PrN_i$ is the total precipitation observed in April through May and September  in 2014, $PrC_i$ is the total precipitation observed in June through August in 2014, and $v_i$ is the error term. To run the econometric model, we need to find crop share and weather variables observed at the grids. We first tackle the crop share variable, and then the precipitation variable.

---

**GIS tasks**

+ download Cropland Data Layer (CDL) data by USDA NASS 
  * use `cdlTools::getCDL()`
+ download PRISM weather data
  * use `prism::get_prism_dailys()`
+ crop PRISM data to the geographic extent of IA 
  * use `raster::crop()`
+ create regular grids within IA, which become the observation units of the econometric analysis
  * use `sf::st_make_grid()` 
+ remove grids that share small area with IA 
  * use `sf::st_intersection()` and `sf::st_area`
+ assign crop share and weather data to each of the generated IA grids (parallelized)
  * use `exactextractr::exact_extract()` and `future.apply::future_lapply()`
+ create maps 
  * use the `tmap` package 

---

**Preparation for replication**

+ Run the following code to install or load (if already installed) the `pacman` package, and then install or load (if already installed) the listed package inside the `pacman::p_load()` function.


```r
if (!require("pacman")) install.packages("pacman")
pacman::p_load(
  sf, # vector data operations
  raster, # raster data operations
  exactextractr, # fast raster data extraction for polygons
  maps, # to get county boundary data
  data.table, # data wrangling
  dplyr, # data wrangling
  lubridate, # Date object handling
  tmap, # for map creation
  stargazer, # regression table generation
  future.apply, # parallel computation
  cdlTools, # download CDL data
  rgdal, # required for cdlTools
  prism, # download PRISM data
  stringr # string manipulation
)  
```

### Project Demonstration

The geographic focus of this project is Iowas. Let's get Iowa state border (see Figure \@ref(fig:IA-map) for its map).


```r
#--- IA state boundary ---#
IA_boundary <- st_as_sf(maps::map("state", "iowa", plot = FALSE, fill = TRUE)) 
```

<div class="figure">
<img src="Demonstration_files/figure-html/IA-map-1.png" alt="Iowa state boundary" width="672" />
<p class="caption">(\#fig:IA-map)Iowa state boundary</p>
</div>

The unit of analysis is artificial grids that we create over Iowa. The grids are regularly-sized rectangles except around the edge of the Iowa state border^[We by no means are saying that this is the right geographical unit of analysis. This is just about demonstrating how R can be used for analysis done at the higher spatial resolution than county.]. So, let's create grids and remove those that do not overlap much with Iowa.


```r
#--- create regular grids (40 cells by 40 columns) over IA ---#
IA_grids <- IA_boundary %>% 
  #--- create grids ---#
  st_make_grid(, n = c(40, 40)) %>% 
  #--- convert to sf ---#
  st_as_sf() %>% 
  #--- find the intersections of IA grids and IA polygon ---#
  st_intersection(., IA_boundary) %>% 
  #--- calculate the area of each grid ---#
  mutate(
    area = as.numeric(st_area(.)),
    area_ratio = area/max(area)
  ) %>% 
  #--- keep only if the intersected area is large enough ---#
  filter(area_ratio > 0.8) %>% 
  #--- assign grid id for future merge ---#
  mutate(grid_id = 1:nrow(.))
```

Here is what the generated grids look like (Figure \@ref(fig:Demo4-IA-grids-map)):


```r
#--- plot the grids over the IA state border ---#
tm_shape(IA_boundary) +
  tm_polygons(col = "green") +
tm_shape(IA_grids) +
  tm_polygons(alpha = 0) +
  tm_layout(frame = FALSE)
```

<div class="figure">
<img src="Demonstration_files/figure-html/Demo4-IA-grids-map-1.png" alt="Map of regular grids generated over IA" width="672" />
<p class="caption">(\#fig:Demo4-IA-grids-map)Map of regular grids generated over IA</p>
</div>

---

Let's work on crop share data. You can download CDL data using the `getCDL()` function from the `cdlTools` package.




```r
#--- download the CDL data for IA in 2015 ---#
(
IA_cdl_2015 <- getCDL("Iowa", 2015)$IA2015
)
```

The cells (30 meter by 30 meter) of the imported raster layer take a value ranging from 0 to 255. Corn and soybean are represented by 1 and 5, respectively (Figure \@ref(fig:overlap-cdl-grid)).

Figure \@ref(fig:overlap-cdl-grid) shows the map of one of the IA grids and the CDL cells it overlaps with.

<div class="figure">
<img src="Demonstration_files/figure-html/overlap-cdl-grid-1.png" alt="Spatial overlap of a IA grid and CDL layer" width="672" />
<p class="caption">(\#fig:overlap-cdl-grid)Spatial overlap of a IA grid and CDL layer</p>
</div>

We would like to extract all the cell values within the blue border. 

We use `exactextractr::exact_extract()` to identify which cells of the CDL raster layer fall within each of the IA grids and extract land use type values. We then find the share of corn and soybean for each of the grids.


```r
#--- reproject grids to the CRS of the CDL data ---#
IA_grids_rp_cdl <- st_transform(IA_grids, projection(IA_cdl_2015))

#--- extract crop type values and find frequencies ---#
cdl_extracted <- exact_extract(IA_cdl_2015, IA_grids_rp_cdl) %>% 
  lapply(., function (x) data.table(x)[,.N, by = value]) %>% 
  #--- combine the list of data.tables into one data.table ---#
  rbindlist(idcol = TRUE) %>% 
  #--- find the share of each land use type ---#
  .[, share := N/sum(N), by = .id] %>% 
  .[, N := NULL] %>% 
  #--- keep only the share of corn and soy ---#
  .[value %in% c(1, 5), ]  
```

We then find the corn to soy ratio for each of the IA grids.


```r
#--- find corn/soy ratio ---#
corn_soy <- cdl_extracted %>% 
  #--- long to wide ---#
  dcast(.id ~ value, value.var = "share") %>% 
  #--- change variable names ---#
  setnames(c(".id", "1", "5"), c("grid_id", "corn_share", "soy_share")) %>% 
  #--- corn share divided by soy share ---#
  .[, c_s_ratio := corn_share / soy_share]
```

---

We are still missing daily precipitation data at the moment. We have decided to use daily weather data from PRISM. Daily PRISM data is a raster data with the cell size of 4 km by 4 km. Figure \@ref(fig:Demo4-show-prism-data) presents precipitation data downloaded for April 1, 2010. It covers the entire contiguous U.S.     

<div class="figure">
<img src="Demonstration_files/figure-html/Demo4-show-prism-data-1.png" alt="Map of PRISM raster data layer" width="672" />
<p class="caption">(\#fig:Demo4-show-prism-data)Map of PRISM raster data layer</p>
</div>

Let's now download PRISM data^[You do not have to run this code to download the data. It is included in the data folder for replication ([here](https://www.dropbox.com/sh/cyx9clgmshwc8eo/AAApv03Qpx84IGKCyF5v2rJ6a?dl=0)).]. This can be done using the `get_prism_dailys()` function from the `prism` package.^[[prism github page](https://github.com/ropensci/prism)]  

<!-- not to be seen -->

```r
options(prism.path = "./Data/PRISM")

get_prism_dailys(
  type = "ppt", 
  minDate = "2014-04-01", 
  maxDate = "2014-09-30", 
  keepZip = FALSE 
)
```

When we use `get_prism_dailys()` to download data^[For this project, I could have just used monthly PRISM data, which can be downloaded using the `get_prism_monthlys()` function. But, in many applications, daily data is necessary, so I wanted to illustrate how to download and process them.], it creates one folder for each day. So, I have about 180 folders inside the folder I designated as the download destination above with the `options()` function. 

<!-- The name of the folder is expressive about what the data inside it is about. For example, the precipitation data for April 1st, 2010 is stored in the folder called "PRISM_ppt_stable_4kmD2_20100401_bil." Inside it, you will see bunch of files with exactly the same prefix, but with different extensions.   --> 

---

We now try to extract precipitation value by day for each of the IA grids by geographically overlaying IA grids onto the PRISM data layer and identify which PRISM cells each of the IA grid encompass. Figure \@ref(fig:Demo4-prism-crop) shows how the first IA grid overlaps with the PRISM cells^[Do not use `st_buffer()` for spatial objects in geographic coordinates (latitude, longitude) if you intend to use the created buffers for any serious IA (it is difficult to get the right distance parameter anyway.). Significant distortion will be introduced to the buffer due to the fact that one degree in latitude and longitude means different distances at the latitude of IA. Here, I am just creating a buffer to extract PRISM cells to display on the map.]. 


```r
#--- read a PRISM dataset ---#
prism_whole <- raster("./Data/PRISM/PRISM_ppt_stable_4kmD2_20140401_bil/PRISM_ppt_stable_4kmD2_20140401_bil.bil") 

#--- align the CRS ---#
IA_grids_rp_prism <- st_transform(IA_grids, projection(prism_whole))

#--- crop the PRISM data for the 1st IA grid ---#
PRISM_1 <- crop(prism_whole, st_buffer(IA_grids_rp_prism[1, ], dist = 0.05))

#--- map them ---#
tm_shape(PRISM_1) +
  tm_raster() +
tm_shape(IA_grids_rp_prism[1, ]) +
  tm_polygons(alpha = 0) +
  tm_layout(frame = NA)
```

<div class="figure">
<img src="Demonstration_files/figure-html/Demo4-prism-crop-1.png" alt="Spatial overlap of an IA grid over PRISM cells" width="672" />
<p class="caption">(\#fig:Demo4-prism-crop)Spatial overlap of an IA grid over PRISM cells</p>
</div>

As you can see, some PRISM grids are fully inside the analysis grid, while others are partially inside it. So, when assigning precipitation values to grids, we will use the coverage-weighted mean of precipitations^[In practice, this may not be advisable. The coverage fraction calculation by `exact_extract()` is done using latitude and longitude. Therefore, the relative magnitude of the fraction numbers incorrectly reflects the actual relative magnitude of the overlapped area. When the spatial resolution of the sources grids (grids from which you extract values) is much smaller relative to that of the target grids (grids to which you assign values to), then a simple average would be very similar to a coverage-weighted mean. For example, CDL consists of 30m by 30m grids, and more than $1,000$ grids are inside one analysis grid.]. 

Unlike the CDL layer, we have 183 raster layers to process. Fortunately, we can process many raster files at the same time very quickly by first "stacking" many raster files first and then applying the `exact_extract()` function. Using `future_lapply()`, we let $6$ cores take care of this task with each processing 31 files, except one of them handling only 28 files.^[Parallelization of extracting values from many raster layers for polygons are discussed in much more detail in Chapter \@ref(Efficient). When I tried stacking all 183 files into one stack and applying `exact_extract`, it did not finish the job after over five minutes. So, I terminated the process in the middle. The parallelized version gets the job done in about $30$ seconds on my desktop.]
We first get all the paths to the PRISM files. 


```r
#--- get all the dates ---#
dates_ls <- seq(as.Date("2014-04-01"), as.Date("2014-09-30"), "days") 

#--- remove hyphen ---#
dates_ls_no_hyphen <- str_remove_all(dates_ls, "-")

#--- get all the prism file names ---#
folder_name <- paste0("PRISM_ppt_stable_4kmD2_", dates_ls_no_hyphen, "_bil") 
file_name <- paste0("PRISM_ppt_stable_4kmD2_", dates_ls_no_hyphen, "_bil.bil") 
file_paths <- paste0("./Data/PRISM/", folder_name, "/", file_name)

#--- take a look ---#
head(file_paths)
```

```
[1] "./Data/PRISM/PRISM_ppt_stable_4kmD2_20140401_bil/PRISM_ppt_stable_4kmD2_20140401_bil.bil"
[2] "./Data/PRISM/PRISM_ppt_stable_4kmD2_20140402_bil/PRISM_ppt_stable_4kmD2_20140402_bil.bil"
[3] "./Data/PRISM/PRISM_ppt_stable_4kmD2_20140403_bil/PRISM_ppt_stable_4kmD2_20140403_bil.bil"
[4] "./Data/PRISM/PRISM_ppt_stable_4kmD2_20140404_bil/PRISM_ppt_stable_4kmD2_20140404_bil.bil"
[5] "./Data/PRISM/PRISM_ppt_stable_4kmD2_20140405_bil/PRISM_ppt_stable_4kmD2_20140405_bil.bil"
[6] "./Data/PRISM/PRISM_ppt_stable_4kmD2_20140406_bil/PRISM_ppt_stable_4kmD2_20140406_bil.bil"
```

We now prepare for parallelized extractions and then implement them using `future_apply()` (you can have a look at Chapter \@ref(par-comp) to familiarize yourself with parallel computation using the `future.apply` package).


```r
#--- define the number of cores to use ---#
num_core <- 6

#--- prepare some parameters for parallelization ---#
file_len <- length(file_paths)
files_per_core <- ceiling(file_len/num_core)

#--- prepare for parallel processing ---#
plan(multiprocess, workers = num_core)

#--- reproject IA grids to the CRS of PRISM data ---#
IA_grids_reprojected <- st_transform(IA_grids, projection(prism_whole))
```

Here is the function that we run in parallel over 6 cores. 


```r
#--- define the function to extract PRISM values by block of files ---#
extract_by_block <- function(i, files_per_core) {

  #--- files processed by core  ---#
  start_file_index <- (i-1) * files_per_core + 1

  #--- indexes for files to process ---#
  file_index <- seq(
    from = start_file_index,
    to = min((start_file_index + files_per_core), file_len),
    by = 1
  )

  #--- extract values ---# 
  data_temp <- file_paths[file_index] %>% # get file names
    #--- stack files ---#
    stack() %>% 
    #--- extract ---#
    exact_extract(., IA_grids_reprojected) %>% 
    #--- combine into one data set ---#
    rbindlist(idcol = "ID") %>% 
    #--- wide to long ---#
    melt(id.var = c("ID", "coverage_fraction")) %>% 
    #--- calculate "area"-weighted mean ---#
    .[, .(value = sum(value * coverage_fraction)/sum(coverage_fraction)), by = .(ID, variable)]

  return(data_temp)
}
```

Now, let's run the function in parallel and calculate precipitation by period.


```r
#--- run the function ---#
precip_by_period <- future_lapply(1:num_core, function(x) extract_by_block(x, files_per_core)) %>% rbindlist() %>% 
  #--- recover the date ---#
  .[, variable := as.Date(str_extract(variable, "[0-9]{8}"), "%Y%m%d")] %>% 
  #--- change the variable name to date ---#
  setnames("variable", "date") %>% 
  #--- define critical period ---#
  .[,critical := "non_critical"] %>% 
  .[month(date) %in% 6:8, critical := "critical"] %>% 
  #--- total precipitation by critical dummy  ---#
  .[, .(precip=sum(value)), by = .(ID, critical)] %>%
  #--- wide to long ---#
  dcast(ID ~ critical, value.var = "precip")
```



We now have grid-level crop share and precipitation data. 

---

Let's merge them and run regression.^[We can match on `grid_id` from `corn_soy` and `ID` from "precip_by_period" because `grid_id` is identical with the row number and ID variables were created so that the ID value of $i$ corresponds to $i$ th row of `IA_grids`.]


```r
#--- crop share ---#
reg_data <- corn_soy[precip_by_period, on = c(grid_id = "ID")]

#--- OLS ---#
reg_results <- lm(c_s_ratio ~ critical + non_critical, data = reg_data)
```

Here is the regression results table.



```r
#--- regression table ---#
stargazer(reg_results, type = "html")
```


<table style="text-align:center"><tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"></td><td><em>Dependent variable:</em></td></tr>
<tr><td></td><td colspan="1" style="border-bottom: 1px solid black"></td></tr>
<tr><td style="text-align:left"></td><td>c_s_ratio</td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">critical</td><td>-0.002<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.0003)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">non_critical</td><td>-0.0003</td></tr>
<tr><td style="text-align:left"></td><td>(0.0003)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">Constant</td><td>2.701<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.161)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Observations</td><td>1,218</td></tr>
<tr><td style="text-align:left">R<sup>2</sup></td><td>0.058</td></tr>
<tr><td style="text-align:left">Adjusted R<sup>2</sup></td><td>0.056</td></tr>
<tr><td style="text-align:left">Residual Std. Error</td><td>0.743 (df = 1215)</td></tr>
<tr><td style="text-align:left">F Statistic</td><td>37.234<sup>***</sup> (df = 2; 1215)</td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"><em>Note:</em></td><td style="text-align:right"><sup>*</sup>p<0.1; <sup>**</sup>p<0.05; <sup>***</sup>p<0.01</td></tr>
</table>

Again, do not read into the results as the econometric model is terrible.  

---

## Demonstration 4: The Impact of Railroad Presence on Corn Planted Acreage {#demo4}


<style>
.book .book-body .page-wrapper .page-inner section.normal table
{
  width:auto;
}
.book .book-body .page-wrapper .page-inner section.normal table td,
.book .book-body .page-wrapper .page-inner section.normal table th,
.book .book-body .page-wrapper .page-inner section.normal table tr
{
  padding:0;
  border:0;
  background-color:#fff;
}
</style>

### Project Overview

---

**Objective**

+ Understand the impact of railroad on corn planted acreage in Illinois

---

**Datasets**

+ USDA corn planted acreage for Illinois downloaded from the USDA  NationalAgricultural Statistics Service (NASS) QuickStats service using `tidyUSDA` package 
+ US railroads (line data) downloaded from [here](https://catalog.data.gov/dataset/tiger-line-shapefile-2015-nation-u-s-rails-national-shapefile)

---

**Econometric Model**

We will estimate the following model:

$$
  y_i = \beta_0 + \beta_1 RL_i + v_i
$$

where $y_i$ is corn planted acreage in county $i$ in Illinois, $RL_i$ is the total length of railroad, and $v_i$ is the error term.

---

**GIS tasks**

+ Download USDA corn planted acreage by county as a spatial dataset (`sf` object)
  * use `tidyUSDA::getQuickStat()`
+ Import US railroad shape file as a spatial dataset (`sf` object) 
  * use `sf:st_read()`
+ Spatially subset (crop) the railroad data to the geographic boundary of Illinois 
  * use `sf_1[sf_2, ]`
+ Find railroads for each county (cross-county railroad will be chopped into pieces for them to fit within a single county)
  * use `sf::st_intersection()`        
+ Calculate the travel distance of each railroad piece
  * use `sf::st_length()`

---

**Preparation for replication**

+ Run the following code to install or load (if already installed) the `pacman` package, and then install or load (if already installed) the listed package inside the `pacman::p_load()` function.


```r
if (!require("pacman")) install.packages("pacman")
pacman::p_load(
  tidyUSDA, # access USDA NASS data
  sf, # vector data operations
  dplyr, # data wrangling
  ggplot2, # for map creation
  stargazer, # regression table generation
  keyring # API management
)  
```

+ Run the following code to define the theme for map:


```r
theme_for_map <- theme(
  axis.ticks = element_blank(),
  axis.text= element_blank(), 
  axis.line = element_blank(),
  panel.border = element_blank(),
  panel.grid.major = element_line(color='transparent'),
  panel.grid.minor = element_line(color='transparent'),
  panel.background = element_blank(),
  plot.background = element_rect(fill = "transparent",color='transparent')
)  
```

### Project Demonstration

We first download corn planted acreage data for 2018 from USDA NASS QuickStat service using `tidyUSDA` package^[In order to actually download the data, you need to obtain the API key [here](https://quickstats.nass.usda.gov/api). Once the API key was obtained, I stored it using `set_key()` from the `keyring` package, which was named "usda_nass_qs_api". In the code to the left, I retrieve the API key using `key_get("usda_nass_qs_api")` in the code. For your replication, replace `key_get("usda_nass_qs_api")` with your own API key.].


```r
(
IL_corn_planted <- getQuickstat(
    #--- use your own API key here fore replication ---#
    key = key_get("usda_nass_qs_api"),
    program = "SURVEY",
    data_item = "CORN - ACRES PLANTED",
    geographic_level = "COUNTY",
    state = "ILLINOIS",
    year = "2018",
    geometry = TRUE
  )  %>% 
  #--- keep only some of the variables ---#
  dplyr::select(year, NAME, county_code, short_desc, Value)
)
```


```
Simple feature collection with 90 features and 5 fields (with 6 geometries empty)
geometry type:  MULTIPOLYGON
dimension:      XY
bbox:           xmin: -91.51308 ymin: 36.9703 xmax: -87.4952 ymax: 42.50848
CRS:            +proj=longlat +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +no_defs
First 10 features:
   year        NAME county_code           short_desc  Value
1  2018      Bureau         011 CORN - ACRES PLANTED 264000
2  2018     Carroll         015 CORN - ACRES PLANTED 134000
3  2018       Henry         073 CORN - ACRES PLANTED 226500
4  2018  Jo Daviess         085 CORN - ACRES PLANTED  98500
5  2018         Lee         103 CORN - ACRES PLANTED 236500
6  2018      Mercer         131 CORN - ACRES PLANTED 141000
7  2018        Ogle         141 CORN - ACRES PLANTED 217000
8  2018      Putnam         155 CORN - ACRES PLANTED  32300
9  2018 Rock Island         161 CORN - ACRES PLANTED  68400
10 2018  Stephenson         177 CORN - ACRES PLANTED 166500
                         geometry
1  MULTIPOLYGON (((-89.8569 41...
2  MULTIPOLYGON (((-90.16133 4...
3  MULTIPOLYGON (((-90.43227 4...
4  MULTIPOLYGON (((-90.50668 4...
5  MULTIPOLYGON (((-89.63118 4...
6  MULTIPOLYGON (((-90.99255 4...
7  MULTIPOLYGON (((-89.68598 4...
8  MULTIPOLYGON (((-89.33303 4...
9  MULTIPOLYGON (((-90.33573 4...
10 MULTIPOLYGON (((-89.9205 42...
```

A nice thing about this function is that the data is downloaded as an `sf` object with county geometry with `geometry = TRUE`. So, you can immediately plot it (Figure \@ref(fig:map-il-corn-acreage)) and use it for later spatial interactions without having to merge the downloaded data to an independent county boundary data.^[`theme_for_map` is a user defined object that defines the theme of figures generated using `ggplot2` for this section. You can find it in **Chap_1_Demonstration.R**.] 


```r
ggplot(IL_corn_planted) +
  geom_sf(aes(fill = Value/1000)) +
  scale_fill_distiller(name = "Planted Acreage (1000 acres)", palette = "YlOrRd", trans = "reverse") +
  theme(
    legend.position = "bottom"
  ) +
  theme_for_map
```

<div class="figure">
<img src="Demonstration_files/figure-html/map-il-corn-acreage-1.png" alt="Map of Con Planted Acreage in Illinois in 2018" width="672" />
<p class="caption">(\#fig:map-il-corn-acreage)Map of Con Planted Acreage in Illinois in 2018</p>
</div>

---

Let's import the U.S. railroad data and reproject to the CRS of `IL_corn_planted`:


```r
rail_roads <- st_read(dsn = "./Data/", layer = "tl_2015_us_rails") %>% 
  #--- reproject to the CRS of IL_corn_planted ---#
  st_transform(st_crs(IL_corn_planted))
```

```
Reading layer `tl_2015_us_rails' from data source `/Users/tmieno2/Dropbox/TeachingUNL/RGIS_Econ/Data' using driver `ESRI Shapefile'
Simple feature collection with 180958 features and 3 fields
geometry type:  MULTILINESTRING
dimension:      XY
bbox:           xmin: -165.4011 ymin: 17.95174 xmax: -65.74931 ymax: 65.00006
CRS:            4269
```

Here is what it looks like:


```r
ggplot(rail_roads) +
  geom_sf() +
  theme_for_map
```

<div class="figure">
<img src="Demonstration_files/figure-html/Demo5-rail-plot-1.png" alt="Map of Railroads" width="672" />
<p class="caption">(\#fig:Demo5-rail-plot)Map of Railroads</p>
</div>

We now crop it to the Illinois state border (Figure \@ref(fig:Demo5-rail-IL-plot)) using `sf_1[sf_2, ]`:




```r
rail_roads_IL <- rail_roads[IL_corn_planted, ]
```


```r
ggplot() +
  geom_sf(data = rail_roads_IL) +
  theme_for_map
```

<div class="figure">
<img src="Demonstration_files/figure-html/Demo5-rail-IL-plot-1.png" alt="Map of railroads in Illinois" width="672" />
<p class="caption">(\#fig:Demo5-rail-IL-plot)Map of railroads in Illinois</p>
</div>

Let's now find railroads for each county, where cross-county railroads will be chopped into pieces so each piece fits completely within a single county, using `st_intersection()`.


```r
rails_IL_segmented <- st_intersection(rail_roads_IL, IL_corn_planted) 
```

Here are the railroads for Richland County:


```r
ggplot() + 
  geom_sf(data = dplyr::filter(IL_corn_planted, NAME == "Richland")) +
  geom_sf(data = dplyr::filter(rails_IL_segmented, NAME == "Richland"), aes( color = LINEARID )) +
  theme(
    legend.position = "bottom"
  ) +
  theme_for_map
```

<img src="Demonstration_files/figure-html/map_seg_rail-1.png" width="672" />

We now calculate the travel distance (Great-circle distance) of each railroad piece using `st_length()` and then sum them up by county to find total railroad length by county.


```r
(
rail_length_county <- mutate(
    rails_IL_segmented, 
    length_in_m = as.numeric(st_length(rails_IL_segmented)),
  ) %>% 
  #--- group by county ID ---#
  group_by(county_code) %>% 
  #--- sum rail length by county ---#
  summarize(length_in_m = sum(length_in_m)) %>% 
  #--- geometry no longer needed ---#
  st_drop_geometry()
)
```

```
# A tibble: 82 x 2
   county_code length_in_m
 * <chr>             <dbl>
 1 001              77221.
 2 003              77290.
 3 007              36764.
 4 011             255441.
 5 015             161726.
 6 017              30585.
 7 019             389226.
 8 021             155794.
 9 023              78587.
10 025              92030.
# … with 72 more rows
```

---

We merge the railroad length data to the corn planted acreage data and estimate the model.


```r
reg_data <- left_join(IL_corn_planted, rail_length_county, by = "county_code") 
```


```r
lm(Value ~ length_in_m, data = reg_data) %>% 
  stargazer(type = "html")
```


<table style="text-align:center"><tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"></td><td><em>Dependent variable:</em></td></tr>
<tr><td></td><td colspan="1" style="border-bottom: 1px solid black"></td></tr>
<tr><td style="text-align:left"></td><td>Value</td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">length_in_m</td><td>0.092<sup>*</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.047)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">Constant</td><td>108,154.800<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(11,418.900)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Observations</td><td>82</td></tr>
<tr><td style="text-align:left">R<sup>2</sup></td><td>0.046</td></tr>
<tr><td style="text-align:left">Adjusted R<sup>2</sup></td><td>0.034</td></tr>
<tr><td style="text-align:left">Residual Std. Error</td><td>69,040.680 (df = 80)</td></tr>
<tr><td style="text-align:left">F Statistic</td><td>3.866<sup>*</sup> (df = 1; 80)</td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"><em>Note:</em></td><td style="text-align:right"><sup>*</sup>p<0.1; <sup>**</sup>p<0.05; <sup>***</sup>p<0.01</td></tr>
</table>

---

## Demonstration 5: Groundwater use for agricultural irrigation


<style>
.book .book-body .page-wrapper .page-inner section.normal table
{
  width:auto;
}
.book .book-body .page-wrapper .page-inner section.normal table td,
.book .book-body .page-wrapper .page-inner section.normal table th,
.book .book-body .page-wrapper .page-inner section.normal table tr
{
  padding:0;
  border:0;
  background-color:#fff;
}
</style>

### Project Overview

---

**Objective**

+ Understand the impact of monthly precipitation on groundwater use for agricultural irrigation

---

**Datasets**

+ Annual groundwater pumping by irrigation wells in Kansas for 2010 and 2011 (originally obtained from the Water Information Management & Analysis System (WIMAS) database)
+ Daymet^[[Daymet website](https://daymet.ornl.gov/)] daily precipitation and maximum temperature downloaded using `daymetr` package

---

**Econometric Model**

The econometric model we would like to estimate is:

$$
   y_{i,t}  = \alpha +  P_{i,t} \beta + T_{i,t} \gamma + \phi_i + \eta_t + v_{i,t}
$$

where $y$ is the total groundwater extracted in year $t$, $P_{i,t}$ and $T_{i,t}$ is the collection of monthly total precipitation and mean maximum temperature April through September in year $t$, respectively, $\phi_i$ is the well fixed effect, $\eta_t$ is the year fixed effect, and $v_{i,t}$ is the error term. 

---

**GIS tasks**

+ download Daymet precipitation and maximum temperature data for each well from within R in parallel
  * use `daymetr::download_daymet()` and `future.apply::future_lapply()`

---

**Preparation for replication**

+ Run the following code to install or load (if already installed) the `pacman` package, and then install or load (if already installed) the listed package inside the `pacman::p_load()` function.


```r
if (!require("pacman")) install.packages("pacman")
pacman::p_load(
  daymetr, # get Daymet data
  sf, #vector data operations
  dplyr, #data wrangling
  data.table, #data wrangling
  ggplot2, #for map creation
  RhpcBLASctl, #to get the number of available cores
  future.apply, #parallelization
  lfe, #fast regression with many fixed effects 
  stargazer #regression table generation
)  
```

### Project Demonstration

We have already collected annual groundwater pumping data by irrigation wells in 2010 and 2011 in Kansas from the Water Information Management & Analysis System (WIMAS) database. Let's read in the groundwater use data.  


```r
#--- read in the data ---#
(
gw_KS_sf <- readRDS( "./Data/gw_KS_sf.rds") 
)
```

```
Simple feature collection with 56225 features and 3 fields
geometry type:  POINT
dimension:      XY
bbox:           xmin: -102.0495 ymin: 36.99561 xmax: -94.70746 ymax: 40.00191
CRS:            EPSG:4269
First 10 features:
   well_id year   af_used                   geometry
1        1 2010  67.00000 POINT (-100.4423 37.52046)
2        1 2011 171.00000 POINT (-100.4423 37.52046)
3        3 2010  30.93438 POINT (-100.7118 39.91526)
4        3 2011  12.00000 POINT (-100.7118 39.91526)
5        7 2010   0.00000 POINT (-101.8995 38.78077)
6        7 2011   0.00000 POINT (-101.8995 38.78077)
7       11 2010 154.00000 POINT (-101.7114 39.55035)
8       11 2011 160.00000 POINT (-101.7114 39.55035)
9       12 2010  28.17239 POINT (-95.97031 39.16121)
10      12 2011  89.53479 POINT (-95.97031 39.16121)
```

We have 28553 wells in total, and each well has records of groundwater pumping (`af_used`) for years 2010 and 2011. Here is the spatial distribution of the wells. 

<img src="Demonstration_files/figure-html/Demo5_map-1.png" width="672" />

<!-- 
#=========================================
# Daymet data download and processing 
#=========================================
-->

--- 

We now need to get monthly precipitation and maximum temperature data. We have decided that we use [Daymet](https://daymet.ornl.gov/) weather data. Here we use the `download_daymet()` function from the `daymetr` package^[[daymetr vignette](https://cran.r-project.org/web/packages/daymetr/vignettes/daymetr-vignette.html)] that allows us to download all the weather variables for a specified geographic location and time period^[See [here]() for a fuller explanation of how to use the `daymetr` package.]. We write a wrapper function that downloads Daymet data and then processes it to find monthly total precipitation and mean maximum temperature^[This may not be ideal for a real research project because the original raw data is not kept. It is often the case that your econometric plan changes on the course of your project (e.g., using other weather variables or using different temporal aggregation of weather variables instead of monthly aggregation). When this happens, you need to download the same data all over again.]. We then loop over the 56225 wells, which is parallelized using the `future_apply()` function^[For parallelized computation, see Chapter \@ref(par-comp)] from the `future.apply` package. This process takes about an hour on my Mac with parallelization on 7 cores. The data is available in the data repository for this course (named as "all_daymet.rds"). 


```r
#--- get the geographic coordinates of the wells ---#
well_locations <- gw_KS_sf %>%
  unique(by = "well_id") %>% 
  dplyr::select(well_id) %>% 
  cbind(., st_coordinates(.))

#--- define a function that downloads Daymet data by well and process it ---#
get_daymet <- function(i) {

  temp_site <- well_locations[i, ]$well_id
  temp_long <- well_locations[i, ]$X
  temp_lat <- well_locations[i, ]$Y

  data_temp <- download_daymet(
      site = temp_site,
      lat = temp_lat,
      lon = temp_long,
      start = 2010,
      end = 2011,
      #--- if TRUE, tidy data is returned ---#
      simplify = TRUE,
      #--- if TRUE, the downloaded data can be assigned to an R object ---#
      internal = TRUE
    ) %>% 
    data.table() %>% 
    #--- keep only precip and tmax ---#
    .[measurement %in% c("prcp..mm.day.", "tmax..deg.c."), ] %>%  
    #--- recover calender date from Julian day ---#
    .[, date := as.Date(paste(year, yday, sep = "-"), "%Y-%j")] %>% 
    #--- get month ---#
    .[, month := month(date)] %>% 
    #--- keep only April through September ---#
    .[month %in% 4:9,] %>% 
    .[, .(site, year, month, date, measurement, value)] %>% 
    #--- long to wide ---#
    dcast(site + year + month + date~ measurement, value.var = "value") %>% 
    #--- change variable names ---#
    setnames(c("prcp..mm.day.", "tmax..deg.c."), c("prcp", "tmax")) %>% 
    #--- find the total precip and mean tmax by month-year ---#
    .[, .(prcp = sum(prcp), tmax = mean(tmax)) , by = .(month, year)] %>% 
    .[, well_id := temp_site]

  return(data_temp)
  gc()
}
```

Here is what one run (for the first well) of `get_daymet()` returns 


```r
#--- one run ---#
(
returned_data <- get_daymet(1)[]
)
```

```
    month year prcp     tmax well_id
 1:     4 2010   42 20.96667       1
 2:     5 2010   94 24.19355       1
 3:     6 2010   70 32.51667       1
 4:     7 2010   89 33.50000       1
 5:     8 2010   63 34.17742       1
 6:     9 2010   15 31.43333       1
 7:     4 2011   25 21.91667       1
 8:     5 2011   26 26.30645       1
 9:     6 2011   23 35.16667       1
10:     7 2011   35 38.62903       1
11:     8 2011   37 36.90323       1
12:     9 2011    9 28.66667       1
```

We get the number of cores you can use by `RhpcBLASctl::get_num_procs()` and parallelize the loop over wells using `future_lapply()`.^[For Mac users, `mclapply` or `pbmclapply` (`mclapply` with progress bar) are good alternatives.]


```r
#--- prepare for parallelization ---#
num_cores <- get_num_procs() - 1 # number of cores
plan(multiprocess, workers = num_cores) # set up cores  

#--- run get_daymet with parallelization ---#
(
all_daymet <- future_lapply(1:nrow(well_locations), get_daymet) %>% 
  rbindlist() 
)
```


```
        month year prcp     tmax well_id
     1:     4 2010   42 20.96667       1
     2:     5 2010   94 24.19355       1
     3:     6 2010   70 32.51667       1
     4:     7 2010   89 33.50000       1
     5:     8 2010   63 34.17742       1
    ---                                 
336980:     5 2011   18 26.11290   78051
336981:     6 2011   25 34.61667   78051
336982:     7 2011    6 38.37097   78051
336983:     8 2011   39 36.66129   78051
336984:     9 2011   23 28.45000   78051
```

---

Before merging the Daymet data, we need to reshape the data into a wide format to get monthly precipitation and maximum temperature as columns.  


```r
#--- long to wide ---#
daymet_to_merge <- dcast(all_daymet, well_id + year ~ month, value.var = c("prcp", "tmax"))

#--- take a look ---#
daymet_to_merge
```

```
       well_id year prcp_4 prcp_5 prcp_6 prcp_7 prcp_8 prcp_9   tmax_4   tmax_5
    1:       1 2010     42     94     70     89     63     15 20.96667 24.19355
    2:       1 2011     25     26     23     35     37      9 21.91667 26.30645
    3:       3 2010     85     62    109    112     83     41 19.93333 21.64516
    4:       3 2011     80    104     44    124    118     14 18.40000 22.62903
    5:       7 2010     44     83     23     99    105     13 18.81667 22.14516
   ---                                                                         
56160:   78049 2011     27      6     38     37     34     36 22.81667 26.70968
56161:   78050 2010     35     48     68    111     56      9 21.38333 24.85484
56162:   78050 2011     26      7     44     38     34     35 22.76667 26.70968
56163:   78051 2010     30     62     48     29     76      3 21.05000 24.14516
56164:   78051 2011     33     18     25      6     39     23 21.90000 26.11290
         tmax_6   tmax_7   tmax_8   tmax_9
    1: 32.51667 33.50000 34.17742 31.43333
    2: 35.16667 38.62903 36.90323 28.66667
    3: 30.73333 32.80645 33.56452 28.93333
    4: 30.08333 35.08065 32.90323 25.81667
    5: 31.30000 33.12903 32.67742 30.16667
   ---                                    
56160: 35.01667 38.32258 36.54839 28.80000
56161: 33.16667 33.88710 34.40323 32.11667
56162: 34.91667 38.32258 36.54839 28.83333
56163: 32.90000 33.83871 34.38710 31.56667
56164: 34.61667 38.37097 36.66129 28.45000
```

Now, let's merge the weather data to the groundwater pumping dataset.


```r
(
reg_data <- data.table(gw_KS_sf) %>% 
  #--- keep only the relevant variables ---#
  .[, .(well_id, year, af_used)] %>% 
  #--- join ---#
  daymet_to_merge[., on = c("well_id", "year")]
)
```

```
       well_id year prcp_4 prcp_5 prcp_6 prcp_7 prcp_8 prcp_9   tmax_4   tmax_5
    1:       1 2010     42     94     70     89     63     15 20.96667 24.19355
    2:       1 2011     25     26     23     35     37      9 21.91667 26.30645
    3:       3 2010     85     62    109    112     83     41 19.93333 21.64516
    4:       3 2011     80    104     44    124    118     14 18.40000 22.62903
    5:       7 2010     44     83     23     99    105     13 18.81667 22.14516
   ---                                                                         
56221:   79348 2011     NA     NA     NA     NA     NA     NA       NA       NA
56222:   79349 2011     NA     NA     NA     NA     NA     NA       NA       NA
56223:   79367 2011     NA     NA     NA     NA     NA     NA       NA       NA
56224:   79372 2011     NA     NA     NA     NA     NA     NA       NA       NA
56225:   80930 2011     NA     NA     NA     NA     NA     NA       NA       NA
         tmax_6   tmax_7   tmax_8   tmax_9   af_used
    1: 32.51667 33.50000 34.17742 31.43333  67.00000
    2: 35.16667 38.62903 36.90323 28.66667 171.00000
    3: 30.73333 32.80645 33.56452 28.93333  30.93438
    4: 30.08333 35.08065 32.90323 25.81667  12.00000
    5: 31.30000 33.12903 32.67742 30.16667   0.00000
   ---                                              
56221:       NA       NA       NA       NA  76.00000
56222:       NA       NA       NA       NA 182.00000
56223:       NA       NA       NA       NA   0.00000
56224:       NA       NA       NA       NA 134.00000
56225:       NA       NA       NA       NA  23.69150
```

---

Let's run regression and display the results.


```r
#--- run FE ---#
reg_results <- felm(
  af_used ~ 
  prcp_4 + prcp_5 + prcp_6 + prcp_7 + prcp_8 + prcp_9 +
  tmax_4 + tmax_5 + tmax_6 + tmax_7 + tmax_8 + tmax_9
  |well_id + year| 0 | well_id,
  data = reg_data
)

#--- display regression results ---#
stargazer(reg_results, type = "html")
```


<table style="text-align:center"><tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"></td><td><em>Dependent variable:</em></td></tr>
<tr><td></td><td colspan="1" style="border-bottom: 1px solid black"></td></tr>
<tr><td style="text-align:left"></td><td>af_used</td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">prcp_4</td><td>-0.053<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.017)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">prcp_5</td><td>0.112<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.010)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">prcp_6</td><td>-0.073<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.008)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">prcp_7</td><td>0.014</td></tr>
<tr><td style="text-align:left"></td><td>(0.010)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">prcp_8</td><td>0.093<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.014)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">prcp_9</td><td>-0.177<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.025)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">tmax_4</td><td>9.159<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(1.227)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">tmax_5</td><td>-7.505<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(1.062)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">tmax_6</td><td>15.134<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(1.360)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">tmax_7</td><td>3.969<sup>**</sup></td></tr>
<tr><td style="text-align:left"></td><td>(1.618)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">tmax_8</td><td>3.420<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(1.066)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">tmax_9</td><td>-11.803<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(1.801)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Observations</td><td>55,754</td></tr>
<tr><td style="text-align:left">R<sup>2</sup></td><td>0.942</td></tr>
<tr><td style="text-align:left">Adjusted R<sup>2</sup></td><td>0.883</td></tr>
<tr><td style="text-align:left">Residual Std. Error</td><td>46.864 (df = 27659)</td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"><em>Note:</em></td><td style="text-align:right"><sup>*</sup>p<0.1; <sup>**</sup>p<0.05; <sup>***</sup>p<0.01</td></tr>
</table>

That's it. Do not bother to try to read into the regression results. Again, this is just an illustration of how R can be used to prepare a regression-ready dataset with spatial variables.  


<!--chapter:end:Demonstration.Rmd-->

# Vector Data Handling with `sf` {#vector-basics}







## Before you start {-}

In this chapter we learn how to use the `sf` package to handle and operate on spatial datasets. The `sf` package uses the class of simple feature (`sf`)^[Yes, it is the same as the package name.] for spatial objects in R. We first learn how `sf` objects store and represent spatial datasets. We then move on to the following practical topics:

+ read and write a shapefile and spatial data in other formats (and why you might not want to use the shapefile system any more, but use other alternative formats)
+ project and reproject spatial objects
+ convert `sf` objects into `sp` objects, vice versa
+ confirm that `dplyr` works well with `sf` objects
+ implement non-interactive (does not involve two `sf` objects) geometric operations on `sf` objects
  * create buffers 
  * find the area of polygons
  * find the centroid of polygons
  * calculate the length of lines

### `sf` or `sp`? {-}

The `sf` package was designed to replace the `sp` package, which has been one of the most popular and powerful spatial packages in R for more than a decade. It has been about four years since the `sf` package was first registered on CRAN. A couple of years back, many other spatial packages did not have support for the package yet. In this [blog post](https://www.r-bloggers.com/should-i-learn-sf-or-sp-for-spatial-r-programming/) the author responded to the questions of whether one should learn `sp` or `sf` saying,

"That's a tough question. If you have time, I would say, learn to use both. sf is pretty new, so a lot of packages that depend on spatial classes still rely on sp. So you will need to know sp if you want to do any integration with many other packages, including raster (as of March 2018).

However, in the future we should see an increasing shift toward the sf package and greater use of sf classes in other packages. I also think that sf is easier to learn to use than sp."

The future has come, and it's not a tough question anymore. I cannot think of any major spatial packages that do not support `sf` package, and `sf` has largely becomes the standard for handling vector data in $R$^[Even if there are packages that do not support `sf`, you can always go back and forth between `sp` and `sf` objects, which we will learn in Chapter \@ref(conv_sp)]. Thus, this lecture note does not cover how to use `sp` at all.

`sf` has several advantages over the `sp` package [@pebesma2018simple].^[There are cases where `sp` is faster completing the same task than `sf`. For example, see the answer to [this question](https://gis.stackexchange.com/questions/324952/spover-vs-sfst-intersection-in-r). But, I doubt the difference between the two is practically important even with bigger data than the test data.] First, it cut off the tie that `sp` had with ESRI shapefile system, which has a somewhat loose way of representing spatial data. Instead, it uses _simple feature access_, which is an open standard supported by Open Geospatial Consortium (OGC). Another important benefit is its compatibility with the `tidyverse` package, which includes widely popular packages like `ggplot2` and `dplyr`. Consequently, map-making with `ggplot()` and data wrangling with a family of `dplyr` functions come very natural to many $R$ users. `sp` objects have different slots for spatial information and attributes data, and they are not amenable to `dplyr` way of data transformation.

### Direction for replication {-}

**Datasets**

All the datasets that you need to import are available [here](https://www.dropbox.com/sh/c2mxn7bfxepd3wm/AAB77AgOnaCg27gosbAJvrLKa?dl=0). In this chapter, the path to files is set relative to my own working directory (which is hidden). To run the codes without having to mess with paths to the files, follow these steps:

+ set a folder (any folder) as the working directory using `setwd()`  
+ create a folder called "Data" inside the folder designated as the working directory (if you have created a "Data" folder to replicate demonstrations in Chapter \@ref(demo), then skip this step)
+ download the pertinent datasets from [here](https://www.dropbox.com/sh/c2mxn7bfxepd3wm/AAB77AgOnaCg27gosbAJvrLKa?dl=0) and put them in the "Data" folder

**Packages**

Run the following code to install or load (if already installed) the `pacman` package, and then install or load (if already installed) the listed package inside the `pacman::p_load()` function.


```r
if (!require("pacman")) install.packages("pacman")
pacman::p_load(
  sf, # vector data operations
  dplyr, # data wrangling
  data.table, # data wrangling
  tmap, # make maps
  mapview # create an interactive map 
)  
```

## Spatial Data Structure

Here we learn how the `sf` package stores spatial data along with the definition of three key `sf` object classes: simple feature geometry (`sfg`), simple feature geometry list-column (`sfc`), and simple feature (`sf`). The `sf` package provides a simply way of storing geographic information and the attributes of the geographic units in a single dataset. This special type of dataset is called simple feature (`sf`). It is best to take a look at an example to see how this is achieved. We use North Carolina county boundaries with county attributes (Figure \@ref(fig:nc-county)).  


```r
#--- a dataset that comes with the sf package ---#
nc <- st_read(system.file("shape/nc.shp", package="sf")) 
```

```
Reading layer `nc' from data source `/Library/Frameworks/R.framework/Versions/4.0/Resources/library/sf/shape/nc.shp' using driver `ESRI Shapefile'
Simple feature collection with 100 features and 14 fields
geometry type:  MULTIPOLYGON
dimension:      XY
bbox:           xmin: -84.32385 ymin: 33.88199 xmax: -75.45698 ymax: 36.58965
CRS:            4267
```

<div class="figure">
<img src="VectorDataBasics_files/figure-html/nc-county-1.png" alt="North Carolina county boundary" width="672" />
<p class="caption">(\#fig:nc-county)North Carolina county boundary</p>
</div>

As you can see below, this dataset is of class `sf` (and `data.frame` at the same time).


```r
class(nc)
```

```
[1] "sf"         "data.frame"
```

Now, let's take a look inside of `nc`.


```r
#--- take a look at the data ---#
head(nc)
```

```
Simple feature collection with 6 features and 14 fields
geometry type:  MULTIPOLYGON
dimension:      XY
bbox:           xmin: -81.74107 ymin: 36.07282 xmax: -75.77316 ymax: 36.58965
CRS:            4267
   AREA PERIMETER CNTY_ CNTY_ID        NAME  FIPS FIPSNO CRESS_ID BIR74 SID74
1 0.114     1.442  1825    1825        Ashe 37009  37009        5  1091     1
2 0.061     1.231  1827    1827   Alleghany 37005  37005        3   487     0
3 0.143     1.630  1828    1828       Surry 37171  37171       86  3188     5
4 0.070     2.968  1831    1831   Currituck 37053  37053       27   508     1
5 0.153     2.206  1832    1832 Northampton 37131  37131       66  1421     9
6 0.097     1.670  1833    1833    Hertford 37091  37091       46  1452     7
  NWBIR74 BIR79 SID79 NWBIR79                       geometry
1      10  1364     0      19 MULTIPOLYGON (((-81.47276 3...
2      10   542     3      12 MULTIPOLYGON (((-81.23989 3...
3     208  3616     6     260 MULTIPOLYGON (((-80.45634 3...
4     123   830     2     145 MULTIPOLYGON (((-76.00897 3...
5    1066  1606     3    1197 MULTIPOLYGON (((-77.21767 3...
6     954  1838     5    1237 MULTIPOLYGON (((-76.74506 3...
```

Just like a regular `data.frame`, you see a number of variables (attributes) except that you have a variable called `geometry` at the end. Each row represents a single geographic unit (here, county). Ashe County (1st row) has area of $0.114$, FIPS code of $37009$, and so on. And the entry in `geometry` column at the first row represents the geographic information of Ashe County. An entry in the `geometry` column is a simple feature geometry (`sfg`), which is an $R$ object that represents the geographic information of a single geometric feature (county in this example). There are different types of `sfg`s (`POINT`, `LINESTRING`, `POLYGON`, `MULTIPOLYGON`, etc). Here, `sfg`s representing counties in NC are of type `MULTIPOLYGON`. Let's take a look inside the `sfg` for Ashe County using `st_geometry()`.


```r
st_geometry(nc[1, ])[[1]][[1]]
```

```
[[1]]
           [,1]     [,2]
 [1,] -81.47276 36.23436
 [2,] -81.54084 36.27251
 [3,] -81.56198 36.27359
 [4,] -81.63306 36.34069
 [5,] -81.74107 36.39178
 [6,] -81.69828 36.47178
 [7,] -81.70280 36.51934
 [8,] -81.67000 36.58965
 [9,] -81.34530 36.57286
[10,] -81.34754 36.53791
[11,] -81.32478 36.51368
[12,] -81.31332 36.48070
[13,] -81.26624 36.43721
[14,] -81.26284 36.40504
[15,] -81.24069 36.37942
[16,] -81.23989 36.36536
[17,] -81.26424 36.35241
[18,] -81.32899 36.36350
[19,] -81.36137 36.35316
[20,] -81.36569 36.33905
[21,] -81.35413 36.29972
[22,] -81.36745 36.27870
[23,] -81.40639 36.28505
[24,] -81.41233 36.26729
[25,] -81.43104 36.26072
[26,] -81.45289 36.23959
[27,] -81.47276 36.23436
```

As you can see, the `sfg` consists of a number of points (pairs of two numbers). Connecting the points in the order they are stored delineates the Ashe County boundary.


```r
plot(st_geometry(nc[1, ])) 
```

<img src="VectorDataBasics_files/figure-html/unnamed-chunk-4-1.png" width="672" />

We will take a closer look at different types of `sfg` in the next section. 

Finally, the `geometry` variable is a list of individual `sfg`s, called simple feature geometry list-column (`sfc`).


```r
dplyr::select(nc, geometry)
```

```
Simple feature collection with 100 features and 0 fields
geometry type:  MULTIPOLYGON
dimension:      XY
bbox:           xmin: -84.32385 ymin: 33.88199 xmax: -75.45698 ymax: 36.58965
CRS:            4267
First 10 features:
                         geometry
1  MULTIPOLYGON (((-81.47276 3...
2  MULTIPOLYGON (((-81.23989 3...
3  MULTIPOLYGON (((-80.45634 3...
4  MULTIPOLYGON (((-76.00897 3...
5  MULTIPOLYGON (((-77.21767 3...
6  MULTIPOLYGON (((-76.74506 3...
7  MULTIPOLYGON (((-76.00897 3...
8  MULTIPOLYGON (((-76.56251 3...
9  MULTIPOLYGON (((-78.30876 3...
10 MULTIPOLYGON (((-80.02567 3...
```

Elements of a geometry list-column are allowed to be different in nature from other elements^[This is just like a regular `list` object that can contain mixed types of elements: numeric, character, etc]. In the `nc` data, all the elements (`sfg`s) in `geometry` column are `MULTIPOLYGON`. However, you could also have `LINESTRING` or `POINT` objects mixed with `MULTIPOLYGONS` objects in a single `sf` object if you would like. 

## Simple feature geometry, simple feature geometry list-column, and simple feature

Here, we learn how different types of `sfg` are constructed. We also learn how to create `sfc` and `sf` from `sfg` from scratch.^[Creating spatial objects from scratch yourself is an unnecessary skill for many of us as economists. But, it is still good to know the underlying structure of the data. Also, occasionally the need arises. For example, I had to construct spatial objects from scratch when I designed on-farm randomized nitrogen trials. In such cases, it is of course necessary to understand how different types of `sfg` are constructed, create `sfc` from a collection of `sfg`s, and then create an `sf` from an `sfc`.]    

### Simple feature geometry (`sfg`)

The `sf` package uses a class of `sfg` (simple feature geometry) objects to represent a geometry of a single geometric feature (say, a city as a point, a river as a line, county and school district as polygons). There are different types of `sfg`s. Here are some example feature types that we commonly encounter as an economist^[You will hardly see the other geometry types: MULTIPOINT and GEOMETRYCOLLECTION. You may see GEOMETRYCOLLECTION after intersecting two spatial objects. You can see [here](https://r-spatial.github.io/sf/articles/sf1.html#sfg-simple-feature-geometry-1) if you are interested in learning what they are.]:

+ `POINT`: area-less feature that represents a point (e.g., well, city, farmland) 
+ `LINESTRING`: (e.g., a tributary of a river) 
+ `MULTILINESTRING`: (e.g., river with more than one tributary) 
+ `POLYGON`: geometry with a positive area (e.g., county, state, country)
+ `MULTIPOLYGON`: collection of polygons to represent a single object (e.g., countries with islands: U.S., Japan)

---

`POINT` is the simplest geometry type and is represented by a vector of two^[or three to represent a point in the three-dimensional space] numeric values. An example below shows how a `POINT` feature can be made from scratch:


```r
#--- create a POINT ---#
a_point <- st_point(c(2,1))
```

The `st_point()` function creates a `POINT` object when supplied with a vector of two numeric values. If you check the class of the newly created object,


```r
#--- check the class of the object ---#
class(a_point)
```

```
[1] "XY"    "POINT" "sfg"  
```

you can see that it's indeed a `POINT` object. But, it's also an `sfg` object. So, `a_point` is an `sfg` object of type `POINT`. 

---

A `LINESTRING` objects are represented by a sequence of points:  


```r
#--- collection of points in a matrix form ---#
s1 <- rbind(c(2,3),c(3,4),c(3,5),c(1,5))

#--- see what s1 looks like ---#
s1
```

```
     [,1] [,2]
[1,]    2    3
[2,]    3    4
[3,]    3    5
[4,]    1    5
```

```r
#--- create a "LINESTRING" ---#
a_linestring <- st_linestring(s1)

#--- check the class ---#
class(a_linestring)
```

```
[1] "XY"         "LINESTRING" "sfg"       
```

`s1` is a matrix where each row represents a point. By applying `st_linestring()` function to `s1`, you create a `LINESTRING` object. Let's see what the line looks like.


```r
plot(a_linestring)
```

<img src="VectorDataBasics_files/figure-html/plot_line-1.png" width="672" />

As you can see, each pair of consecutive points in the matrix are connected by a straight line to form a line. 

---

A `POLYGON` is very similar to `LINESTRING` in the manner it is represented. 


```r
#--- collection of points in a matrix form ---#
p1 <- rbind(c(0,0), c(3,0), c(3,2), c(2,5), c(1,3), c(0,0))

#--- see what s1 looks like ---#
p1
```

```
     [,1] [,2]
[1,]    0    0
[2,]    3    0
[3,]    3    2
[4,]    2    5
[5,]    1    3
[6,]    0    0
```

```r
 #--- create a "LINESTRING" ---#
a_polygon <- st_polygon(list(p1))

#--- check the class ---#
class(a_polygon)
```

```
[1] "XY"      "POLYGON" "sfg"    
```

```r
#--- see what it looks like ---#
plot(a_polygon)
```

<img src="VectorDataBasics_files/figure-html/polygon_1-1.png" width="672" />

Just like the `LINESTRING` object we created earlier, a `POLYGON` is represented by a collection of points. The biggest difference between them is that we need to have some positive area enclosed by lines connecting the points. To do that, you have the the same point for the first and last points to close the loop: here, it's `c(0,0)`. A `POLYGON` can have a hole in it. The first matrix of a list becomes the exterior ring, and all the subsequent matrices will be holes within the exterior ring.  


```r
#--- a hole within p1 ---#
p2 <- rbind(c(1,1), c(1,2), c(2,2), c(1,1))

#--- create a polygon with hole ---#
a_plygon_with_a_hole <- st_polygon(list(p1,p2))

#--- see what it looks like ---#
plot(a_plygon_with_a_hole)
```

<img src="VectorDataBasics_files/figure-html/polygon_hole-1.png" width="672" />

---

You can create a `MULTIPOLYGON` object in a similar manner. The only difference is that you supply a list of lists of matrices, with each inner list representing a polygon. An example below: 


```r
#--- second polygon ---#
p3 <- rbind(c(4,0), c(5,0), c(5,3), c(4,2), c(4,0)) 

#--- create a multipolygon ---#
a_multipolygon <- st_multipolygon(list(list(p1,p2), list(p3)))

#--- see what it looks like ---#
plot(a_multipolygon)
```

<img src="VectorDataBasics_files/figure-html/multi_polygon-1.png" width="672" />

Each of `list(p1,p2)`, `list(p3,p4)`, `list(p5)` represents a polygon. You supply a list of these lists to the `st_multipolygon()` function to make a `MULTIPOLYGON` object.


### Create simple feature geometry list-column (`sfc`) and simple feature (`sf`) from scratch

To make a simple feature geometry list-column (`sfc`), you can simply supply a list of `sfg` to the `st_sfc()` function as follows:


```r
#--- create an sfc ---#
sfc_ex <- st_sfc(list(a_point,a_linestring,a_polygon,a_multipolygon))
```

To create an `sf` object, you first add an `sfc` as a column to a `data.frame`.  


```r
#--- create a data.frame ---#
df_ex <- data.frame(
  name=c('A','B','C','D')
)

#--- add the sfc as a column ---#
df_ex$geometry <- sfc_ex 

#--- take a look ---#
df_ex
```

```
  name                       geometry
1    A                    POINT (2 1)
2    B LINESTRING (2 3, 3 4, 3 5, ...
3    C POLYGON ((0 0, 3 0, 3 2, 2 ...
4    D MULTIPOLYGON (((0 0, 3 0, 3...
```

At this point, it is not yet recognized as an `sf` by R yet.


```r
#--- see what it looks like (this is not an sf object yet) ---#
class(df_ex)
```

```
[1] "data.frame"
```

You can register it as an `sf` object using `st_as_sf()`.


```r
#--- let R recognize the data frame as sf ---#
sf_ex <- st_as_sf(df_ex)

#--- see what it looks like ---#
sf_ex
```

```
Simple feature collection with 4 features and 1 field
geometry type:  GEOMETRY
dimension:      XY
bbox:           xmin: 0 ymin: 0 xmax: 5 ymax: 5
CRS:            NA
  name                       geometry
1    A                    POINT (2 1)
2    B LINESTRING (2 3, 3 4, 3 5, ...
3    C POLYGON ((0 0, 3 0, 3 2, 2 ...
4    D MULTIPOLYGON (((0 0, 3 0, 3...
```

As you can see `sf_ex` is now recognized also as an `sf` object.  


```r
#--- check the class ---#
class(sf_ex)
```

```
[1] "sf"         "data.frame"
```

## Reading and writing vector data

The vast majority of people still use ArcGIS to handle spatial data, which has its own system of storing spatial data^[See [here]() for how spatial datasets can be stores in various other formats.] called shapefile. So, chances are that your collaborators use shapefiles. Moreover, there are many GIS data online that are available only as shapefiles. So, it is important to learn how to read and write shapefiles. 

### Reading a shapefile

We can use `st_read()` to read a shapefile. It reads in a shapefile and then turn the data into an sf object. Let's take a look at an example. 


```r
#--- read a NE county boundary shapefile ---#
nc_loaded <- st_read(dsn = "./Data", "nc") 
```

Typically, you have two arguments to specify for `st_read()`. The first one is `dsn`, which is basically the path to folder in which the shapefile you want to import is stored. The second one is the name of the shapefile. Notice that you do not add `.shp` extension to the file name: `nc`, not `nc.shp`.^[When storing a spatial dataset, ArcGIS divides the information into separate files. All of them have the same prefix, but have different extensions. We typically say we read a shapefile, but we really are importing all these files including the shapefile with the .shp extension. When you read those data, you just refer to the common prefix because you really are importing all the files, not just a .shp file.].

### Writing to a shapefile

Writing an `sf` object as a shapefile is just as easy. You use the `st_write()` function, with the first argument being the name of the `sf` object you are exporting, and the second being the name of the new shapefile. For example, the code below will export an `sf` object called `nc_loaded` as `nc2.shp` (along with other supporting files). 


```r
st_write(nc_loaded, dsn="./Data", "nc2", driver="ESRI Shapefile", append = FALSE)
```

`append = FALSE` forces writing the data when a file already exists with the same name. Without the option, this happens.


```r
st_write(nc_loaded, dsn="./Data", "nc2", driver="ESRI Shapefile")
```

```
Layer nc2 in dataset ./Data already exists:
use either append=TRUE to append to layer or append=FALSE to overwrite layer
```

```
Error in CPL_write_ogr(obj, dsn, layer, driver, as.character(dataset_options), : Dataset already exists.
```

### Better alternatives 

Now, if your collaborator is using ArcGIS and demanding that he/she needs a shapefile for his/her work, sure you can use the above command to write a shapefile. But, there is really no need to work with the shapefile system. One of the alternative data formats that is considered superior to the shapefile system is GeoPackage^[[here](https://www.geopackage.org/)], which overcomes various limitations associated with shapefile^[see the last paragraph of [chapter 7.5 of this book](https://csgillespie.github.io/efficientR/data-carpentry.html#data-processing-with-data.table), [this blogpost](https://carto.com/blog/fgdb-gpkg/), or [this](http://switchfromshapefile.org/)]. Unlike the shapefile system, it produces only a single file with .gpkg extension.^[Am I the only one who gets very frustrated when your collaborator attaches 15 files for three geographic objects to an email? It could have been just three files using the GeoPackage format.] Note that GeoPackage files can also be easily read into ArcGIS. So, it might be worthwhile to convince your collaborators to stop using shapefiles and start using GeoPackage.  


```r
#--- write as a gpkg file ---#
st_write(nc, dsn = "./Data/nc.gpkg", append = FALSE)

#--- read a gpkg file ---#
nc <- st_read("./Data/nc.gpkg")
```

Or better yet, if your collaborator uses R (or if it is only you who is going to use the data), then just save it as an rds file using `saveRDS()`, which can be of course read using `readRDS()`.


```r
#--- save as an rds ---#
saveRDS(nc, "/Users/tmieno2/Box/Teaching/AAEA R/GIS/nc_county.rds")

#--- read an rds ---#
nc <- readRDS("/Users/tmieno2/Box/Teaching/AAEA R/GIS/nc_county.rds")
```

The use of rds files can be particularly attractive when the dataset is large because rds files are typically more memory efficient than shapefiles, eating up less of your disk memory. 

As you can see here, it is a myth that spatial datasets have to be stored as shapefiles.


## Interactive view of an `sf` object

Sometimes it is useful to be able to tell where spatial objects are and what values are associated with them on a map. The `mapview()` function from the `mapview` package can create an interactive map where you can point to a spatial object and the associated information is revealed on the map. Let's use the North Carolina county map as an example here:


```r
#--- read the NC county map data ---#
nc <- st_read(system.file("shape/nc.shp", package="sf")) 

#--- generate an interactive map ---#
mapview(nc)  
```


```r
mapview(breweries, zcol = c("brewery", "village", "founded"), burst = TRUE) 
```

<!--html_preserve--><div id="htmlwidget-c915bd0a0cf1d4d06e5c" style="width:672px;height:480px;" class="leaflet html-widget"></div>
<script type="application/json" data-for="htmlwidget-c915bd0a0cf1d4d06e5c">{"x":{"options":{"minZoom":1,"maxZoom":52,"crs":{"crsClass":"L.CRS.EPSG3857","code":null,"proj4def":null,"projectedBounds":null,"options":{}},"preferCanvas":false,"bounceAtZoomLimits":false,"maxBounds":[[[-90,-370]],[[90,370]]]},"calls":[{"method":"addProviderTiles","args":["CartoDB.Positron",1,"CartoDB.Positron",{"errorTileUrl":"","noWrap":false,"detectRetina":false}]},{"method":"addProviderTiles","args":["CartoDB.DarkMatter",2,"CartoDB.DarkMatter",{"errorTileUrl":"","noWrap":false,"detectRetina":false}]},{"method":"addProviderTiles","args":["OpenStreetMap",3,"OpenStreetMap",{"errorTileUrl":"","noWrap":false,"detectRetina":false}]},{"method":"addProviderTiles","args":["Esri.WorldImagery",4,"Esri.WorldImagery",{"errorTileUrl":"","noWrap":false,"detectRetina":false}]},{"method":"addProviderTiles","args":["OpenTopoMap",5,"OpenTopoMap",{"errorTileUrl":"","noWrap":false,"detectRetina":false}]},{"method":"createMapPane","args":["point",440]},{"method":"addCircleMarkers","args":[[49.71979,50.125793,49.420804,50.161975,49.977199,49.884051,49.502098,49.892226,49.897336,49.904426,49.889283,49.891858,49.897233,49.274716,49.938394,49.970583,49.963579,49.861905,49.794334,50.136445,49.801844,49.802012,49.701477,49.067436,49.070292,49.93162,50.06583,49.77994,50.008198,49.060542,49.561804,49.595108,49.602554,49.966609,50.243835,49.72581,49.7202,50.075903,49.441712,49.880748,49.881398,49.637445,49.644533,49.645651,49.838404,49.80798,49.615866,50.070044,50.163521,49.534947,49.50683,49.850985,49.81609,48.900742,49.707329,49.884229,50.323171,49.677827,49.792701,49.932447,49.985221,49.450083,49.710838,49.276265,49.82981,50.079029,49.958237,49.554706,49.812436,49.92479,50.441619,49.882777,49.502098,49.861905,49.070292,49.77994,49.060542,49.602554,49.645651,49.50683,48.900742,49.707329,49.276265,49.727998,49.737703,49.755953,49.812489,49.81292,49.794041,49.830486,49.830437,49.844875,49.850922,49.845112,49.864998,49.863762,49.932354,49.914055,49.977985,50.005459,49.86779,49.834755,49.819016,49.868614,49.98679,49.950073,50.035191,49.845585,49.807022,49.770496,49.741394,49.877293,49.311859,49.884663,49.845516,49.841331,49.84127,49.415714,49.358559,49.328201,49.422028,49.912956,49.906281,50.085987,49.800175,49.792423,49.82259,49.882729,49.918533,49.916447,49.916149,49.955219,49.926094,49.756046,49.769699,49.759079,49.759056,49.759369,49.75658,49.75547,49.757275,49.759151,49.759079,49.769176,49.76992,49.760632,49.756065,49.780872,49.681698,50.032757,50.070938,50.03249,50.077583,50.077785,50.116096,50.114841,49.865234,49.969555,49.807861,49.814632,49.924839,49.93961,49.948387,49.866604,49.857391,49.856892,49.894829,49.828178,49.822258,49.75914,49.777538,49.959484,49.958523,50.020378,50.061211,50.180557,49.686993,49.67157,49.67107,49.819889,49.953476,50.079861,50.096077,50.096272,50.066387,50.061771,49.910885,49.923359,49.929886,49.969193,50.019966,49.85778,49.710793,49.701366,50.124729,50.167877,49.979191,48.975365,49.66864,49.977264,49.976788,49.976379,49.67775,49.644543,49.629822,49.524117,49.757332,49.761578,49.756756,49.590153,49.590439,49.970467,49.932583,49.959499,49.940926,50.163834,50.187668,49.505432,49.504299,50.126839,50.072201,50.015476,49.98518,50.0617237],[10.889217,11.238725,10.85194,10.078368,9.97323,11.228988,10.416021,10.884839,10.89281,10.852339,10.887001,10.884856,10.89273,10.928096,11.551041,10.882412,10.882666,11.291932,11.509409,11.24823,11.031657,11.032392,11.163238,10.34418,10.316987,10.859199,10.959201,11.186931,10.905149,10.965571,11.368508,11.009011,11.005049,10.701621,11.935392,11.059662,11.056749,9.462785,10.996247,11.011369,11.014066,10.120459,11.252699,11.248618,10.764381,11.076758,10.630027,10.311011,10.827669,11.157948,11.428338,11.45417,10.986859,11.029479,10.806113,11.267583,11.911507,11.252911,9.623338,11.149891,11.773399,11.308721,11.172792,10.685605,11.53456,11.02381,10.956485,11.22997,11.353455,10.665279,10.11763,11.129541,10.416021,11.291932,10.316987,11.186931,10.965571,11.005049,11.248618,11.428338,11.029479,10.806113,10.685605,11.202701,11.223148,11.175664,11.353571,11.524621,11.510147,11.533767,11.534137,11.509316,11.454213,11.346436,11.337445,11.170451,11.150093,11.235613,11.154755,11.15476,10.997014,10.860221,10.677697,10.812709,10.850785,10.861525,10.879812,10.731019,11.116785,11.057176,10.666598,11.010854,10.390196,10.586872,10.729609,10.733358,10.732983,10.42308,10.697315,10.688195,10.588937,10.751747,10.711439,11.068516,10.924503,10.886357,10.887907,11.129483,11.074138,11.044945,11.050299,11.020145,11.006171,11.175526,10.948509,10.955316,10.954791,10.954485,10.949357,10.947157,10.978947,10.975272,10.982155,11.005458,11.011348,10.894425,10.914654,10.842025,10.882693,11.126538,11.061478,11.126356,11.047853,11.04399,10.965482,11.055395,10.998409,10.885327,11.076536,10.896045,10.664581,10.676866,10.596753,10.65544,10.691162,10.69114,10.724936,10.741153,10.736695,10.900771,11.10287,10.958288,10.956261,10.877286,10.864127,10.995174,11.247886,10.721835,10.723262,11.074085,10.874963,10.990276,10.949882,10.951824,10.958208,10.965541,10.832258,10.778682,10.694327,10.802162,10.827091,10.857496,11.172908,11.163225,10.879002,10.963615,11.090071,10.90459,10.73482,11.036384,11.035488,11.033838,11.253052,11.250622,11.25257,11.585255,10.620051,10.619633,10.617878,10.769536,10.76099,10.721189,10.95277,11.000506,10.971659,10.827574,10.842425,11.737261,11.741035,10.930734,11.545622,11.503718,11.558312,11.0738897],6,null,"brewery",{"crs":{"crsClass":"L.CRS.EPSG3857","code":null,"proj4def":null,"projectedBounds":null,"options":{}},"pane":"point","stroke":true,"color":"#333333","weight":1,"opacity":[0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9],"fill":true,"fillColor":["#008B98","#00B388","#471E67","#D8E12C","#471E67","#46226A","#00B18A","#481C66","#2F437F","#34C86F","#46CA6A","#009097","#009996","#005A8C","#45266C","#23C771","#00B08B","#4CCB69","#00AE8C","#7CD358","#68D060","#B6DD3A","#412F72","#005D8D","#D2E02E","#363D7C","#00B686","#A3DA44","#A3DA44","#00BC80","#31427F","#41CA6C","#BADD38","#00B884","#98D84A","#00578A","#005E8E","#D2E02E","#C6DF32","#00598B","#007896","#00C179","#254A83","#5FCE63","#007495","#00AF8B","#00A890","#007E97","#92D74D","#83D454","#00B487","#7FD456","#007696","#01C574","#00B289","#00A193","#95D74C","#006D93","#01C574","#2CC770","#007194","#51CC68","#1B4E86","#00C17A","#432B70","#BADD38","#00A392","#00A791","#00C278","#00BD7F","#78D259","#175087","#00B18A","#4CCB69","#D2E02E","#A3DA44","#00BC80","#BADD38","#5FCE63","#00B487","#01C574","#00B289","#00C17A","#008498","#008097","#008298","#00C377","#00568A","#006490","#007A97","#007A97","#00A094","#009D95","#00608E","#009297","#422D71","#00BE7D","#009A95","#16C673","#00A890","#008E98","#3C3777","#00AA8F","#70D15D","#009896","#CFE02F","#2D4580","#006691","#2D4580","#009796","#BDDD37","#00BF7C","#87D553","#3E3475","#6CD05E","#6CD05E","#00C475","#005C8D","#00B586","#63CF61","#008A98","#007996","#009397","#C9DF31","#00A592","#403073","#008197","#008498","#006A92","#006992","#008998","#006E94","#007595","#74D25B","#284882","#00BA82","#5ACE65","#8AD651","#8ED64F","#008D98","#56CD66","#87D553","#007C97","#00A98F","#DBE12C","#284882","#2D4580","#006390","#00B08B","#393A7A","#006691","#007094","#3A3878","#006390","#008898","#00A094","#3AC96D","#007194","#008E98","#9CD848","#008D98","#3D3576","#00AB8E","#009796","#00A691","#45246B","#44286D","#009497","#A6DA43","#00B983","#A9DB41","#007295","#00A293","#015489","#B0DC3D","#343F7D","#00B785","#00AD8D","#008598","#ADDB3F","#00A293","#007A97","#00618F","#009F94","#1F4D85","#33407E","#00AC8D","#007D97","#009E94","#2B4781","#009297","#008197","#383C7B","#422D71","#008F97","#00B785","#00C07B","#D5E12D","#CCE030","#9FD946","#3F3274","#3A3878","#006B93","#224B84","#46CA6A","#008698","#C0DE35","#C3DE34","#009597","#175087","#125187","#481A65","#006791","#3A3878","#0A5388","#008E98","#00BE7D","#00BB81","#B3DC3C","#009B95","#005C8D","#43296E","#472069","#481864"],"fillOpacity":[0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6]},null,null,["<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>1&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Rittmayer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Adelsdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1422&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>2&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauhaus Leikeim&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Altenkunstadt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1887&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>3&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Ammerndorfer Bier Dorn-Braeu H. Murmann GmbH & Co. KG&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ammerndorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1730&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>4&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Wittelsbacher Turm Braeu GmbH&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bad Kissingen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>5&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Arnsteiner Brauerei&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Arnstein&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1885&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>6&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Aufsesser Brauerei&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Aufsess&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1886&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>7&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauhaus Doebler&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bad Windsheim&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1867&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>8&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Ambraeusianum GmbH&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bamberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>2004&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>9&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Faessla GmbH & Co. KG&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bamberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1694&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>10&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Kaiserdom Specialitaeten Brauerei GmbH Bamberg&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bamberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1718&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>11&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Klosterbraeu Bamberg&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bamberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1533&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>12&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Schlenkerla&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bamberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1405&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>13&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Spezial&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bamberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1536&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>14&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Gundel GmbH&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Barthelmesaurach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1887&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>15&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Becher Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bayreuth&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1781&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>16&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Huemmer Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Breitenguessbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>2011&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>17&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauhaus Binkert GmbH & Co. KG&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Breitenguessbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>2012&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>18&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Krug-Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Waischenfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1834&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>19&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei-Gasthof Herold&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Buechenbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1568&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>20&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Privatbrauerei Guenther&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Burgkunstadt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1840&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>21&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Loewenbraeu Buttenheim&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Buttenheim&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1880&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>22&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>St. GeorgenBraeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Buttenheim&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1624&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>23&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Alt Dietzhof&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Leutenbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1886&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>24&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hauf KG&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Dinkelsbuehl&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1901&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>25&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Weib's Brauhaus Dinkelsbuehl&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Dinkelsbuehl&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1999&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>26&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Eichhorn&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hallstadt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>27&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Ebensfelder Brauhaus&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ebensfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1752&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>28&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Schwanenbraeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ebermannstadt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1812&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>29&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Schwanen-Braeu Ebing&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ebing&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1859&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>30&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Fuerst Carl Schlossbrauerei Ellingen&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ellingen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1690&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>31&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Enzensteiner&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schnaittach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1998&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>32&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Kitzmann-Braeu GmbH & Co. Kg&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Erlangen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1712&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>33&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Steinbach Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Erlangen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1653&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>34&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Eschenbacher Privatbrauerei GmbH&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Eltmann&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1958&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>35&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Schlossbrauerei Stelzer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Oberkotzau&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1353&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>36&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Greif&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Forchheim&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1848&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>37&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hebendanz GmbH&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Forchheim&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1579&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>38&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Waldschlossbrauerei Frammersbach&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Frammersbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1868&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>39&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Tucher Braeu GmbH & Co. KG&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Fuerth&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1672&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>40&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Griess&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Geisfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1872&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>41&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Krug&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Geisfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1834&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>42&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Hausbrauerei Duell&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Marktbreit&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1654&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>43&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Friedmann&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Graefenberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1875&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>44&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Lindenbraeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Graefenberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1866&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>45&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Kaiser&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Burgebrach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1783&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>46&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei-Gasthof Sauer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Gunzendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1612&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>47&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Windsheimer GmbH&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Gutenstetten&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1767&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>48&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Martin&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hausen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>2008&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>49&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Scharpf Heilgersdorf&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Heilgersdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1870&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>50&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Red Castle Brew&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Heroldsberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>2013&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>51&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Buergerbraeu Hersbruck, Deinlein & Co.&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hersbruck&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1920&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>52&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Privatbrauerei Stoeckel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ahorntal&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1866&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>53&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Kraus&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hirschaid&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1664&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>54&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Hochholzer Brauhaus Poeverlein GbR&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Solnhofen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>2005&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>55&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauhaus Hoechstadt&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hoechstadt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1926&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>56&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei und Gasthof Reichold GmbH&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Aufsess&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1906&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>57&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Scherdel Bier GmbH & Co. KG&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hof&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1831&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>58&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hofmann/Nentwig GbR&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Graefenberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1897&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>59&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Homburger Brauscheuere&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Homburg a. M.&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>2007&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>60&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Huppendorfer Bier&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Koenigsfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>61&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Huetten&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Warmensteinach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1887&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>62&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Leinburger Bier&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Leinburg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1617&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>63&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Gasthof Drummer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Lautenbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1738&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>64&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Hauff Braeu Lichtnerau GmbH & Co. KG&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Lichtenau&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>65&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei & Gastwirtschaft Kuerzdoerfer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Lindenhardt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1866&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>66&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Staffelberg-Braeu GmbH & Co. KG&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Loffeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1856&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>67&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Wagner GmbH&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Merkendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1797&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>68&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Wiethaler&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Neunhof bei Lauf a.d. Pegnitz&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1498&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>69&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Held Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ahorntal&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>70&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Gasthaus & Brauerei Roppelt&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Trossenfurt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1889&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>71&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Pax Braeu e.K.&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Oberelsbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>72&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Gasthof Ott&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Heiligenstadt i. Ofr.&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1678&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>73&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauhaus Doebler&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bad Windsheim&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1867&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>74&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Krug-Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Waischenfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1834&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>75&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Weib's Brauhaus Dinkelsbuehl&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Dinkelsbuehl&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1999&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>76&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Schwanenbraeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ebermannstadt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1812&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>77&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Fuerst Carl Schlossbrauerei Ellingen&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ellingen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1690&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>79&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Steinbach Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Erlangen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1653&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>80&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Lindenbraeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Graefenberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1866&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>81&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Buergerbraeu Hersbruck, Deinlein & Co.&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hersbruck&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1920&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>82&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Hochholzer Brauhaus Poeverlein GbR&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Solnhofen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>2005&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>83&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauhaus Hoechstadt&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hoechstadt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1926&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>84&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Hauff Braeu Lichtnerau GmbH & Co. KG&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Lichtenau&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>85&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Penning-Zeissler&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Pretzfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1623&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>86&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Meister&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Pretzfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1865&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>87&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Nikl&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Pretzfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>2008&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>88&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Held-Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Oberailsfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>89&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Gradl&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Leups&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>90&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Herold&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Buechenbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>91&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Kurzdoerfer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Lindenhardt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>92&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Kurzdoerfer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Lindenhardt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>93&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Uebelhack&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Weiglathal&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>94&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Stoeckel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hintergereuth&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>95&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Heckel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Waischenfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>96&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Schroll&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Nankendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>97&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Aichinger&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Heiligenstadt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>98&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Grauerei Grasser&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Huppendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>99&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Stadter&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Sachsendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>100&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Huebner Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Steinfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>101&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Will&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schederndorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>102&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Sauer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Rossdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>103&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Buettner&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Untergreuth&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>104&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Zehender&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Moenchsambach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>105&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Muehlenbraeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Muehlendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>106&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Sippel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Baunach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>107&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Wagner-Braeu Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Kemmern&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>108&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Fischer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Freudeneck&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>109&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Herrmann&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ampferbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>110&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Foerst&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Druegendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>111&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Schwarzes Kreuz&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Eggolsheim&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>112&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Sternbraeu Elsendorf&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Elsendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>113&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Griess Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Geisfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>114&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Reindler Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Jochsberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>115&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Bayer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Theinheim&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>116&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Max-Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ampferbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>117&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Max-Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ampferbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>118&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Herrmann's Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ampferbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>119&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Haag&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Oberdachstetten&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>120&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Dorn-Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bruckberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>121&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Loewenbraeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Vestenberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>122&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Reuter&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Unternbibert&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>123&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Kundmueller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Weiher&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>124&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Schruefer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Priesendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>125&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Uetzing&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hausbrauerei Reichert&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>126&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Weber&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Roebersdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>127&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Barnikel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Herrnsdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>128&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Mueller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Reundorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>129&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Ott&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Oberleinleiter&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>130&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hoenig&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Tiefenellern&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>131&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hoelzlein&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Lohndorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>132&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Reh&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Lohndorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>133&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hoh&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Koettensdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>134&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Knoblach&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schammelsdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>135&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Nikl-Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Pretzfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>136&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Friedel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schnaid&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>137&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Friedels Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Kreuzberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>138&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Lieberth Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Kreuzberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>139&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Rittmayer Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Kreuzberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>140&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Roppelt Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Stiebarlimbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>141&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Roppelt&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Stiebarlimbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>142&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Lieberth Dorfkeller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hallerndorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>143&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Rittmayer Gartenkeller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hallerndorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>144&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Lieberth&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hallerndorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>145&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Witzgall&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schlammersdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>146&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Witzgall Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schlammersdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>147&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Friedel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Zentbechhofen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>148&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Fischer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Greuth&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>149&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hennemann&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Sambach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>150&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauereikeller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Zum Loewenbraeu&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>151&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Dremel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Wattendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>152&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hetzel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Frauendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>153&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Huebner&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Wattendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>154&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Dinkel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Stublang&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>155&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hennemann&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Stublang&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>157&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Reblitz&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Nedensdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>158&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Trunk&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Vierzehnheiligen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>159&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Keller Brauerei Sauer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Rossdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>160&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Huemmer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Breitenguessbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>161&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Sauer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Gunzendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>162&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Schmausenkeller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Reundorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>163&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Roppelt&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Trossenfurt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>164&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Braeutigam&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Weisbrunn&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>165&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Zenglein&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Oberschleichach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>166&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Seelmann&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Zettmansdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>167&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Wernsdoerfer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schoenbrunn&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>168&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Baer-Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schoenbrunn&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>169&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Beck Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Trabelsdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>170&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Schwan&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Burgebrach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>171&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Schwanenkeller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Burgebrach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>172&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Fischer's Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Zentbechhofen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>173&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Schwarzer Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Weigelshofen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>174&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hummel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Merkendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>175&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Wagner&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Merkendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>176&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Goldener Adler&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hoefen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>177&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Sonnenbraeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Muersbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>178&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Eller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bikach a. Forst&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>179&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Elch Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Thuisbrunn&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>180&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Zwanzger&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Uehlfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>181&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Prechtel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Uehlfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>182&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Senftenberger Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Senftenberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>183&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Wagner&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Kemmern&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>184&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Leicht&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Pferdsfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>185&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hellmuth&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Wiesen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>186&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Thomann&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Wiesen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>187&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Gasthaus Schwan&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ebensfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>188&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Engelhardt (Schwanenbraeu)&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ebensfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>189&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei zur Sonne&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bischberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>190&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Mainlust&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Viereth&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>191&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Thein&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Lembach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>192&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Foessel-Mazour&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Appendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>193&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Schroll&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Reckendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>194&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Mueller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Debring&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>195&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Drummer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Leutenbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>196&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Alt&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Dietzhof&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>197&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Schleicher&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Kaltenbrunn&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>198&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>ehem. Brauerei Gick&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Zilgendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>199&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Hartmann-Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Wuergau&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>200&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Wettelsheimer Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Wettelsheim&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>201&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Voggendorfer Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Voggendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>202&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Schmitt Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schesslitz&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>203&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Barth-Senger&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schesslitz&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>204&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Drei Kronen&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schesslitz&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>205&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hofmann&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hohenschwaerz&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>206&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Friedmann Braeustueberl&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Graefenberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>207&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Klosterbrauerei&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Weissenohe&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>208&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Puerner Etzelwang&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Penzenhof&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>209&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Sternbraeu-Scheubel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schluesselfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>210&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Sternbraeu-Scheubel-Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schluesselfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>211&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Schwarzer Adler&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schluesselfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>212&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Geyer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Oberreichenbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>213&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Geyer Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Oberreichenbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>214&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Adler-Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Stettfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>215&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hoehn&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Memmelsdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>216&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Drei Kronen&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Strassgiech&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>217&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Goeller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Drosendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>218&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Scharpf&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Heilgersdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>219&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Gasthof Reinwand&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Sesslach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>220&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Fuchsbeck&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Sulzbach-Rosenberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>221&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Sperber-Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Sulzbach-Rosenberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>222&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Stirnweiss&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Herreth&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>223&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Haberstumpf&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Trebgast&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>224&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Braeuwerck&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Neudrossenfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>225&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Auf der Theta&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hochtheta&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>226&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Adler Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>End&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1791&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>"],{"maxWidth":800,"minWidth":50,"autoPan":true,"keepInView":false,"closeButton":true,"closeOnClick":true,"className":""},["Brauerei Rittmayer","Brauhaus Leikeim","Ammerndorfer Bier Dorn-Braeu H. Murmann GmbH &amp; Co. KG","Wittelsbacher Turm Braeu GmbH","Arnsteiner Brauerei","Aufsesser Brauerei","Brauhaus Doebler","Ambraeusianum GmbH","Brauerei Faessla GmbH &amp; Co. KG","Kaiserdom Specialitaeten Brauerei GmbH Bamberg","Klosterbraeu Bamberg","Brauerei Schlenkerla","Brauerei Spezial","Brauerei Gundel GmbH","Becher Braeu","Huemmer Braeu","Brauhaus Binkert GmbH &amp; Co. KG","Krug-Braeu","Brauerei-Gasthof Herold","Privatbrauerei Guenther","Loewenbraeu Buttenheim","St. GeorgenBraeu","Brauerei Alt Dietzhof","Brauerei Hauf KG","Weib's Brauhaus Dinkelsbuehl","Brauerei Eichhorn","Ebensfelder Brauhaus","Schwanenbraeu","Schwanen-Braeu Ebing","Fuerst Carl Schlossbrauerei Ellingen","Brauerei Enzensteiner","Kitzmann-Braeu GmbH &amp; Co. Kg","Steinbach Braeu","Eschenbacher Privatbrauerei GmbH","Schlossbrauerei Stelzer","Brauerei Greif","Brauerei Hebendanz GmbH","Waldschlossbrauerei Frammersbach","Tucher Braeu GmbH &amp; Co. KG","Brauerei Griess","Brauerei Krug","Hausbrauerei Duell","Brauerei Friedmann","Lindenbraeu","Brauerei Kaiser","Brauerei-Gasthof Sauer","Brauerei Windsheimer GmbH","Brauerei Martin","Scharpf Heilgersdorf","Red Castle Brew","Buergerbraeu Hersbruck, Deinlein &amp; Co.","Privatbrauerei Stoeckel","Brauerei Kraus","Hochholzer Brauhaus Poeverlein GbR","Brauhaus Hoechstadt","Brauerei und Gasthof Reichold GmbH","Scherdel Bier GmbH &amp; Co. KG","Brauerei Hofmann/Nentwig GbR","Homburger Brauscheuere","Huppendorfer Bier","Brauerei Huetten","Leinburger Bier","Brauerei Gasthof Drummer","Hauff Braeu Lichtnerau GmbH &amp; Co. KG","Brauerei &amp; Gastwirtschaft Kuerzdoerfer","Staffelberg-Braeu GmbH &amp; Co. KG","Brauerei Wagner GmbH","Brauerei Wiethaler","Held Braeu","Gasthaus &amp; Brauerei Roppelt","Pax Braeu e.K.","Brauerei Gasthof Ott","Brauhaus Doebler","Krug-Braeu","Weib's Brauhaus Dinkelsbuehl","Schwanenbraeu","Fuerst Carl Schlossbrauerei Ellingen","Steinbach Braeu","Lindenbraeu","Buergerbraeu Hersbruck, Deinlein &amp; Co.","Hochholzer Brauhaus Poeverlein GbR","Brauhaus Hoechstadt","Hauff Braeu Lichtnerau GmbH &amp; Co. KG","Brauerei Penning-Zeissler","Brauerei Meister","Brauerei Nikl","Held-Braeu","Brauerei Gradl","Brauerei Herold","Brauerei Kurzdoerfer","Brauerei Kurzdoerfer","Brauerei Uebelhack","Brauerei Stoeckel","Brauerei Heckel","Brauerei Schroll","Brauerei Aichinger","Grauerei Grasser","Brauerei Stadter","Huebner Braeu","Brauerei Will","Brauerei Sauer","Brauerei Buettner","Brauerei Zehender","Muehlenbraeu","Brauerei Sippel","Wagner-Braeu Keller","Brauerei Fischer","Brauerei Herrmann","Brauerei Foerst","Brauerei Schwarzes Kreuz","Sternbraeu Elsendorf","Griess Keller","Reindler Braeu","Brauerei Bayer","Max-Braeu","Max-Keller","Herrmann's Keller","Brauerei Haag","Dorn-Braeu","Loewenbraeu","Brauerei Reuter","Brauerei Kundmueller","Brauerei Schruefer","Uetzing","Brauerei Weber","Brauerei Barnikel","Brauerei Mueller","Brauerei Ott","Brauerei Hoenig","Brauerei Hoelzlein","Brauerei Reh","Brauerei Hoh","Brauerei Knoblach","Nikl-Braeu","Brauerei Friedel","Friedels Keller","Lieberth Keller","Rittmayer Keller","Roppelt Keller","Brauerei Roppelt","Lieberth Dorfkeller","Rittmayer Gartenkeller","Brauerei Lieberth","Brauerei Witzgall","Witzgall Keller","Brauerei Friedel","Brauerei Fischer","Brauerei Hennemann","Brauereikeller","Brauerei Dremel","Brauerei Hetzel","Brauerei Huebner","Brauerei Dinkel","Brauerei Hennemann","Brauerei Reblitz","Brauerei Trunk","Keller Brauerei Sauer","Brauerei Huemmer","Brauerei Sauer","Schmausenkeller","Brauerei Roppelt","Brauerei Braeutigam","Brauerei Zenglein","Brauerei Seelmann","Brauerei Wernsdoerfer","Baer-Keller","Beck Braeu","Brauerei Schwan","Schwanenkeller","Fischer's Keller","Schwarzer Keller","Brauerei Hummel","Brauerei Wagner","Brauerei Goldener Adler","Sonnenbraeu","Brauerei Eller","Elch Braeu","Brauerei Zwanzger","Brauerei Prechtel","Senftenberger Keller","Brauerei Wagner","Brauerei Leicht","Brauerei Hellmuth","Brauerei Thomann","Brauerei Gasthaus Schwan","Brauerei Engelhardt (Schwanenbraeu)","Brauerei zur Sonne","Brauerei Mainlust","Brauerei Thein","Brauerei Foessel-Mazour","Brauerei Schroll","Brauerei Mueller","Brauerei Drummer","Brauerei Alt","Brauerei Schleicher","ehem. Brauerei Gick","Hartmann-Braeu","Wettelsheimer Keller","Voggendorfer Keller","Schmitt Braeu","Brauerei Barth-Senger","Brauerei Drei Kronen","Brauerei Hofmann","Brauerei Friedmann Braeustueberl","Klosterbrauerei","Brauerei Puerner Etzelwang","Sternbraeu-Scheubel","Sternbraeu-Scheubel-Keller","Brauerei Schwarzer Adler","Brauerei Geyer","Brauerei Geyer Keller","Adler-Braeu","Brauerei Hoehn","Brauerei Drei Kronen","Brauerei Goeller","Brauerei Scharpf","Gasthof Reinwand","Fuchsbeck","Sperber-Braeu","Brauerei Stirnweiss","Brauerei Haberstumpf","Braeuwerck","Auf der Theta","Adler Braeu"],{"interactive":false,"permanent":false,"direction":"auto","opacity":1,"offset":[0,0],"textsize":"10px","textOnly":false,"className":"","sticky":true},null]},{"method":"addScaleBar","args":[{"maxWidth":100,"metric":true,"imperial":true,"updateWhenIdle":true,"position":"bottomleft"}]},{"method":"addHomeButton","args":[9.462785,48.900742,11.935392,50.441619,"brewery","Zoom to brewery","<strong> brewery <\/strong>","bottomright"]},{"method":"addLegend","args":[{"colors":["#481864","#481A65","#481C66","#471E67","#471E67","#472069","#46226A","#45246B","#45266C","#44286D","#43296E","#432B70","#422D71","#422D71","#412F72","#403073","#3F3274","#3E3475","#3D3576","#3C3777","#3A3878","#3A3878","#393A7A","#383C7B","#363D7C","#343F7D","#33407E","#31427F","#2F437F","#2D4580","#2D4580","#2B4781","#284882","#254A83","#224B84","#1F4D85","#1B4E86","#175087","#175087","#125187","#0A5388","#015489","#00568A","#00578A","#00598B","#005A8C","#005C8D","#005C8D","#005D8D","#005E8E","#00608E","#00618F","#006390","#006490","#006691","#006691","#006791","#006992","#006A92","#006B93","#006D93","#006E94","#007094","#007194","#007194","#007295","#007495","#007595","#007696","#007896","#007996","#007A97","#007A97","#007C97","#007D97","#007E97","#008097","#008197","#008298","#008498","#008498","#008598","#008698","#008898","#008998","#008A98","#008B98","#008D98","#008E98","#008E98","#008F97","#009097","#009297","#009397","#009497","#009597","#009796","#009796","#009896","#009996","#009A95","#009B95","#009D95","#009E94","#009F94","#00A094","#00A094","#00A193","#00A293","#00A392","#00A592","#00A691","#00A791","#00A890","#00A890","#00A98F","#00AA8F","#00AB8E","#00AC8D","#00AD8D","#00AE8C","#00AF8B","#00B08B","#00B08B","#00B18A","#00B289","#00B388","#00B487","#00B586","#00B686","#00B785","#00B785","#00B884","#00B983","#00BA82","#00BB81","#00BC80","#00BD7F","#00BE7D","#00BE7D","#00BF7C","#00C07B","#00C17A","#00C179","#00C278","#00C377","#00C475","#01C574","#01C574","#16C673","#23C771","#2CC770","#34C86F","#3AC96D","#41CA6C","#46CA6A","#46CA6A","#4CCB69","#51CC68","#56CD66","#5ACE65","#5FCE63","#63CF61","#68D060","#6CD05E","#6CD05E","#70D15D","#74D25B","#78D259","#7CD358","#7FD456","#83D454","#87D553","#87D553","#8AD651","#8ED64F","#92D74D","#95D74C","#98D84A","#9CD848","#9FD946","#A3DA44","#A3DA44","#A6DA43","#A9DB41","#ADDB3F","#B0DC3D","#B3DC3C","#B6DD3A","#BADD38","#BADD38","#BDDD37","#C0DE35","#C3DE34","#C6DF32","#C9DF31","#CCE030","#CFE02F","#D2E02E","#D2E02E","#D5E12D","#D8E12C","#DBE12C"],"labels":["Adler Braeu","Adler-Braeu","Ambraeusianum GmbH","Ammerndorfer Bier Dorn-Braeu H. Murmann GmbH & Co. KG","Arnsteiner Brauerei","Auf der Theta","Aufsesser Brauerei","Baer-Keller","Becher Braeu","Beck Braeu","Braeuwerck","Brauerei & Gastwirtschaft Kuerzdoerfer","Brauerei Aichinger","Brauerei Alt","Brauerei Alt Dietzhof","Brauerei Barnikel","Brauerei Barth-Senger","Brauerei Bayer","Brauerei Braeutigam","Brauerei Buettner","Brauerei Dinkel","Brauerei Drei Kronen","Brauerei Dremel","Brauerei Drummer","Brauerei Eichhorn","Brauerei Eller","Brauerei Engelhardt (Schwanenbraeu)","Brauerei Enzensteiner","Brauerei Faessla GmbH & Co. KG","Brauerei Fischer","Brauerei Foerst","Brauerei Foessel-Mazour","Brauerei Friedel","Brauerei Friedmann","Brauerei Friedmann Braeustueberl","Brauerei Gasthaus Schwan","Brauerei Gasthof Drummer","Brauerei Gasthof Ott","Brauerei Geyer","Brauerei Geyer Keller","Brauerei Goeller","Brauerei Goldener Adler","Brauerei Gradl","Brauerei Greif","Brauerei Griess","Brauerei Gundel GmbH","Brauerei Haag","Brauerei Haberstumpf","Brauerei Hauf KG","Brauerei Hebendanz GmbH","Brauerei Heckel","Brauerei Hellmuth","Brauerei Hennemann","Brauerei Herold","Brauerei Herrmann","Brauerei Hetzel","Brauerei Hoehn","Brauerei Hoelzlein","Brauerei Hoenig","Brauerei Hofmann","Brauerei Hofmann/Nentwig GbR","Brauerei Hoh","Brauerei Huebner","Brauerei Huemmer","Brauerei Huetten","Brauerei Hummel","Brauerei Kaiser","Brauerei Knoblach","Brauerei Kraus","Brauerei Krug","Brauerei Kundmueller","Brauerei Kurzdoerfer","Brauerei Leicht","Brauerei Lieberth","Brauerei Mainlust","Brauerei Martin","Brauerei Meister","Brauerei Mueller","Brauerei Nikl","Brauerei Ott","Brauerei Penning-Zeissler","Brauerei Prechtel","Brauerei Puerner Etzelwang","Brauerei Reblitz","Brauerei Reh","Brauerei Reuter","Brauerei Rittmayer","Brauerei Roppelt","Brauerei Sauer","Brauerei Scharpf","Brauerei Schleicher","Brauerei Schlenkerla","Brauerei Schroll","Brauerei Schruefer","Brauerei Schwan","Brauerei Schwarzer Adler","Brauerei Schwarzes Kreuz","Brauerei Seelmann","Brauerei Sippel","Brauerei Spezial","Brauerei Stadter","Brauerei Stirnweiss","Brauerei Stoeckel","Brauerei Thein","Brauerei Thomann","Brauerei Trunk","Brauerei Uebelhack","Brauerei und Gasthof Reichold GmbH","Brauerei Wagner","Brauerei Wagner GmbH","Brauerei Weber","Brauerei Wernsdoerfer","Brauerei Wiethaler","Brauerei Will","Brauerei Windsheimer GmbH","Brauerei Witzgall","Brauerei Zehender","Brauerei Zenglein","Brauerei zur Sonne","Brauerei Zwanzger","Brauerei-Gasthof Herold","Brauerei-Gasthof Sauer","Brauereikeller","Brauhaus Binkert GmbH & Co. KG","Brauhaus Doebler","Brauhaus Hoechstadt","Brauhaus Leikeim","Buergerbraeu Hersbruck, Deinlein & Co.","Dorn-Braeu","Ebensfelder Brauhaus","ehem. Brauerei Gick","Elch Braeu","Eschenbacher Privatbrauerei GmbH","Fischer's Keller","Friedels Keller","Fuchsbeck","Fuerst Carl Schlossbrauerei Ellingen","Gasthaus & Brauerei Roppelt","Gasthof Reinwand","Grauerei Grasser","Griess Keller","Hartmann-Braeu","Hauff Braeu Lichtnerau GmbH & Co. KG","Hausbrauerei Duell","Held Braeu","Held-Braeu","Herrmann's Keller","Hochholzer Brauhaus Poeverlein GbR","Homburger Brauscheuere","Huebner Braeu","Huemmer Braeu","Huppendorfer Bier","Kaiserdom Specialitaeten Brauerei GmbH Bamberg","Keller Brauerei Sauer","Kitzmann-Braeu GmbH & Co. Kg","Klosterbraeu Bamberg","Klosterbrauerei","Krug-Braeu","Leinburger Bier","Lieberth Dorfkeller","Lieberth Keller","Lindenbraeu","Loewenbraeu","Loewenbraeu Buttenheim","Max-Braeu","Max-Keller","Muehlenbraeu","Nikl-Braeu","Pax Braeu e.K.","Privatbrauerei Guenther","Privatbrauerei Stoeckel","Red Castle Brew","Reindler Braeu","Rittmayer Gartenkeller","Rittmayer Keller","Roppelt Keller","Scharpf Heilgersdorf","Scherdel Bier GmbH & Co. KG","Schlossbrauerei Stelzer","Schmausenkeller","Schmitt Braeu","Schwanen-Braeu Ebing","Schwanenbraeu","Schwanenkeller","Schwarzer Keller","Senftenberger Keller","Sonnenbraeu","Sperber-Braeu","St. GeorgenBraeu","Staffelberg-Braeu GmbH & Co. KG","Steinbach Braeu","Sternbraeu Elsendorf","Sternbraeu-Scheubel","Sternbraeu-Scheubel-Keller","Tucher Braeu GmbH & Co. KG","Uetzing","Voggendorfer Keller","Wagner-Braeu Keller","Waldschlossbrauerei Frammersbach","Weib's Brauhaus Dinkelsbuehl","Wettelsheimer Keller","Wittelsbacher Turm Braeu GmbH","Witzgall Keller"],"na_color":null,"na_label":"NA","opacity":1,"position":"topright","type":"factor","title":"brewery","extra":null,"layerId":null,"className":"info legend","group":"brewery"}]},{"method":"addCircleMarkers","args":[[49.71979,50.125793,49.420804,50.161975,49.977199,49.884051,49.502098,49.892226,49.897336,49.904426,49.889283,49.891858,49.897233,49.274716,49.938394,49.970583,49.963579,49.861905,49.794334,50.136445,49.801844,49.802012,49.701477,49.067436,49.070292,49.93162,50.06583,49.77994,50.008198,49.060542,49.561804,49.595108,49.602554,49.966609,50.243835,49.72581,49.7202,50.075903,49.441712,49.880748,49.881398,49.637445,49.644533,49.645651,49.838404,49.80798,49.615866,50.070044,50.163521,49.534947,49.50683,49.850985,49.81609,48.900742,49.707329,49.884229,50.323171,49.677827,49.792701,49.932447,49.985221,49.450083,49.710838,49.276265,49.82981,50.079029,49.958237,49.554706,49.812436,49.92479,50.441619,49.882777,49.502098,49.861905,49.070292,49.77994,49.060542,49.602554,49.645651,49.50683,48.900742,49.707329,49.276265,49.727998,49.737703,49.755953,49.812489,49.81292,49.794041,49.830486,49.830437,49.844875,49.850922,49.845112,49.864998,49.863762,49.932354,49.914055,49.977985,50.005459,49.86779,49.834755,49.819016,49.868614,49.98679,49.950073,50.035191,49.845585,49.807022,49.770496,49.741394,49.877293,49.311859,49.884663,49.845516,49.841331,49.84127,49.415714,49.358559,49.328201,49.422028,49.912956,49.906281,50.085987,49.800175,49.792423,49.82259,49.882729,49.918533,49.916447,49.916149,49.955219,49.926094,49.756046,49.769699,49.759079,49.759056,49.759369,49.75658,49.75547,49.757275,49.759151,49.759079,49.769176,49.76992,49.760632,49.756065,49.780872,49.681698,50.032757,50.070938,50.03249,50.077583,50.077785,50.116096,50.114841,49.865234,49.969555,49.807861,49.814632,49.924839,49.93961,49.948387,49.866604,49.857391,49.856892,49.894829,49.828178,49.822258,49.75914,49.777538,49.959484,49.958523,50.020378,50.061211,50.180557,49.686993,49.67157,49.67107,49.819889,49.953476,50.079861,50.096077,50.096272,50.066387,50.061771,49.910885,49.923359,49.929886,49.969193,50.019966,49.85778,49.710793,49.701366,50.124729,50.167877,49.979191,48.975365,49.66864,49.977264,49.976788,49.976379,49.67775,49.644543,49.629822,49.524117,49.757332,49.761578,49.756756,49.590153,49.590439,49.970467,49.932583,49.959499,49.940926,50.163834,50.187668,49.505432,49.504299,50.126839,50.072201,50.015476,49.98518,50.0617237],[10.889217,11.238725,10.85194,10.078368,9.97323,11.228988,10.416021,10.884839,10.89281,10.852339,10.887001,10.884856,10.89273,10.928096,11.551041,10.882412,10.882666,11.291932,11.509409,11.24823,11.031657,11.032392,11.163238,10.34418,10.316987,10.859199,10.959201,11.186931,10.905149,10.965571,11.368508,11.009011,11.005049,10.701621,11.935392,11.059662,11.056749,9.462785,10.996247,11.011369,11.014066,10.120459,11.252699,11.248618,10.764381,11.076758,10.630027,10.311011,10.827669,11.157948,11.428338,11.45417,10.986859,11.029479,10.806113,11.267583,11.911507,11.252911,9.623338,11.149891,11.773399,11.308721,11.172792,10.685605,11.53456,11.02381,10.956485,11.22997,11.353455,10.665279,10.11763,11.129541,10.416021,11.291932,10.316987,11.186931,10.965571,11.005049,11.248618,11.428338,11.029479,10.806113,10.685605,11.202701,11.223148,11.175664,11.353571,11.524621,11.510147,11.533767,11.534137,11.509316,11.454213,11.346436,11.337445,11.170451,11.150093,11.235613,11.154755,11.15476,10.997014,10.860221,10.677697,10.812709,10.850785,10.861525,10.879812,10.731019,11.116785,11.057176,10.666598,11.010854,10.390196,10.586872,10.729609,10.733358,10.732983,10.42308,10.697315,10.688195,10.588937,10.751747,10.711439,11.068516,10.924503,10.886357,10.887907,11.129483,11.074138,11.044945,11.050299,11.020145,11.006171,11.175526,10.948509,10.955316,10.954791,10.954485,10.949357,10.947157,10.978947,10.975272,10.982155,11.005458,11.011348,10.894425,10.914654,10.842025,10.882693,11.126538,11.061478,11.126356,11.047853,11.04399,10.965482,11.055395,10.998409,10.885327,11.076536,10.896045,10.664581,10.676866,10.596753,10.65544,10.691162,10.69114,10.724936,10.741153,10.736695,10.900771,11.10287,10.958288,10.956261,10.877286,10.864127,10.995174,11.247886,10.721835,10.723262,11.074085,10.874963,10.990276,10.949882,10.951824,10.958208,10.965541,10.832258,10.778682,10.694327,10.802162,10.827091,10.857496,11.172908,11.163225,10.879002,10.963615,11.090071,10.90459,10.73482,11.036384,11.035488,11.033838,11.253052,11.250622,11.25257,11.585255,10.620051,10.619633,10.617878,10.769536,10.76099,10.721189,10.95277,11.000506,10.971659,10.827574,10.842425,11.737261,11.741035,10.930734,11.545622,11.503718,11.558312,11.0738897],6,null,"village",{"crs":{"crsClass":"L.CRS.EPSG3857","code":null,"proj4def":null,"projectedBounds":null,"options":{}},"pane":"point","stroke":true,"color":"#333333","weight":1,"opacity":[0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9],"fill":true,"fillColor":["#481864","#471E67","#472068","#432A6F","#45256C","#44286D","#422C70","#412E72","#412E72","#412E72","#412E72","#412E72","#412E72","#403173","#3E3375","#3A3979","#3A3979","#AADB41","#363E7C","#31427E","#31427E","#31427E","#009B95","#284882","#284882","#006F94","#1C4E86","#165087","#0E5288","#035489","#35C86E","#005C8D","#005C8D","#00588B","#00B389","#005E8E","#005E8E","#00608E","#006390","#006591","#006591","#00A193","#006791","#006791","#34407D","#006B93","#006D93","#007295","#007495","#007A96","#007D97","#481B65","#008197","#4DCB69","#008498","#44286D","#008898","#006791","#008998","#009297","#AEDB3E","#009796","#009597","#009D95","#009E94","#00A094","#00A492","#00AD8D","#481B65","#8AD551","#00B18A","#007896","#422C70","#AADB41","#284882","#165087","#035489","#005C8D","#006791","#007D97","#4DCB69","#008498","#009D95","#00B983","#00B983","#00B983","#00AF8C","#009A96","#363E7C","#009E94","#009E94","#B7DD3A","#007F97","#AADB41","#00AA8F","#007696","#008B98","#00BF7C","#53CC67","#00C377","#00BF7C","#8ED64F","#00A691","#00A790","#403173","#009097","#00628F","#472068","#214C85","#0E5288","#00568A","#006591","#008D98","#70D15C","#472068","#472068","#472068","#00B08B","#383B7A","#98D84A","#93D74D","#BBDD38","#00BA81","#007194","#00BE7D","#007B97","#00BD7F","#00B488","#7BD358","#00A193","#00A193","#009397","#00C279","#00B983","#2BC770","#009597","#009597","#009597","#60CE63","#60CE63","#006F94","#006F94","#006F94","#06C574","#06C574","#CFE02F","#006992","#00C07A","#DBE12C","#B2DC3C","#00608E","#B2DC3C","#65CF61","#65CF61","#00AC8E","#A1D945","#00BF7C","#3A3979","#006B93","#00BD7F","#8AD551","#BFDE36","#00B785","#D3E02E","#35C86E","#35C86E","#80D456","#34407D","#34407D","#CFE02F","#B2DC3C","#00A492","#00A492","#008698","#00A98F","#3D3576","#76D25A","#8ED64F","#8ED64F","#3EC96D","#009097","#00B884","#CBDF30","#CBDF30","#1C4E86","#1C4E86","#3B3778","#9CD948","#009896","#46236A","#00BC80","#2F4480","#009B95","#2C4681","#008E97","#D7E12D","#CFE02F","#C7DF32","#A5DA43","#00C475","#00C475","#00C475","#008998","#006791","#C3DE34","#00B785","#1DC672","#1DC672","#1DC672","#00B586","#00B586","#5ACD65","#00A392","#65CF61","#254A83","#007495","#46CA6B","#6BD05F","#6BD05F","#007B97","#85D553","#00AC8E","#008398","#005A8C"],"fillOpacity":[0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6]},null,null,["<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>1&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Rittmayer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Adelsdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1422&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>2&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauhaus Leikeim&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Altenkunstadt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1887&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>3&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Ammerndorfer Bier Dorn-Braeu H. Murmann GmbH & Co. KG&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ammerndorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1730&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>4&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Wittelsbacher Turm Braeu GmbH&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bad Kissingen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>5&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Arnsteiner Brauerei&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Arnstein&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1885&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>6&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Aufsesser Brauerei&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Aufsess&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1886&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>7&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauhaus Doebler&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bad Windsheim&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1867&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>8&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Ambraeusianum GmbH&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bamberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>2004&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>9&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Faessla GmbH & Co. KG&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bamberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1694&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>10&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Kaiserdom Specialitaeten Brauerei GmbH Bamberg&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bamberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1718&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>11&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Klosterbraeu Bamberg&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bamberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1533&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>12&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Schlenkerla&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bamberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1405&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>13&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Spezial&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bamberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1536&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>14&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Gundel GmbH&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Barthelmesaurach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1887&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>15&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Becher Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bayreuth&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1781&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>16&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Huemmer Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Breitenguessbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>2011&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>17&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauhaus Binkert GmbH & Co. KG&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Breitenguessbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>2012&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>18&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Krug-Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Waischenfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1834&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>19&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei-Gasthof Herold&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Buechenbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1568&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>20&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Privatbrauerei Guenther&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Burgkunstadt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1840&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>21&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Loewenbraeu Buttenheim&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Buttenheim&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1880&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>22&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>St. GeorgenBraeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Buttenheim&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1624&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>23&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Alt Dietzhof&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Leutenbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1886&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>24&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hauf KG&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Dinkelsbuehl&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1901&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>25&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Weib's Brauhaus Dinkelsbuehl&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Dinkelsbuehl&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1999&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>26&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Eichhorn&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hallstadt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>27&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Ebensfelder Brauhaus&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ebensfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1752&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>28&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Schwanenbraeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ebermannstadt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1812&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>29&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Schwanen-Braeu Ebing&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ebing&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1859&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>30&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Fuerst Carl Schlossbrauerei Ellingen&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ellingen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1690&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>31&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Enzensteiner&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schnaittach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1998&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>32&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Kitzmann-Braeu GmbH & Co. Kg&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Erlangen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1712&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>33&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Steinbach Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Erlangen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1653&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>34&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Eschenbacher Privatbrauerei GmbH&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Eltmann&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1958&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>35&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Schlossbrauerei Stelzer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Oberkotzau&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1353&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>36&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Greif&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Forchheim&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1848&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>37&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hebendanz GmbH&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Forchheim&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1579&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>38&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Waldschlossbrauerei Frammersbach&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Frammersbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1868&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>39&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Tucher Braeu GmbH & Co. KG&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Fuerth&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1672&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>40&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Griess&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Geisfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1872&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>41&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Krug&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Geisfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1834&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>42&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Hausbrauerei Duell&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Marktbreit&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1654&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>43&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Friedmann&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Graefenberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1875&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>44&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Lindenbraeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Graefenberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1866&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>45&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Kaiser&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Burgebrach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1783&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>46&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei-Gasthof Sauer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Gunzendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1612&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>47&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Windsheimer GmbH&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Gutenstetten&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1767&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>48&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Martin&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hausen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>2008&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>49&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Scharpf Heilgersdorf&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Heilgersdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1870&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>50&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Red Castle Brew&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Heroldsberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>2013&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>51&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Buergerbraeu Hersbruck, Deinlein & Co.&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hersbruck&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1920&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>52&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Privatbrauerei Stoeckel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ahorntal&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1866&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>53&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Kraus&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hirschaid&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1664&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>54&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Hochholzer Brauhaus Poeverlein GbR&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Solnhofen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>2005&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>55&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauhaus Hoechstadt&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hoechstadt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1926&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>56&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei und Gasthof Reichold GmbH&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Aufsess&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1906&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>57&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Scherdel Bier GmbH & Co. KG&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hof&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1831&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>58&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hofmann/Nentwig GbR&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Graefenberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1897&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>59&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Homburger Brauscheuere&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Homburg a. M.&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>2007&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>60&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Huppendorfer Bier&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Koenigsfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>61&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Huetten&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Warmensteinach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1887&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>62&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Leinburger Bier&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Leinburg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1617&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>63&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Gasthof Drummer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Lautenbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1738&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>64&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Hauff Braeu Lichtnerau GmbH & Co. KG&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Lichtenau&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>65&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei & Gastwirtschaft Kuerzdoerfer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Lindenhardt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1866&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>66&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Staffelberg-Braeu GmbH & Co. KG&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Loffeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1856&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>67&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Wagner GmbH&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Merkendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1797&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>68&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Wiethaler&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Neunhof bei Lauf a.d. Pegnitz&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1498&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>69&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Held Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ahorntal&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>70&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Gasthaus & Brauerei Roppelt&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Trossenfurt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1889&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>71&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Pax Braeu e.K.&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Oberelsbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>72&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Gasthof Ott&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Heiligenstadt i. Ofr.&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1678&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>73&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauhaus Doebler&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bad Windsheim&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1867&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>74&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Krug-Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Waischenfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1834&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>75&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Weib's Brauhaus Dinkelsbuehl&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Dinkelsbuehl&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1999&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>76&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Schwanenbraeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ebermannstadt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1812&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>77&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Fuerst Carl Schlossbrauerei Ellingen&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ellingen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1690&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>79&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Steinbach Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Erlangen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1653&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>80&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Lindenbraeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Graefenberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1866&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>81&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Buergerbraeu Hersbruck, Deinlein & Co.&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hersbruck&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1920&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>82&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Hochholzer Brauhaus Poeverlein GbR&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Solnhofen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>2005&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>83&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauhaus Hoechstadt&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hoechstadt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1926&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>84&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Hauff Braeu Lichtnerau GmbH & Co. KG&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Lichtenau&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>85&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Penning-Zeissler&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Pretzfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1623&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>86&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Meister&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Pretzfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1865&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>87&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Nikl&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Pretzfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>2008&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>88&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Held-Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Oberailsfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>89&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Gradl&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Leups&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>90&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Herold&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Buechenbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>91&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Kurzdoerfer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Lindenhardt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>92&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Kurzdoerfer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Lindenhardt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>93&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Uebelhack&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Weiglathal&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>94&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Stoeckel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hintergereuth&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>95&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Heckel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Waischenfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>96&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Schroll&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Nankendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>97&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Aichinger&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Heiligenstadt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>98&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Grauerei Grasser&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Huppendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>99&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Stadter&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Sachsendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>100&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Huebner Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Steinfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>101&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Will&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schederndorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>102&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Sauer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Rossdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>103&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Buettner&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Untergreuth&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>104&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Zehender&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Moenchsambach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>105&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Muehlenbraeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Muehlendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>106&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Sippel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Baunach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>107&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Wagner-Braeu Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Kemmern&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>108&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Fischer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Freudeneck&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>109&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Herrmann&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ampferbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>110&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Foerst&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Druegendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>111&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Schwarzes Kreuz&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Eggolsheim&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>112&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Sternbraeu Elsendorf&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Elsendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>113&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Griess Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Geisfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>114&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Reindler Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Jochsberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>115&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Bayer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Theinheim&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>116&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Max-Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ampferbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>117&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Max-Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ampferbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>118&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Herrmann's Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ampferbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>119&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Haag&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Oberdachstetten&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>120&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Dorn-Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bruckberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>121&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Loewenbraeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Vestenberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>122&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Reuter&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Unternbibert&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>123&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Kundmueller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Weiher&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>124&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Schruefer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Priesendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>125&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Uetzing&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hausbrauerei Reichert&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>126&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Weber&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Roebersdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>127&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Barnikel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Herrnsdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>128&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Mueller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Reundorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>129&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Ott&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Oberleinleiter&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>130&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hoenig&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Tiefenellern&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>131&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hoelzlein&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Lohndorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>132&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Reh&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Lohndorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>133&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hoh&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Koettensdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>134&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Knoblach&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schammelsdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>135&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Nikl-Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Pretzfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>136&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Friedel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schnaid&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>137&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Friedels Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Kreuzberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>138&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Lieberth Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Kreuzberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>139&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Rittmayer Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Kreuzberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>140&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Roppelt Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Stiebarlimbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>141&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Roppelt&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Stiebarlimbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>142&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Lieberth Dorfkeller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hallerndorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>143&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Rittmayer Gartenkeller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hallerndorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>144&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Lieberth&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hallerndorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>145&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Witzgall&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schlammersdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>146&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Witzgall Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schlammersdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>147&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Friedel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Zentbechhofen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>148&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Fischer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Greuth&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>149&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hennemann&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Sambach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>150&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauereikeller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Zum Loewenbraeu&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>151&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Dremel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Wattendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>152&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hetzel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Frauendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>153&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Huebner&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Wattendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>154&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Dinkel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Stublang&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>155&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hennemann&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Stublang&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>157&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Reblitz&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Nedensdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>158&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Trunk&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Vierzehnheiligen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>159&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Keller Brauerei Sauer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Rossdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>160&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Huemmer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Breitenguessbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>161&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Sauer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Gunzendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>162&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Schmausenkeller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Reundorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>163&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Roppelt&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Trossenfurt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>164&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Braeutigam&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Weisbrunn&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>165&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Zenglein&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Oberschleichach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>166&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Seelmann&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Zettmansdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>167&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Wernsdoerfer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schoenbrunn&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>168&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Baer-Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schoenbrunn&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>169&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Beck Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Trabelsdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>170&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Schwan&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Burgebrach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>171&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Schwanenkeller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Burgebrach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>172&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Fischer's Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Zentbechhofen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>173&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Schwarzer Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Weigelshofen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>174&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hummel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Merkendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>175&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Wagner&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Merkendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>176&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Goldener Adler&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hoefen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>177&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Sonnenbraeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Muersbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>178&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Eller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bikach a. Forst&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>179&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Elch Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Thuisbrunn&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>180&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Zwanzger&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Uehlfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>181&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Prechtel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Uehlfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>182&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Senftenberger Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Senftenberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>183&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Wagner&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Kemmern&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>184&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Leicht&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Pferdsfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>185&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hellmuth&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Wiesen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>186&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Thomann&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Wiesen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>187&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Gasthaus Schwan&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ebensfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>188&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Engelhardt (Schwanenbraeu)&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ebensfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>189&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei zur Sonne&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bischberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>190&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Mainlust&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Viereth&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>191&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Thein&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Lembach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>192&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Foessel-Mazour&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Appendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>193&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Schroll&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Reckendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>194&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Mueller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Debring&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>195&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Drummer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Leutenbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>196&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Alt&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Dietzhof&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>197&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Schleicher&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Kaltenbrunn&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>198&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>ehem. Brauerei Gick&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Zilgendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>199&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Hartmann-Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Wuergau&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>200&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Wettelsheimer Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Wettelsheim&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>201&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Voggendorfer Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Voggendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>202&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Schmitt Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schesslitz&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>203&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Barth-Senger&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schesslitz&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>204&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Drei Kronen&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schesslitz&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>205&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hofmann&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hohenschwaerz&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>206&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Friedmann Braeustueberl&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Graefenberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>207&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Klosterbrauerei&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Weissenohe&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>208&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Puerner Etzelwang&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Penzenhof&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>209&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Sternbraeu-Scheubel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schluesselfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>210&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Sternbraeu-Scheubel-Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schluesselfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>211&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Schwarzer Adler&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schluesselfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>212&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Geyer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Oberreichenbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>213&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Geyer Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Oberreichenbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>214&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Adler-Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Stettfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>215&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hoehn&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Memmelsdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>216&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Drei Kronen&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Strassgiech&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>217&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Goeller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Drosendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>218&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Scharpf&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Heilgersdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>219&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Gasthof Reinwand&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Sesslach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>220&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Fuchsbeck&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Sulzbach-Rosenberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>221&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Sperber-Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Sulzbach-Rosenberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>222&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Stirnweiss&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Herreth&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>223&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Haberstumpf&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Trebgast&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>224&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Braeuwerck&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Neudrossenfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>225&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Auf der Theta&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hochtheta&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>226&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Adler Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>End&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1791&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>"],{"maxWidth":800,"minWidth":50,"autoPan":true,"keepInView":false,"closeButton":true,"closeOnClick":true,"className":""},["Adelsdorf","Altenkunstadt","Ammerndorf","Bad Kissingen","Arnstein","Aufsess","Bad Windsheim","Bamberg","Bamberg","Bamberg","Bamberg","Bamberg","Bamberg","Barthelmesaurach","Bayreuth","Breitenguessbach","Breitenguessbach","Waischenfeld","Buechenbach","Burgkunstadt","Buttenheim","Buttenheim","Leutenbach","Dinkelsbuehl","Dinkelsbuehl","Hallstadt","Ebensfeld","Ebermannstadt","Ebing","Ellingen","Schnaittach","Erlangen","Erlangen","Eltmann","Oberkotzau","Forchheim","Forchheim","Frammersbach","Fuerth","Geisfeld","Geisfeld","Marktbreit","Graefenberg","Graefenberg","Burgebrach","Gunzendorf","Gutenstetten","Hausen","Heilgersdorf","Heroldsberg","Hersbruck","Ahorntal","Hirschaid","Solnhofen","Hoechstadt","Aufsess","Hof","Graefenberg","Homburg a. M.","Koenigsfeld","Warmensteinach","Leinburg","Lautenbach","Lichtenau","Lindenhardt","Loffeld","Merkendorf","Neunhof bei Lauf a.d. Pegnitz","Ahorntal","Trossenfurt","Oberelsbach","Heiligenstadt i. Ofr.","Bad Windsheim","Waischenfeld","Dinkelsbuehl","Ebermannstadt","Ellingen","Erlangen","Graefenberg","Hersbruck","Solnhofen","Hoechstadt","Lichtenau","Pretzfeld","Pretzfeld","Pretzfeld","Oberailsfeld","Leups","Buechenbach","Lindenhardt","Lindenhardt","Weiglathal","Hintergereuth","Waischenfeld","Nankendorf","Heiligenstadt","Huppendorf","Sachsendorf","Steinfeld","Schederndorf","Rossdorf","Untergreuth","Moenchsambach","Muehlendorf","Baunach","Kemmern","Freudeneck","Ampferbach","Druegendorf","Eggolsheim","Elsendorf","Geisfeld","Jochsberg","Theinheim","Ampferbach","Ampferbach","Ampferbach","Oberdachstetten","Bruckberg","Vestenberg","Unternbibert","Weiher","Priesendorf","Hausbrauerei Reichert","Roebersdorf","Herrnsdorf","Reundorf","Oberleinleiter","Tiefenellern","Lohndorf","Lohndorf","Koettensdorf","Schammelsdorf","Pretzfeld","Schnaid","Kreuzberg","Kreuzberg","Kreuzberg","Stiebarlimbach","Stiebarlimbach","Hallerndorf","Hallerndorf","Hallerndorf","Schlammersdorf","Schlammersdorf","Zentbechhofen","Greuth","Sambach","Zum Loewenbraeu","Wattendorf","Frauendorf","Wattendorf","Stublang","Stublang","Nedensdorf","Vierzehnheiligen","Rossdorf","Breitenguessbach","Gunzendorf","Reundorf","Trossenfurt","Weisbrunn","Oberschleichach","Zettmansdorf","Schoenbrunn","Schoenbrunn","Trabelsdorf","Burgebrach","Burgebrach","Zentbechhofen","Weigelshofen","Merkendorf","Merkendorf","Hoefen","Muersbach","Bikach a. Forst","Thuisbrunn","Uehlfeld","Uehlfeld","Senftenberg","Kemmern","Pferdsfeld","Wiesen","Wiesen","Ebensfeld","Ebensfeld","Bischberg","Viereth","Lembach","Appendorf","Reckendorf","Debring","Leutenbach","Dietzhof","Kaltenbrunn","Zilgendorf","Wuergau","Wettelsheim","Voggendorf","Schesslitz","Schesslitz","Schesslitz","Hohenschwaerz","Graefenberg","Weissenohe","Penzenhof","Schluesselfeld","Schluesselfeld","Schluesselfeld","Oberreichenbach","Oberreichenbach","Stettfeld","Memmelsdorf","Strassgiech","Drosendorf","Heilgersdorf","Sesslach","Sulzbach-Rosenberg","Sulzbach-Rosenberg","Herreth","Trebgast","Neudrossenfeld","Hochtheta","End"],{"interactive":false,"permanent":false,"direction":"auto","opacity":1,"offset":[0,0],"textsize":"10px","textOnly":false,"className":"","sticky":true},null]},{"method":"addHomeButton","args":[9.462785,48.900742,11.935392,50.441619,"village","Zoom to village","<strong> village <\/strong>","bottomright"]},{"method":"addLegend","args":[{"colors":["#481864","#481B65","#471E67","#472068","#472068","#46236A","#45256C","#44286D","#432A6F","#422C70","#412E72","#403173","#403173","#3E3375","#3D3576","#3B3778","#3A3979","#383B7A","#363E7C","#34407D","#31427E","#31427E","#2F4480","#2C4681","#284882","#254A83","#214C85","#1C4E86","#165087","#0E5288","#0E5288","#035489","#00568A","#00588B","#005A8C","#005C8D","#005E8E","#00608E","#00608E","#00628F","#006390","#006591","#006791","#006992","#006B93","#006D93","#006F94","#006F94","#007194","#007295","#007495","#007696","#007896","#007A96","#007B97","#007B97","#007D97","#007F97","#008197","#008398","#008498","#008698","#008898","#008998","#008998","#008B98","#008D98","#008E97","#009097","#009297","#009397","#009597","#009597","#009796","#009896","#009A96","#009B95","#009D95","#009E94","#00A094","#00A193","#00A193","#00A392","#00A492","#00A691","#00A790","#00A98F","#00AA8F","#00AC8E","#00AC8E","#00AD8D","#00AF8C","#00B08B","#00B18A","#00B389","#00B488","#00B586","#00B785","#00B785","#00B884","#00B983","#00BA81","#00BC80","#00BD7F","#00BE7D","#00BF7C","#00BF7C","#00C07A","#00C279","#00C377","#00C475","#06C574","#1DC672","#2BC770","#35C86E","#35C86E","#3EC96D","#46CA6B","#4DCB69","#53CC67","#5ACD65","#60CE63","#65CF61","#65CF61","#6BD05F","#70D15C","#76D25A","#7BD358","#80D456","#85D553","#8AD551","#8ED64F","#8ED64F","#93D74D","#98D84A","#9CD948","#A1D945","#A5DA43","#AADB41","#AEDB3E","#B2DC3C","#B2DC3C","#B7DD3A","#BBDD38","#BFDE36","#C3DE34","#C7DF32","#CBDF30","#CFE02F","#CFE02F","#D3E02E","#D7E12D","#DBE12C"],"labels":["Adelsdorf","Ahorntal","Altenkunstadt","Ammerndorf","Ampferbach","Appendorf","Arnstein","Aufsess","Bad Kissingen","Bad Windsheim","Bamberg","Barthelmesaurach","Baunach","Bayreuth","Bikach a. Forst","Bischberg","Breitenguessbach","Bruckberg","Buechenbach","Burgebrach","Burgkunstadt","Buttenheim","Debring","Dietzhof","Dinkelsbuehl","Drosendorf","Druegendorf","Ebensfeld","Ebermannstadt","Ebing","Eggolsheim","Ellingen","Elsendorf","Eltmann","End","Erlangen","Forchheim","Frammersbach","Frauendorf","Freudeneck","Fuerth","Geisfeld","Graefenberg","Greuth","Gunzendorf","Gutenstetten","Hallerndorf","Hallstadt","Hausbrauerei Reichert","Hausen","Heilgersdorf","Heiligenstadt","Heiligenstadt i. Ofr.","Heroldsberg","Herreth","Herrnsdorf","Hersbruck","Hintergereuth","Hirschaid","Hochtheta","Hoechstadt","Hoefen","Hof","Hohenschwaerz","Homburg a. M.","Huppendorf","Jochsberg","Kaltenbrunn","Kemmern","Koenigsfeld","Koettensdorf","Kreuzberg","Lautenbach","Leinburg","Lembach","Leups","Leutenbach","Lichtenau","Lindenhardt","Loffeld","Lohndorf","Marktbreit","Memmelsdorf","Merkendorf","Moenchsambach","Muehlendorf","Muersbach","Nankendorf","Nedensdorf","Neudrossenfeld","Neunhof bei Lauf a.d. Pegnitz","Oberailsfeld","Oberdachstetten","Oberelsbach","Oberkotzau","Oberleinleiter","Oberreichenbach","Oberschleichach","Penzenhof","Pferdsfeld","Pretzfeld","Priesendorf","Reckendorf","Reundorf","Roebersdorf","Rossdorf","Sachsendorf","Sambach","Schammelsdorf","Schederndorf","Schesslitz","Schlammersdorf","Schluesselfeld","Schnaid","Schnaittach","Schoenbrunn","Senftenberg","Sesslach","Solnhofen","Steinfeld","Stettfeld","Stiebarlimbach","Strassgiech","Stublang","Sulzbach-Rosenberg","Theinheim","Thuisbrunn","Tiefenellern","Trabelsdorf","Trebgast","Trossenfurt","Uehlfeld","Untergreuth","Unternbibert","Vestenberg","Viereth","Vierzehnheiligen","Voggendorf","Waischenfeld","Warmensteinach","Wattendorf","Weigelshofen","Weiglathal","Weiher","Weisbrunn","Weissenohe","Wettelsheim","Wiesen","Wuergau","Zentbechhofen","Zettmansdorf","Zilgendorf","Zum Loewenbraeu"],"na_color":null,"na_label":"NA","opacity":1,"position":"topright","type":"factor","title":"village","extra":null,"layerId":null,"className":"info legend","group":"village"}]},{"method":"addCircleMarkers","args":[[49.71979,50.125793,49.420804,50.161975,49.977199,49.884051,49.502098,49.892226,49.897336,49.904426,49.889283,49.891858,49.897233,49.274716,49.938394,49.970583,49.963579,49.861905,49.794334,50.136445,49.801844,49.802012,49.701477,49.067436,49.070292,49.93162,50.06583,49.77994,50.008198,49.060542,49.561804,49.595108,49.602554,49.966609,50.243835,49.72581,49.7202,50.075903,49.441712,49.880748,49.881398,49.637445,49.644533,49.645651,49.838404,49.80798,49.615866,50.070044,50.163521,49.534947,49.50683,49.850985,49.81609,48.900742,49.707329,49.884229,50.323171,49.677827,49.792701,49.932447,49.985221,49.450083,49.710838,49.276265,49.82981,50.079029,49.958237,49.554706,49.812436,49.92479,50.441619,49.882777,49.502098,49.861905,49.070292,49.77994,49.060542,49.602554,49.645651,49.50683,48.900742,49.707329,49.276265,49.727998,49.737703,49.755953,49.812489,49.81292,49.794041,49.830486,49.830437,49.844875,49.850922,49.845112,49.864998,49.863762,49.932354,49.914055,49.977985,50.005459,49.86779,49.834755,49.819016,49.868614,49.98679,49.950073,50.035191,49.845585,49.807022,49.770496,49.741394,49.877293,49.311859,49.884663,49.845516,49.841331,49.84127,49.415714,49.358559,49.328201,49.422028,49.912956,49.906281,50.085987,49.800175,49.792423,49.82259,49.882729,49.918533,49.916447,49.916149,49.955219,49.926094,49.756046,49.769699,49.759079,49.759056,49.759369,49.75658,49.75547,49.757275,49.759151,49.759079,49.769176,49.76992,49.760632,49.756065,49.780872,49.681698,50.032757,50.070938,50.03249,50.077583,50.077785,50.116096,50.114841,49.865234,49.969555,49.807861,49.814632,49.924839,49.93961,49.948387,49.866604,49.857391,49.856892,49.894829,49.828178,49.822258,49.75914,49.777538,49.959484,49.958523,50.020378,50.061211,50.180557,49.686993,49.67157,49.67107,49.819889,49.953476,50.079861,50.096077,50.096272,50.066387,50.061771,49.910885,49.923359,49.929886,49.969193,50.019966,49.85778,49.710793,49.701366,50.124729,50.167877,49.979191,48.975365,49.66864,49.977264,49.976788,49.976379,49.67775,49.644543,49.629822,49.524117,49.757332,49.761578,49.756756,49.590153,49.590439,49.970467,49.932583,49.959499,49.940926,50.163834,50.187668,49.505432,49.504299,50.126839,50.072201,50.015476,49.98518,50.0617237],[10.889217,11.238725,10.85194,10.078368,9.97323,11.228988,10.416021,10.884839,10.89281,10.852339,10.887001,10.884856,10.89273,10.928096,11.551041,10.882412,10.882666,11.291932,11.509409,11.24823,11.031657,11.032392,11.163238,10.34418,10.316987,10.859199,10.959201,11.186931,10.905149,10.965571,11.368508,11.009011,11.005049,10.701621,11.935392,11.059662,11.056749,9.462785,10.996247,11.011369,11.014066,10.120459,11.252699,11.248618,10.764381,11.076758,10.630027,10.311011,10.827669,11.157948,11.428338,11.45417,10.986859,11.029479,10.806113,11.267583,11.911507,11.252911,9.623338,11.149891,11.773399,11.308721,11.172792,10.685605,11.53456,11.02381,10.956485,11.22997,11.353455,10.665279,10.11763,11.129541,10.416021,11.291932,10.316987,11.186931,10.965571,11.005049,11.248618,11.428338,11.029479,10.806113,10.685605,11.202701,11.223148,11.175664,11.353571,11.524621,11.510147,11.533767,11.534137,11.509316,11.454213,11.346436,11.337445,11.170451,11.150093,11.235613,11.154755,11.15476,10.997014,10.860221,10.677697,10.812709,10.850785,10.861525,10.879812,10.731019,11.116785,11.057176,10.666598,11.010854,10.390196,10.586872,10.729609,10.733358,10.732983,10.42308,10.697315,10.688195,10.588937,10.751747,10.711439,11.068516,10.924503,10.886357,10.887907,11.129483,11.074138,11.044945,11.050299,11.020145,11.006171,11.175526,10.948509,10.955316,10.954791,10.954485,10.949357,10.947157,10.978947,10.975272,10.982155,11.005458,11.011348,10.894425,10.914654,10.842025,10.882693,11.126538,11.061478,11.126356,11.047853,11.04399,10.965482,11.055395,10.998409,10.885327,11.076536,10.896045,10.664581,10.676866,10.596753,10.65544,10.691162,10.69114,10.724936,10.741153,10.736695,10.900771,11.10287,10.958288,10.956261,10.877286,10.864127,10.995174,11.247886,10.721835,10.723262,11.074085,10.874963,10.990276,10.949882,10.951824,10.958208,10.965541,10.832258,10.778682,10.694327,10.802162,10.827091,10.857496,11.172908,11.163225,10.879002,10.963615,11.090071,10.90459,10.73482,11.036384,11.035488,11.033838,11.253052,11.250622,11.25257,11.585255,10.620051,10.619633,10.617878,10.769536,10.76099,10.721189,10.95277,11.000506,10.971659,10.827574,10.842425,11.737261,11.741035,10.930734,11.545622,11.503718,11.558312,11.0738897],6,null,"founded",{"crs":{"crsClass":"L.CRS.EPSG3857","code":null,"proj4def":null,"projectedBounds":null,"options":{}},"pane":"point","stroke":true,"color":"#333333","weight":1,"opacity":[0.9,0.9,0.9,0.6,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.6,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.6,0.9,0.9,0.9,0.6,0.9,0.9,0.9,0.9,0.6,0.9,0.6,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.6,0.9,0.9,0.9,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.9],"fill":true,"fillColor":["#3C3777","#6CD05E","#00A890","#BEBEBE","#6CD05E","#6CD05E","#4FCC68","#D9E12C","#009D95","#00A592","#006791","#3F3174","#006791","#6CD05E","#00B983","#D9E12C","#D9E12C","#00C475","#007495","#25C771","#5FCE63","#008698","#6CD05E","#79D359","#CFE02F","#BEBEBE","#00AF8C","#00BE7D","#3EC96D","#009D95","#CFE02F","#00A592","#009297","#B1DC3D","#491261","#3EC96D","#007996","#4FCC68","#009996","#5FCE63","#00C475","#009297","#5FCE63","#4FCC68","#00B983","#008197","#00B586","#D9E12C","#4FCC68","#E2E22B","#91D74E","#4FCC68","#009697","#D9E12C","#91D74E","#79D359","#00C475","#79D359","#D9E12C","#BEBEBE","#6CD05E","#008698","#00AC8E","#BEBEBE","#4FCC68","#3EC96D","#00BC80","#00598B","#BEBEBE","#6CD05E","#BEBEBE","#009996","#4FCC68","#00C475","#CFE02F","#00BE7D","#009D95","#009297","#4FCC68","#91D74E","#D9E12C","#91D74E","#BEBEBE","#008698","#4FCC68","#D9E12C","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#BEBEBE","#00BC80"],"fillOpacity":[0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6,0.6]},null,null,["<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>1&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Rittmayer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Adelsdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1422&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>2&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauhaus Leikeim&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Altenkunstadt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1887&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>3&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Ammerndorfer Bier Dorn-Braeu H. Murmann GmbH & Co. KG&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ammerndorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1730&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>4&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Wittelsbacher Turm Braeu GmbH&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bad Kissingen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>5&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Arnsteiner Brauerei&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Arnstein&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1885&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>6&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Aufsesser Brauerei&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Aufsess&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1886&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>7&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauhaus Doebler&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bad Windsheim&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1867&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>8&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Ambraeusianum GmbH&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bamberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>2004&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>9&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Faessla GmbH & Co. KG&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bamberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1694&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>10&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Kaiserdom Specialitaeten Brauerei GmbH Bamberg&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bamberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1718&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>11&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Klosterbraeu Bamberg&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bamberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1533&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>12&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Schlenkerla&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bamberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1405&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>13&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Spezial&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bamberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1536&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>14&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Gundel GmbH&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Barthelmesaurach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1887&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>15&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Becher Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bayreuth&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1781&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>16&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Huemmer Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Breitenguessbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>2011&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>17&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauhaus Binkert GmbH & Co. KG&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Breitenguessbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>2012&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>18&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Krug-Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Waischenfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1834&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>19&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei-Gasthof Herold&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Buechenbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1568&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>20&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Privatbrauerei Guenther&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Burgkunstadt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1840&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>21&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Loewenbraeu Buttenheim&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Buttenheim&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1880&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>22&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>St. GeorgenBraeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Buttenheim&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1624&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>23&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Alt Dietzhof&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Leutenbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1886&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>24&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hauf KG&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Dinkelsbuehl&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1901&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>25&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Weib's Brauhaus Dinkelsbuehl&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Dinkelsbuehl&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1999&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>26&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Eichhorn&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hallstadt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>27&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Ebensfelder Brauhaus&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ebensfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1752&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>28&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Schwanenbraeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ebermannstadt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1812&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>29&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Schwanen-Braeu Ebing&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ebing&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1859&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>30&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Fuerst Carl Schlossbrauerei Ellingen&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ellingen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1690&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>31&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Enzensteiner&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schnaittach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1998&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>32&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Kitzmann-Braeu GmbH & Co. Kg&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Erlangen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1712&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>33&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Steinbach Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Erlangen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1653&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>34&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Eschenbacher Privatbrauerei GmbH&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Eltmann&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1958&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>35&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Schlossbrauerei Stelzer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Oberkotzau&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1353&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>36&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Greif&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Forchheim&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1848&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>37&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hebendanz GmbH&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Forchheim&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1579&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>38&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Waldschlossbrauerei Frammersbach&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Frammersbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1868&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>39&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Tucher Braeu GmbH & Co. KG&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Fuerth&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1672&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>40&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Griess&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Geisfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1872&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>41&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Krug&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Geisfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1834&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>42&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Hausbrauerei Duell&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Marktbreit&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1654&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>43&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Friedmann&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Graefenberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1875&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>44&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Lindenbraeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Graefenberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1866&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>45&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Kaiser&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Burgebrach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1783&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>46&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei-Gasthof Sauer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Gunzendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1612&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>47&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Windsheimer GmbH&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Gutenstetten&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1767&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>48&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Martin&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hausen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>2008&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>49&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Scharpf Heilgersdorf&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Heilgersdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1870&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>50&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Red Castle Brew&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Heroldsberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>2013&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>51&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Buergerbraeu Hersbruck, Deinlein & Co.&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hersbruck&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1920&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>52&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Privatbrauerei Stoeckel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ahorntal&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1866&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>53&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Kraus&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hirschaid&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1664&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>54&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Hochholzer Brauhaus Poeverlein GbR&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Solnhofen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>2005&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>55&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauhaus Hoechstadt&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hoechstadt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1926&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>56&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei und Gasthof Reichold GmbH&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Aufsess&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1906&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>57&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Scherdel Bier GmbH & Co. KG&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hof&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1831&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>58&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hofmann/Nentwig GbR&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Graefenberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1897&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>59&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Homburger Brauscheuere&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Homburg a. M.&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>2007&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>60&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Huppendorfer Bier&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Koenigsfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>61&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Huetten&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Warmensteinach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1887&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>62&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Leinburger Bier&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Leinburg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1617&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>63&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Gasthof Drummer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Lautenbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1738&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>64&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Hauff Braeu Lichtnerau GmbH & Co. KG&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Lichtenau&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>65&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei & Gastwirtschaft Kuerzdoerfer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Lindenhardt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1866&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>66&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Staffelberg-Braeu GmbH & Co. KG&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Loffeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1856&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>67&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Wagner GmbH&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Merkendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1797&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>68&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Wiethaler&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Neunhof bei Lauf a.d. Pegnitz&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1498&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>69&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Held Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ahorntal&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>70&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Gasthaus & Brauerei Roppelt&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Trossenfurt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1889&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>71&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Pax Braeu e.K.&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Oberelsbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>72&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Gasthof Ott&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Heiligenstadt i. Ofr.&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1678&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>73&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauhaus Doebler&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bad Windsheim&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1867&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>74&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Krug-Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Waischenfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1834&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>75&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Weib's Brauhaus Dinkelsbuehl&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Dinkelsbuehl&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1999&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>76&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Schwanenbraeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ebermannstadt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1812&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>77&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Fuerst Carl Schlossbrauerei Ellingen&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ellingen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1690&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>79&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Steinbach Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Erlangen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1653&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>80&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Lindenbraeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Graefenberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1866&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>81&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Buergerbraeu Hersbruck, Deinlein & Co.&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hersbruck&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1920&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>82&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Hochholzer Brauhaus Poeverlein GbR&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Solnhofen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>2005&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>83&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauhaus Hoechstadt&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hoechstadt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1926&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>84&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Hauff Braeu Lichtnerau GmbH & Co. KG&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Lichtenau&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>85&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Penning-Zeissler&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Pretzfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1623&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>86&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Meister&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Pretzfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1865&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>87&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Nikl&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Pretzfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>2008&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>88&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Held-Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Oberailsfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>89&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Gradl&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Leups&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>90&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Herold&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Buechenbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>91&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Kurzdoerfer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Lindenhardt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>92&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Kurzdoerfer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Lindenhardt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>93&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Uebelhack&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Weiglathal&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>94&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Stoeckel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hintergereuth&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>95&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Heckel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Waischenfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>96&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Schroll&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Nankendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>97&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Aichinger&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Heiligenstadt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>98&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Grauerei Grasser&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Huppendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>99&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Stadter&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Sachsendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>100&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Huebner Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Steinfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>101&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Will&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schederndorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>102&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Sauer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Rossdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>103&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Buettner&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Untergreuth&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>104&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Zehender&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Moenchsambach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>105&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Muehlenbraeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Muehlendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>106&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Sippel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Baunach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>107&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Wagner-Braeu Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Kemmern&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>108&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Fischer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Freudeneck&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>109&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Herrmann&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ampferbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>110&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Foerst&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Druegendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>111&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Schwarzes Kreuz&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Eggolsheim&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>112&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Sternbraeu Elsendorf&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Elsendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>113&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Griess Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Geisfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>114&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Reindler Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Jochsberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>115&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Bayer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Theinheim&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>116&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Max-Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ampferbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>117&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Max-Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ampferbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>118&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Herrmann's Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ampferbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>119&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Haag&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Oberdachstetten&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>120&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Dorn-Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bruckberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>121&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Loewenbraeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Vestenberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>122&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Reuter&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Unternbibert&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>123&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Kundmueller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Weiher&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>124&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Schruefer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Priesendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>125&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Uetzing&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hausbrauerei Reichert&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>126&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Weber&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Roebersdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>127&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Barnikel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Herrnsdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>128&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Mueller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Reundorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>129&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Ott&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Oberleinleiter&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>130&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hoenig&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Tiefenellern&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>131&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hoelzlein&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Lohndorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>132&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Reh&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Lohndorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>133&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hoh&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Koettensdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>134&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Knoblach&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schammelsdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>135&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Nikl-Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Pretzfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>136&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Friedel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schnaid&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>137&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Friedels Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Kreuzberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>138&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Lieberth Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Kreuzberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>139&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Rittmayer Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Kreuzberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>140&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Roppelt Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Stiebarlimbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>141&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Roppelt&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Stiebarlimbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>142&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Lieberth Dorfkeller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hallerndorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>143&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Rittmayer Gartenkeller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hallerndorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>144&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Lieberth&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hallerndorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>145&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Witzgall&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schlammersdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>146&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Witzgall Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schlammersdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>147&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Friedel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Zentbechhofen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>148&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Fischer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Greuth&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>149&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hennemann&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Sambach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>150&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauereikeller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Zum Loewenbraeu&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>151&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Dremel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Wattendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>152&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hetzel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Frauendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>153&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Huebner&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Wattendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>154&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Dinkel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Stublang&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>155&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hennemann&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Stublang&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>157&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Reblitz&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Nedensdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>158&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Trunk&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Vierzehnheiligen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>159&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Keller Brauerei Sauer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Rossdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>160&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Huemmer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Breitenguessbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>161&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Sauer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Gunzendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>162&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Schmausenkeller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Reundorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>163&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Roppelt&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Trossenfurt&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>164&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Braeutigam&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Weisbrunn&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>165&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Zenglein&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Oberschleichach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>166&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Seelmann&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Zettmansdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>167&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Wernsdoerfer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schoenbrunn&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>168&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Baer-Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schoenbrunn&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>169&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Beck Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Trabelsdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>170&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Schwan&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Burgebrach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>171&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Schwanenkeller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Burgebrach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>172&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Fischer's Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Zentbechhofen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>173&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Schwarzer Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Weigelshofen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>174&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hummel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Merkendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>175&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Wagner&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Merkendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>176&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Goldener Adler&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hoefen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>177&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Sonnenbraeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Muersbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>178&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Eller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bikach a. Forst&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>179&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Elch Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Thuisbrunn&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>180&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Zwanzger&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Uehlfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>181&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Prechtel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Uehlfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>182&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Senftenberger Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Senftenberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>183&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Wagner&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Kemmern&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>184&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Leicht&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Pferdsfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>185&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hellmuth&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Wiesen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>186&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Thomann&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Wiesen&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>187&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Gasthaus Schwan&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ebensfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>188&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Engelhardt (Schwanenbraeu)&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Ebensfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>189&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei zur Sonne&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Bischberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>190&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Mainlust&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Viereth&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>191&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Thein&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Lembach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>192&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Foessel-Mazour&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Appendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>193&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Schroll&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Reckendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>194&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Mueller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Debring&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>195&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Drummer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Leutenbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>196&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Alt&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Dietzhof&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>197&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Schleicher&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Kaltenbrunn&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>198&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>ehem. Brauerei Gick&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Zilgendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>199&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Hartmann-Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Wuergau&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>200&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Wettelsheimer Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Wettelsheim&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>201&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Voggendorfer Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Voggendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>202&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Schmitt Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schesslitz&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>203&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Barth-Senger&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schesslitz&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>204&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Drei Kronen&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schesslitz&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>205&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hofmann&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hohenschwaerz&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>206&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Friedmann Braeustueberl&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Graefenberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>207&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Klosterbrauerei&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Weissenohe&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>208&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Puerner Etzelwang&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Penzenhof&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>209&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Sternbraeu-Scheubel&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schluesselfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>210&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Sternbraeu-Scheubel-Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schluesselfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>211&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Schwarzer Adler&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Schluesselfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>212&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Geyer&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Oberreichenbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>213&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Geyer Keller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Oberreichenbach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>214&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Adler-Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Stettfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>215&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Hoehn&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Memmelsdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>216&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Drei Kronen&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Strassgiech&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>217&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Goeller&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Drosendorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>218&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Scharpf&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Heilgersdorf&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>219&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Gasthof Reinwand&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Sesslach&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>220&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Fuchsbeck&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Sulzbach-Rosenberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>221&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Sperber-Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Sulzbach-Rosenberg&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>222&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Stirnweiss&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Herreth&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>223&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Brauerei Haberstumpf&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Trebgast&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>224&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Braeuwerck&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Neudrossenfeld&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>225&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Auf der Theta&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>Hochtheta&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>NA&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>","<html><head><link rel=\"stylesheet\" type=\"text/css\" href=\"lib/popup/popup.css\"><\/head><body><div class=\"scrollableContainer\"><table class=\"popup scrollable\" id=\"popup\"><tr class='coord'><td><\/td><td><b>Feature ID<\/b><\/td><td align='right'>226&emsp;<\/td><\/tr><tr class='alt'><td>1<\/td><td><b>brewery&emsp;<\/b><\/td><td align='right'>Adler Braeu&emsp;<\/td><\/tr><tr><td>2<\/td><td><b>village&emsp;<\/b><\/td><td align='right'>End&emsp;<\/td><\/tr><tr class='alt'><td>3<\/td><td><b>founded&emsp;<\/b><\/td><td align='right'>1791&emsp;<\/td><\/tr><tr><td>4<\/td><td><b>geometry&emsp;<\/b><\/td><td align='right'>sfc_POINT&emsp;<\/td><\/tr><\/table><\/div><\/body><\/html>"],{"maxWidth":800,"minWidth":50,"autoPan":true,"keepInView":false,"closeButton":true,"closeOnClick":true,"className":""},["1422","1887","1730",null,"1885","1886","1867","2004","1694","1718","1533","1405","1536","1887","1781","2011","2012","1834","1568","1840","1880","1624","1886","1901","1999",null,"1752","1812","1859","1690","1998","1712","1653","1958","1353","1848","1579","1868","1672","1872","1834","1654","1875","1866","1783","1612","1767","2008","1870","2013","1920","1866","1664","2005","1926","1906","1831","1897","2007",null,"1887","1617","1738",null,"1866","1856","1797","1498",null,"1889",null,"1678","1867","1834","1999","1812","1690","1653","1866","1920","2005","1926",null,"1623","1865","2008",null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,null,"1791"],{"interactive":false,"permanent":false,"direction":"auto","opacity":1,"offset":[0,0],"textsize":"10px","textOnly":false,"className":"","sticky":true},null]},{"method":"addHomeButton","args":[9.462785,48.900742,11.935392,50.441619,"founded","Zoom to founded","<strong> founded <\/strong>","bottomright"]},{"method":"addLayersControl","args":[["CartoDB.Positron","CartoDB.DarkMatter","OpenStreetMap","Esri.WorldImagery","OpenTopoMap"],["brewery","village","founded"],{"collapsed":true,"autoZIndex":true,"position":"topleft"}]},{"method":"addLegend","args":[{"colors":["#4B0055 , #481D67 7.12121212121212%, #185086 22.2727272727273%, #007B97 37.4242424242424%, #00A193 52.5757575757576%, #00C07A 67.7272727272727%, #93D74D 82.8787878787879%, #F3E32E 98.030303030303%, #FDE333 "],"labels":["1,400","1,500","1,600","1,700","1,800","1,900","2,000"],"na_color":"#BEBEBE","na_label":"NA","opacity":1,"position":"topright","type":"numeric","title":"founded","extra":{"p_1":0.0712121212121212,"p_n":0.98030303030303},"layerId":null,"className":"info legend","group":"founded"}]},{"method":"addHomeButton","args":[9.462785,48.900742,11.935392,50.441619,null,"Zoom to full extent","<strong>Zoom full<\/strong>","bottomleft"]}],"limits":{"lat":[48.900742,50.441619],"lng":[9.462785,11.935392]},"fitBounds":[48.900742,9.462785,50.441619,11.935392,[]]},"evals":[],"jsHooks":{"render":[{"code":"function(el, x, data) {\n  return (\n      function(el, x, data) {\n      // get the leaflet map\n      var map = this; //HTMLWidgets.find('#' + el.id);\n      // we need a new div element because we have to handle\n      // the mouseover output separately\n      // debugger;\n      function addElement () {\n      // generate new div Element\n      var newDiv = $(document.createElement('div'));\n      // append at end of leaflet htmlwidget container\n      $(el).append(newDiv);\n      //provide ID and style\n      newDiv.addClass('lnlt');\n      newDiv.css({\n      'position': 'relative',\n      'bottomleft':  '0px',\n      'background-color': 'rgba(255, 255, 255, 0.7)',\n      'box-shadow': '0 0 2px #bbb',\n      'background-clip': 'padding-box',\n      'margin': '0',\n      'padding-left': '5px',\n      'color': '#333',\n      'font': '9px/1.5 \"Helvetica Neue\", Arial, Helvetica, sans-serif',\n      'z-index': '700',\n      });\n      return newDiv;\n      }\n\n\n      // check for already existing lnlt class to not duplicate\n      var lnlt = $(el).find('.lnlt');\n\n      if(!lnlt.length) {\n      lnlt = addElement();\n\n      // grab the special div we generated in the beginning\n      // and put the mousmove output there\n\n      map.on('mousemove', function (e) {\n      if (e.originalEvent.ctrlKey) {\n      if (document.querySelector('.lnlt') === null) lnlt = addElement();\n      lnlt.text(\n                           ' lon: ' + (e.latlng.lng).toFixed(5) +\n                           ' | lat: ' + (e.latlng.lat).toFixed(5) +\n                           ' | zoom: ' + map.getZoom() +\n                           ' | x: ' + L.CRS.EPSG3857.project(e.latlng).x.toFixed(0) +\n                           ' | y: ' + L.CRS.EPSG3857.project(e.latlng).y.toFixed(0) +\n                           ' | epsg: 3857 ' +\n                           ' | proj4: +proj=merc +a=6378137 +b=6378137 +lat_ts=0.0 +lon_0=0.0 +x_0=0.0 +y_0=0 +k=1.0 +units=m +nadgrids=@null +no_defs ');\n      } else {\n      if (document.querySelector('.lnlt') === null) lnlt = addElement();\n      lnlt.text(\n                      ' lon: ' + (e.latlng.lng).toFixed(5) +\n                      ' | lat: ' + (e.latlng.lat).toFixed(5) +\n                      ' | zoom: ' + map.getZoom() + ' ');\n      }\n      });\n\n      // remove the lnlt div when mouse leaves map\n      map.on('mouseout', function (e) {\n      var strip = document.querySelector('.lnlt');\n      strip.remove();\n      });\n\n      };\n\n      //$(el).keypress(67, function(e) {\n      map.on('preclick', function(e) {\n      if (e.originalEvent.ctrlKey) {\n      if (document.querySelector('.lnlt') === null) lnlt = addElement();\n      lnlt.text(\n                      ' lon: ' + (e.latlng.lng).toFixed(5) +\n                      ' | lat: ' + (e.latlng.lat).toFixed(5) +\n                      ' | zoom: ' + map.getZoom() + ' ');\n      var txt = document.querySelector('.lnlt').textContent;\n      console.log(txt);\n      //txt.innerText.focus();\n      //txt.select();\n      setClipboardText('\"' + txt + '\"');\n      }\n      });\n\n      }\n      ).call(this.getMap(), el, x, data);\n}","data":null}]}}</script><!--/html_preserve-->

## Projection with a different Coordinate Reference Systems 

You often need to reproject an `sf` using a different coordinate reference system (CRS) because you need it to have the same CRS as an `sf` object that you are interacting it with (spatial join) or mapping it with. In order to check the current CRS for an `sf` object, you can use the `st_crs()` function. 


```r
st_crs(nc)
```

```
Coordinate Reference System:
  User input: 4267 
  wkt:
GEOGCS["NAD27",
    DATUM["North_American_Datum_1927",
        SPHEROID["Clarke 1866",6378206.4,294.9786982138982,
            AUTHORITY["EPSG","7008"]],
        AUTHORITY["EPSG","6267"]],
    PRIMEM["Greenwich",0,
        AUTHORITY["EPSG","8901"]],
    UNIT["degree",0.0174532925199433,
        AUTHORITY["EPSG","9122"]],
    AUTHORITY["EPSG","4267"]]
```

`wkt` stands for **Well Known Text**^[`sf` versions prior to 0.9 provides CRS information in the form of `proj4string`. The newer version of `sf` presents CRS in the form of `wtk` (see [this slide](https://nowosad.github.io/whyr_webinar004/#25)). You can find the reason behind this change in the same slide, starting from [here](https://nowosad.github.io/whyr_webinar004/#18).], which is one of many many formats to store CRS information.^[See [here](https://spatialreference.org/ref/epsg/nad27/) for numerous other formats that represent the same CRS.] 4267 is the SRID (Spatial Reference System Identifier) defined by the European Petroleum Survey Group (EPSG) for the CRS^[You can find the CRS-EPSG number correspondence [here](http://spatialreference.org/ref/epsg/).]. 

When you transform your `sf` using a different CRS, you can use its EPSG number if the CRS has an EPSG number.^[Potential pool of CRS is infinite. Only the commonly-used CRS have been assigned EPSG SRID.] Let's transform the `sf` to `WGS 84` (another commonly used GCS), whose EPSG number is 4326. We can use the `st_transform()` function to achieve that, with the first argument being the `sf` object you are transforming and the second being the EPSG number of the new CRS.


```r
#--- transform ---#
nc_wgs84 <- st_transform(nc, 4326)

#--- check if the transformation was successful ---#
st_crs(nc_wgs84)
```

```
Coordinate Reference System:
  User input: EPSG:4326 
  wkt:
GEOGCS["WGS 84",
    DATUM["WGS_1984",
        SPHEROID["WGS 84",6378137,298.257223563,
            AUTHORITY["EPSG","7030"]],
        AUTHORITY["EPSG","6326"]],
    PRIMEM["Greenwich",0,
        AUTHORITY["EPSG","8901"]],
    UNIT["degree",0.0174532925199433,
        AUTHORITY["EPSG","9122"]],
    AUTHORITY["EPSG","4326"]]
```

Notice that `wkt` was also altered accordingly to reflect the change in CRS: datum was changed to WGS 84. Now, let's transform (reproject) the data using `NAD83 / UTM zone 17N` CRS. Its EPSG number is $26917$.^[See [here](http://spatialreference.org/ref/epsg/nad83-utm-zone-14n/).] So, the following code does the job.


```r
#--- transform ---#
nc_utm17N <- st_transform(nc_wgs84, 26917)

#--- check if the transformation was successful ---#
st_crs(nc_utm17N)
```

```
Coordinate Reference System:
  User input: EPSG:26917 
  wkt:
PROJCS["NAD83 / UTM zone 17N",
    GEOGCS["NAD83",
        DATUM["North_American_Datum_1983",
            SPHEROID["GRS 1980",6378137,298.257222101,
                AUTHORITY["EPSG","7019"]],
            TOWGS84[0,0,0,0,0,0,0],
            AUTHORITY["EPSG","6269"]],
        PRIMEM["Greenwich",0,
            AUTHORITY["EPSG","8901"]],
        UNIT["degree",0.0174532925199433,
            AUTHORITY["EPSG","9122"]],
        AUTHORITY["EPSG","4269"]],
    PROJECTION["Transverse_Mercator"],
    PARAMETER["latitude_of_origin",0],
    PARAMETER["central_meridian",-81],
    PARAMETER["scale_factor",0.9996],
    PARAMETER["false_easting",500000],
    PARAMETER["false_northing",0],
    UNIT["metre",1,
        AUTHORITY["EPSG","9001"]],
    AXIS["Easting",EAST],
    AXIS["Northing",NORTH],
    AUTHORITY["EPSG","26917"]]
```

As you can see in its CRS information, the projection system is now UTM zone 17N. 

You often need to change the CRS of an `sf` object when you interact (e.g., spatial subsetting, joining, etc) it with another `sf` object. In such a case, you can extract the CRS of the other `sf` object using `st_crs()` and use it for transformation.^[In this example, we are using the same data with two different CRS. But, you get the point.] So, you do not need to find the EPSG of the CRS of the `sf` object you are interacting it with.


```r
#--- transform ---#
nc_utm17N_2 <- st_transform(nc_wgs84, st_crs(nc_utm17N))

#--- check if the transformation was successful ---#
st_crs(nc_utm17N_2)
```

```
Coordinate Reference System:
  User input: EPSG:26917 
  wkt:
PROJCS["NAD83 / UTM zone 17N",
    GEOGCS["NAD83",
        DATUM["North_American_Datum_1983",
            SPHEROID["GRS 1980",6378137,298.257222101,
                AUTHORITY["EPSG","7019"]],
            TOWGS84[0,0,0,0,0,0,0],
            AUTHORITY["EPSG","6269"]],
        PRIMEM["Greenwich",0,
            AUTHORITY["EPSG","8901"]],
        UNIT["degree",0.0174532925199433,
            AUTHORITY["EPSG","9122"]],
        AUTHORITY["EPSG","4269"]],
    PROJECTION["Transverse_Mercator"],
    PARAMETER["latitude_of_origin",0],
    PARAMETER["central_meridian",-81],
    PARAMETER["scale_factor",0.9996],
    PARAMETER["false_easting",500000],
    PARAMETER["false_northing",0],
    UNIT["metre",1,
        AUTHORITY["EPSG","9001"]],
    AXIS["Easting",EAST],
    AXIS["Northing",NORTH],
    AUTHORITY["EPSG","26917"]]
```


<!-- However, notice that the `epsg` component of CRS is $NA$. ESRI shapefile format uses WTK (Well-known Text) format to refer to the CRS in use, which is saved in .prj file. So, if there is no corresponding SRID number for the CRS in use, the `epsg` component number would get lost when you save an `sf` object when you save it as an ESRI shapefile. This is exactly what happened to 
 -->

## Turning a data.frame of points into an `sf` 

Often times, you have a dataset with geographic coordinates as variables in a csv or other formats, which would not be recognized as a spatial dataset by R immediately when it is read into R. In this case, you need to identify which variables represent the geographic coordinates from the data set, and create an `sf` yourself. Fortunately, it is easy to do so using the `st_as_sf()` function. Let's first read a dataset (irrigation wells in Nebraska):


```r
#--- read irrigation well registration data ---#
(
wells <- readRDS('./Data/well_registration.rds') 
)
```

```
        wellid ownerid        nrdname acres  regdate section     longdd
     1:      2  106106 Central Platte   160 12/30/55       3  -99.58401
     2:      3   14133   South Platte    46  4/29/31       8 -102.62495
     3:      4   14133   South Platte    46  4/29/31       8 -102.62495
     4:      5   14133   South Platte    46  4/29/31       8 -102.62495
     5:      6   15837 Central Platte   160  8/29/32      20  -99.62580
    ---                                                                
105818: 244568  135045 Upper Big Blue    NA  8/26/16      30  -97.58872
105819: 244569  105428    Little Blue    NA  8/26/16      24  -97.60752
105820: 244570  135045 Upper Big Blue    NA  8/26/16      30  -97.58294
105821: 244571  135045 Upper Big Blue    NA  8/26/16      25  -97.59775
105822: 244572  105428    Little Blue    NA  8/26/16      15  -97.64086
           latdd
     1: 40.69825
     2: 41.11699
     3: 41.11699
     4: 41.11699
     5: 40.73268
    ---         
105818: 40.89017
105819: 40.13257
105820: 40.88722
105821: 40.89639
105822: 40.13380
```

```r
#--- check the class ---#
class(wells)
```

```
[1] "data.table" "data.frame"
```

As you can see the data is not an `sf` object. In this dataset, `longdd` and `latdd` represent longitude and latitude, respectively. We now turn the dataset into an `sf` object:


```r
#--- recognize it as an sf ---#
wells_sf <- st_as_sf(wells, coords = c("longdd","latdd"))

#--- take a look at the data ---#
head(wells_sf[,1:5])
```

```
Simple feature collection with 6 features and 5 fields
geometry type:  POINT
dimension:      XY
bbox:           xmin: -102.6249 ymin: 40.69824 xmax: -99.58401 ymax: 41.11699
CRS:            NA
  wellid ownerid        nrdname acres  regdate                   geometry
1      2  106106 Central Platte   160 12/30/55 POINT (-99.58401 40.69825)
2      3   14133   South Platte    46  4/29/31 POINT (-102.6249 41.11699)
3      4   14133   South Platte    46  4/29/31 POINT (-102.6249 41.11699)
4      5   14133   South Platte    46  4/29/31 POINT (-102.6249 41.11699)
5      6   15837 Central Platte   160  8/29/32  POINT (-99.6258 40.73268)
6      7   90248 Central Platte   120  2/15/35 POINT (-99.64524 40.73164)
```

Note that the CRS of `wells_sf` is NA. Obviously, $R$ does not know the reference system without you telling it. We know^[Yes, YOU need to know the CRS of your data.] that the geographic coordinates in the wells data is NAD 83 ($epsg=4269$) for this dataset. So, we can assign the right CRS using either `st_set_crs()` or `st_crs()`.


```r
#--- set CRS ---#
wells_sf <- st_set_crs(wells_sf, 4269) 

#--- or this ---#
st_crs(wells_sf) <- 4269

#--- see the change ---#
head(wells_sf[,1:5])
```

```
Simple feature collection with 6 features and 5 fields
geometry type:  POINT
dimension:      XY
bbox:           xmin: -102.6249 ymin: 40.69824 xmax: -99.58401 ymax: 41.11699
CRS:            EPSG:4269
  wellid ownerid        nrdname acres  regdate                   geometry
1      2  106106 Central Platte   160 12/30/55 POINT (-99.58401 40.69825)
2      3   14133   South Platte    46  4/29/31 POINT (-102.6249 41.11699)
3      4   14133   South Platte    46  4/29/31 POINT (-102.6249 41.11699)
4      5   14133   South Platte    46  4/29/31 POINT (-102.6249 41.11699)
5      6   15837 Central Platte   160  8/29/32  POINT (-99.6258 40.73268)
6      7   90248 Central Platte   120  2/15/35 POINT (-99.64524 40.73164)
```

## Conversion to and from sp {#conv_sp}

Though unlikely, you may find instances where `sp` objects are necessary or desirable.^[For example, those who run spatial econometric methods using `spdep`, creating neighbors from polygons is a bit faster using `sp` objects than using `sf` objects.] In that case, it is good to know how to convert an `sf` object to an `sp` object, vice versa. You can convert an `sf` object to its `sp` counterpart using `as(sf_object, "Spatial")`:


```r
#--- conversion ---#
wells_sp <- as(wells_sf, "Spatial")

#--- check the class ---#
class(wells_sp)
```

```
[1] "SpatialPointsDataFrame"
attr(,"package")
[1] "sp"
```

<!-- ```{r }
list(a_point, a_polygon) %>% st_sfc() %>% st_as_sf() %>% 
  as("Spatial")
```

 -->
As you can see `wells_sp` is a class of `SpatialPointsDataFrame`, points with a `data.frame` supported by the `sp` package. The above syntax works for converting an `sf` of polygons into `SpatialPolygonsDataFrame` as well^[The function does not work for an `sf` object that consists of different geometry types (e.g., POINT and POLYGON). This is because `sp` objects do not allow different types of geometries in the single `sp` object. For example, `SpatialPointsDataFrame` consists only of points data.].     

You can revert `wells_sp` back to an `sf` object using the `st_as_sf()` function, as follows:


```r
#--- revert back to sf ---#
wells_sf <- st_as_sf(wells_sp)

#--- check the class ---#
class(wells_sf)
```

```
[1] "sf"         "data.frame"
```

We do not cover how to use the `sp` package as the benefit of learning it has become marginal compared to when `sf` was just introduced a few years back^[For those interested in learning the `sp` package, [this website](https://rspatial.org/) is a good resource.]. 

## Non-spatial transformation of sf 

An important feature of an `sf` object is that it is basically a `data.frame` with geometric information stored as a variable (column). This means that transforming an `sf` object works just like transforming a `data.frame`. Basically, everything you can do to a `data.frame`, you can do to an `sf` as well. The code below just provides an example of basic operations including `dplyr::select()`, `dplyr::filter()`, and `dplyr::mutate()` in action with an `sf` object to just confirm that `dplyr`  operations works with an `sf` object just like a `data.frame`.   


```r
#--- here is what the data looks like ---#
dplyr::select(wells_sf, wellid, nrdname, acres, regdate, nrdname)
```

```
Simple feature collection with 105822 features and 4 fields
geometry type:  POINT
dimension:      XY
bbox:           xmin: -104.0531 ymin: 40.00161 xmax: -96.87681 ymax: 41.85942
CRS:            unknown
First 10 features:
   wellid           nrdname acres  regdate                   geometry
1       2    Central Platte   160 12/30/55 POINT (-99.58401 40.69825)
2       3      South Platte    46  4/29/31 POINT (-102.6249 41.11699)
3       4      South Platte    46  4/29/31 POINT (-102.6249 41.11699)
4       5      South Platte    46  4/29/31 POINT (-102.6249 41.11699)
5       6    Central Platte   160  8/29/32  POINT (-99.6258 40.73268)
6       7    Central Platte   120  2/15/35 POINT (-99.64524 40.73164)
7       8      South Platte   113   8/7/37 POINT (-103.5257 41.24492)
8      10      South Platte   160   5/4/38 POINT (-103.0284 41.13243)
9      11 Middle Republican   807   5/6/38  POINT (-101.1193 40.3527)
10     12 Middle Republican   148 11/29/77 POINT (-101.1146 40.35631)
```

Notice that `geometry` column will be retained after `dplyr::select()` even if you did not tell R to keep it above.

Let's apply `dplyr::select()`, `dplyr::filter()`, and `dplyr::mutate()` to the dataset.


```r
#--- do some transformations ---#
wells_sf %>% 
  #--- select variables (geometry will always remain after select) ---#
  dplyr::select(wellid, nrdname, acres, regdate, nrdname) %>% 
  #--- removes observations with acre < 30  ---#
  dplyr::filter(acres > 30) %>% 
  #--- hectare instead of acre ---#
  dplyr::mutate(hectare = acres * 0.404686) 
```

```
Simple feature collection with 63271 features and 5 fields
geometry type:  POINT
dimension:      XY
bbox:           xmin: -104.0529 ymin: 40.00161 xmax: -96.87681 ymax: 41.73599
CRS:            unknown
First 10 features:
   wellid           nrdname acres  regdate                   geometry   hectare
1       2    Central Platte   160 12/30/55 POINT (-99.58401 40.69825)  64.74976
2       3      South Platte    46  4/29/31 POINT (-102.6249 41.11699)  18.61556
3       4      South Platte    46  4/29/31 POINT (-102.6249 41.11699)  18.61556
4       5      South Platte    46  4/29/31 POINT (-102.6249 41.11699)  18.61556
5       6    Central Platte   160  8/29/32  POINT (-99.6258 40.73268)  64.74976
6       7    Central Platte   120  2/15/35 POINT (-99.64524 40.73164)  48.56232
7       8      South Platte   113   8/7/37 POINT (-103.5257 41.24492)  45.72952
8      10      South Platte   160   5/4/38 POINT (-103.0284 41.13243)  64.74976
9      11 Middle Republican   807   5/6/38  POINT (-101.1193 40.3527) 326.58160
10     12 Middle Republican   148 11/29/77 POINT (-101.1146 40.35631)  59.89353
```

Now, let's try to get a summary of a variable by group using the  `group_by()` and `summarize()` functions.


```r
#--- summary by group ---#
wells_by_nrd <- wells_sf %>% 
  #--- group by nrdname ---#
  dplyr::group_by(nrdname) %>% 
  #--- summarize ---#
  dplyr::summarize(tot_acres = sum(acres, na.rm = TRUE))

#--- take a look ---#
wells_by_nrd
```

```
Simple feature collection with 9 features and 2 fields
geometry type:  MULTIPOINT
dimension:      XY
bbox:           xmin: -104.0531 ymin: 40.00161 xmax: -96.87681 ymax: 41.85942
CRS:            unknown
# A tibble: 9 x 3
  nrdname       tot_acres                                               geometry
  <chr>             <dbl>                                       <MULTIPOINT [°]>
1 Central Plat…  1890918. ((-100.2329 41.14385), (-100.2328 41.05678), (-100.23…
2 Little Blue     995900. ((-98.72659 40.30463), (-98.72434 40.68021), (-98.724…
3 Lower Republ…   543079. ((-100.1968 40.32314), (-100.196 40.33553), (-100.195…
4 Middle Repub…   443472. ((-101.3691 40.1208), (-101.3448 40.64638), (-101.344…
5 South Platte    216109. ((-104.0531 41.18248), (-104.053 41.19347), (-104.052…
6 Tri-Basin       847058. ((-100.0927 40.42312), (-100.0904 40.43158), (-100.08…
7 Twin Platte     452678. ((-102.0557 41.05204), (-102.0556 41.05488), (-102.05…
8 Upper Big Bl…  1804782. ((-98.83619 40.85932), (-98.81149 40.78093), (-98.549…
9 Upper Republ…   551906. ((-102.0516 40.24644), (-102.0515 40.6287), (-102.051…
```

So, we got total acres by NRD as we expected. One interesting change that happened is geometry variable. Each NRD now has `multipoint` sfg, which is the combination of all the wells (points) located inside the NRD as you can see below.   


```r
tm_shape(wells_by_nrd) + 
  tm_symbols(col = "nrdname", size = 0.2) +
  tm_layout(
    frame = NA,
    legend.outside = TRUE,
    legend.outside.position = "bottom"
  )
```

<img src="VectorDataBasics_files/figure-html/map_combine-1.png" width="672" />

This feature is unlikely to be of much use to us. If you would like to drop a geometry column, you can use the `st_drop_geometry()` function:


```r
#--- remove geometry ---#
wells_no_longer_sf <- st_drop_geometry(wells_by_nrd)

#--- take a look ---#
wells_no_longer_sf
```

```
# A tibble: 9 x 2
  nrdname           tot_acres
* <chr>                 <dbl>
1 Central Platte     1890918.
2 Little Blue         995900.
3 Lower Republican    543079.
4 Middle Republican   443472.
5 South Platte        216109.
6 Tri-Basin           847058.
7 Twin Platte         452678.
8 Upper Big Blue     1804782.
9 Upper Republican    551906.
```

Finally, `data.table` does not work as well with sf objects as `dplyr` does. 


```r
#--- convert an sf to data.table ---#
(
wells_by_nrd_dt <- data.table(wells_by_nrd)
)
```

```
             nrdname tot_acres                           geometry
1:    Central Platte 1890918.2 MULTIPOINT ((-100.2329 41.1...,...
2:       Little Blue  995900.3 MULTIPOINT ((-98.72659 40.3...,...
3:  Lower Republican  543079.2 MULTIPOINT ((-100.1968 40.3...,...
4: Middle Republican  443472.2 MULTIPOINT ((-101.3691 40.1...,...
5:      South Platte  216109.0 MULTIPOINT ((-104.0531 41.1...,...
6:         Tri-Basin  847058.4 MULTIPOINT ((-100.0927 40.4...,...
7:       Twin Platte  452677.6 MULTIPOINT ((-102.0557 41.0...,...
8:    Upper Big Blue 1804781.5 MULTIPOINT ((-98.83619 40.8...,...
9:  Upper Republican  551906.2 MULTIPOINT ((-102.0516 40.2...,...
```

```r
#--- check the class ---#
class(wells_by_nrd_dt)
```

```
[1] "data.table" "data.frame"
```

You see that `wells_by_nrd_dt` is no longer an sf object even though geometry still remains in the data. If you try to run `sf` operations on it, it will of course give you an error. Like this:


```r
st_buffer(wells_by_nrd_dt, dist = 2)
```

```
Error in UseMethod("st_buffer"): no applicable method for 'st_buffer' applied to an object of class "c('data.table', 'data.frame')"
```

But, it is easy to revert it back to an `sf` object again by using the `st_as_sf()` function^[This was not the case before. Turning an `sf` object to a `data.table` object used to replace sfg with NA.]. 


```r
wells_by_nrd_sf_again <- st_as_sf(wells_by_nrd_dt)
```

So, this means that if you need fast data transformation, you can first turn an `sf` to a `data.table`, transform the data using the `data.table` functionality, and then revert back to `sf`. However, for most economists, the geometry variable itself is not of interest in the sense that it never enters econometric models. For most of us, the geographic information contained in the geometry variable is just a glue to tie two datasets together by geographic referencing. Once we get values of spatial variables of interest, there is no point in keeping your data as an sf object. Personally, whenever I no longer need to carry around the geometry variable, I immediately turn an sf object into a `data.table` for fast data transformation especially when the data is large. 

Those who know the [`dtplyr` package](https://github.com/tidyverse/dtplyr) (it takes advantage of the speed of `data.table` while you can keep using `dplyr` syntax and functions) may wonder if it works well with `sf` objects. Nope:


```r
library(dtplyr)

#--- convert an "lazy" data.table ---#
wells_ldt <- lazy_dt(wells_sf)  

#--- try  ---#
st_buffer(wells_ldt, dist = 2)
```

```
Error in UseMethod("st_buffer"): no applicable method for 'st_buffer' applied to an object of class "c('dtplyr_step_first', 'dtplyr_step')"
```

By the way, this package is awesome if you really love `dplyr`, but want the speed of `data.table`. `dtplyr` is of course slightly slower than `data.table` because an internal translation of `dplyr` language to `data.table` language has to happen first.^[I personally use `data.table` unless it is necessary to use `dplyr` like when dealing with `sf` objects. It is more concise than `dplyr`, which is somewhat verbose (yet expressive because of it). Ultimately, it is your personal preference which to use. You might be interested in reading [this discussion](https://stackoverflow.com/questions/21435339/data-table-vs-dplyr-can-one-do-something-well-the-other-cant-or-does-poorly) about the comparative advantages and disadvantages of the two packages.] 

## Non-interactive geometrical operations

There are various geometrical operations that are particularly useful for economists. Here, some of the most commonly used geometrical operations are introduced^[For the complete list of available geometrical operations under the `sf` package, see [here](https://cran.r-project.org/web/packages/sf/vignettes/sf1.html).]. You can see the practical use of some of these functions in Chapter \@ref(demo4).
 
### st_buffer 

`st_buffer()` creates a buffer around points, lines, or the border of polygons. 

Let's create buffers around points. First, we read well locations data. 


```r
#--- read wells location data ---#
urnrd_wells_sf <- readRDS("./Data/urnrd_wells.rds") %>% 
  #--- project to UTM 14N WGS 84 ---#
  st_transform(32614)  
```

Here is the spatial distribution of the wells (Figure \@ref(fig:urnrd-wells)). 


```r
tm_shape(urnrd_wells_sf) +
  tm_symbols(col = "red", size = 0.1) +
  tm_layout(frame = FALSE)
```

<div class="figure">
<img src="VectorDataBasics_files/figure-html/urnrd-wells-1.png" alt="Map of the wells" width="672" />
<p class="caption">(\#fig:urnrd-wells)Map of the wells</p>
</div>

Let's create buffers around the wells.


```r
#--- create a one-mile buffer around the wells ---#
wells_buffer <- st_buffer(urnrd_wells_sf, dist = 1600)
```

As you can see, there are many circles around wells with the radius of $1,600$ meters (Figure \@ref(fig:buffer-points-map)).   


```r
tm_shape(wells_buffer) +
  tm_polygons(alpha = 0) +
tm_shape(urnrd_wells_sf) +
  tm_symbols(col = "red", size = 0.1) +
  tm_layout(frame = NA)
```

<div class="figure">
<img src="VectorDataBasics_files/figure-html/buffer-points-map-1.png" alt="Buffers around wells" width="672" />
<p class="caption">(\#fig:buffer-points-map)Buffers around wells</p>
</div>

A practical application of buffer creation can be seen in Chapter \@ref(Demo1).

---

We now create buffers around polygons. First, read NE county boundary data and select three counties (Chase, Dundy, and Perkins).


```r
NE_counties <- readRDS("./Data/NE_county_borders.rds") %>%
  filter(NAME %in% c("Perkins", "Dundy", "Chase")) %>% 
  st_transform(32614)
```

Here is what they look like (Figure \@ref(fig:map-three-counties)):


```r
tm_shape(NE_counties) +
  tm_polygons('NAME', palette="RdYlGn", contrast=.3, title="County") +
  tm_layout(frame = NA)
```

<div class="figure">
<img src="VectorDataBasics_files/figure-html/map-three-counties-1.png" alt="Map of the three counties" width="672" />
<p class="caption">(\#fig:map-three-counties)Map of the three counties</p>
</div>

The following code creates buffers around polygons (see the results in Figure \@ref(fig:buffer-county)):


```r
NE_buffer <- st_buffer(NE_counties, dist = 2000)
```


```r
tm_shape(NE_buffer) +
  tm_polygons(col='blue',alpha=0.2) +
tm_shape(NE_counties) +
  tm_polygons('NAME', palette="RdYlGn", contrast=.3, title="County") + 
  tm_layout(
    legend.outside=TRUE,
    frame=FALSE
  )
```

<div class="figure">
<img src="VectorDataBasics_files/figure-html/buffer-county-1.png" alt="Buffers around the three counties" width="672" />
<p class="caption">(\#fig:buffer-county)Buffers around the three counties</p>
</div>

For example, this can be useful to identify observations which are close to the border of political boundaries when you want to take advantage of spatial discontinuity of policies across adjacent political boundaries.    

### st_area 

The `st_area()` function calculates the area of polygons. 


```r
#--- generate area by polygon ---#
(
  NE_counties <- mutate(NE_counties, area = st_area(NE_counties))
)
```

```
Simple feature collection with 3 features and 10 fields
geometry type:  MULTIPOLYGON
dimension:      XY
bbox:           xmin: 239494.1 ymin: 4430632 xmax: 310778.1 ymax: 4543676
CRS:            EPSG:32614
  STATEFP COUNTYFP COUNTYNS       AFFGEOID GEOID    NAME LSAD      ALAND
1      31      135 00835889 0500000US31135 31135 Perkins   06 2287828025
2      31      029 00835836 0500000US31029 31029   Chase   06 2316533447
3      31      057 00835850 0500000US31057 31057   Dundy   06 2381956151
   AWATER                       geometry             area
1 2840176 MULTIPOLYGON (((243340.2 45... 2302174854 [m^2]
2 7978172 MULTIPOLYGON (((241201.4 44... 2316908196 [m^2]
3 3046331 MULTIPOLYGON (((240811.3 44... 2389890531 [m^2]
```

Now, as you can see below, the default class of the results of `st_area()` is `units`, which does not accept numerical operations.


```r
class(NE_counties$area)
```

```
[1] "units"
```

So, let's turn it into double.


```r
(
NE_counties <- mutate(NE_counties, area = as.numeric(area))
)
```

```
Simple feature collection with 3 features and 10 fields
geometry type:  MULTIPOLYGON
dimension:      XY
bbox:           xmin: 239494.1 ymin: 4430632 xmax: 310778.1 ymax: 4543676
CRS:            EPSG:32614
  STATEFP COUNTYFP COUNTYNS       AFFGEOID GEOID    NAME LSAD      ALAND
1      31      135 00835889 0500000US31135 31135 Perkins   06 2287828025
2      31      029 00835836 0500000US31029 31029   Chase   06 2316533447
3      31      057 00835850 0500000US31057 31057   Dundy   06 2381956151
   AWATER                       geometry       area
1 2840176 MULTIPOLYGON (((243340.2 45... 2302174854
2 7978172 MULTIPOLYGON (((241201.4 44... 2316908196
3 3046331 MULTIPOLYGON (((240811.3 44... 2389890531
```

`st_area()` is useful when you want to find area-weighted average of characteristics after spatially joining two polygon layers using the `st_intersection()` function (See Chapter \@ref(polygon-polygon)).

### st_centroid

The `st_centroid()` function finds the centroid of each polygon.


```r
#--- create centroids ---#
(
NE_centroids <- st_centroid(NE_counties)
)
```

```
Simple feature collection with 3 features and 10 fields
geometry type:  POINT
dimension:      XY
bbox:           xmin: 271156.7 ymin: 4450826 xmax: 276594.1 ymax: 4525635
CRS:            EPSG:32614
  STATEFP COUNTYFP COUNTYNS       AFFGEOID GEOID    NAME LSAD      ALAND
1      31      135 00835889 0500000US31135 31135 Perkins   06 2287828025
2      31      029 00835836 0500000US31029 31029   Chase   06 2316533447
3      31      057 00835850 0500000US31057 31057   Dundy   06 2381956151
   AWATER                 geometry       area
1 2840176 POINT (276594.1 4525635) 2302174854
2 7978172 POINT (271469.9 4489429) 2316908196
3 3046331 POINT (271156.7 4450826) 2389890531
```

Here's the map of the output (Figure \@ref(fig:map-centroids)).


```r
tm_shape(NE_counties) +
  tm_polygons() +
tm_shape(NE_centroids)+
  tm_symbols(size=0.5) +
tm_layout(
    legend.outside=TRUE,
    frame=FALSE
  )
```

<div class="figure">
<img src="VectorDataBasics_files/figure-html/map-centroids-1.png" alt="The centroids of the polygons" width="672" />
<p class="caption">(\#fig:map-centroids)The centroids of the polygons</p>
</div>

It can be useful when creating a map with labels because the centroid of polygons tend to be a good place to place labels (Figure \@ref(fig:cent-label)).^[When creating maps with the ggplot2 package, you can use `geom_sf_text()` or `geom_sf_label()`, which automatically finds where to put texts. See some examples [here](https://yutani.rbind.io/post/geom-sf-text-and-geom-sf-label-are-coming/).]


```r
tm_shape(NE_counties) +
  tm_polygons() +
tm_shape(NE_centroids)+
  tm_text("NAME") +
tm_layout(
    legend.outside=TRUE,
    frame=FALSE
  )
```

<div class="figure">
<img src="VectorDataBasics_files/figure-html/cent-label-1.png" alt="County names placed at the centroids of the counties" width="672" />
<p class="caption">(\#fig:cent-label)County names placed at the centroids of the counties</p>
</div>

It may be also useful when you need to calculate the "distance" between polygons.  

### st_length

We can use `st_length()` to calculate great circle distances^[Great circle distance is the shortest distance between two points on the surface of a sphere (earth)] of `LINESTRING` and `MULTILINESTRING` when they are represented in geodetic coordinates. On the other hand, if they are projected and use a Cartesian coordinate system, it will calculate Euclidean distance. We use U.S. railroad data for a demonstration. 


```r
#--- import US railroad data and take only the first 10 of it ---#
(
a_railroad <- rail_roads <- st_read(dsn = "./Data", layer = "tl_2015_us_rails")[1:10, ]
)
```

```
Reading layer `tl_2015_us_rails' from data source `/Users/tmieno2/Dropbox/TeachingUNL/RGIS_Econ/Data' using driver `ESRI Shapefile'
Simple feature collection with 180958 features and 3 fields
geometry type:  MULTILINESTRING
dimension:      XY
bbox:           xmin: -165.4011 ymin: 17.95174 xmax: -65.74931 ymax: 65.00006
CRS:            4269
```

```
Simple feature collection with 10 features and 3 fields
geometry type:  MULTILINESTRING
dimension:      XY
bbox:           xmin: -79.74031 ymin: 35.0571 xmax: -79.2377 ymax: 35.51776
CRS:            4269
      LINEARID                    FULLNAME MTFCC                       geometry
1  11020239500       Norfolk Southern Rlwy R1011 MULTILINESTRING ((-79.47058...
2  11020239501       Norfolk Southern Rlwy R1011 MULTILINESTRING ((-79.46687...
3  11020239502       Norfolk Southern Rlwy R1011 MULTILINESTRING ((-79.66819...
4  11020239503       Norfolk Southern Rlwy R1011 MULTILINESTRING ((-79.46687...
5  11020239504       Norfolk Southern Rlwy R1011 MULTILINESTRING ((-79.74031...
6  11020239575      Seaboard Coast Line RR R1011 MULTILINESTRING ((-79.43695...
7  11020239576      Seaboard Coast Line RR R1011 MULTILINESTRING ((-79.47852...
8  11020239577      Seaboard Coast Line RR R1011 MULTILINESTRING ((-79.43695...
9  11020239589    Aberdeen and Rockfish RR R1011 MULTILINESTRING ((-79.38736...
10 11020239591 Aberdeen and Briar Patch RR R1011 MULTILINESTRING ((-79.53848...
```

```r
#--- check CRS ---#
st_crs(a_railroad)
```

```
Coordinate Reference System:
  User input: 4269 
  wkt:
GEOGCS["NAD83",
    DATUM["North_American_Datum_1983",
        SPHEROID["GRS 1980",6378137,298.257222101,
            AUTHORITY["EPSG","7019"]],
        TOWGS84[0,0,0,0,0,0,0],
        AUTHORITY["EPSG","6269"]],
    PRIMEM["Greenwich",0,
        AUTHORITY["EPSG","8901"]],
    UNIT["degree",0.0174532925199433,
        AUTHORITY["EPSG","9122"]],
    AUTHORITY["EPSG","4269"]]
```

It uses geodetic coordinate system. Let's calculate the great circle distance of the lines (Chapter \@ref(demo4) for a practical use case of this function).


```r
(
a_railroad <- mutate(a_railroad, length = st_length(a_railroad))
)
```

```
Simple feature collection with 10 features and 4 fields
geometry type:  MULTILINESTRING
dimension:      XY
bbox:           xmin: -79.74031 ymin: 35.0571 xmax: -79.2377 ymax: 35.51776
CRS:            4269
      LINEARID                    FULLNAME MTFCC                       geometry
1  11020239500       Norfolk Southern Rlwy R1011 MULTILINESTRING ((-79.47058...
2  11020239501       Norfolk Southern Rlwy R1011 MULTILINESTRING ((-79.46687...
3  11020239502       Norfolk Southern Rlwy R1011 MULTILINESTRING ((-79.66819...
4  11020239503       Norfolk Southern Rlwy R1011 MULTILINESTRING ((-79.46687...
5  11020239504       Norfolk Southern Rlwy R1011 MULTILINESTRING ((-79.74031...
6  11020239575      Seaboard Coast Line RR R1011 MULTILINESTRING ((-79.43695...
7  11020239576      Seaboard Coast Line RR R1011 MULTILINESTRING ((-79.47852...
8  11020239577      Seaboard Coast Line RR R1011 MULTILINESTRING ((-79.43695...
9  11020239589    Aberdeen and Rockfish RR R1011 MULTILINESTRING ((-79.38736...
10 11020239591 Aberdeen and Briar Patch RR R1011 MULTILINESTRING ((-79.53848...
           length
1    661.3381 [m]
2    657.4261 [m]
3  19982.5998 [m]
4  13888.3385 [m]
5   7194.7745 [m]
6   1061.2335 [m]
7   7824.0945 [m]
8  31756.9803 [m]
9   4547.1970 [m]
10 17103.0691 [m]
```



<!-- ## st_distance

The `st_distance` function calculates distances between spatial objects. Its output is the matrix of distances whose $i,j$ element is the distance between the $i$th `sfg` of the first `sf` and $j$th `sfg` of the second `sf`. The following code find the distance between the first 10 wells in `urnrd_wells_sf`.


```r
st_distance(urnrd_wells_sf[1:10, ], urnrd_wells_sf[1:10, ])
```

```
Units: [m]
          [,1]       [,2]       [,3]      [,4]     [,5]      [,6]       [,7]
 [1,]     0.00 24224.9674 24007.0480 19678.000 26974.04 25490.735 21710.2051
 [2,] 24224.97     0.0000   386.7858  8137.875 44632.84  2180.034 14285.1996
 [3,] 24007.05   386.7858     0.0000  7754.775 44610.53  2067.106 13901.5230
 [4,] 19678.00  8137.8750  7754.7753     0.000 43774.40  7892.077  6753.1200
 [5,] 26974.04 44632.8443 44610.5290 43774.401     0.00 46610.537 47713.4334
 [6,] 25490.73  2180.0343  2067.1055  7892.077 46610.54     0.000 13362.2041
 [7,] 21710.21 14285.1996 13901.5230  6753.120 47713.43 13362.204     0.0000
 [8,] 22029.35 14345.1319 13962.6364  6909.932 48027.14 13377.109   319.3365
 [9,] 17243.59 18297.5078 17916.8399 10172.159 44061.21 17978.355  6297.5682
[10,] 25184.43  6052.8916  5747.7464  5709.523 48215.58  4340.971  9505.0583
            [,8]      [,9]     [,10]
 [1,] 22029.3535 17243.589 25184.434
 [2,] 14345.1319 18297.508  6052.892
 [3,] 13962.6364 17916.840  5747.746
 [4,]  6909.9319 10172.159  5709.523
 [5,] 48027.1382 44061.209 48215.582
 [6,] 13377.1087 17978.355  4340.971
 [7,]   319.3365  6297.568  9505.058
 [8,]     0.0000  6543.339  9465.084
 [9,]  6543.3389     0.000 14832.639
[10,]  9465.0840 14832.639     0.000
```
 -->

<!--chapter:end:VectorDataBasics.Rmd-->

# Spatial Interactions of Vector Data: Subsetting and Joining








## Before you start {-}

In this chapter we learn the spatial interactions of two spatial objects. We first look at the **topological relations** of two spatial objects (how they are spatially related with each other): specifically, `st_intersects()` and `st_is_within_distance()`. `st_intersects()` is particularly important as it is by far the most common topological relation economists will use and also because it is the default topological relation that `sf` uses for spatial subsetting and spatial joining. 

We then follow with spatial subsetting: filtering spatial data by the geographic features of another spatial data. Finally, we will learn spatial joining. Spatial joining is the act of assigning attribute values from one spatial data to another spatial data based on how the two spatial datasets are spatially related (topological relation). This is the most important spatial operation for economists who want to use spatial variables in their econometric analysis. For those who have used the `sp` package, these operations are akin to `sp::over()`.

### Direction for replication {-}

**Datasets**

All the datasets that you need to import are available [here](https://www.dropbox.com/sh/fk149061msu06cj/AACMzyavFJrtOobST3KuxDsQa?dl=0). In this chapter, the path to files is set relative to my own working directory (which is hidden). To run the codes without having to mess with paths to the files, follow these steps:

+ set a folder (any folder) as the working directory using `setwd()`  
+ create a folder called "Data" inside the folder designated as the working directory (if you have created a "Data" folder previously, skip this step)
+ download the pertinent datasets from [here](https://www.dropbox.com/sh/fk149061msu06cj/AACMzyavFJrtOobST3KuxDsQa?dl=0) and put them in the "Data" folder

**Packages**

Run the following code to install or load (if already installed) the `pacman` package, and then install or load (if already installed) the listed package inside the `pacman::p_load()` function.


```r
if (!require("pacman")) install.packages("pacman")
pacman::p_load(
  sf, # vector data operations 
  dplyr, # data wrangling 
  data.table, # data wrangling 
  ggplot2, # for map creation 
  tmap # for map creation 
) 
```

## Topological relations

Before we learn spatial subsetting and joining, we first look at topological relations. Topological relations refer to the way multiple spatial objects are spatially related to one another. You can identify various types of spatial relations using the `sf` package. Our main focus is on the intersections of spatial objects, which can be found using `st_intersects()`.^[I would say it is very rare that you use other topological relations like `st_within()` or `st_touches()`.] We also briefly cover `st_is_within_distance()`^[Run `?geos_binary_pred` to see other topological relations you can find.]. 

---

We first create `sf` objects we are going to use for illustrations.

**POINTS**


```r
#--- create points ---#
point_1 <- st_point(c(2, 2))
point_2 <- st_point(c(1, 1))
point_3 <- st_point(c(1, 3))

#--- combine the points to make a single  sf of points ---#
(
points <- list(point_1, point_2, point_3) %>% 
  st_sfc() %>% 
  st_as_sf() %>% 
  mutate(point_name = c("point 1", "point 2", "point 3"))
)
```

```
Simple feature collection with 3 features and 1 field
geometry type:  POINT
dimension:      XY
bbox:           xmin: 1 ymin: 1 xmax: 2 ymax: 3
CRS:            NA
            x point_name
1 POINT (2 2)    point 1
2 POINT (1 1)    point 2
3 POINT (1 3)    point 3
```

---

**LINES**


```r
#--- create points ---#
line_1 <- st_linestring(rbind(c(0, 0), c(2.5, 0.5)))
line_2 <- st_linestring(rbind(c(1.5, 0.5), c(2.5, 2)))

#--- combine the points to make a single  sf of points ---#
(
lines <- list(line_1, line_2) %>% 
  st_sfc() %>% 
  st_as_sf() %>% 
  mutate(line_name = c("line 1", "line 2"))
)
```

```
Simple feature collection with 2 features and 1 field
geometry type:  LINESTRING
dimension:      XY
bbox:           xmin: 0 ymin: 0 xmax: 2.5 ymax: 2
CRS:            NA
                            x line_name
1   LINESTRING (0 0, 2.5 0.5)    line 1
2 LINESTRING (1.5 0.5, 2.5 2)    line 2
```

---

**POLYGONS**


```r
#--- create polygons ---#
polygon_1 <- st_polygon(list(
  rbind(c(0, 0), c(2, 0), c(2, 2), c(0, 2), c(0, 0)) 
))

polygon_2 <- st_polygon(list(
  rbind(c(0.5, 1.5), c(0.5, 3.5), c(2.5, 3.5), c(2.5, 1.5), c(0.5, 1.5)) 
))

polygon_3 <- st_polygon(list(
  rbind(c(0.5, 2.5), c(0.5, 3.2), c(2.3, 3.2), c(2, 2), c(0.5, 2.5)) 
))

#--- combine the polygons to make an sf of polygons ---#
(
polygons <- list(polygon_1, polygon_2, polygon_3) %>% 
  st_sfc() %>% 
  st_as_sf() %>% 
  mutate(polygon_name = c("polygon 1", "polygon 2", "polygon 3"))
)
```

```
Simple feature collection with 3 features and 1 field
geometry type:  POLYGON
dimension:      XY
bbox:           xmin: 0 ymin: 0 xmax: 2.5 ymax: 3.5
CRS:            NA
                               x polygon_name
1 POLYGON ((0 0, 2 0, 2 2, 0 ...    polygon 1
2 POLYGON ((0.5 1.5, 0.5 3.5,...    polygon 2
3 POLYGON ((0.5 2.5, 0.5 3.2,...    polygon 3
```

---

Figure \@ref(fig:plot-point-polygons) shows how they look: 


```r
ggplot() +
  geom_sf(data = polygons, aes(fill = polygon_name), alpha = 0.3) +
  scale_fill_discrete(name = "Polygons") +
  geom_sf(data = lines, aes(color = line_name)) +
  scale_color_discrete(name = "Lines") + 
  geom_sf(data = points, aes(shape = point_name), size = 3) +
  scale_shape_discrete(name = "Points")  
```

<div class="figure">
<img src="SpatialInteractionVectorVector_files/figure-html/plot-point-polygons-1.png" alt="Visualization of the points, lines, and polygons" width="672" />
<p class="caption">(\#fig:plot-point-polygons)Visualization of the points, lines, and polygons</p>
</div>

### st_intersects()  

This function identifies which `sfg` object in an `sf` (or `sfc`) intersects with `sfg` object(s) in another `sf`. For example, you can use the function to identify which well is located within which county. `st_intersects()` is the most commonly used topological relation. It is important to understand what it does as it is the default topological relation used when performing spatial subsetting and joining, which we will cover later.  

---

**points and polygons**


```r
st_intersects(points, polygons)
```

```
Sparse geometry binary predicate list of length 3, where the predicate was `intersects'
 1: 1, 2, 3
 2: 1
 3: 2, 3
```
As you can see, the output is a list of which polygon(s) each of the points intersect with. The numbers 1, 2, and 3 in the first row mean that 1st (polygon 1), 2nd (polygon 2), and 3rd (polygon 3) objects of the `polygons` intersect with the first point (point 1) of the `points` object. The fact that point 1 is considered to be intersecting with polygon 2 means that the area inside the border is considered a part of the polygon (of course). 

If you would like the results of `st_intersects()` in a matrix form with boolean values filling the matrix, you can add `sparse = FALSE` option. 


```r
st_intersects(points, polygons, sparse = FALSE)
```

```
      [,1]  [,2]  [,3]
[1,]  TRUE  TRUE  TRUE
[2,]  TRUE FALSE FALSE
[3,] FALSE  TRUE  TRUE
```

---

**lines and polygons**


```r
st_intersects(lines, polygons)
```

```
Sparse geometry binary predicate list of length 2, where the predicate was `intersects'
 1: 1
 2: 1, 2
```

The output is a list of which polygon(s) each of the lines intersect with. 

---

**polygons and polygons**

For polygons vs polygons interaction, `st_intersects()` identifies any polygons that either touches (even at a point like polygons 1 and 3) or share some area.


```r
st_intersects(polygons, polygons)
```

```
Sparse geometry binary predicate list of length 3, where the predicate was `intersects'
 1: 1, 2, 3
 2: 1, 2, 3
 3: 1, 2, 3
```


### st_is_within_distance()  

This function identifies whether two spatial objects are within the distance you specify as the name suggests^[This function can be useful to identify neighbors. For example, you may want to find irrigation wells located around well $i$ to label them as well $i$'s neighbor.].  

Let's first create two sets of points. 


```r
set.seed(38424738)

points_set_1 <- lapply(1:5, function(x) st_point(runif(2))) %>% 
  st_sfc() %>% st_as_sf() %>% 
  mutate(id = 1:nrow(.))

points_set_2 <- lapply(1:5, function(x) st_point(runif(2))) %>% 
  st_sfc() %>% st_as_sf() %>% 
  mutate(id = 1:nrow(.))
```

Here is how they are spatially distributed (Figure \@ref(fig:map-points-points-points)). Instead of circles of points, their corresponding `id` (or equivalently row number here) values are displayed.


```r
ggplot() +
  geom_sf_text(data = points_set_1, aes(label = id), color = "red") +
  geom_sf_text(data = points_set_2, aes(label = id), color = "blue") 
```

<div class="figure">
<img src="SpatialInteractionVectorVector_files/figure-html/map-points-points-points-1.png" alt="The locations of the set of points" width="672" />
<p class="caption">(\#fig:map-points-points-points)The locations of the set of points</p>
</div>

We want to know which of the blue points (points_set_2) are located within 0.2 from each of the red points (points_set_1). The following figure (Figure \@ref(fig:points-points-within)) gives us the answer visually.


```r
#--- create 0.2 buffers around points in points_set_1 ---#
buffer_1 <- st_buffer(points_set_1, dist = 0.2)

ggplot() +
  geom_sf(data = buffer_1, color = "red", fill = NA) +
  geom_sf_text(data = points_set_1, aes(label = id), color = "red") +
  geom_sf_text(data = points_set_2, aes(label = id), color = "blue") 
```

<div class="figure">
<img src="SpatialInteractionVectorVector_files/figure-html/points-points-within-1.png" alt="The blue points within 0.2 radius of the red points" width="672" />
<p class="caption">(\#fig:points-points-within)The blue points within 0.2 radius of the red points</p>
</div>

Confirm your visual inspection results with the outcome of the following code using `st_is_within_distance()` function.


```r
st_is_within_distance(points_set_1, points_set_2, dist = 0.2)
```

```
Sparse geometry binary predicate list of length 5, where the predicate was `is_within_distance'
 1: 1
 2: (empty)
 3: (empty)
 4: (empty)
 5: 3
```

## Spatial Subsetting (or Flagging)

Spatial subsetting refers to operations that narrow down the geographic scope of a spatial object based on another spatial object. We illustrate spatial subsetting using Kansas county borders, the boundary of the High-Plains Aquifer (HPA), and agricultural irrigation wells in Kansas.    

First, let's import all the files we will use in this section. 


```r
#--- Kansas county borders ---#
KS_counties <- readRDS("./Data/KS_county_borders.rds")

#--- HPA boundary ---#
hpa <- st_read(dsn = "./Data", layer = "hp_bound2010") %>% 
  .[1, ] %>% 
  st_transform(st_crs(KS_counties))  

#--- all the irrigation wells in KS ---#
KS_wells <- readRDS("./Data/Kansas_wells.rds") %>% 
  st_transform(st_crs(KS_counties))

#--- US railroad ---#
rail_roads <- st_read(dsn = "./Data", layer = "tl_2015_us_rails") %>% 
  st_transform(st_crs(KS_counties)) 
```

### polygons vs polygons

The following map (Figure \@ref(fig:overlap-KS-county-HPA)) shows the Kansas portion of the HPA and Kansas counties.


```r
#--- add US counties layer ---#
tm_shape(KS_counties) +
  tm_polygons() +
#--- add High-Plains Aquifer layer ---#
tm_shape(hpa) +
  tm_fill(col = "blue", alpha = 0.3)
```

<div class="figure">
<img src="SpatialInteractionVectorVector_files/figure-html/overlap-KS-county-HPA-1.png" alt="Kansas portion of High-Plains Aquifer and Kansas counties" width="672" />
<p class="caption">(\#fig:overlap-KS-county-HPA)Kansas portion of High-Plains Aquifer and Kansas counties</p>
</div>

The goal here is to select only the counties that intersect with the HPA boundary. When subsetting a data.frame by specifying the row numbers you would like to select, you can do 


```r
#--- NOT RUN ---#
data.frame[vector of row numbers, ]
```

Spatial subsetting of sf objects works in a similar syntax:   


```r
#--- NOT RUN ---#
sf_1[sf_2, ]
```

where you are subsetting sf_1 based on sf_2. Instead of row numbers, you provide another sf object in place. The following code spatially subsets Kansas counties based on the HPA boundary.


```r
counties_in_hpa <- KS_counties[hpa, ]
```

See the results below in Figure \@ref(fig:default-subset).


```r
#--- add US counties layer ---#
tm_shape(counties_in_hpa) +
  tm_polygons() +
#--- add High-Plains Aquifer layer ---#
tm_shape(hpa) +
  tm_fill(col = "blue", alpha = 0.3)
```

<div class="figure">
<img src="SpatialInteractionVectorVector_files/figure-html/default-subset-1.png" alt="The results of spatially subsetting Kansas counties based on HPA boundary" width="672" />
<p class="caption">(\#fig:default-subset)The results of spatially subsetting Kansas counties based on HPA boundary</p>
</div>

You can see that only the counties that intersect with the HPA boundary remained. This is because when you use the above syntax of `sf_1[sf_2, ]`, the default underlying topological relations is `st_intersects()`. So, if an object in `sf_1` intersects with any of the objects in `sf_2` even slightly, then it will remain after subsetting. 

You can specify the spatial operation to be used as an option as in 


```r
#--- NOT RUN ---#
sf_1[sf_2, op = topological_relation_type] 
```

For example, if you only want counties that are completely within the HPA boundary, you can do the following (the map of the results in Figure \@ref(fig:within-subset)):


```r
counties_within_hpa <- KS_counties[hpa, , op = st_within]
```
 

```r
#--- add US counties layer ---#
tm_shape(counties_within_hpa) +
  tm_polygons() +
#--- add High-Plains Aquifer layer ---#
tm_shape(hpa) +
  tm_fill(col = "blue", alpha = 0.3)
```

<div class="figure">
<img src="SpatialInteractionVectorVector_files/figure-html/within-subset-1.png" alt="Kansas counties that are completely within HPA boundary" width="672" />
<p class="caption">(\#fig:within-subset)Kansas counties that are completely within HPA boundary</p>
</div>

<!-- 
#%%%%%%%%%%%%%%%%%%%%%
# Points vs Polygons 
#%%%%%%%%%%%%%%%%%%%%%
-->

### points vs polygons

The following map (Figure \@ref(fig:map-wells-county)) shows the Kansas portion of the HPA and all the irrigation wells in Kansas.


```r
tm_shape(KS_wells) +
  tm_symbols(size = 0.1) +
tm_shape(hpa) +
  tm_polygons(col = "blue", alpha = 0.1) 
```

<div class="figure">
<img src="SpatialInteractionVectorVector_files/figure-html/map-wells-county-1.png" alt="A map of Kansas irrigation wells and HPA" width="672" />
<p class="caption">(\#fig:map-wells-county)A map of Kansas irrigation wells and HPA</p>
</div>

We can select only wells that reside within the HPA boundary using the same syntax as the above example.


```r
KS_wells_in_hpa <- KS_wells[hpa, ]
```

As you can see in Figure \@ref(fig:map-wells-in-hpa) below, only the wells that are inside (or intersect with) the HPA remained because the default topological relation is `st_intersects()`.  


```r
tm_shape(KS_wells_in_hpa) +
  tm_symbols(size = 0.1) +
tm_shape(hpa) +
  tm_polygons(col = "blue", alpha = 0.1) 
```

<div class="figure">
<img src="SpatialInteractionVectorVector_files/figure-html/map-wells-in-hpa-1.png" alt="A map of Kansas irrigation wells and HPA" width="672" />
<p class="caption">(\#fig:map-wells-in-hpa)A map of Kansas irrigation wells and HPA</p>
</div>

<!-- 
#%%%%%%%%%%%%%%%%%%%%%
# Lines vs Polygons 
#%%%%%%%%%%%%%%%%%%%%%
-->

### lines vs polygons {#lines_polygons}

The following map (Figure \@ref(fig:mapl-lines-county)) shows the Kansas counties and U.S. railroads.


```r
ggplot() +
  geom_sf(data = rail_roads, col = "blue") +
  geom_sf(data = KS_counties, fill = NA)  
```

<div class="figure">
<img src="SpatialInteractionVectorVector_files/figure-html/mapl-lines-county-1.png" alt="U.S. railroads and Kansas county boundaries" width="672" />
<p class="caption">(\#fig:mapl-lines-county)U.S. railroads and Kansas county boundaries</p>
</div>

We can select only railroads that intersect with Kansas.


```r
railroads_KS <- rail_roads[KS_counties, ]
```

As you can see in Figure \@ref(fig:map-rail-ks) below, only the railroads that intersect with Kansas were selected. Note the lines that go beyond the Kansas boundary are also selected. Remember, the default is `st_intersect()`. If you would like the lines beyond the state boundary to be cut out but the intersecting parts of those lines to remain, use `st_intersection()`, which is explained in Chapter \@ref(st_intersection).


```r
tm_shape(railroads_KS) +
  tm_lines(col = "blue") +
tm_shape(KS_counties) +
  tm_polygons(alpha = 0)  +
  tm_layout(frame = FALSE) 
```

<div class="figure">
<img src="SpatialInteractionVectorVector_files/figure-html/map-rail-ks-1.png" alt="Railroads that intersect Kansas county boundaries" width="672" />
<p class="caption">(\#fig:map-rail-ks)Railroads that intersect Kansas county boundaries</p>
</div>

### Flagging instead of subsetting

Sometimes, you just want to flag whether two spatial objects intersect or not, instead of dropping non-overlapping observations. In that case, you can use `st_intersects()`.

---

**Counties (polygons) against HPA boundary (polygons)**


```r
#--- county ---#
KS_counties <- mutate(KS_counties, intersects_hpa  = st_intersects(KS_counties, hpa, sparse = FALSE))

#--- take a look ---#
dplyr::select(KS_counties, COUNTYFP, intersects_hpa)
```

```
Simple feature collection with 105 features and 2 fields
geometry type:  MULTIPOLYGON
dimension:      XY
bbox:           xmin: -102.0517 ymin: 36.99308 xmax: -94.59193 ymax: 40.00308
CRS:            EPSG:4269
First 10 features:
   COUNTYFP intersects_hpa                       geometry
1       133          FALSE MULTIPOLYGON (((-95.5255 37...
2       075           TRUE MULTIPOLYGON (((-102.0446 3...
3       123          FALSE MULTIPOLYGON (((-98.48738 3...
4       189           TRUE MULTIPOLYGON (((-101.5566 3...
5       155           TRUE MULTIPOLYGON (((-98.47279 3...
6       129           TRUE MULTIPOLYGON (((-102.0419 3...
7       073          FALSE MULTIPOLYGON (((-96.52278 3...
8       023           TRUE MULTIPOLYGON (((-102.0517 4...
9       089           TRUE MULTIPOLYGON (((-98.50445 4...
10      059          FALSE MULTIPOLYGON (((-95.50827 3...
```

---

**Wells (points) against HPA boundary (polygons)**


```r
#--- wells ---#
KS_wells <- mutate(KS_wells, in_hpa  = st_intersects(KS_wells, hpa, sparse = FALSE))

#--- take a look ---#
dplyr::select(KS_wells, site, in_hpa)
```

```
Simple feature collection with 37647 features and 2 fields
geometry type:  POINT
dimension:      XY
bbox:           xmin: -102.0495 ymin: 36.99552 xmax: -94.62089 ymax: 40.00199
CRS:            EPSG:4269
First 10 features:
   site in_hpa                   geometry
1     1   TRUE POINT (-100.4423 37.52046)
2     3   TRUE POINT (-100.7118 39.91526)
3     5   TRUE POINT (-99.15168 38.48849)
4     7   TRUE POINT (-101.8995 38.78077)
5     8   TRUE  POINT (-100.7122 38.0731)
6     9  FALSE POINT (-97.70265 39.04055)
7    11   TRUE POINT (-101.7114 39.55035)
8    12  FALSE POINT (-95.97031 39.16121)
9    15   TRUE POINT (-98.30759 38.26787)
10   17   TRUE POINT (-100.2785 37.71539)
```

---

**U.S. railroads (lines) against Kansas counties (polygons)**

Unlike the previous two cases, multiple objects (lines) are checked against multiple objects (polygons) for intersection^[Of course, this situation arises for a polygons-polygons case as well. The above polygons-polygons example was an exception because `hpa` had only one polygon object.]. Therefore, we cannot use the strategy we took above of returning a vector of true or false using `sparse = TRUE` option. Here, we need to count the number of intersecting counties and then assign `TRUE` if the number is greater than 0. 


```r
#--- check the number of intersecting KS counties ---#
int_mat <- st_intersects(rail_roads, KS_counties) %>% 
  lapply(length) %>% 
  unlist() 

#--- railroads ---#
rail_roads <- mutate(rail_roads, intersect_ks  = int_mat > 0)

#--- take a look ---#
dplyr::select(rail_roads, LINEARID, intersect_ks)
```

```
Simple feature collection with 180958 features and 2 fields
geometry type:  MULTILINESTRING
dimension:      XY
bbox:           xmin: -165.4011 ymin: 17.95174 xmax: -65.74931 ymax: 65.00006
CRS:            4269
First 10 features:
      LINEARID intersect_ks                       geometry
1  11020239500        FALSE MULTILINESTRING ((-79.47058...
2  11020239501        FALSE MULTILINESTRING ((-79.46687...
3  11020239502        FALSE MULTILINESTRING ((-79.66819...
4  11020239503        FALSE MULTILINESTRING ((-79.46687...
5  11020239504        FALSE MULTILINESTRING ((-79.74031...
6  11020239575        FALSE MULTILINESTRING ((-79.43695...
7  11020239576        FALSE MULTILINESTRING ((-79.47852...
8  11020239577        FALSE MULTILINESTRING ((-79.43695...
9  11020239589        FALSE MULTILINESTRING ((-79.38736...
10 11020239591        FALSE MULTILINESTRING ((-79.53848...
```

## Spatial Join

By spatial join, we mean spatial operations that involve all of the following:

+ overlay one spatial layer (target layer) onto another spatial layer (source layer) 
+ for each of the observation in the target layer
  * identify which objects in the source layer it geographically intersects (or a different  topological relation) with  
  * extract values associated with the intersecting objects in the source layer (and summarize if necessary), 
  * assign the extracted value to the object in the target layer

For economists, this is probably the most common motivation for using GIS software, with the ultimate goal being to include the spatially joined variables as covariates in regression analysis. 

We can classify spatial join into four categories by the type of the underlying spatial objects:

+ vector-vector: vector data (target) against vector data (source)  
+ vector-raster: vector data (target) against raster data (source)  
+ raster-vector: raster data (target) against vector data (source)  
+ raster-raster: raster data (target) against raster data (source)  

Among the four, our focus here is the first case. The second case will be discussed in Chapter 5. We will not cover the third and fourth cases in this course because it is almost always the case that our target data is a vector data (e.g., city or farm fields as points, political boundaries as polygons, etc).  

Category 1 can be further broken down into different sub categories depending on the type of spatial object (point, line, and polygon). Here, we will ignore any spatial joins that involve lines. This is because objects represented by lines are rarely observation units in econometric analysis nor the source data from which we will extract values.^[Note that we did not extract any attribute values of the railroads in Chapter 1, Demonstration 4. We just calculated the travel length of the railroads, meaning that the geometry of railroads themselves were of interest instead of values associated with the railroads.] Here is the list of the types of spatial joins we will learn.  

1. points (target) against polygons (source)
2. polygons (target) against points (source)
3. polygons (target) against polygons (source)

<!-- 
#=========================================
# Spatial Joining 
#=========================================
-->

### Case 1: points (target) vs polygons (source)

Case 1, for each of the observations (points) in the target data, finds which polygon in the source file it intersects, and then assign the value associated with the polygon to the point^[You can see a practical example of this case in action in Demonstration 1 of Chapter X.]. In order to achieve this, we can use the `st_join()` function, whose syntax is as follows:    


```r
#--- NOT RUN ---#
st_join(target_sf, source_sf)
```

Similar to spatial subsetting, the default topological relation is `st_intersects()`^[While it is unlikely you will face the need to change the topological relation, you could do so using the `join` option.]. 

We use the Kansas irrigation well data (points) and Kansas county boundary data (polygons) for a demonstration. Our goal is to assign the county-level corn price information from the Kansas county data to wells. First let me create and add a fake county-level corn price variable to the Kansas county data.  


```r
KS_corn_price <- KS_counties %>%  
  mutate(
    corn_price = seq(3.2, 3.9, length = nrow(.)) 
  ) %>% 
  dplyr::select(COUNTYFP, corn_price)
```

Here is the map of Kansas counties color-differentiated by fake corn price (Figure \@ref(fig:map-corn-price)):


```r
tm_shape(KS_corn_price) + 
  tm_polygons(col = "corn_price") +
  tm_layout(frame = FALSE, legend.outside = TRUE)
```

<div class="figure">
<img src="SpatialInteractionVectorVector_files/figure-html/map-corn-price-1.png" alt="Map of county-level fake corn price" width="672" />
<p class="caption">(\#fig:map-corn-price)Map of county-level fake corn price</p>
</div>

For this particular context, the following code will do the job: 


```r
#--- spatial join ---#
(
KS_wells_County <- st_join(KS_wells, KS_corn_price)
)
```

```
Simple feature collection with 37647 features and 5 fields
geometry type:  POINT
dimension:      XY
bbox:           xmin: -102.0495 ymin: 36.99552 xmax: -94.62089 ymax: 40.00199
CRS:            EPSG:4269
First 10 features:
   site    af_used in_hpa COUNTYFP corn_price                   geometry
1     1 232.099948   TRUE      069   3.556731 POINT (-100.4423 37.52046)
2     3  13.183940   TRUE      039   3.449038 POINT (-100.7118 39.91526)
3     5  99.187052   TRUE      165   3.287500 POINT (-99.15168 38.48849)
4     7   0.000000   TRUE      199   3.644231 POINT (-101.8995 38.78077)
5     8 145.520499   TRUE      055   3.832692  POINT (-100.7122 38.0731)
6     9   3.614535  FALSE      143   3.799038 POINT (-97.70265 39.04055)
7    11 188.423543   TRUE      181   3.590385 POINT (-101.7114 39.55035)
8    12  77.335960  FALSE      177   3.550000 POINT (-95.97031 39.16121)
9    15   0.000000   TRUE      159   3.610577 POINT (-98.30759 38.26787)
10   17 167.819034   TRUE      069   3.556731 POINT (-100.2785 37.71539)
```

You can see from Figure \@ref(fig:map-corn-wells) below that all the wells inside the same county have the same corn price value. 


```r
tm_shape(KS_counties) +
  tm_polygons() +
tm_shape(KS_wells_County) +
  tm_symbols(col = "corn_price", size = 0.1) +
  tm_layout(frame = FALSE, legend.outside = TRUE)
```

<div class="figure">
<img src="SpatialInteractionVectorVector_files/figure-html/map-corn-wells-1.png" alt="Map of wells color-differentiated by corn price" width="672" />
<p class="caption">(\#fig:map-corn-wells)Map of wells color-differentiated by corn price</p>
</div>

### Case 2: polygons (target) vs points (source)

Case 2, for each of the observations (polygons) in the target data, find which observations (points) in the source file it intersects, and then assign the values associated with the points to the polygon. We use the same function: `st_join()`^[You can see a practical example of this case in action in Demonstration 2 of Chapter X.]. 

Suppose you are now interested in county-level analysis and you would like to get county-level total groundwater pumping. The target file is `KS_counties`, and the source file is `KS_wells`.


```r
#--- spatial join ---#
KS_County_wells <- st_join(KS_counties, KS_wells)

#--- take a look ---#
dplyr::select(KS_County_wells, COUNTYFP, site, af_used)
```

```
Simple feature collection with 37652 features and 3 fields
geometry type:  MULTIPOLYGON
dimension:      XY
bbox:           xmin: -102.0517 ymin: 36.99308 xmax: -94.59193 ymax: 40.00308
CRS:            EPSG:4269
First 10 features:
    COUNTYFP  site   af_used                       geometry
1        133 53861  17.01790 MULTIPOLYGON (((-95.5255 37...
1.1      133 70592   0.00000 MULTIPOLYGON (((-95.5255 37...
2        075   328 394.04513 MULTIPOLYGON (((-102.0446 3...
2.1      075   336  80.65036 MULTIPOLYGON (((-102.0446 3...
2.2      075   436 568.25359 MULTIPOLYGON (((-102.0446 3...
2.3      075  1007 215.80416 MULTIPOLYGON (((-102.0446 3...
2.4      075  1170   0.00000 MULTIPOLYGON (((-102.0446 3...
2.5      075  1192  77.39120 MULTIPOLYGON (((-102.0446 3...
2.6      075  1249   0.00000 MULTIPOLYGON (((-102.0446 3...
2.7      075  1300 320.22612 MULTIPOLYGON (((-102.0446 3...
```

As you can see in the resulting dataset, all the unique polygon - point intersecting combinations comprise the observations. For each of the polygons, you will have as many observations as the number of wells that intersect with the polygon. Once you join the two layers, you can find statistics by polygon (county here). Since we want groundwater extraction by county, the following does the job.


```r
KS_County_wells %>% 
  group_by(COUNTYFP) %>% 
  summarize(af_used = sum(af_used, na.rm = TRUE)) 
```

```
Simple feature collection with 105 features and 2 fields
geometry type:  MULTIPOLYGON
dimension:      XY
bbox:           xmin: -102.0517 ymin: 36.99308 xmax: -94.59193 ymax: 40.00308
CRS:            EPSG:4269
# A tibble: 105 x 3
   COUNTYFP af_used                                                     geometry
   <fct>      <dbl>                                           <MULTIPOLYGON [°]>
 1 001           0  (((-95.51931 37.82026, -95.51897 38.03823, -95.07788 38.037…
 2 003           0  (((-95.50833 38.39028, -95.06583 38.38994, -95.07788 38.037…
 3 005         771. (((-95.56413 39.65287, -95.33974 39.65298, -95.11519 39.652…
 4 007        4972. (((-99.0126 37.47042, -98.46466 37.47101, -98.46493 37.3841…
 5 009       61083. (((-99.03297 38.69676, -98.48611 38.69688, -98.47991 38.681…
 6 011           0  (((-95.08808 37.73248, -95.07969 37.8198, -95.07788 38.0377…
 7 013         480. (((-95.78811 40.00047, -95.78457 40.00046, -95.3399 40.0000…
 8 015         343. (((-97.15248 37.91273, -97.15291 38.0877, -96.84077 38.0856…
 9 017           0  (((-96.83765 38.34864, -96.81951 38.52245, -96.35378 38.521…
10 019           0  (((-96.52487 37.30273, -95.9644 37.29923, -95.96427 36.9992…
# … with 95 more rows
```

Of course, it is just as easy to get other types of statistics by simply modifying the `summarize()` part.

However, this two-step process can actually be done in one step using `aggregate()`, in which you specify how you want to aggregate with the `FUN` option as follows:


```r
#--- mean ---#
aggregate(KS_wells, KS_counties, FUN = mean)
```

```
Simple feature collection with 105 features and 3 fields
geometry type:  MULTIPOLYGON
dimension:      XY
bbox:           xmin: -102.0517 ymin: 36.99308 xmax: -94.59193 ymax: 40.00308
CRS:            EPSG:4269
First 10 features:
       site    af_used    in_hpa                       geometry
1  62226.50   8.508950 0.0000000 MULTIPOLYGON (((-95.5255 37...
2  35184.64 176.390742 0.4481793 MULTIPOLYGON (((-102.0446 3...
3  40086.82  35.465123 0.0000000 MULTIPOLYGON (((-98.48738 3...
4  40179.41 285.672916 1.0000000 MULTIPOLYGON (((-101.5566 3...
5  51249.39  46.048048 0.9743783 MULTIPOLYGON (((-98.47279 3...
6  33033.13 202.612377 1.0000000 MULTIPOLYGON (((-102.0419 3...
7  29840.40   0.000000 0.0000000 MULTIPOLYGON (((-96.52278 3...
8  28235.82  94.585634 0.9736842 MULTIPOLYGON (((-102.0517 4...
9  36180.06  44.033911 0.3000000 MULTIPOLYGON (((-98.50445 4...
10 40016.00   1.142775 0.0000000 MULTIPOLYGON (((-95.50827 3...
```

```r
#--- sum ---#
aggregate(KS_wells, KS_counties, FUN = sum)
```

```
Simple feature collection with 105 features and 3 fields
geometry type:  MULTIPOLYGON
dimension:      XY
bbox:           xmin: -102.0517 ymin: 36.99308 xmax: -94.59193 ymax: 40.00308
CRS:            EPSG:4269
First 10 features:
       site      af_used in_hpa                       geometry
1    124453 1.701790e+01      0 MULTIPOLYGON (((-95.5255 37...
2  12560917 6.297149e+04    160 MULTIPOLYGON (((-102.0446 3...
3   1964254 1.737791e+03      0 MULTIPOLYGON (((-98.48738 3...
4  42389277 3.013849e+05   1055 MULTIPOLYGON (((-101.5566 3...
5  68007942 6.110576e+04   1293 MULTIPOLYGON (((-98.47279 3...
6  15756801 9.664610e+04    477 MULTIPOLYGON (((-102.0419 3...
7    149202 0.000000e+00      0 MULTIPOLYGON (((-96.52278 3...
8  17167377 5.750807e+04    592 MULTIPOLYGON (((-102.0517 4...
9   1809003 2.201696e+03     15 MULTIPOLYGON (((-98.50445 4...
10   160064 4.571102e+00      0 MULTIPOLYGON (((-95.50827 3...
```

Notice that the `mean()` function was applied to all the columns in `KS_wells`, including site id number. So, you might want to select variables you want to join before you apply the `aggregate()` function like this:  


```r
aggregate(dplyr::select(KS_wells, af_used), KS_counties, FUN = mean)
```

```
Simple feature collection with 105 features and 1 field
geometry type:  MULTIPOLYGON
dimension:      XY
bbox:           xmin: -102.0517 ymin: 36.99308 xmax: -94.59193 ymax: 40.00308
CRS:            EPSG:4269
First 10 features:
      af_used                       geometry
1    8.508950 MULTIPOLYGON (((-95.5255 37...
2  176.390742 MULTIPOLYGON (((-102.0446 3...
3   35.465123 MULTIPOLYGON (((-98.48738 3...
4  285.672916 MULTIPOLYGON (((-101.5566 3...
5   46.048048 MULTIPOLYGON (((-98.47279 3...
6  202.612377 MULTIPOLYGON (((-102.0419 3...
7    0.000000 MULTIPOLYGON (((-96.52278 3...
8   94.585634 MULTIPOLYGON (((-102.0517 4...
9   44.033911 MULTIPOLYGON (((-98.50445 4...
10   1.142775 MULTIPOLYGON (((-95.50827 3...
```

### Case 3: polygons (target) vs polygons (source) {#polygon-polygon}

For this case, `st_join(target_sf, source_sf)` will return all the unique intersecting polygon-polygon combinations with the information of the polygon from source_sf attached.  

We will use county-level corn acres in Iowa in 2018 from USDA NASS^[See [here](link_here) for how to download Quick Stats data from within R.] and Hydrologic Units^[See [here](https://water.usgs.gov/GIS/huc.html) for an explanation of what they are. You do not really need to know what HUC units are to understand what's done in this section.] Our objective here is to find corn acres by HUC units based on the county-level corn acres data^[Yes, there will be substantial measurement errors as the source polygons (corn acres by county) are large relative to the target polygons (HUC units). But, this serves as a good illustration of a polygon-polygon join.].   

We first import the Iowa corn acre data:


```r
#--- IA boundary ---#
IA_corn <- readRDS("./Data/IA_corn.rds")

#--- take a look ---#
IA_corn
```

```
Simple feature collection with 93 features and 3 fields
geometry type:  MULTIPOLYGON
dimension:      XY
bbox:           xmin: 203228.6 ymin: 4470941 xmax: 736832.9 ymax: 4822687
CRS:            EPSG:26915
First 10 features:
   county_code year  acres                       geometry
1          083 2018 183500 MULTIPOLYGON (((458997 4711...
2          141 2018 167000 MULTIPOLYGON (((267700.8 47...
3          081 2018 184500 MULTIPOLYGON (((421231.2 47...
4          019 2018 189500 MULTIPOLYGON (((575285.6 47...
5          023 2018 165500 MULTIPOLYGON (((497947.5 47...
6          195 2018 111500 MULTIPOLYGON (((459791.6 48...
7          063 2018 110500 MULTIPOLYGON (((345214.3 48...
8          027 2018 183000 MULTIPOLYGON (((327408.5 46...
9          121 2018  70000 MULTIPOLYGON (((396378.1 45...
10         077 2018 107000 MULTIPOLYGON (((355180.1 46...
```

Here is the map of Iowa counties color-differentiated by corn acres (Figure \@ref(fig:map-IA-corn)):


```r
#--- here is the map ---#
tm_shape(IA_corn) +
  tm_polygons(col = "acres") +
  tm_layout(frame = FALSE, legend.outside = TRUE)
```

<div class="figure">
<img src="SpatialInteractionVectorVector_files/figure-html/map-IA-corn-1.png" alt="Map of Iowa counties color-differentiated by corn planted acreage" width="672" />
<p class="caption">(\#fig:map-IA-corn)Map of Iowa counties color-differentiated by corn planted acreage</p>
</div>

Now import the HUC units data:


```r
#--- import HUC units ---#
HUC_IA <- st_read(dsn = "./Data", layer = "huc250k") %>% 
  dplyr::select(HUC_CODE) %>% 
  #--- reproject to the CRS of IA ---#
  st_transform(st_crs(IA_corn)) %>% 
  #--- select HUC units that overlaps with IA ---#
  .[IA_corn, ]
```

Here is the map of HUC units (Figure \@ref(fig:HUC-map)):


```r
tm_shape(HUC_IA) +
  tm_polygons() +
  tm_layout(frame = FALSE, legend.outside = TRUE)
```

<div class="figure">
<img src="SpatialInteractionVectorVector_files/figure-html/HUC-map-1.png" alt="Map of HUC units that intersect with Iowa state boundary" width="672" />
<p class="caption">(\#fig:HUC-map)Map of HUC units that intersect with Iowa state boundary</p>
</div>

Here is a map of Iowa counties with HUC units superimposed on top (Figure \@ref(fig:HUC-county-map)):


```r
tm_shape(IA_corn) +
  tm_polygons(col = "acres") +
tm_shape(HUC_IA) +
  tm_polygons(alpha = 0) +
  tm_layout(frame = FALSE, legend.outside = TRUE)
```

<div class="figure">
<img src="SpatialInteractionVectorVector_files/figure-html/HUC-county-map-1.png" alt="Map of HUC units superimposed on the counties in Iowas" width="672" />
<p class="caption">(\#fig:HUC-county-map)Map of HUC units superimposed on the counties in Iowas</p>
</div>

Spatial joining will produce the following. 


```r
(
HUC_joined <- st_join(HUC_IA, IA_corn)
)
```

```
Simple feature collection with 349 features and 4 fields
geometry type:  POLYGON
dimension:      XY
bbox:           xmin: 154970 ymin: 4346324 xmax: 773307 ymax: 4907737
CRS:            EPSG:26915
First 10 features:
      HUC_CODE county_code year  acres                       geometry
608   10170203         149 2018 226500 POLYGON ((235577 4907515, 2...
608.1 10170203         167 2018 249000 POLYGON ((235577 4907515, 2...
608.2 10170203         193 2018 201000 POLYGON ((235577 4907515, 2...
608.3 10170203         119 2018 184500 POLYGON ((235577 4907515, 2...
621   07020009         063 2018 110500 POLYGON ((408600.2 4880800,...
621.1 07020009         109 2018 304000 POLYGON ((408600.2 4880800,...
621.2 07020009         189 2018 120000 POLYGON ((408600.2 4880800,...
627   10170204         141 2018 167000 POLYGON ((248140.3 4891654,...
627.1 10170204         143 2018 116000 POLYGON ((248140.3 4891654,...
627.2 10170204         167 2018 249000 POLYGON ((248140.3 4891654,...
```

Each of the intersecting HUC-county combinations becomes an observation with its resulting geometry the same as the geometry of the HUC unit. To see this, let's take a look at one of the HUC units.

The HUC unit with `HUC_CODE ==10170203` intersects with four County.


```r
#--- get the HUC unit with `HUC_CODE ==10170203`  ---#
(
temp_HUC_county <- filter(HUC_joined, HUC_CODE == 10170203)
)
```

```
Simple feature collection with 4 features and 4 fields
geometry type:  POLYGON
dimension:      XY
bbox:           xmin: 154970 ymin: 4709628 xmax: 248140.3 ymax: 4907737
CRS:            EPSG:26915
  HUC_CODE county_code year  acres                       geometry
1 10170203         149 2018 226500 POLYGON ((235577 4907515, 2...
2 10170203         167 2018 249000 POLYGON ((235577 4907515, 2...
3 10170203         193 2018 201000 POLYGON ((235577 4907515, 2...
4 10170203         119 2018 184500 POLYGON ((235577 4907515, 2...
```

Figure \@ref(fig:four-county-huc) shows the map of the four observations. 


```r
tm_shape(temp_HUC_county) +
  tm_polygons() +
  tm_layout(frame = FALSE)
```

<div class="figure">
<img src="SpatialInteractionVectorVector_files/figure-html/four-county-huc-1.png" alt="Map of the HUC unit" width="672" />
<p class="caption">(\#fig:four-county-huc)Map of the HUC unit</p>
</div>

So, all of the four observations have identical geometry, which is the geometry of the HUC unit, meaning that the `st_join()` did not leave the information about the nature of the intersection of the HUC unit and the four counties. Again, remember that the default option is `st_intersects()`, which checks whether spatial objects intersect or not, nothing more. If you are just calculating the simple average of corn acres ignoring the degree of spatial overlaps, this is just fine. However, if you would like to calculate area-weighted average, you do not have sufficient information. You will see how to find area-weighted average below.

## Spatial Intersection (transformative join)

Sometimes you face the need to crop spatial objects by polygon boundaries. For example, we found the total length of the railroads **inside** of each county in Demonstration 4 in Chapter \@ref(demo4) by cutting off the parts of the railroads that extend beyond the boundary of counties. Also, we just saw that area-weighted averages cannot be found using `st_join()` because it does not provide information about how much area of each HUC unit is intersecting with each of its intersecting counties. If we can get the geometry of the intersecting part of the HUC unit and the county, then we can calculate its area, which in turn allows us to find area-weighted averages of joined attributes. For these purposes, we can use `sf::st_intersection()`. Below, how `st_intersection()` works for lines-polygons and polygons-polygons intersections. Intersections that involve points using `st_intersection()` is the same as using `st_join()` because points are length-less and area-less (nothing to cut). Thus, it is not discussed here.

### `st_intersection()` {#st_intersection}

While `st_intersects()` returns the indices of intersecting objects, `st_intersection()` returns intersecting spatial objects with the non-intersecting parts of the `sf` objects cut out. Moreover, attribute values of the source `sf` will be merged to its intersecting `sfg` in the target `sf`. We will see how it works for lines-polygons and polygons-polygons cases using the toy examples we used to explain how `st_intersects()` work. Here is the figure of the lines and polygons (Figure \@ref(fig:plot-lines-polygons)):


```r
ggplot() +
  geom_sf(data = polygons, aes(fill = name), alpha = 0.3) +
  scale_fill_discrete(name = "Polygons") +
  geom_sf(data = lines, aes(color = name)) +
  scale_color_discrete(name = "Lines") 
```

<div class="figure">
<img src="SpatialInteractionVectorVector_files/figure-html/plot-lines-polygons-1.png" alt="Visualization of the points, lines, and polygons" width="672" />
<p class="caption">(\#fig:plot-lines-polygons)Visualization of the points, lines, and polygons</p>
</div>

---

**lines and polygons**

The following code gets the intersection of the lines and the polygons.


```r
(
intersections_lp <- st_intersection(lines, polygons) %>% 
  mutate(int_name = paste0(line_name, "-", polygon_name))
)
```

```
Simple feature collection with 3 features and 3 fields
geometry type:  LINESTRING
dimension:      XY
bbox:           xmin: 0 ymin: 0 xmax: 2.5 ymax: 2
CRS:            NA
  line_name polygon_name                              x         int_name
1    line 1    polygon 1        LINESTRING (0 0, 2 0.4) line 1-polygon 1
2    line 2    polygon 1   LINESTRING (1.5 0.5, 2 1.25) line 2-polygon 1
3    line 2    polygon 2 LINESTRING (2.166667 1.5, 2... line 2-polygon 2
```

As you can see in the output, each instance of the intersections of the lines and polygons become an observation (line 1-polygon 1, line 2-polygon 1, and line 2-polygon 2). The part of the lines that did not intersect with any of the polygons is cut out and does not remain in the returned `sf`. To see this, see Figure \@ref(fig:lines-polygons-int) below: 


```r
ggplot() +
  #--- here are all the original polygons  ---#
  geom_sf(data = polygons, aes(fill = polygon_name), alpha = 0.3) +
  #--- here is what is returned after st_intersection ---#
  geom_sf(data = intersections_lp, aes(color = int_name), size = 1.5)
```

<div class="figure">
<img src="SpatialInteractionVectorVector_files/figure-html/lines-polygons-int-1.png" alt="The outcome of the intersections of the lines and polygons" width="672" />
<p class="caption">(\#fig:lines-polygons-int)The outcome of the intersections of the lines and polygons</p>
</div>

This further allows us to calculate the length of the part of the lines that are completely contained in polygons, just like we did in Chapter \@ref(demo4). Note also that the attribute (`polygon_name`) of the source `sf` (the polygons) are merged to their intersecting lines. Therefore, `st_intersection()` is transforming the original geometries while joining attributes (this is why I call this transformative join). 


---

**polygons and polygons**

The following code gets the intersection of polygon 1 and polygon 3 with polygon 2.


```r
(
intersections_pp <- st_intersection(polygons[c(1,3), ], polygons[2, ]) %>% 
  mutate(int_name = paste0(polygon_name, "-", polygon_name.1))
)
```

```
Simple feature collection with 2 features and 3 fields
geometry type:  POLYGON
dimension:      XY
bbox:           xmin: 0.5 ymin: 1.5 xmax: 2.3 ymax: 3.2
CRS:            NA
  polygon_name polygon_name.1                              x
1    polygon 1      polygon 2 POLYGON ((0.5 2, 2 2, 2 1.5...
2    polygon 3      polygon 2 POLYGON ((0.5 2.5, 0.5 3.2,...
             int_name
1 polygon 1-polygon 2
2 polygon 3-polygon 2
```

As you can see in Figure \@ref(fig:polygons-polygons-int), each instance of the intersections of polygons 1 and 3 against polygon 2 becomes an observation (polygon 1-polygon 2 and polygon 3-polygon 2). Just like the lines-polygons case, the non-intersecting part of polygons 1 and 3 are cut out and do not remain in the returned `sf`. We will see later that `st_intersection()` can be used to find area-weighted values from the intersecting polygons with help from `st_area()`.  


```r
ggplot() +
  #--- here are all the original polygons  ---#
  geom_sf(data = polygons, aes(fill = polygon_name), alpha = 0.3) +
  #--- here is what is returned after st_intersection ---#
  geom_sf(data = intersections_pp, aes(fill = int_name))
```

<div class="figure">
<img src="SpatialInteractionVectorVector_files/figure-html/polygons-polygons-int-1.png" alt="The outcome of the intersections of polygon 2 and polygons 1 and 3" width="672" />
<p class="caption">(\#fig:polygons-polygons-int)The outcome of the intersections of polygon 2 and polygons 1 and 3</p>
</div>

### Area-weighted average 

Let's now get back to the example of HUC units and county-level corn acres data. We would like to find area-weighted average of corn acres instead of the simple average of corn acres.

Using `st_intersection()`, for each of the HUC polygons, we find the intersecting counties, and then divide it into parts based on the boundary of the intersecting polygons. 


```r
(
HUC_intersections <- st_intersection(HUC_IA, IA_corn) %>% 
  mutate(huc_county = paste0(HUC_CODE, "-", county_code))
)
```

```
Simple feature collection with 349 features and 5 fields
geometry type:  GEOMETRY
dimension:      XY
bbox:           xmin: 203228.6 ymin: 4470941 xmax: 736832.9 ymax: 4822687
CRS:            EPSG:26915
First 10 features:
   HUC_CODE county_code year  acres                       geometry   huc_county
1  07080207         083 2018 183500 POLYGON ((482916.4 4711686,... 07080207-083
2  07080205         083 2018 183500 POLYGON ((499779.4 4696836,... 07080205-083
3  07080105         083 2018 183500 POLYGON ((461846.1 4683469,... 07080105-083
4  10170204         141 2018 167000 POLYGON ((269432.3 4793329,... 10170204-141
5  10230003         141 2018 167000 POLYGON ((271607.5 4754542,... 10230003-141
6  10230002         141 2018 167000 POLYGON ((267630 4790936, 2... 10230002-141
7  07100003         081 2018 184500 POLYGON ((436142.9 4789503,... 07100003-081
8  07080203         081 2018 184500 MULTIPOLYGON (((459473.3 47... 07080203-081
9  07080207         081 2018 184500 POLYGON ((429601.9 4779600,... 07080207-081
10 07100005         081 2018 184500 POLYGON ((420999.1 4772191,... 07100005-081
```

The key difference from the `st_join()` example is that each observation of the returned data is a unique HUC-county intersection. Figure \@ref(fig:inter-ex) below is a map of all the intersections of the HUC unit with `HUC_CODE ==10170203` and the four intersecting counties. 


```r
tm_shape(filter(HUC_intersections, HUC_CODE == "10170203")) + 
  tm_polygons(col = "huc_county") +
  tm_layout(frame = FALSE)
```

<div class="figure">
<img src="SpatialInteractionVectorVector_files/figure-html/inter-ex-1.png" alt="Intersections of a HUC unit and Iowa counties" width="672" />
<p class="caption">(\#fig:inter-ex)Intersections of a HUC unit and Iowa counties</p>
</div>

Note also that the attributes of county data are joined as you can see `acres` in the output above. As I said earlier, `st_intersection()` is a spatial kind of spatial join where the resulting observations are the intersections of the target and source `sf` objects. 

In order to find the area-weighted average of corn acres, you can use `st_area()` first to calculate the area of the intersections, and then find the area-weighted average as follows:


```r
(
HUC_aw_acres <- HUC_intersections %>% 
  #--- get area ---#
  mutate(area = as.numeric(st_area(.))) %>% 
  #--- get area-weight by HUC unit ---#
  group_by(HUC_CODE) %>% 
  mutate(weight = area / sum(area)) %>% 
  #--- calculate area-weighted corn acreage by HUC unit ---#
  summarize(aw_acres = sum(weight * acres))
)
```

```
Simple feature collection with 55 features and 2 fields
geometry type:  GEOMETRY
dimension:      XY
bbox:           xmin: 203228.6 ymin: 4470941 xmax: 736832.9 ymax: 4822687
CRS:            EPSG:26915
# A tibble: 55 x 3
   HUC_CODE aw_acres                                                    geometry
   <chr>       <dbl>                                              <GEOMETRY [m]>
 1 07020009  251140. POLYGON ((421317.4 4797758, 421179.2 4797632, 421079.3 479…
 2 07040008  165000  POLYGON ((602943.6 4817205, 602935.1 4817167, 602875.1 481…
 3 07060001  105224. MULTIPOLYGON (((631611.9 4817707, 631609.2 4817706, 631519…
 4 07060002  140192. POLYGON ((593286.7 4817067, 593403.8 4817047, 593583.8 481…
 5 07060003  149000  MULTIPOLYGON (((646504.9 4762382, 646518 4762383, 646554.8…
 6 07060004  162121. POLYGON ((653200.4 4718423, 652967.7 4718504, 652457.7 471…
 7 07060005  142428. POLYGON ((735347.8 4642385, 734779.1 4642296, 734459 46422…
 8 07060006  159628. POLYGON ((692755.3 4694862, 692788.3 4694758, 692788.3 469…
 9 07080101  115574. POLYGON ((667472 4558778, 667391.8 4558691, 667221.7 45585…
10 07080102  160017. POLYGON ((635032.8 4675786, 635247.8 4675644, 635367.8 467…
# … with 45 more rows
```


<!--chapter:end:SpatialInteractionVectorVector.Rmd-->

# Raster Data Handling with `terra` {#raster-basics}







## Before you start {-}

In this chapter, we will learn how to use the `terra` package to handle raster data. The `raster` package has been (and I must say still is) **THE** package for raster data handling. However, we are in the period of transitioning from the `raster` package to the `terra` package. The `terra` package has been under active development to replace the `raster` package, and its first beta version^[[github page](https://github.com/rspatial/terra)] was just released on CRAN on March 20, 2020. `terra` is written in C++ and thus is faster than the `raster` package in many raster data operations. The `raster` and `terra` packages share the same function name for most of the raster operations. Therefore, learning the `terra` package is almost the same as learning the `raster` package. Key differences will be discussed and will become clear later.

For economists, raster data extraction for vector data will be by far the most common use case of raster data and also the most time-consuming part of the whole raster data handling experience. Therefore, we will introduce only the essential knowledge of raster data operation required to effectively implement the task of extracting values, which will be covered extensively in Chapter \@ref(int-RV). For example, we do not cover raster arithmetic, focal operations, or aggregation. Those who are interested in a fuller treatment of the `terra` or `raster` package are referred to [Spatial Data Science with R and “terra”](https://rspatial.org/terra/spatial/index.html) or Chapters 3, 4, and 5 of [Geocomputation with R](https://geocompr.robinlovelace.net/), respectively.

We still learn the raster object classes defined by the `raster` package and how to switch between the `raster` and `terra` object classes. This is because other useful packages for us economists were written to work with the `raster` object classes and have still not been adapted to support `terra` object classes at the moment. In particular, `exactextractr` is critical for economists who regularly use large spatially fine raster datasets with many temporal dimensions because of its speed advantage over the `terra` package. `terra::extract()` is much faster than `raster::extract()`, which is unbearably slow for large datasets. Unfortunately, `terra::extract()` is still much slower than the extraction function provided by `exactextractr` packages for large datasets^[CDL data is a good example]. Since `exactextractr` works only with objects defined by the `raster` package, you need to convert a `terra` object to a `raster` object if you would like to take advantage of the function. This also means that we need to learn the difference in raster object classes between the two packages. This problem should be resolved in a matter of a year, and most of the spatial packages will add support for `terra`.

### `stars` package {-}

Finally, another package you might want to keep an eye on for raster (and vector data) handling is the `stars` package. It provides a more consistent way of handling spatiotemporal data than the `raster` and `terra` packages. We do not cover this package in this chapter because it is not clear (at least to me) how much benefit this package brings. However, this might change quickly. For those who are interested in exploring this package, [this](https://r-spatial.github.io/stars/index.html) is the best resource. A few vignettes are available from the "Articles" tab. 

### Direction for replication {-}

**Datasets**
No external datasets are necessary for this Chapter.

**Packages**

Run the following code to install or load (if already installed) the `pacman` package, and then install or load (if already installed) the listed package inside the `pacman::p_load()` function.


```r
if (!require("pacman")) install.packages("pacman")
pacman::p_load(
  terra, # handle raster data
  raster, # handle raster data
  cdlTools # download CDL data
)  
```

## Raster data object classes  

### `raster` package: `RasterLayer`, `RasterStack`, and `RasterBrick`

Let's start with taking a look at raster data. We will download CDL data for Iowa in 2015. 


```r
library(cdlTools)

#--- download the CDL data for Iowa in 2015 ---#
IA_cdl_2015 <- getCDL("Iowa", 2015)$IA2015

#--- take a look ---#
IA_cdl_2015
```

```
class      : RasterLayer 
dimensions : 11671, 17795, 207685445  (nrow, ncol, ncell)
resolution : 30, 30  (x, y)
extent     : -52095, 481755, 1938165, 2288295  (xmin, xmax, ymin, ymax)
crs        : +proj=aea +lat_1=29.5 +lat_2=45.5 +lat_0=23 +lon_0=-96 +x_0=0 +y_0=0 +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +units=m +no_defs 
source     : /private/var/folders/t4/5gnqprbn38nftyxkyk5hdwmd8hnypy/T/Rtmp5g1PCO/CDL_2015_19.tif 
names      : CDL_2015_19 
values     : 0, 255  (min, max)
```

Evaluating the imported raster object provides you with information about the raster data, such as dimensions (number of cells, number of columns, number of cells), spatial resolution (30 meter by 30 meter for this raster data), extent, CRS and the minimum and maximum values recorded in this raster layer. The class of the downloaded data is `RasterLayer`, which is a raster data class defined by the `raster` package.^[This is what I meant by the `raster` being THE package for raster data handling. The default object class for many raster-related packages is a `raster` object class, instead of a `terra` object class.] A `RasterLayer` consists of only one layer, meaning that only a single variable is associated with the cells (here it is land use category code in integer). Among these spatial characteristics, you often need to extract the CRS of a raster object before you interact it with vector data^[e.g., extracting values from a raster layer to vector data, or cropping a raster layer to the spatial extent of vector data.], which can be done using `projection()`: 


```r
projection(IA_cdl_2015)
```

```
[1] "+proj=aea +lat_1=29.5 +lat_2=45.5 +lat_0=23 +lon_0=-96 +x_0=0 +y_0=0 +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +units=m +no_defs"
```

---

You can stack multiple raster layers of the **same spatial resolution and extent** to create a `RasterStack` using `raster::stack()`. Often times, processing a multi-layer object has computational advantages over processing multiple single-layer one by one^[You will see this in Chapter \@ref(int-RV) where we learn how to extract values from a raster layer for a vector data.]. 

To create a `RasterStack` and `RasterBrick`, let's download the CDL data for IA in 2016 and stack it with the 2015 data.


```r
#--- download the CDL data for Iowa in 2016 ---#
IA_cdl_2016 <- getCDL("Iowa", 2016)$IA2016 

#--- stack the two ---#
IA_cdl_stack <- stack(IA_cdl_2015, IA_cdl_2016)

#--- take a look ---#
IA_cdl_stack
```

```
class      : RasterStack 
dimensions : 11671, 17795, 207685445, 2  (nrow, ncol, ncell, nlayers)
resolution : 30, 30  (x, y)
extent     : -52095, 481755, 1938165, 2288295  (xmin, xmax, ymin, ymax)
crs        : +proj=aea +lat_1=29.5 +lat_2=45.5 +lat_0=23 +lon_0=-96 +x_0=0 +y_0=0 +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +units=m +no_defs 
names      : CDL_2015_19, CDL_2016_19 
min values :           0,           0 
max values :         255,         255 
```

`IA_cdl_stack` is of class `RasterStack`, and it has two layers of variables: CDL for 2015 and 2016. You can make it a `RasterBrick` using `raster::brick()`:


```r
#--- stack the two ---#
IA_cdl_brick <- brick(IA_cdl_stack)

#--- or this works as well ---#
# IA_cdl_brick <- brick(IA_cdl_2015, IA_cdl_2016)

#--- take a look ---#
IA_cdl_brick
```

```
class      : RasterBrick 
dimensions : 11671, 17795, 207685445, 2  (nrow, ncol, ncell, nlayers)
resolution : 30, 30  (x, y)
extent     : -52095, 481755, 1938165, 2288295  (xmin, xmax, ymin, ymax)
crs        : +proj=aea +lat_1=29.5 +lat_2=45.5 +lat_0=23 +lon_0=-96 +x_0=0 +y_0=0 +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +units=m +no_defs 
source     : /private/var/folders/t4/5gnqprbn38nftyxkyk5hdwmd8hnypy/T/Rtmp5g1PCO/raster/r_tmp_2020-05-27_205015_64951_93006.grd 
names      : CDL_2015_19, CDL_2016_19 
min values :           0,           0 
max values :         229,         241 
```

You probably noticed that it took some time to create the `RasterBrick` object^[Read [here](https://geocompr.robinlovelace.net/spatial-class.html#raster-classes) for the subtle difference between `RasterStack` and `RasterBrick`]. While spatial operations on `RasterBrick` are supposedly faster than `RasterStack`, the time to create a `RasterBrick` object itself is often long enough to kill the speed advantage entirely^[We will see this in Chapter , where we compare the speed of data extraction from `RasterStack` and `RasterBrick` objects.]. Often, the three raster object types are collectively referred to as `Raster`$^*$ objects for shorthand in the documentation of the `raster` and other related packages.

### `terra` package: `SpatRaster`

`terra` package has only one object class for raster data, `SpatRaster` and no distinctions between one-layer and multi-layer rasters are necessary. Let's first convert a `RasterLayer` to a `SpatRaster` using `terra::rast()` function.


```r
#--- convert to a SpatRaster ---#
IA_cdl_2015_sr <- rast(IA_cdl_2015)

#--- take a look ---#
IA_cdl_2015_sr
```

```
class       : SpatRaster 
dimensions  : 11671, 17795, 1  (nrow, ncol, nlyr)
resolution  : 30, 30  (x, y)
extent      : -52095, 481755, 1938165, 2288295  (xmin, xmax, ymin, ymax)
coord. ref. : +proj=aea +lat_1=29.5 +lat_2=45.5 +lat_0=23 +lon_0=-96 +x_0=0 +y_0=0 +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +units=m +no_defs  
data source : /private/var/folders/t4/5gnqprbn38nftyxkyk5hdwmd8hnypy/T/Rtmp5g1PCO/CDL_2015_19.tif 
names       : Layer_1 
```

You can see that the number of layers (`nlyr`) is $1$ because the original object is a `RasterLayer`, which by definition has only one layer. Now, let's convert a `RasterStack` to a `SpatRaster` using `terra::rast()`.  


```r
#--- convert to a SpatRaster ---#
IA_cdl_stack_sr <- rast(IA_cdl_stack)

#--- take a look ---#
IA_cdl_stack_sr
```

```
class       : SpatRaster 
dimensions  : 11671, 17795, 2  (nrow, ncol, nlyr)
resolution  : 30, 30  (x, y)
extent      : -52095, 481755, 1938165, 2288295  (xmin, xmax, ymin, ymax)
coord. ref. : +proj=aea +lat_1=29.5 +lat_2=45.5 +lat_0=23 +lon_0=-96 +x_0=0 +y_0=0 +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +units=m +no_defs  
data sources: /private/var/folders/t4/5gnqprbn38nftyxkyk5hdwmd8hnypy/T/Rtmp5g1PCO/CDL_2015_19.tif  
              /private/var/folders/t4/5gnqprbn38nftyxkyk5hdwmd8hnypy/T/Rtmp5g1PCO/CDL_2016_19.tif  
names       : Layer_1, Layer_1 
```

Again, it is a `SpatRaster`, and you now see that the number of layers is 2. We just confirmed that `terra` has only one class for raster data whether it is single-layer or multiple-layer ones.

Instead of `projection()`, you use `crs()` to extract the CRS.


```r
crs(IA_cdl_2015_sr)
```

```
[1] "+proj=aea +lat_1=29.5 +lat_2=45.5 +lat_0=23 +lon_0=-96 +x_0=0 +y_0=0 +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +units=m +no_defs "
```

### Converting a `SpatRaster` object to a `Raster`$^*$ object.

You can convert a `SpatRaster` object to a `Raster`$^*$ object using `raster()`, `stack()`, and `brick()`. Keep in mind that if you use `rater()` even though `SpatRaster` has multiple layers, the resulting `RasterLayer` object has only the first of the multiple layers. 


```r
#--- RasterLayer (only 1st layer) ---#
IA_cdl_stack_sr %>% raster()
```

```
class      : RasterLayer 
dimensions : 11671, 17795, 207685445  (nrow, ncol, ncell)
resolution : 30, 30  (x, y)
extent     : -52095, 481755, 1938165, 2288295  (xmin, xmax, ymin, ymax)
crs        : +proj=aea +lat_1=29.5 +lat_2=45.5 +lat_0=23 +lon_0=-96 +x_0=0 +y_0=0 +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +units=m +no_defs 
source     : /private/var/folders/t4/5gnqprbn38nftyxkyk5hdwmd8hnypy/T/Rtmp5g1PCO/CDL_2015_19.tif 
names      : Layer_1 
values     : 0, 255  (min, max)
```

```r
#--- RasterLayer ---#
IA_cdl_stack_sr %>% stack()
```

```
class      : RasterStack 
dimensions : 11671, 17795, 207685445, 2  (nrow, ncol, ncell, nlayers)
resolution : 30, 30  (x, y)
extent     : -52095, 481755, 1938165, 2288295  (xmin, xmax, ymin, ymax)
crs        : +proj=aea +lat_1=29.5 +lat_2=45.5 +lat_0=23 +lon_0=-96 +x_0=0 +y_0=0 +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +units=m +no_defs 
names      : CDL_2015_19, CDL_2016_19 
min values :           0,           0 
max values :         255,         255 
```

```r
#--- RasterLayer (this takes some time) ---#
IA_cdl_stack_sr %>% brick()
```

```
class      : RasterBrick 
dimensions : 11671, 17795, 207685445, 2  (nrow, ncol, ncell, nlayers)
resolution : 30, 30  (x, y)
extent     : -52095, 481755, 1938165, 2288295  (xmin, xmax, ymin, ymax)
crs        : +proj=aea +lat_1=29.5 +lat_2=45.5 +lat_0=23 +lon_0=-96 +x_0=0 +y_0=0 +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +units=m +no_defs 
source     : /private/var/folders/t4/5gnqprbn38nftyxkyk5hdwmd8hnypy/T/Rtmp5g1PCO/raster/r_tmp_2020-05-27_205102_64951_75945.grd 
names      : CDL_2015_19, CDL_2016_19 
min values :           0,           0 
max values :         229,         241 
```

### Vector data in the `terra` package

`terra` package has its own class for vector data, called `SpatVector`. While we do not use any of the vector data functionality provided by the `terra` package, we learn how to convert an `sf` object to `SpatVector` because `terra` functions do not support `sf` as of now (this will likely be resolved very soon). We will see some use cases of this conversion in the next chapter when we learn raster value extractions for vector data using `terra::extract()`. 

As an example, let's use Illinois county border data. 


```r
library(maps)
#--- Illinois county boundary ---#
(
IL_county <- st_as_sf(map("county", "illinois", plot = FALSE, fill = TRUE))
)
```

```
Simple feature collection with 102 features and 1 field
geometry type:  MULTIPOLYGON
dimension:      XY
bbox:           xmin: -91.50136 ymin: 37.00161 xmax: -87.49638 ymax: 42.50774
CRS:            EPSG:4326
First 10 features:
                   ID                           geom
1      illinois,adams MULTIPOLYGON (((-91.49563 4...
2  illinois,alexander MULTIPOLYGON (((-89.21526 3...
3       illinois,bond MULTIPOLYGON (((-89.27828 3...
4      illinois,boone MULTIPOLYGON (((-88.94024 4...
5      illinois,brown MULTIPOLYGON (((-90.91121 3...
6     illinois,bureau MULTIPOLYGON (((-89.63351 4...
7    illinois,calhoun MULTIPOLYGON (((-90.93414 3...
8    illinois,carroll MULTIPOLYGON (((-89.91999 4...
9       illinois,cass MULTIPOLYGON (((-90.51014 3...
10 illinois,champaign MULTIPOLYGON (((-88.46468 4...
```

You cannot convert an `sf` object directly to `SpatVector`. You first need to turn an `sf` into a spatial object class supported by the `sp` package, and then turn that into a `SpatVector` object using `terra::vect()`.


```r
IL_county_sv <-	as(IL_county, "Spatial") %>% # to SpatialPolygonsDataFrame  
	#--- to SpatVectgor ---#
	vect()
```

You just need to put the name of your `sf` object in place of `IL_county`. You do not have to understand what `SpatialPolygonsDataFrame` is if you are not familiar with the `sp` package. It is just an intermediate object that you do not really need to understand. Indeed, when the next version of the `terra` packages comes out (The version currently available on CRAN is `0.6-9` at the time of writing), `vect()` will be able to convert a `SpatVector` object to an `sf` object directly without the intermediate step like this (see [here](https://github.com/rspatial/terra/issues/38)):


```r
#--- NOT RUN ---#	
IL_county_sv <- vect(IL_county)
```

To install the development version, visit [here](https://github.com/rspatial/terra) and follow the direction.


## Read and write a raster data file  

Sometimes we can download raster data as we saw in Section 3.1. But, most of the time you need to read raster data stored in a file. Raster data files come in numerous formats. For example, PRPISM comes in the Band interleaved by line (BIL) format, some of the Daymet data comes in netCDF format. Other popular formats include GeoTiff, SAGA, ENVI, and many others. 

### Read raster file(s)

You use `terra::rast()` to read raster data of many common formats, and it should be almost always the case that the raster data you got can be read using this function. Here, we read a GeoTiff file (a file with .tif extension):


```r
(
IA_cdl_2015_sr <- rast("./Data/IA_cdl_2015.tif") 
)
```

```
class       : SpatRaster 
dimensions  : 11671, 17795, 1  (nrow, ncol, nlyr)
resolution  : 30, 30  (x, y)
extent      : -52095, 481755, 1938165, 2288295  (xmin, xmax, ymin, ymax)
coord. ref. : +proj=aea +lat_1=29.5 +lat_2=45.5 +lat_0=23 +lon_0=-96 +x_0=0 +y_0=0 +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +units=m +no_defs  
data source : ./Data/IA_cdl_2015.tif 
names       : IA_cdl_2015 
```

One important thing to note here is that the cell values of the raster data are actually not in memory when you "read" raster data from a file. 
You basically just established a connection to the file. This helps to reduce the memory footprint of raster data handling. You can check this by `raster::inMemory()` function for `Raster`$^*$ objects, but the same function has not been implemented for `terra` yet. 

You can read multiple single-layer raster datasets of the same spatial extent and resolution at the same time to have a multi-layer `SpatRaster` object. Here, we import two single-layer raster datasets (IA_cdl_2015.tif and IA_cdl_2016.tif) to create a two-layer `SpatRaster` object.


```r
#--- the list of path to the files ---#
files_list <- c("./Data/IA_cdl_2015.tif", "./Data/IA_cdl_2016.tif")

#--- read the two at the same time ---#
(
multi_layer_sr <- rast(files_list) 
)
```

```
class       : SpatRaster 
dimensions  : 11671, 17795, 2  (nrow, ncol, nlyr)
resolution  : 30, 30  (x, y)
extent      : -52095, 481755, 1938165, 2288295  (xmin, xmax, ymin, ymax)
coord. ref. : +proj=aea +lat_1=29.5 +lat_2=45.5 +lat_0=23 +lon_0=-96 +x_0=0 +y_0=0 +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +units=m +no_defs  
data sources: ./Data/IA_cdl_2015.tif  
              ./Data/IA_cdl_2016.tif  
names       : IA_cdl_2015, Layer_1 
min values  :          ? ,       0 
max values  :          ? ,     241 
```

Of course, this only works because the two datasets have the identical spatial extent and resolution. There are, however, no restrictions on what variable each of the raster layers represent. For example, you can combine PRISM temperature and precipitation raster layers.

### Write raster files

You can write a `SpatRaster` object using `terra::writeRaster()`.


```r
terra::writeRaster(IA_cdl_2015_sr, "./Data/IA_cdl_stack.tif", format = "GTiff", overwrite = TRUE)
```

The above code saves `IA_cdl_2015_sr` (a `SpatRaster` object) as a GeoTiff file.^[There are many other alternative formats (see [here](https://www.rdocumentation.org/packages/raster/versions/3.0-12/topics/writeRaster))] The format option can be dropped as `writeRaster()` infers the format from the extension of the file name. The `overwrite = TRUE` option is necessary if a file with the same name already exists and you are overwriting it. This is one of the many areas `terra` is better than `raster`. `raster::writeRaster()` can be frustratingly slow for a large `Raster`$^*$ object. `terra::writeRaster()` is much faster.

You can also save a multi-layer `SpatRster` object just like you save a single-layer `SpatRster` object. 


```r
terra::writeRaster(IA_cdl_stack_sr, "./Data/IA_cdl_stack.tif", format = "GTiff", overwrite = TRUE)
```

The saved file is a multi-band raster datasets. So, if you have many raster files of the same spatial extent and resolution, you can "stack" them on R and then export it to a single multi-band raster datasets, which cleans up your data folder.
 
## Access values and quick plotting

Sometime, it is nice to be able to see the data values in a raster dataset or visualize the data for various kinds of checks. You can access the values stored in a `SpatRaster` object using `values()` function:


```r
#--- terra::values ---#
values_from_rs <- values(IA_cdl_stack_sr) 

#--- take a look ---#
head(values_from_rs)
```

```
     Layer_1 Layer_1
[1,]       0       0
[2,]       0       0
[3,]       0       0
[4,]       0       0
[5,]       0       0
[6,]       0       0
```

The returned values come in a matrix form of two columns because we are getting values from a two-layer `SpatRaster` object (one column for each layer). 

To have a quick visualization of the data values of `SpatRaster` objects, you can simply use `plot()`:


```r
plot(IA_cdl_stack_sr)
```

<img src="RasterDataBasics_files/figure-html/plot_stack-1.png" width="672" />

<!-- Instead of getting all the values, you could get a portion of them by using `getValuesBlock()` by specifying the region for which you would like to get values. It is used extensively in `exact_extract()` function, which we show as one of the fastest ways to extract values. However, if you are finding yourself using `getValuesBlock()`, it is very much likely that you are wasting your time by not using a faster alternative. See Chapter X for further discussion of fast value extraction.   -->


<!-- [RasterOption](https://www.gis-blog.com/increasing-the-speed-of-raster-processing-with-r-part-13/) -->

<!-- ### Speed comparison

Here, we compare the speed of writing raster data using  


```r
#--- terra::writeRaster (faster) ---#
tic()
terra::writeRaster(IA_cdl_2015_sr, "./Data/IA_cdl_stack.tif", format = "GTiff", overwrite = TRUE)
tic()

#--- raster::writeRaster (slow) ---#
tic()
raster::writeRaster(IA_cdl_stack, "./Data/IA_cdl_stack.tif", format = "GTiff", overwrite = TRUE)
toc()
```

You can save a `Raster`$^*$ object using `terra::writeaRaster()`, but you do not get any speed advantage.


```r
#--- terra::writeRaster with RasterStack (no speed advantage) ---#
tic()
terra::writeRaster(IA_cdl_stack, "./Data/IA_cdl_stack.tif", format = "GTiff", overwrite = TRUE)
toc() 
``` -->
```

<!--chapter:end:RasterDataBasics.Rmd-->

# Spatial Interactions of Vector and Raster Data {#int-RV}










## Before you start {-}

In this chapter we learn the spatial interactions of a vector and raster dataset. We first look at how to crop (spatially subset) a raster dataset based on the geographic extent of a vector dataset. We then cover how to extract values from raster data for points and polygons. To be precise, here is what we mean by raster data extraction and what it does for points and polygons data:

+ **Points**: For each of the points, find which raster cell it is located within, and assign the value of the cell to the point.  
 
+ **Polygons**: For each of the polygons, identify all the raster cells that intersect with the polygon, and assign a vector of the cell values to the polygon

This is probably the most important operation economists run on raster datasets. 

You will see conversions between `Raster`$^*$ (`raster` package) objects and `SpatRaster` object (`terra` package) because of the incompatibility of object classes across the key packages. I believe that these hassles will go away soon when they start supporting each other.  

### Direction for replication {-}

**Datasets**

All the datasets that you need to import are available [here](https://www.dropbox.com/sh/ayw6rz1wg0fmz2v/AADgKprG9P5xRBjvWE4eRSN2a?dl=0). In this chapter, the path to files is set relative to my own working directory (which is hidden). To run the codes without having to mess with paths to the files, follow these steps:

+ set a folder (any folder) as the working directory using `setwd()`  
+ create a folder called "Data" inside the folder designated as the working directory (if you have created a "Data" folder previously, skip this step)
+ download the pertinent datasets from [here](https://www.dropbox.com/sh/ayw6rz1wg0fmz2v/AADgKprG9P5xRBjvWE4eRSN2a?dl=0) and put them in the "Data" folder

**Packages**

Run the following code to install or load (if already installed) the `pacman` package, and then install or load (if already installed) the listed package inside the `pacman::p_load()` function.


```r
if (!require("pacman")) install.packages("pacman")
pacman::p_load(
  terra, # handle raster data
  raster, # handle raster data
  exactextractr, # fast extractions
  sf, # vector data operations
  dplyr, # data wrangling
  data.table, # data wrangling
  prism # download PRISM data
)  
```

## Cropping (Spatial subsetting) to the Area of Interest {#raster-crop}

Here we use PRISM maximum temperature (tmax) data as a raster dataset and Kansas county boundaries as a vector dataset. 

Let's download the tmax data for July 1, 2018 (Figure \@ref(fig:prism-tmax-map)).


```r
#--- set the path to the folder to which you save the downloaded PRISM data ---#
# This code sets the current working directory as the designated folder
options(prism.path = "./Data")

#--- download PRISM precipitation data ---#
get_prism_dailys(
  type = "tmax", 
  date = "2018-07-01", 
  keepZip = FALSE 
)

#--- the file name of the PRISM data just downloaded ---#
prism_file <- "./Data/PRISM_tmax_stable_4kmD2_20180701_bil/PRISM_tmax_stable_4kmD2_20180701_bil.bil"

#--- read in the prism data ---#
prism_tmax_0701 <- rast(prism_file) 
```

<div class="figure">
<img src="SpatialInteractionVectorRaster_files/figure-html/prism-tmax-map-1.png" alt="Map of PRISM tmax data on July 1, 2018" width="672" />
<p class="caption">(\#fig:prism-tmax-map)Map of PRISM tmax data on July 1, 2018</p>
</div>

We now get Kansas county data from the `maps` package (Figure \@ref(fig:ks-county-map)) as `sf`. We then convert it to a `SpatVector` object. Remember that the `terra` packages does not support an `sf` object yet. So, an `sf` object needs to be converted to a `SpatVector` object before we use any functions from the `terra` packages that involve vector data including `terra::crop()` and `terra::extract()` for raster data cropping and extraction, respectively.^[See Chapter \@ref(raster-basics) to learn what `SpatVector` is and how to convert `sf` to `SpatRaster`.] 


```r
library(maps)

#--- Kansas boundary (sf) ---#
KS_county_sf <- st_as_sf(maps::map("county", "kansas", plot = FALSE, fill = TRUE)) %>% 
  #--- transform using the CRS of the PRISM tmax data  ---#
  st_transform(crs(prism_tmax_0701)) 

#--- Kansas boundary (SpatVector) ---#
KS_county_sv <- KS_county_sf %>% 
  #--- convert to a SpatVector object ---#
  as(., "Spatial") %>% vect()
```

<div class="figure">
<img src="SpatialInteractionVectorRaster_files/figure-html/ks-county-map-1.png" alt="Kansas county boundaries" width="672" />
<p class="caption">(\#fig:ks-county-map)Kansas county boundaries</p>
</div>

---

Sometimes, it is convenient to crop a raster layer to the specific area of interest so that you do not have to carry around unnecessary parts of the raster layer. Moreover, it takes less time to extract values from a raster layer when the size of the raster layer is smaller. You can crop a raster layer by using `terra::crop()`. It works like this:


```r
#--- syntax (NOT RUN) ---#
crop(raster object, geographic extent)
```

To find the geographic extent of a vector data, you can use `terra::ext()` (it is `raster::extent()` if you are using the `raster` package). 


```r
KS_extent <- terra::ext(KS_county_sv)
```

As you can see, it consists of four points. Four pairs of these values (xmin, ymin), (xmin, ymax), (xmax, ymin), and (xmax, ymax) form a rectangle that encompasses the Kansas state boundary. We will crop the PRISM raster layer to the rectangle:


```r
#--- crop the entire PRISM to its KS portion---#
prism_tmax_0701_KS_sr <- terra::crop(prism_tmax_0701, KS_extent)
```

The figure below (Figure \@ref(fig:prism-ks-viz)) shows the PRISM tmax raster data cropped to the geographic extent of Kansas. Notice that the cropped raster layer extends beyond the outer boundary of Kansas state boundary (it is a bit hard to see, but look at the upper right corner).  

<div class="figure">
<img src="SpatialInteractionVectorRaster_files/figure-html/prism-ks-viz-1.png" alt="PRISM tmax raster data cropped to the geographic extent of Kansas" width="672" />
<p class="caption">(\#fig:prism-ks-viz)PRISM tmax raster data cropped to the geographic extent of Kansas</p>
</div>

<!-- 
You can mask the values (set values to NA) outside of the vectors data.


```r
#--- syntax ---#
mask(raster object, sf object)

#--- example ---#
masked_prism_IL <- mask(prism_for_IL, IL_county)
```




```r
tm_shape(masked_prism_IL) +
  tm_raster() +
tm_shape(IL_county) +
  tm_polygons(alpha = 0)
```

 -->


## Extracting Values from Raster Layers for Vector Data 

In this section, we will learn how to extract information from raster layers for spatial units represented as vector data (points and polygons). For the illustrations in this section, we use the following datasets:

+ Raster: PRISM tmax data cropped to Kansas state border for 07/01/2018 (obtained in \@ref(raster-crop)) and 07/02/2018 (downloaded below)
+ Polygons: Kansas county boundaries (obtained in \@ref(raster-crop))
+ Points: Irrigation wells in Kansas (imported below) 

**PRISM tmax data for 07/02/2018**


```r
#--- download PRISM precipitation data ---#
get_prism_dailys(
  type = "tmax", 
  date = "2018-07-02", 
  keepZip = FALSE 
)

#--- the file name of the PRISM data just downloaded ---#
prism_file <- "Data/PRISM_tmax_stable_4kmD2_20180702_bil/PRISM_tmax_stable_4kmD2_20180702_bil.bil"

#--- read in the prism data and crop it to Kansas state border ---#
prism_tmax_0702_KS_sr <- rast(prism_file) %>% 
  terra::crop(KS_extent)
```

**Irrigation wells in Kansas:**


```r
#--- read in the KS points data ---#
(
KS_wells <- readRDS("./Data/Chap_5_wells_KS.rds") 
)
```

```
Simple feature collection with 37647 features and 1 field
geometry type:  POINT
dimension:      XY
bbox:           xmin: -102.0495 ymin: 36.99552 xmax: -94.62089 ymax: 40.00199
CRS:            EPSG:4269
First 10 features:
   well_id                   geometry
1        1 POINT (-100.4423 37.52046)
2        3 POINT (-100.7118 39.91526)
3        5 POINT (-99.15168 38.48849)
4        7 POINT (-101.8995 38.78077)
5        8  POINT (-100.7122 38.0731)
6        9 POINT (-97.70265 39.04055)
7       11 POINT (-101.7114 39.55035)
8       12 POINT (-95.97031 39.16121)
9       15 POINT (-98.30759 38.26787)
10      17 POINT (-100.2785 37.71539)
```

---

Here is how the wells are spatially distributed over the PRISM grids and Kansas county borders (Figure \@ref(fig:tmax-prism-wells)):

<div class="figure">
<img src="SpatialInteractionVectorRaster_files/figure-html/tmax-prism-wells-1.png" alt="Map of Kansas county borders, irrigation wells, and PRISM tmax" width="672" />
<p class="caption">(\#fig:tmax-prism-wells)Map of Kansas county borders, irrigation wells, and PRISM tmax</p>
</div>


### Points 

You can extract values from raster layers to points using `terra::extract()`. `terra::extract()` finds which raster cell each of the points is located within and assigns the value of the cell to the point. One complication that we have to deal with at the moment is the fact that `terra` does not support `sf` yet. However, `terra::extract()` accepts a longitude and latitude matrix. Therefore, the following works:^[I believe this issue will be resolved soon and you can just supply an `sf` object instead of coordinates.]  


```r
#--- syntax (NOT RUN) ---#
terra::extract(raster object, st_coordinates(sf object)) 
```

Let's extract tmax values from the PRISM tmax layer to the irrigation wells:


```r
#--- extract tmax values ---#
tmax_from_prism <- terra::extract(prism_tmax_0701_KS_sr, st_coordinates(KS_wells))

#--- take a look ---#
head(tmax_from_prism)
```

```
     PRISM_tmax_stable_4kmD2_20180701_bil
[1,]                               34.241
[2,]                               29.288
[3,]                               32.585
[4,]                               30.104
[5,]                               34.232
[6,]                               35.168
```

`terra::extract()` returns the extracted values as a vector when the raster object is single-layer raster data. Since the order of the values are consistent with the order of the observations in the points data, you can simply assign the vector as a new variable of the points data as follows:


```r
KS_wells$tmax_07_01 <- tmax_from_prism
```

Extracting values from a multi-layer `SpatRaster` works the same way. Here, we combine `prism_tmax_0701_KS_sr` and `prism_tmax_0703_KS_sr` to create a multi-layer `SpatRaster`.


```r
#--- create a multi-layer SpatRaster ---#
prism_tmax_stack <- c(prism_tmax_0701_KS_sr, prism_tmax_0702_KS_sr)

#--- extract tmax values ---#
tmax_from_prism_stack <- raster::extract(prism_tmax_stack, st_coordinates(KS_wells))

#--- take a look ---#
head(tmax_from_prism_stack)
```

```
     PRISM_tmax_stable_4kmD2_20180701_bil PRISM_tmax_stable_4kmD2_20180702_bil
[1,]                               34.241                               30.544
[2,]                               29.288                               29.569
[3,]                               32.585                               29.866
[4,]                               30.104                               29.819
[5,]                               34.232                               30.481
[6,]                               35.168                               30.640
```

Instead of a vector, the returned object is a matrix with each of the raster layers forming a column.    

### Polygons (`terra` way)

**Caution:** Recently, `terra::extract()` crashed R sessions on RStudio several times when I tried to extract values from a large raster dataset (1.6 GB) for polygons. I did not see any problem when extracting for points data even if the raster data is very large, For now, I recommend `exact_extract()` to extract values for polygons, which is detailed in the next section. `exact_extract()` is faster for a large raster dataset anyway.  
You can use the same `terra::extract()` function to extract values from a raster layer for polygons. For each of the polygons, it will identify all the raster cells whose center lies inside the polygon and assign the vector of values of the cells to the polygon (You can change this to the cells that intersect with polygons using the `touch = TRUE` option). 


```r
#--- extract values from the raster for each county ---#
tmax_by_county <- terra::extract(prism_tmax_0701_KS_sr, KS_county_sv)  

#--- take a look at the first 2 elements of the list ---#
tmax_by_county[1:2] 
```

```
[[1]]
[[1]][[1]]
 [1] 34.393 34.456 34.450 34.460 34.483 34.566 34.588 34.616 34.623 34.556
[11] 34.231 34.145 34.188 34.247 34.289 34.279 34.282 34.417 34.483 34.585
[21] 34.151 34.114 34.174 34.138 34.136 34.235 34.351 34.494 34.591 34.563
[31] 34.183 34.198 34.117 34.049 34.100 34.193 34.294 34.431 34.521 34.484
[41] 34.088 34.121 34.170 34.142 34.156 34.203 34.176 34.249 34.356 34.454
[51] 34.126 34.072 34.088 34.091 34.123 34.145 34.238 34.306 34.359 34.511
[61] 34.043 34.079 34.048 33.977 34.113 34.203 34.273 34.308 34.387 34.543


[[2]]
[[2]][[1]]
 [1] 35.316 35.289 35.072 35.014 34.995 34.996 34.990 34.953 35.042 35.034
[11] 35.112 35.186 35.087 34.977 34.890 34.838 34.838 34.844 34.926 34.858
[21] 34.948 35.037 35.182 35.080 34.945 34.856 34.897 34.911 35.021 34.990
[31] 34.919 34.963 35.055 34.935 34.899 34.910 34.903 34.936 34.961 34.983
[41] 35.006 34.827 35.215 35.093 34.867 34.876 34.856 34.932 34.919 35.003
[51] 34.943 34.835 35.491 35.215 34.927 34.818 34.854 34.834 34.758 34.763
[61] 34.749 34.717 35.285 35.008 34.869 34.801 34.774 34.798 34.694 34.626
[71] 34.644 34.637 34.821 34.819 35.000 34.877 34.821 34.780 34.714 34.658
[81] 34.552 34.547 34.605 34.672 34.599 34.678 34.666 34.822 34.825 34.721
[91] 34.666 34.616
```

`terra::extract()` returns a list, where its $i$th element corresponds to the $i$th row of observation in the polygon data (`KS_county_sv`). Each of the list elements is also a list, and the list has a vector of extracted values for the corresponding polygon.  


```r
#--- see the first element of the list ---#
tmax_by_county[[1]]
```

```
[[1]]
 [1] 34.393 34.456 34.450 34.460 34.483 34.566 34.588 34.616 34.623 34.556
[11] 34.231 34.145 34.188 34.247 34.289 34.279 34.282 34.417 34.483 34.585
[21] 34.151 34.114 34.174 34.138 34.136 34.235 34.351 34.494 34.591 34.563
[31] 34.183 34.198 34.117 34.049 34.100 34.193 34.294 34.431 34.521 34.484
[41] 34.088 34.121 34.170 34.142 34.156 34.203 34.176 34.249 34.356 34.454
[51] 34.126 34.072 34.088 34.091 34.123 34.145 34.238 34.306 34.359 34.511
[61] 34.043 34.079 34.048 33.977 34.113 34.203 34.273 34.308 34.387 34.543
```

```r
#--- check the class ---#
tmax_by_county[[1]] %>% class()
```

```
[1] "list"
```

In order to make the results usable, you can process them to get a single `data.frame`, taking advantage of `dplyr::bind_rows()` to combine the list of the datasets into one dataset. In doing so, you can use `.id` option to create a new identifier column that links each row to its original data (`data.table` users can use `rbindlist()` with the `idcol` option).


```r
(
tmax_by_county_df <- tmax_by_county %>%  
  #--- apply unlist to the lists to have vectors as the list elements ---#
  lapply(unlist) %>% 
  #--- convert vectors to data.frames ---#
  lapply(as_tibble) %>% 
  #--- combine the list of data.frames ---#
  bind_rows(., .id = "rowid") %>% 
  #--- rename the value variable ---# 
  rename(tmax = value)
)
```

```
# A tibble: 12,730 x 2
   rowid  tmax
   <chr> <dbl>
 1 1      34.4
 2 1      34.5
 3 1      34.5
 4 1      34.5
 5 1      34.5
 6 1      34.6
 7 1      34.6
 8 1      34.6
 9 1      34.6
10 1      34.6
# … with 12,720 more rows
```

Note that `rowid` represents the row number of polygons in `KS_county_sv`. Now, we can easily summarize the data by polygon (county). For example, the code below finds a simple average of tmax by county.


```r
tmax_by_county_df %>% 
  group_by(rowid) %>% 
  summarize(tmax = mean(tmax))
```

```
# A tibble: 105 x 2
   rowid  tmax
   <chr> <dbl>
 1 1      34.3
 2 10     33.4
 3 100    30.7
 4 101    34.1
 5 102    32.6
 6 103    34.2
 7 104    33.8
 8 105    35.2
 9 11     34.8
10 12     29.3
# … with 95 more rows
```

For `data.table` users, here is how you can do the same:


```r
tmax_by_county %>%  
  #--- apply unlist to the lists to have vectors as the list elements ---#
  lapply(unlist) %>% 
  #--- convert vectors to data.frames ---#
  lapply(data.table) %>% 
  #--- combine the list ---#
  rbindlist(., idcol = "rowid") %>% 
  #--- rename the value variable ---# 
  setnames("V1", "tmax") %>% 
  #--- find the mean of tmax ---#
  .[, .(tmax = mean(tmax)), by = rowid]
```

```
     rowid     tmax
  1:     1 34.28574
  2:     2 34.89871
  3:     3 35.30388
  4:     4 34.04197
  5:     5 32.92819
 ---               
101:   101 34.12691
102:   102 32.58354
103:   103 34.20999
104:   104 33.80209
105:   105 35.19768
```

---

Extracting values from a multi-layer raster data works exactly the same way except that data processing after the value extraction is slightly more complicated. 


```r
#--- extract from a multi-layer raster object ---#
tmax_by_county_from_stack <- terra::extract(prism_tmax_stack, KS_county_sv) 

#--- take a look at the first element ---#
tmax_by_county_from_stack[[1]]
```

```
[[1]]
 [1] 34.393 34.456 34.450 34.460 34.483 34.566 34.588 34.616 34.623 34.556
[11] 34.231 34.145 34.188 34.247 34.289 34.279 34.282 34.417 34.483 34.585
[21] 34.151 34.114 34.174 34.138 34.136 34.235 34.351 34.494 34.591 34.563
[31] 34.183 34.198 34.117 34.049 34.100 34.193 34.294 34.431 34.521 34.484
[41] 34.088 34.121 34.170 34.142 34.156 34.203 34.176 34.249 34.356 34.454
[51] 34.126 34.072 34.088 34.091 34.123 34.145 34.238 34.306 34.359 34.511
[61] 34.043 34.079 34.048 33.977 34.113 34.203 34.273 34.308 34.387 34.543

[[2]]
 [1] 29.570 29.562 29.528 29.497 29.484 29.493 29.468 29.455 29.413 29.347
[11] 29.508 29.472 29.491 29.502 29.485 29.472 29.443 29.450 29.440 29.373
[21] 29.506 29.448 29.451 29.401 29.404 29.440 29.466 29.447 29.428 29.365
[31] 29.470 29.449 29.384 29.328 29.381 29.430 29.454 29.442 29.441 29.341
[41] 29.462 29.442 29.444 29.420 29.424 29.435 29.344 29.339 29.365 29.321
[51] 29.504 29.464 29.454 29.445 29.428 29.353 29.376 29.387 29.377 29.399
[61] 29.517 29.481 29.479 29.415 29.362 29.390 29.402 29.381 29.392 29.424
```

Just like the single-layer case, $i$th element of the list corresponds to $i$th polygon. However, each element of the list has two lists of extracted values because we are extracting from a two-layer raster object. This makes it a bit complicated to process them to have nicely-formatted data. The following code transform the list to a single `data.frame`: 


```r
#--- extraction from a multi-layer raster object ---#
tmax_long_from_stack <- tmax_by_county_from_stack %>% 
  lapply(., function(x) bind_rows(lapply(x, as_tibble), .id = "layer")) %>% 
  bind_rows(., .id = "rowid")

#--- take a look ---#
head(tmax_long_from_stack)
```

```
# A tibble: 6 x 3
  rowid layer value
  <chr> <chr> <dbl>
1 1     1      34.4
2 1     1      34.5
3 1     1      34.5
4 1     1      34.5
5 1     1      34.5
6 1     1      34.6
```

Note that this code works for a raster object with any number of layers including the single-layer case we saw above. 

We can then summarize the extracted data by polygon and raster layer.   


```r
tmax_long_from_stack %>% 
  group_by(rowid, layer) %>% 
  summarize(tmax = mean(value))
```

```
# A tibble: 210 x 3
# Groups:   rowid [105]
   rowid layer  tmax
   <chr> <chr> <dbl>
 1 1     1      34.3
 2 1     2      29.4
 3 10    1      33.4
 4 10    2      29.7
 5 100   1      30.7
 6 100   2      29.9
 7 101   1      34.1
 8 101   2      28.6
 9 102   1      32.6
10 102   2      30.6
# … with 200 more rows
```

Here is the `data.table` way:


```r
(
tmax_by_county_layer <- tmax_by_county_from_stack %>% 
  lapply(., function(x) rbindlist(lapply(x, data.table), idcol = "layer")) %>% 
  rbindlist(., idcol = "rowid") %>% 
  .[, .(tmax = mean(V1)), by = .(rowid, layer)]
)
```

```
     rowid layer     tmax
  1:     1     1 34.28574
  2:     1     2 29.43364
  3:     2     1 34.89871
  4:     2     2 29.42585
  5:     3     1 35.30388
 ---                     
206:   103     2 29.73917
207:   104     1 33.80209
208:   104     2 29.56691
209:   105     1 35.19768
210:   105     2 29.32252
```

### Polygons (`exactextractr` way)

`exact_extract()` function from the `exactextractr` package is a faster alternative than `terra::extract()` for large raster data as we confirm later (`exact_extract()` does not work with points data at the moment).^[See [here](https://github.com/isciences/exactextract) for how it does extraction tasks differently from other major GIS software.] `exact_extract()` also provides a coverage fraction value for each of the cell-polygon intersections. However, as mentioned in Chapter \@ref(raster-basics), it only works with `Raster`$^*$ objects. So, we first need to convert a `SpatRaster` object to a `Raster`$^*$ object. The syntax of `exact_extract()` is very much similar to `terra::extract()`. 


```r
#--- syntax (NOT RUN) ---#
exact_extract(raster, sf) 
```

So, to get tmax values from the PRISM raster layer for Kansas county polygons, the following does the job: 


```r
#--- convert to a RasterLayer ---#
prism_tmax_0701_KS_rl <- raster(prism_tmax_0701_KS_sr)

library("exactextractr")

#--- extract values from the raster for each county ---#
tmax_by_county <- exact_extract(prism_tmax_0701_KS_rl, KS_county_sf)  

#--- take a look at the first 6 rows of the first two list elements ---#
tmax_by_county[1:2] %>% lapply(function(x) head(x))
```




```
[[1]]
   value coverage_fraction
1 34.431         0.1455645
2 34.605         0.4065359
3 34.672         0.3808859
4 34.599         0.3552359
5 34.678         0.3295859
6 34.666         0.3039359

[[2]]
   value coverage_fraction
1 35.404        0.04257641
2 35.316        0.95364380
3 35.289        0.95364380
4 35.072        0.95364380
5 35.014        0.95364380
6 34.995        0.95364380
```

`exact_extract()` returns a list, where its $i$th element corresponds to the $i$th row of observation in the polygon data (`KS_county_sf`). For each element of the list, you see `value` and `coverage_fraction`. `value` is the tmax value of the intersecting raster cells, and `coverage_fraction` is the fraction of the intersecting area relative to the full raster grid, which can help find coverage-weighted summary of the extracted values. 


```r
#--- combine ---#
tmax_combined <- bind_rows(tmax_by_county, .id = "id")

#--- take a look ---#
head(tmax_combined)
```

```
  id  value coverage_fraction
1  1 34.431         0.1455645
2  1 34.605         0.4065359
3  1 34.672         0.3808859
4  1 34.599         0.3552359
5  1 34.678         0.3295859
6  1 34.666         0.3039359
```

We can now summarize the data by `id`. Here, we calculate coverage-weighted mean of tmax.


```r
tmax_by_id <- tmax_combined %>% 
  #--- convert from character to numeric  ---#
  mutate(id = as.numeric(id)) %>% 
  #--- group summary ---#
  group_by(id) %>% 
  summarise(tmax = sum(value * coverage_fraction) / sum(coverage_fraction))

#--- take a look ---#
head(tmax_by_id)
```

```
# A tibble: 6 x 2
     id  tmax
  <dbl> <dbl>
1     1  34.3
2     2  34.9
3     3  35.3
4     4  34.1
5     5  32.9
6     6  34.8
```

Remember that `id` values are row numbers in the polygon data (`KS_county_sf`). So, we can assign the tmax values to KS_county_sf as follows:


```r
KS_county_sf$tmax_07_01 <- tmax_by_id$tmax
```

---

Extracting values from `RasterStack` works in exactly the same manner as `RasterLayer`. Do not forget that you need to use `stack()` instead of `raster()` to convert a multi-layer `SpatRaster` to `RasterStack`.


```r
tmax_by_county_stack <- stack(prism_tmax_stack) %>% # convert to RasterStack
  #--- extract from a stack ---#
  exact_extract(., KS_county_sf, progress = F) 

#--- take a look at the first 6 lines of the first element---#
tmax_by_county_stack[[1]] %>% head()
```

```
  PRISM_tmax_stable_4kmD2_20180701_bil PRISM_tmax_stable_4kmD2_20180702_bil
1                               34.431                               29.558
2                               34.605                               29.643
3                               34.672                               29.653
4                               34.599                               29.568
5                               34.678                               29.580
6                               34.666                               29.540
  coverage_fraction
1         0.1455645
2         0.4065359
3         0.3808859
4         0.3552359
5         0.3295859
6         0.3039359
```

As you can see above, `exact_extract()` appends additional columns for additional layers, unlike the results of `terra::extract()` that creates additional lists for additional layers. This makes the post-extraction processing much simpler.


```r
#--- combine them ---#
tmax_all_combined <- tmax_by_county_stack %>% 
  bind_rows(.id = "id") 

#--- take a look ---#
head(tmax_all_combined)
```

```
  id PRISM_tmax_stable_4kmD2_20180701_bil PRISM_tmax_stable_4kmD2_20180702_bil
1  1                               34.431                               29.558
2  1                               34.605                               29.643
3  1                               34.672                               29.653
4  1                               34.599                               29.568
5  1                               34.678                               29.580
6  1                               34.666                               29.540
  coverage_fraction
1         0.1455645
2         0.4065359
3         0.3808859
4         0.3552359
5         0.3295859
6         0.3039359
```

In order to find the coverage-weighted tmax by date, you can first pivot it to a long format using `dplyr::pivot_longer()`.


```r
#--- pivot to a longer format ---#
(
tmax_long <- pivot_longer(
  tmax_all_combined, 
  -c(id, coverage_fraction), 
  names_to = "date",
  values_to = "tmax"
  )  
)
```

```
# A tibble: 30,302 x 4
   id    coverage_fraction date                                  tmax
   <chr>             <dbl> <chr>                                <dbl>
 1 1                 0.146 PRISM_tmax_stable_4kmD2_20180701_bil  34.4
 2 1                 0.146 PRISM_tmax_stable_4kmD2_20180702_bil  29.6
 3 1                 0.407 PRISM_tmax_stable_4kmD2_20180701_bil  34.6
 4 1                 0.407 PRISM_tmax_stable_4kmD2_20180702_bil  29.6
 5 1                 0.381 PRISM_tmax_stable_4kmD2_20180701_bil  34.7
 6 1                 0.381 PRISM_tmax_stable_4kmD2_20180702_bil  29.7
 7 1                 0.355 PRISM_tmax_stable_4kmD2_20180701_bil  34.6
 8 1                 0.355 PRISM_tmax_stable_4kmD2_20180702_bil  29.6
 9 1                 0.330 PRISM_tmax_stable_4kmD2_20180701_bil  34.7
10 1                 0.330 PRISM_tmax_stable_4kmD2_20180702_bil  29.6
# … with 30,292 more rows
```

And then find coverage-weighted tmax by date:


```r
(
tmax_long %>% 
  group_by(id, date) %>% 
  summarize(tmax = sum(tmax * coverage_fraction) / sum(coverage_fraction))
)
```

```
# A tibble: 210 x 3
# Groups:   id [105]
   id    date                                  tmax
   <chr> <chr>                                <dbl>
 1 1     PRISM_tmax_stable_4kmD2_20180701_bil  34.3
 2 1     PRISM_tmax_stable_4kmD2_20180702_bil  29.4
 3 10    PRISM_tmax_stable_4kmD2_20180701_bil  33.4
 4 10    PRISM_tmax_stable_4kmD2_20180702_bil  29.6
 5 100   PRISM_tmax_stable_4kmD2_20180701_bil  30.8
 6 100   PRISM_tmax_stable_4kmD2_20180702_bil  29.9
 7 101   PRISM_tmax_stable_4kmD2_20180701_bil  34.1
 8 101   PRISM_tmax_stable_4kmD2_20180702_bil  28.6
 9 102   PRISM_tmax_stable_4kmD2_20180701_bil  32.6
10 102   PRISM_tmax_stable_4kmD2_20180702_bil  30.6
# … with 200 more rows
```

For `data.table` users, this does the same:


```r
(
tmax_all_combined %>% 
  data.table() %>% 
  melt(id.var = c("id", "coverage_fraction")) %>% 
  .[, .(tmax = sum(value * coverage_fraction) / sum(coverage_fraction)), by = .(id, variable)]
)
```

```
      id                             variable     tmax
  1:   1 PRISM_tmax_stable_4kmD2_20180701_bil 34.29958
  2:   2 PRISM_tmax_stable_4kmD2_20180701_bil 34.89949
  3:   3 PRISM_tmax_stable_4kmD2_20180701_bil 35.26522
  4:   4 PRISM_tmax_stable_4kmD2_20180701_bil 34.10113
  5:   5 PRISM_tmax_stable_4kmD2_20180701_bil 32.93172
 ---                                                  
206: 101 PRISM_tmax_stable_4kmD2_20180702_bil 28.60475
207: 102 PRISM_tmax_stable_4kmD2_20180702_bil 30.61366
208: 103 PRISM_tmax_stable_4kmD2_20180702_bil 29.74647
209: 104 PRISM_tmax_stable_4kmD2_20180702_bil 29.57155
210: 105 PRISM_tmax_stable_4kmD2_20180702_bil 29.33646
```

## Extraction speed comparison

Here we compare the extraction speed of `raster::extract()`, `terra::extract()`, and `exact_extract()`. 

### Points: `terra::extract()` and `raster::extract()`

`exact_extract()` uses C++ as the backend. Therefore, it is considerably faster than `raster::extract()`.


```r
#--- terra ---#
tic()
temp <- terra::extract(prism_tmax_0701_KS_sr, st_coordinates(KS_wells))
toc()
```

```
0.006 sec elapsed
```

```r
#--- raster ---#
tic()
temp <- raster::extract(raster(prism_tmax_0701_KS_sr), KS_wells)
toc()
```

```
0.343 sec elapsed
```

As you can see, `terra::extract()` is much faster. The time differential between the two packages can be substantial as the raster data becomes larger.

### Polygons: `exact_extract()`, `terra::extract()`, and `raster::extract()`

`terra::extract()` is faster than `exact_extract()` for a relatively small raster data. Let's time them and see the difference.  


```r
library(tictoc)

#--- terra extract ---#
tic()
terra_extract_temp <- terra::extract(prism_tmax_0701_KS_sr, KS_county_sv, progress = FALSE)  
toc()
```

```
0.047 sec elapsed
```

```r
#--- exact extract ---#
tic()
exact_extract_temp <- exact_extract(prism_tmax_0701_KS_rl, KS_county_sf, progress = FALSE)  
toc()
```

```
0.257 sec elapsed
```

```r
#--- raster::extract ---#
tic()
raster_extract_temp <- raster::extract(prism_tmax_0701_KS_rl, KS_county_sf)  
toc()
```

```
2.469 sec elapsed
```

As you can see, `raster::extract()` is by far the slowest. `terra::extract()` is faster than `exact_extract()`. However, once the raster data becomes larger (or spatially finer), then `exact_extact()` starts to shine. 

---

Let's disaggregate the prism data by a factor of 10 to create a much larger raster data.^[We did not introduce this function as it is very rare that you need this function in research projects.]


```r
#--- disaggregate ---#
(
prism_tmax_0701_KS_sr_10 <- terra::disaggregate(prism_tmax_0701_KS_sr, fact = 10)
)
```

```
class       : SpatRaster 
dimensions  : 730, 1790, 1  (nrow, ncol, nlyr)
resolution  : 0.004166667, 0.004166667  (x, y)
extent      : -102.0625, -94.60417, 36.97917, 40.02083  (xmin, xmax, ymin, ymax)
coord. ref. : +proj=longlat +datum=NAD83 +no_defs  
data source : memory 
names       : PRISM_tmax_stable_4kmD2_20180701_bil 
min values  :                               27.711 
max values  :                               37.656 
```

```r
#--- convert the disaggregated PRISM data to RasterLayer ---#
prism_tmax_0701_KS_rl_10 <- raster(prism_tmax_0701_KS_sr_10)
```

The disaggregated PRISM data now has 10 times more rows and columns (see below).   


```r
#--- original ---#
dim(prism_tmax_0701_KS_sr)  
```

```
[1]  73 179   1
```

```r
#--- disaggregated ---#
dim(prism_tmax_0701_KS_sr_10)  
```

```
[1]  730 1790    1
```

---

Now, let's compare `terra::extrct()` and `exact_extrct()` using the disaggregated data.


```r
#--- terra extract ---#
tic()
terra_extract_temp <- terra::extract(prism_tmax_0701_KS_sr_10, KS_county_sv)  
toc()
```

```
3.699 sec elapsed
```

```r
#--- exact extract ---#
tic()
exact_extract_temp <- exact_extract(prism_tmax_0701_KS_rl_10, KS_county_sf, progress = FALSE)  
toc()
```

```
0.465 sec elapsed
```

As you can see, `exact_extract()` is considerably faster. The difference in time becomes even more pronounced as the size of the raster data becomes larger and the number of polygons are greater. The time difference of several seconds seem nothing, but imagine processing PRISM files for the entire US over 20 years, then you would appreciate the speed of `exact_extract()`. 

### Single-layer vs multi-layer

Pretend that you have five dates of PRISM tmax data (here we repeat the same file five times) and would like to extract values from all of them. Extracting values from a multi-layer raster objects (`RasterStack` for `raster` package) takes less time than extracting values from the individual layers one at a time. This can be observed below.   

---

**`terra::extract()`**




```r
#--- extract from 5 layers one at a time ---#
tic()
temp <- terra::extract(prism_tmax_0701_KS_sr_10, KS_county_sv)
temp <- terra::extract(prism_tmax_0701_KS_sr_10, KS_county_sv)
temp <- terra::extract(prism_tmax_0701_KS_sr_10, KS_county_sv)
temp <- terra::extract(prism_tmax_0701_KS_sr_10, KS_county_sv)
temp <- terra::extract(prism_tmax_0701_KS_sr_10, KS_county_sv)
toc()
```

```
18.808 sec elapsed
```

```r
#--- extract from a 5-layer stack ---#
prism_tmax_ml_5 <- c(
    prism_tmax_0701_KS_sr_10, 
    prism_tmax_0701_KS_sr_10, 
    prism_tmax_0701_KS_sr_10, 
    prism_tmax_0701_KS_sr_10, 
    prism_tmax_0701_KS_sr_10
  )

tic()
temp <- terra::extract(prism_tmax_ml_5, KS_county_sv)
toc()
```

```
4.595 sec elapsed
```

---

**`exact_extract()`**


```r
#--- extract from 5 layers one at a time ---#
tic()
temp <- exact_extract(prism_tmax_0701_KS_rl_10, KS_county_sf, progress = FALSE)
temp <- exact_extract(prism_tmax_0701_KS_rl_10, KS_county_sf, progress = FALSE)
temp <- exact_extract(prism_tmax_0701_KS_rl_10, KS_county_sf, progress = FALSE)
temp <- exact_extract(prism_tmax_0701_KS_rl_10, KS_county_sf, progress = FALSE)
temp <- exact_extract(prism_tmax_0701_KS_rl_10, KS_county_sf, progress = FALSE)
toc()
```

```
2.697 sec elapsed
```

```r
#--- extract from from a 5-layer stack ---#
prism_tmax_stack_5 <- stack(
    prism_tmax_0701_KS_rl_10, 
    prism_tmax_0701_KS_rl_10, 
    prism_tmax_0701_KS_rl_10, 
    prism_tmax_0701_KS_rl_10, 
    prism_tmax_0701_KS_rl_10
  )

tic()
temp <- exact_extract(prism_tmax_stack_5, KS_county_sf, progress = FALSE)
toc()
```

```
1.154 sec elapsed
```

The reduction in computation time for both methods makes sense. Since both layers have exactly the same geographic extent and resolution, finding the polygons-cells correspondence is done once and then it can be used repeatedly across the layers for the multi-layer `SparRaster` and `RasterStack`. This clearly suggests that when you are processing many layers of the same spatial resolution and extent, you should first stack them and then extract values at the same time instead of processing them one by one as long as your memory allows you to do so. 

<!-- There is much more to discuss about the computation speed of raster data extraction for polygons. For those who are interested in this topic, go to Chapter \@ref(EE). -->


<!--chapter:end:SpatialInteractionVectorRaster.Rmd-->

# (APPENDIX) Appendix {-}

# Loop and Parallel Computing {#par-comp}







## Before you start {-}

Here we will learn how to program repetitive operations effectively and fast. We start from the basics of a loop for those who are not familiar with the concept. We then cover parallel computation using the `future.lapply` and `parallel` package. Those who are familiar with `lapply()` can go straight to Chapter \@ref(parcomp). 

Here are the specific learning objectives of this chapter.

1. Learn how to use **for loop** and `lapply()` to complete repetitive jobs 
2. Learn how not to loop things that can be easily vectorized
3. Learn how to parallelize repetitive jobs using the `future_lapply()` function from the `future.apply` package

### Direction for replication {-}

All the data in this Chapter is generated.  

### Packages to install and load {-}

Run the following code to install or load (if already installed) the `pacman` package, and then install or load (if already installed) the listed package inside the `pacman::p_load()` function.


```r
if (!require("pacman")) install.packages("pacman")
pacman::p_load(
  dplyr, # data wrangling
  data.table # data wrangling
)  
```

There are other packages that will be loaded during the demonstration.

---

## Repetitive processes and looping

### What is looping? 

We sometimes need to run the same process over and over again often with slight changes in parameters. In such a case, it is very time-consuming and messy to write all of the steps one bye one. For example, suppose you are interested in knowing the square of 1 through 5 in with a step of 1 ($[1, 2, 3, 4, 5]$). The following code certainly works:


```r
1^2 
```

```
[1] 1
```

```r
2^2 
```

```
[1] 4
```

```r
3^2 
```

```
[1] 9
```

```r
4^2 
```

```
[1] 16
```

```r
5^2 
```

```
[1] 25
```

However, imagine you have to do this for 1000 integers. Yes, you don't want to write each one of them one by one as that would occupy 1000 lines of your code, and it would be time-consuming. Things will be even worse if you need to repeat much more complicated processes like Monte Carlo simulations. So, let's learn how to write a program to do repetitive jobs effectively using loop. 

Looping is repeatedly evaluating the same (except parameters) process over and over again. In the example above, the **same** process is the action of squaring. This does not change among the processes you run. What changes is what you square. Looping can help you write a concise code to implement these repetitive processes.

### For loop

Here is how **for loop** works in general:


```r
#--- NOT RUN ---#
for (x in a_list_of_values){
  you do what you want to do with x
}
```

As an example, let's use this looping syntax to get the same results as the manual squaring of 1 through 5:


```r
for (x in 1:5){
  print(x^2)
}
```

```
[1] 1
[1] 4
[1] 9
[1] 16
[1] 25
```

Here, a list of values is $1, 2, 3, 4, 5]$. For each value in the list, you square it (`x^2`) and then print it (`print()`). If you want to get the square of $1:1000$, the only thing you need to change is the list of values to loop over as in:


```r
#--- evaluation not reported as it's too long ---#
for (x in 1:1000){
  print(x^2)
}
```

So, the length of the code does not depend on how many repeats you do, which is an obvious improvement over manual typing of every single process one by one. Note that you do not have to use $x$ to refer to an object you are going to use. It could be any combination of letters as long as you use it when you code what you want to do inside the loop. So, this would work just fine,


```r
for (bluh_bluh_bluh in 1:5){
  print(bluh_bluh_bluh^2)
}
```

```
[1] 1
[1] 4
[1] 9
[1] 16
[1] 25
```

### For loop using the `lapply()` function

You can do for loop using the `lapply()` function as well.^[`lpply()` in only one of the family of `apply()` functions. We do not talk about other types of `apply()` functions here (e.g., `apply()`, `spply()`, `mapply()`,, `tapply()`). Personally, I found myself only rarely using them. But, if you are interested in learning those, take a look at [here](https://www.datacamp.com/community/tutorials/r-tutorial-apply-family#gs.b=aW_Io) or [here](https://www.r-bloggers.com/using-apply-sapply-lapply-in-r/).] Here is how it works:


```r
#--- NOT RUN ---#  
lapply(A,B)
```

where $A$ is the list of values you go through one by one in the order the values are stored, and $B$ is the function you would like to apply to each of the values in $A$. For example, the following code does exactly the same thing as the above for loop example.


```r
lapply(1:5, function(x){x^2})
```

```
[[1]]
[1] 1

[[2]]
[1] 4

[[3]]
[1] 9

[[4]]
[1] 16

[[5]]
[1] 25
```

Here, $A$ is $[1, 2, 3, 4, 5]$. In $B$ you have a function that takes $x$ and square it. So, the above code applies the function to each of $[1, 2, 3, 4, 5]$ one by one. In many circumstances, you can write the same looping actions in a much more concise manner using the `lapply` function than explicitly writing out the loop process as in the above for loop examples. You might have noticed that the output is a list. Yes, `lapply()` returns the outcomes in a list. That is where **l** in `lapply()` comes from.  

When the operation you would like to repeat becomes complicated (almost always the case), it is advisable that you create a function of that process first. 


```r
#--- define the function first ---#
square_it <- function(x){
  return(x^2)
}

#--- lapply using the pre-defined function ---#
lapply(1:5, square_it)
```

```
[[1]]
[1] 1

[[2]]
[1] 4

[[3]]
[1] 9

[[4]]
[1] 16

[[5]]
[1] 25
```

Finally, it is a myth that you should always use `lapply()` instead of the explicit for loop syntax because `lapply()` (or other `apply()` families) is faster. They are basically the same.^[Check this [discussion](https://stackoverflow.com/questions/7142767/why-are-loops-slow-in-r) on StackOverflow. You might want to check out [this video](https://www.youtube.com/watch?v=GyNqlOjhPCQ) at 6:10 as well.]  

### Looping over multiple variables using lapply()  

`lapply()` allows you to loop over only one variable. However, it is often the case that you want to loop over multiple variables^[the `map()` function from the `purrr` package allows you to loop over two variable.]. However, it is easy to achieve this. The trick is to create a `data.frame` of the variables where the complete list of the combinations of the variables are stored, and then loop over row of the `data.frame`. As an example, suppose we are interested in understanding the sensitivity of corn revenue to corn price and applied nitrogen amount. We consider the range of $3.0/bu to $5.0/bu for corn price and 0 lb/acre to 300/acre for applied nitrogen applied. 


```r
#--- corn price vector ---#
corn_price_vec <- seq(3, 5, by = 1)

#--- nitrogen vector ---#
nitrogen_vec <- seq(0, 300, by = 100)
```

After creating vectors of the parameters, you combine them to create a complete combination of the parameters using the `expand.grid()` function, and then convert it to a `data.frame` object^[Converting to a `data.frame` is not strictly necessary.].


```r
#--- crate a data.frame that holds parameter sets to loop over ---#
parameters_data <- expand.grid(corn_price = corn_price_vec, nitrogen = nitrogen_vec) %>% 
  #--- convert the matrix to a data.frame ---#
  data.frame()

#--- take a look ---#
parameters_data
```

```
   corn_price nitrogen
1           3        0
2           4        0
3           5        0
4           3      100
5           4      100
6           5      100
7           3      200
8           4      200
9           5      200
10          3      300
11          4      300
12          5      300
```

We now define a function that takes a row number, refer to `parameters_data` to extract the parameters stored at the row number, and then calculate corn yield and revenue based on the extracted parameters. 


```r
gen_rev_corn <- function(i) {

  #--- define corn price ---#
  corn_price <- parameters_data[i,'corn_price']

  #--- define nitrogen  ---#
  nitrogen <- parameters_data[i,'nitrogen']

  #--- calculate yield ---#
  yield <- 240 * (1 - exp(0.4 - 0.02 * nitrogen))

  #--- calculate revenue ---#
  revenue <- corn_price * yield 

  #--- combine all the information you would like to have  ---#
  data_to_return <- data.frame(
    corn_price = corn_price,
    nitrogen = nitrogen,
    revenue = revenue
  )

  return(data_to_return)
}
```

This function takes $i$ (act as a row number within the function), extract corn price and nitrogen from the $i$th row of `parameters_mat`, which are then used to calculate yield and revenue^[Yield is generated based on the Mitscherlich-Baule functional form. Yield increases at the decreasing rate as you apply more nitrogen, and yield eventually hits the plateau.]. Finally, it returns a `data.frame` of all the information you used (the parameters and the outcomes).


```r
#--- loop over all the parameter combinations ---#
rev_data <- lapply(1:nrow(parameters_data), gen_rev_corn)

#--- take a look ---#
rev_data
```

```
[[1]]
  corn_price nitrogen   revenue
1          3        0 -354.1138

[[2]]
  corn_price nitrogen   revenue
1          4        0 -472.1517

[[3]]
  corn_price nitrogen   revenue
1          5        0 -590.1896

[[4]]
  corn_price nitrogen  revenue
1          3      100 574.6345

[[5]]
  corn_price nitrogen  revenue
1          4      100 766.1793

[[6]]
  corn_price nitrogen  revenue
1          5      100 957.7242

[[7]]
  corn_price nitrogen  revenue
1          3      200 700.3269

[[8]]
  corn_price nitrogen  revenue
1          4      200 933.7692

[[9]]
  corn_price nitrogen  revenue
1          5      200 1167.212

[[10]]
  corn_price nitrogen  revenue
1          3      300 717.3375

[[11]]
  corn_price nitrogen  revenue
1          4      300 956.4501

[[12]]
  corn_price nitrogen  revenue
1          5      300 1195.563
```

Successful! Now, for us to use the outcome for other purposes like further analysis and visualization, we would need to have all the results combined into a single `data.frame` instead of a list of `data.frame`s. To do this, use either `bind_rows()` from the `dplyr` package or `rbindlist()` from the `data.table` package.


```r
#--- bind_rows ---#
bind_rows(rev_data)
```

```
   corn_price nitrogen   revenue
1           3        0 -354.1138
2           4        0 -472.1517
3           5        0 -590.1896
4           3      100  574.6345
5           4      100  766.1793
6           5      100  957.7242
7           3      200  700.3269
8           4      200  933.7692
9           5      200 1167.2115
10          3      300  717.3375
11          4      300  956.4501
12          5      300 1195.5626
```

```r
#--- rbindlist ---#
rbindlist(rev_data)
```

```
    corn_price nitrogen   revenue
 1:          3        0 -354.1138
 2:          4        0 -472.1517
 3:          5        0 -590.1896
 4:          3      100  574.6345
 5:          4      100  766.1793
 6:          5      100  957.7242
 7:          3      200  700.3269
 8:          4      200  933.7692
 9:          5      200 1167.2115
10:          3      300  717.3375
11:          4      300  956.4501
12:          5      300 1195.5626
```

### Do you really need to loop? 

Actually, we should not have used for loop or `lapply()` in any of the examples above in practice^[By the way, note that `lapply()` is no magic. It's basically a for loop and not rally any faster than for loop.] This is because they can be easily vectorized. Vectorized operations are those that take vectors as inputs and work on each element of the vectors in parallel^[This does not mean that the process is parallelized by using multiple cores.]. 

A typical example of a vectorized operation would be this:


```r
#--- define numeric vectors ---#
x <- 1:1000
y <- 1:1000

#--- element wise addition ---#
z_vec <- x + y 
```

A non-vectorized version of the same calculation is this:


```r
z_la <- lapply(1:1000, function(i) x[i] + y[i]) %>%  unlist

#--- check if identical with z_vec ---#
all.equal(z_la, z_vec)
```

```
[1] TRUE
```

Both produce the same results. However, R is written in a way that is much better at doing vectorized operations. Let's time them using the `microbenchmark()` function from the `microbenchmark` package. Here, we do not `unlist()` after `lapply()` to just focus on the multiplication part.


```r
library(microbenchmark)

microbenchmark(
  #--- vectorized ---#
  "vectorized" = { x + y }, 
  #--- not vectorized ---#
  "not vectorized" = { lapply(1:1000, function(i) x[i] + y[i])},
  times = 100, 
  unit = "ms"
)
```

```
Unit: milliseconds
           expr      min        lq       mean    median        uq      max
     vectorized 0.002537 0.0035535 0.00443277 0.0047210 0.0050235 0.010541
 not vectorized 0.479103 0.5162730 0.56107218 0.5407665 0.5656315 2.221973
 neval
   100
   100
```

As you can see, the vectorized version is faster. The time difference comes from R having to conduct many more internal checks and hidden operations for the non-vectorized one^[See [this](http://www.noamross.net/archives/2014-04-16-vectorization-in-r-why/) or [this](https://stackoverflow.com/questions/7142767/why-are-loops-slow-in-r) to have a better understanding of why non-vectorized operations can be slower than vectorized operations.]. Yes, we are talking about a fraction of milliseconds here. But, as the objects to operate on get larger, the difference between vectorized and non-vectorized operations can become substantial^[See [here](http://www.win-vector.com/blog/2019/01/what-does-it-mean-to-write-vectorized-code-in-r/) for a good example of such a case. R is often regarded very slow compared to other popular software. But, many of such claims come from not vectorizing operations that can be vectorized. Indeed, many of the base and old R functions are written in C. More recent functions relies on C++ via the `Rcpp` package.]. 
    
The `lapply()` examples can be easily vectorized.        

Instead of this:


```r
lapply(1:1000 square_it)
```

You can just do this:


```r
square_it(1:1000)
```

You can also easily vectorize the revenue calculation demonstrated above. First, define the function differently so that revenue calculation can take corn price and nitrogen vectors and return a revenue vector.


```r
gen_rev_corn_short <- function(corn_price, nitrogen) {

  #--- calculate yield ---#
  yield <- 240 * (1 - exp(0.4 - 0.02 * nitrogen))

  #--- calculate revenue ---#
  revenue <- corn_price * yield 

  return(revenue)
}
```

Then use the function to calculate revenue and assign it to a new variable in the `parameters_data` data.


```r
rev_data_2 <- mutate(
  parameters_data,
  revenue = gen_rev_corn_short(corn_price, nitrogen)
) 
```

Let's compare the two:


```r
microbenchmark(

  #--- vectorized ---#
  "vectorized" = { rev_data <- mutate(parameters_data, revenue = gen_rev_corn_short(corn_price, nitrogen)) },
  #--- not vectorized ---#
  "not vectorized" = { parameters_data$revenue <- lapply(1:nrow(parameters_data), gen_rev_corn) },
  times = 100, 
  unit = "ms"
)
```

```
Unit: milliseconds
           expr      min        lq      mean   median        uq      max neval
     vectorized 0.242681 0.2649455 0.3367293 0.285466 0.3126455 4.524093   100
 not vectorized 1.926673 2.0689095 2.2569531 2.158358 2.2819735 6.294044   100
```

Yes, the vectorized version is faster. So, the lesson here is that if you can vectorize, then vectorize instead of using `lapply()`. But, of course, things cannot be vectorized in many cases. 

---

## Parallelization of embarrassingly parallel processes {#parcomp}

Parallelization of computation involves distributing the task at hand to multiple cores so that multiple processes are done in parallel. Here, we learn how to parallelize computation in R. Our focus is on the so called **embarrassingly** parallel processes. Embarrassingly parallel processes refer to a collection of processes where each process is completely independent of any another. That is, one process does not use the outputs of any of the other processes. The example of integer squaring is embarrassingly parallel. In order to calculate $1^2$, you do not need to use the result of $2^2$ or any other squares. Embarrassingly parallel processes are very easy to parallelize because you do not have to worry about which process to complete first to make other processes happen. Fortunately, most of the processes you are interested in parallelizing fall under this category^[A good example of non-embarrassingly parallel process is dynamic optimization via backward induction. You need to know the optimal solution at $t = T$, before you find the optimal solution at $t = T-1$.].  

We will use the `future.apply` package for parallelization^[There are many other options including the `parallel`, `foreach` packages.]. Using the package, parallelizing is really a piece of cake as it is basically the same syntactically as `lapply()`. 


```r
#--- load packages ---#
library(future.apply) 
```

You can find out how many cores you have available for parallel computation on your computer using the `get_num_procs()` function from the `RhpcBLASctl` package.


```r
library(RhpcBLASctl)

#--- number of all cores ---#
get_num_procs()
```

```
[1] 16
```

Before we implement parallelized `lapply()`, we need to declare what backend process we will be using by `plan()`. Here, we use `plan(multiprocess)`^[If you are a Mac or Linux user, then the `multicore` process will be used, while the `multisession` process will be used if you are using a Windows machine. The `multicore` process is superior to the `multisession` process. See [this lecture note](https://raw.githack.com/uo-ec607/lectures/master/12-parallel/12-parallel.html) on parallel programming using R by Dr. Grant McDermott's at the University of Oregon for the distinctions between the two and many other useful concepts for parallelization. At the time of this writing, if you run R through RStudio, `multiprocess` option is always redirected to `multisession` option because of the instability in doing `multiprocess`. If you use Linux or Mac and want to take the full advantage of `future_apply`, you should not run your R programs  through RStudio at least for now.]. In the `plan()` function, we can specify the number of workers. Here I will use the total number of cores less 1^[This way, you can have one more core available to do other tasks comfortably. However, if you don't mind having your computer completely devoted to the processing task at hand, then there is no reason not to use all the cores.].  


```r
plan(multiprocess, workers = get_num_procs() - 1)
```

`future_lapply()` works exactly like `lapply()`. 


```r
sq_ls <- future_lapply(1:1000, function(x) x^2)
```

This is it. The only difference you see from the serialized processing using `lapply()` is that you changed the function name to `future_lapply()`. 

Okay, now we know how we parallelize computation. Let's check how much improvement in implementation time we got by parallelization. 


```r
microbenchmark(
  #--- parallelized ---#
  "parallelized" = { sq_ls <- future_lapply(1:1000, function(x) x^2) }, 
  #--- non-parallelized ---#
  "not parallelized" = { sq_ls <- lapply(1:1000, function(x) x^2) },
  times = 100, 
  unit = "ms"
)
```

```
Unit: milliseconds
             expr       min          lq      mean      median         uq
     parallelized 94.491878 101.5638795 107.46023 105.3525655 109.838995
 not parallelized  0.393667   0.4273325   0.46396   0.4443155   0.476245
       max neval
 253.23095   100
   1.59575   100
```

Hmmmm, okay, so parallelization made the code slower... How could this be? This is because communicating jobs to each core takes some time as well. So, if each of the iterative processes is super fast (like this example where you just square a number), the time spent on communicating with the cores outweighs the time saving due to parallel computation. Parallelization is more beneficial when each of the repetitive processes takes long. 

One of the very good use cases of parallelization is MC simulation. The following MC simulation tests whether the correlation between an independent variable and error term would cause bias (yes, we know the answer). The `MC_sim` function first generates a dataset (50,000 observations) according to the following data generating process:

$$
 y = 1 + x + v
$$

where $\mu \sim N(0,1)$, $x \sim N(0,1) + \mu$, and $v \sim N(0,1) + \mu$. The $\mu$ term cause correlation between $x$ (the covariate) and $v$ (the error term). It then estimates the coefficient on $x$ vis OLS, and return the estimate. We would like to repeat this process 1,000 times to understand the property of the OLS estimators under the data generating process. This Monte Carlo simulation is embarrassingly parallel because each process is independent of any other. 


```r
#--- repeat steps 1-3 B times ---#
MC_sim <- function(i){

  N <- 50000 # sample size

  #--- steps 1 and 2:  ---#
  mu <- rnorm(N) # the common term shared by both x and u
  x <- rnorm(N) + mu # independent variable
  v <- rnorm(N) + mu # error
  y <- 1 + x + v # dependent variable
  data <- data.table(y = y, x = x)

  #--- OLS ---# 
  reg <- lm(y~x, data = data) # OLS

  #--- return the coef ---#
  return(reg$coef['x'])
}
```

Let's run one iteration,


```r
tic()
MC_sim(1)
toc()
```


```
       x 
1.503353 
```

```
elapsed 
  0.024 
```

Okay, so it takes 0.024 second for one iteration. Now, let's run this 1000 times with or without parallelization.

**Not parallelized**


```r
#--- non-parallel ---#
tic()
MC_results <- lapply(1:1000, MC_sim)
toc()
```


```
elapsed 
 24.056 
```

**Parallelized**


```r
#--- parallel ---#
tic()
MC_results <- future_lapply(1:1000, MC_sim)
toc()
```


```
elapsed 
  2.587 
```

As you can see, parallelization makes it much quicker with a noticeable difference in elapsed time. We made the code 9.3 times faster. However, we did not make the process 15 times faster even though we used 15 cores for the parallelized process. This is because of the overhead associated with distributing tasks to the cores. The relative advantage of parallelization would be greater if each iteration took more time. For example, if you are running a process that takes about 2 minutes for 1000 times, it would take approximately 33 hours and 20 minutes. But, it may take only 4 hours if you parallelize it on 15 cores, or maybe even 2 hours if you run it on 30 cores. 

### Mac or Linux users

For Mac users, `parallel::mclapply()` is just as compelling (or `pbmclapply::pbmclapply()` if you want to have a nice progress report, which is very helpful particularly when the process is long). It is just as easy to use as `future_lapply()` because its syntax is the same as `lapply()`. You can control the number of cores to employ by adding `mc.cores` option. Here is an example code that does the same MC simulations we conducted above: 


```r
#--- mclapply ---#
library(parallel)
MC_results <- mclapply(1:1000, MC_sim, mc.cores = get_num_procs() - 1)

#--- or with progress bar ---#
library(pbmclapply)
MC_results <- pbmclapply(1:1000, MC_sim, mc.cores = get_num_procs() - 1)
```

<!-- ### Some background on the parallelization packages 

For Mac and Linux users the coding cost of parallelization was minimal since when `parallel::mclapply()` was available. Parallelization is really a piece of cake for those who know how to use `lapply()` because the syntax is identical. For Windows users, that had not been the case until the arrival of the `future.apply` package. Windows machines create new threads to run on multiple cores, which do not inherit any of the R objects you have created on the environment. This means that if you need to use a dataset (or any other objects that you are not creating within the loop internally) inside the loop, you have to tell R explicitly what objects you want it to carry to the new threads so they can use those objects themselves. This hassle was eliminated by the `future.apply` package.^[To be honest, I do not completely understand how it does what it does.] On Mac and Linux machines, it was not even an issue because parallelization is done by forking (which Windows machines cannot do), which inherits all the available R objects on the environment. Since the `future.apply` package works for all the platforms, I focused on this package.  -->


<!--chapter:end:ParallelComputing.Rmd-->


# References {-}


<!--chapter:end:References.Rmd-->

