---
title: "Recording useful functions"
subtitle:
author:
 | Kailash B P
output: 
 rmarkdown::html_document:
   theme: united
   highlight: tango
   code_folding: hide
   toc: true
   toc_float: true
   df_print: paged
   smooth_scroll: true
   number_sections: false
   self_contained: true  
---

# Contents

<ol>
 <li>Unix</li>
 <ol>
  <li>diff</li>
  <li>split and cat</li>
 </ol>
 <li>Base R</li>
 <ol>
  <li>grep and grepl</li>
  <li>gsub</li>
  <li>apply, lapply, unlist</li>
  <li>sapply</li>
  <li>ls</li>
  <li>stopifnot</li>
  <li>match</li>
  <li>names</li>
  <li>str</li>
  <li>do.call</li>
  <li>scale</li>
  <li>sweep</li>
  <li>model.matrix</li>
  <li>browser</li>
  <li>ifelse</li>
  <li>combn</li>
  <li>subset</li>
  <li>mutate</li>
  <li>mapvalues</li>
  <li>with</li>
  <li>when</li>
  <li>cut</li>
  <li>case_when</li>
  <li>melt</li>
  <li>dcast</li>
  <li>pipes</li>
  <li>merge</li>
  <li>filter</li>
  <li>select</li>
  <li>arrange</li>
  <li>transmute</li>
  <li>summarise</li>
  <li>group_by</li>
  <li>rename</li>
  <li>detectCores</li>
  <li>makeCluster makeCluster & ParLapply() & stopCluster()</li>
  <li>registerDoParallel</li>
  <li>clusterExport</li>
  <li>Sys.time</li>
 </ol>
 </ol>

# Unix

## diff

Following is a linux command to look at differences between 2 files, 
helps a lot when you want to compare two different versions of a file

The -y command switch, shows the differences between the two files column-wise.

```
diff file1.txt file2.txt -y
```
It shows the changes that need to be made in file 1 to become file 2.
Or put another way, it shows the differences between file 1 and file 2, with respect to file 1. 

The format in which it shows the difference is as follows: >File1LineNumber{add (a),change (c), delete(d)}File2LineNumber
Eg: 1a2, means add line number two from file 2 after file 1 

## split and cat

Uploading files of size > 1 GB to the server using remote access is difficult. So, instead large files can be split into smaller files, uploaded to server and then merged.

```
split -b 200m _CellRangerOutput_matrix.mtx cellRanger_segment.mtx
cat cellRanger_segment.mtx* > _CellRangerOutput_matrix.mtx
rm cellRanger_segment.mtx*
```

# Base R

## grep and grepl

grep returns indices of occurences of PATTERN in TEXT:
  ```r
    grep("virus", c("coronavirus", "rhinovirus", "cyanobacteria")
  ```

grepl indicates whether PATTERN occurs in TEXT as a logical vector
```r
  grepl("virus", c("coronavirus", "rhinovirus", "cyanobacteria")
```

## gsub

Base R function to modify characters.

```r
gsub(pattern = "World", replacement = "Universe", x = "Hello, World!")
```

## apply, lapply, unlist

Checking if the gene expression values conform to a normal distribution. If it does, then we can apply statisical tests such as t-test which assumes the sample is normally distributed. The function unlist is useful in extracting data from the list of lists.

apply is basically for loop, if MARGIN = 1, we are looping over rows, else if MARGIN = 2, we are looping over columns.
lapply goes through each element at a time, so if its a matrix it will keep moving from [1,1] to [2,1], [3,1], ... [1, 2] and so on. Else, if it is a list of lists it will go through one list at a time.

```r
  s <- apply(gExpMatrix, 2, shapiro.test)
  s_pVal <- unlist(lapply(s, function(s){s$p.value}))
  s_W <- unlist(lapply(s, function(s) s$statistic))

  s_pVal_adj <- p.adjust(s_pVal, "BH")
  (length(which(s_pVal_adj <= FDR))/length(s_pVal))*100
```
## sapply

 ```r
    sample_ids <- c("S108B355.BM_10_739","S109B355.BM_10_791","S112B394.BM_10_621")
    barcodes <- sapply(strsplit(sample_ids,"_"), function(x){x[3]})
  ```
  The above returns: 739, 791, 621
  
## ls(packageName) and help(package = packageName)

Lists all the functions in a package.

## stopifnot(condition)

The program stops if the condition inside stopifnot is not evaluated to TRUE.

## match

 ```r
    x <- c("Chennai" ,"Bangalore", "Mumbai")
    y <- c("Bangalore", "Mumbai", "Chennai")
    match(x, y)
    match(y,x)
  ```
The above returns:
3, 1, 2
2, 3, 1

## names

 ```r
   t <- t.test(c(1,2), c(3,4))
   names(t)
   t$p.value
  ```
