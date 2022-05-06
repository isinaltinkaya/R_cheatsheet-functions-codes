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


### LIBRARY function: auto install and load multiple R packages.
#### Check if packages are installed; install if they are not, then load them into the R session.
```
LIBRARY<-function(package) ifelse(t =!(x %in% installed.packages()), install.packages(x[t],dependencies=T),lapply(x, require))
```
Usage:
```
packages<-c("ggplot2","dplyr")
packages %>% LIBRARY
LIBRARY("ggplot2")
```
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


___

### Convert columns of a dataframe to factor
```
dff<-function (df,...){
  colname=c(...)
  if(is.null(colname)){
    df[] <- lapply( df, factor)
    return(df)
  }else{
    df[c(colname)] <- lapply( df[c(colname)], factor) 
    return(df)
  }
}
```
Usage:
```
#convert all to factor
dff(my_dataframe) 

#convert specific columns to factor
dff(my_dataframe,c("column_id_1","column_id_2")) 
```
