# R functions


### Read all tables in a given `dPath` matching `dPattern`, add the file names as a new column, merge all into one list
```
getTables<-function(dPath,dPattern){
  require(plyr)
  if (!endsWith(dPath,"/")){
    dPath=paste0(dPath,"/")
  }
  filenames<-paste0(dPath,list.files(dPath, pattern = dPattern))
  mydata = ldply(filenames, function(filename) {
    di = read.table(filename,header=F)
    di$filename = basename(filename)
    return(di)
    })
}
```

Usage:
`mergedData<-getTables("/home/isin/myDir/","\\txt$")`

___


### ipak function: install and load multiple R packages.
#### Check if packages are installed; install if they are not, then load them into the R session.
#### credit: https://gist.github.com/stevenworthington/3178163
```
ipak <- function(pkg){
  new.pkg <- pkg[!(pkg %in% installed.packages()[, "Package"])]
  if (length(new.pkg)) 
    install.packages(new.pkg, dependencies = TRUE)
  sapply(pkg, require, character.only = TRUE)
}
```
Usage:
```ipak(c("ggplot2","dplyr")```


___

### Check string if it is a "proper" directory

```
asDirPath <- function(string){
  if(!endsWith(string,"/"))string=paste0(string,"/")
  if(!dir.exists(string))warning("Directory does not exist, will exit!")
  return(string)
}
```


___

### Check if file is empty
```
fileIsEmpty <- function(filenames) file.info(filenames)$size == 0
```