The above returns:
[1] "statistic"   "parameter"   "p.value"     "conf.int"    "estimate"    "null.value" 
[7] "stderr"      "alternative" "method"      "data.name"
[1] 0.1055728

## str

```r
a <- svd(matrix(rnorm(100), nrow = 25, ncol = 4))
str(a)
```
The above return a compact representation of the structure of the U, sigma, V matrices.

## do.call

```r
readBMfile <- function(fileName){
  file <- read.csv(fileName)
  row.names(file) <- file[,1]
  return(as.data.frame(t(file[,-1])))
}
bm <- do.call(rbind, lapply(list.files(pattern = "geneExp_corrected.csv"), readBMfile))
```
## scale
```r
x <- c(1, 2, 3, 4)
scaled_x <- scale(x)

scaled_x[1] == (x[1] - mean(x))/sqrt(var(x))
```

## sweep
```r
# The following is a function in the package CellCODE
# sweep is used when we want to perform row/column specific operations, as opposed to apply functions where all the rows and columsn are subject to the same operation.
svds <- function(data){
  mm = apply(data,1,mean) # per gene measure
  tmp = sweep(data,1,mm, "-") # mean subtraction
  svd(tmp)
}
```

## model.matrix
```r
sample_gp <- c("Dx", "Ctl", "Ctl", "Ctl", "Ctl", "Dx", "Dx", "Dx")
model.matrix(~1 + sample_gp)
```

## browser

browser() function is used for debugging. It can be called within a function, to explore the different local variables created within a function.

## ifelse

I was today (Sept 27, 2021) years old, when I learnt about the ifelse function
If can only take into account one condition. If else can take into account a vector of conditions.

ifelse(test, what_to_do_if_test_passes, what_to_do_if_test_fails) 

```r
x <- c(9:-4)
sqrt(ifelse(x >= 0, x, NA))
```

## combn

Finds all combinations of the X taken m at a time. Remember that for combinations, the order doesn't matter. For permutations, it does.

```r
combination <- combn(c("BM10", "BM22", "BM36", "BM44"), 2)
apply(combination, 2, function(x){paste0(x[1], "-", x[2])})
```

# Tidyverse

## subset

Subsets based on values, selects particular columns.

```r
head(subset(x = airquality, subset = !is.na(Ozone), select = c(Ozone, Wind, Temp)), n = 10)
```

## mutate

A tidy way to make changes to multiple columns in a dataframe, including the addition of new columns.  
Mutate overwrites the original dataset.

```r
mutate(.data = airquality,
       Ozone = ifelse(is.na(Ozone), mean(Ozone[!is.na(Ozone)]), Ozone)
)
```

## mapvalues

Helps in creating a new vector based on the values from an old vector.

```r
three_idiots <- c("Remember", "that", "silencer", "joke?")
mapvalues(three_idiots, from = "silencer", to = "funny")
```

## with 

## when

## cut

```r
lec6$bmi_cat <- cut(lec6$bmi,
                    breaks = c(0, 18.5, 25, 30, 1000),
                    right = TRUE,
                    labels = c("Underweight", "Healthy weight", "Overweight", "Obesity")
                    )
```

## case_when

## melt

Wrangles a data frame from a wide format to long format. All column names will be in one column, and their correspoding row values in the adjacent column.
id.vars is thee list of columns to ignore.

Also, only numeric values get melted.

```r
a <- cbind(data.frame(matrix(1:10, 5, 2)), rep("Melt", 5))
b <- melt(a, id.vars = 3) # Gives three columns, Var1, Var2, and value

# Var 1 is row index
# Var 2 is column index
# value is the value of a particular entry in the matrix
```

## dcast

Opposite of melt. Convert data from long format to wide format. 

```r

```

## pipes

## merge

## filter

## select

## arrange

## transmute 

## summarise

## group_by

## rename

Rename can be used to rename column names

```r
plyr::rename(airquality, replace = ("Ozone" = "Ozone_quality_index"))
```

# Parallel processing

## detectCores

Detects the number of cores

```r
detectCores()
```

## makeCluster & ParLapply() & stopCluster()

makeCluster is used to create a cluster of cores. ParLapply is the equivalent of lapply for a cluster of cores. It is good practice to stopCluster after usage. Will need to load the snow - Simple Parallel Computing in R package.

## registerDoParallel

## clusterExport

## mclapply

mclapply is part of the parallel package. It can be used to split the elements between multiple cores, to make the function run parallely, hence faster.

```r
ncores <- tectCores()/2
mclapply(lapply_syntax, mc.cores = ncores)   
```

## Sys.time

Estimate time taken by a piece of code.

```r
start.time <- Sys.time()
i <- 0
while(i < 100){
  print(i)
  i = i + 1
}
end.time <- Sys.time()
elapsed.time <- end.time - start.time
```
