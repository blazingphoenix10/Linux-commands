# Recording linux commands of use

## Contents
- diff
- grep and grepl

## diff

Following is a linux command to look at differences between 2 files, 
helps a lot when you want to compare two different versions of a file

```
diff file1.txt file2.txt
```

## grep and grepl

grep returns indices of occurences of PATTERN in TEXT:
  ```r
    grep("virus", c("coronavirus", "rhinovirus", "cyanobacteria")
  ```

grepl indicates whether PATTERN occurs in TEXT as a logical vector
```r
  grepl("virus", c("coronavirus", "rhinovirus", "cyanobacteria")
```
