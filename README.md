# Recording useful one liners

## Contents
- diff
- grep and grepl
- apply, sapply, tapply
- order
- ave
- ls()
- stopifnot()
- match()

## diff

Following is a linux command to look at differences between 2 files, 
helps a lot when you want to compare two different versions of a file

```
diff file1.txt file2.txt
```
It shows the changes that need to be made in file 1 to become file 2.
Or put another way, it shows the differences between file 1 and file 2, with respect to file 1. 

The format in which it shows the difference is as follows: >File1LineNumber{add (a),change (c), delete(d)}File2LineNumber
Eg: 1a2, means add line number two from file 2 after file 1 

## grep and grepl

grep returns indices of occurences of PATTERN in TEXT:
  ```r
    grep("virus", c("coronavirus", "rhinovirus", "cyanobacteria")
  ```

grepl indicates whether PATTERN occurs in TEXT as a logical vector
```r
  grepl("virus", c("coronavirus", "rhinovirus", "cyanobacteria")
```

## sapply

 ```r
    sample_ids <- c("S108B355.BM_10_739","S109B355.BM_10_791","S112B394.BM_10_621")
    barcodes <- sapply(strsplit(sample_ids,"_"), function(x){x[3]})
  ```
  The above returns: 739, 791, 621

## ls(packageName)

Lists all the functions in a package.

## stopifnot(condition)

The program stops if the condition inside stopifnot is not evaluated to TRUE.

## match(vector1, vector2)

x <- c("Chennai" ,"Bangalore", "Mumbai")
y <- c("Bangalore", "Mumbai", "Chennai")

match(x,y) gives 3, 1, 2
match(y,x) gives 2, 3, 1
