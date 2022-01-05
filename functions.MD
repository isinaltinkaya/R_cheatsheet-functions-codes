# R functions


### Read all tables in a given `dPath` matching `dPattern`, add the file names as a new column, merge all into one list
```
getTables<-function(dPath,dPattern){
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


### Get package: If package exists load the package, if not try to install and load

```
getp <- function (packageName){
  packageName<-deparse(substitute(packageName))
  if (!require(packageName, character.only = TRUE)) {
    if(require(packageName,quietly = T,character.only = T)){
      print(paste0("Package ", packageName, " is available to install."))
      install.packages(packageName, dependencies = TRUE)
      library(packageName, character.only = TRUE,verbose = TRUE)
    }else{
      print(paste0("Package ", packageName, " is not found."))
    }
  } else {
    library(packageName, character.only = TRUE,verbose = TRUE)
  }
}
```



Usage:
```getp(ggplot2)```

