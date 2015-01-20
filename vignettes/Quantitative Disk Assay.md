---
title: "Quantitative Disk Assay"
author: "Aleeza C. Gerstein"
date: "2015-01-20"
output: rmarkdown::html_vignette
output:
	html_document:
		fig_width: 2
		fig_height: 2
vignette: >
  %\VignetteIndexEntry{Quantitative Disk Assay}
  %\VignetteEngine{knitr::rmarkdown}
  \usepackage[utf8]{inputenc}
---

# Introduction to diskImageR

### January 19, 2015


Disk diffusion assays
diskImageR provides a quantitative way to analyze photographs taken from disk diffusion assays. Specifically, 

<center>
<img src="../inst/pictures/A01_30_L_b.JPG"  style="width: 50%; height: 50%" style="float:left," alt="" /> 
</center>

## Prepare plates and photographs
The analysis done by diskImageR will only be as good as your disk assay plates and the photographs you take. Plates should always be labelled on the side, not on the bottom. Care should be taken when setting up the camera to take photographs, as you want the lighting conditions to be as uniform as possible, without any shadows on the plates. Camera settings should be manual rather than automatic as much as possible. Once you have the set of photographs that you want to be analyzed together they should be placed in the same directory, with nothing else inside that directory.

Photograph naming can be used downstream to have diskImageR do a number of statistical things (e.g., averaging across replicates, caulculations of variance, t-tests). The general format is "strain_factor1_factor2_rep.pdf". Conversely, if you intend to do all the statistical analysis later, photographs can be named anything, even numbered.

Finally, photographs should be cropped carefully around the disk.

<b> Important! </b> There can not be any spaces or special characters in any of the folder names that are in the path that will lead to your pictures or the directory that you will use as the main project folder (i.e., the place where all the output files from this package will go). 

## Run the imageJ macro on the set of photographs
The first step in the diskImageR pipeline is to run the imageJ macro on the photograph directory. 

<b> Important! </b> imageJ must be installed on your computer. ImageJ is a free, public domain Java image proessing program available for download <a href="http://rsb.info.nih.gov/ij/download.html"> here</a>. Take note of the path to imageJ, as this will be needed for the first function.

From each photograph, the macro (in imageJ) will automatically determine where the disk is located on the plate, find the center of the disk, and draw 40mm lines out from the center of the disk every 5 degrees. For each line, the pixel intensity will be determined at many points along the line. This data will be stored in the folder "imageJ-out" on your computer, with one file for each photograph.

This step can be completed using one of two functions.
```r
#This function allows you to run the imageJ macro through a user-interface with pop-up boxes to select 
#where you want the main project directory to be and where to find the location of the photograph directory.
runIJ("projectName")
```


```r
#If you are more comfortable with R, and don't want to be bothered with pop-up boxes, use the 
#alternate function to supply the desired main project directory and photograph directory locations.
runIJManual("vignette", projectDir= getwd(), pictureDir = file.path(.libPaths(), "diskImageR", "pictures", ""), imageJLoc = "loc2")
```

```
## Error in runIJManual("vignette", projectDir = getwd(), pictureDir = file.path(.libPaths(), : Output files already exist in specified directory. Please delete existing files or change project name before continuing.
```

Depending on where imageJ is located, the script may not run unless you specify the filepath. See
```r
?runIJ
```
for more details.

If you want to access the output of these functions in a later R session you can with
```r
readInExistingIJ("projectName") 	#can be any project name, does not have to be the same as previously used
```

## [optional] Plot the imageJ output
To plot pixel intensity from the average from all photographs use

```r
plotRaw("vignette", savePDF=FALSE)
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2-1.png) 

##Run the maximum likelihood analysis
``{r}
maxLik("vignette", clearHalo=1)
 
## Basic console output
To insert an R code chunk, you can type it manually or just press `Chunks - Insert chunks` or use the shortcut key. This will produce the following code chunk:
 

 
Pressing tab when inside the braces will bring up code chunk options.
 
The following R code chunk labelled `basicconsole` is as follows:
 
    
    ```r
    x <- 1:10
    y <- round(rnorm(10, x, 1), 2)
    df <- data.frame(x, y)
    df
    ```
    
    ```
    ##     x    y
    ## 1   1 2.11
    ## 2   2 1.04
    ## 3   3 1.86
    ## 4   4 3.32
    ## 5   5 5.69
    ## 6   6 5.09
    ## 7   7 6.74
    ## 8   8 7.58
    ## 9   9 9.87
    ## 10 10 9.47
    ```
    
The code chunk input and output is then displayed as follows:
 

```r
x <- 1:10
y <- round(rnorm(10, x, 1), 2)
df <- data.frame(x, y)
df
```

```
##     x    y
## 1   1 2.78
## 2   2 1.51
## 3   3 2.99
## 4   4 2.33
## 5   5 4.89
## 6   6 7.20
## 7   7 7.40
## 8   8 7.96
## 9   9 9.17
## 10 10 9.19
```
 
## Plots
Images generated by `knitr` are saved in a figures folder. However, they also appear to be represented in the HTML output using a [data URI scheme]( http://en.wikipedia.org/wiki/Data_URI_scheme). This means that you can paste the HTML into a blog post or discussion forum and you don't have to worry about finding a place to store the images; they're embedded in the HTML.
 
### Simple plot
Here is a basic plot using base graphics:
 
    
    ```r
    plot(x)
    ```
    
    ![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5-1.png) 
 

```r
plot(x)
```

![plot of chunk simpleplot](figure/simpleplot-1.png) 
 
Note that unlike traditional Sweave, there is no need to write `fig=TRUE`.
 
 
### Multiple plots
Also, unlike traditional Sweave, you can include multiple plots in one code chunk:
 
    
    ```r
    boxplot(1:10~rep(1:2,5))
    ```
    
    ![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-6-1.png) 
    
    ```r
    plot(x, y)
    ```
    
    ![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-6-2.png) 
 


