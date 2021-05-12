# Helpful scripts for creating R packages

## Working directory structure

- Package

<!-- AUTO-GENERATED-CONTENT:START (DIRTREE:dir=./&depth=1) -->
```
. (Working directory)
├── [Your package]/
│           ├── R/
│           ├── inst/
│           │     ├── extdata/
│           │     ├── images/
│           │     └── script/
│           ├── man/
│           ├── vignettes/
│           ├── DESCRIPTION
│           ├── NAMESPACE
│           ├── NEWS
│           └── README.md
├── build_test.R (any R file, a script to build your package)
├── R.Rproj (any R project)
├── github.command (a script to puch your package to GitHub)
├── hoge_dir1/
└── hoge_dir2/
```
<!-- AUTO-GENERATED-CONTENT:END -->

- AnnotationHub / ExperimentHub (Bioconductor)

<!-- AUTO-GENERATED-CONTENT:START (DIRTREE:dir=./&depth=1) -->
```
. (Working directory)
├── [Your package]/
│           ├── inst/
│           │     ├── extdata/
│           │     └── script/
│           ├── man/
│           ├── vignettes/
│           ├── DESCRIPTION
│           ├── NAMESPACE
│           ├── NEWS
│           └── README.md
├── build_test.R (any R file, a script to build your package)
├── R.Rproj (any R project)
├── github.command (a script to puch your package to GitHub)
├── hoge_dir1/
└── hoge_dir2/
```
<!-- AUTO-GENERATED-CONTENT:END -->

## build_test.R

- install a local R package on R (with deleting variables in your environment)

```r
#XXX
```

- reinstall a local R package on R (without deleting any variables in your environment)

```r
#XXX
```

## Batch file (.command) to uploads files to GitHub.

- [github.command](https://github.com/kumeS/Helpful-scripts-for-creating-R-packages/blob/main/github.command)

```sh
#!/bin/bash

# If your need any proxy setting
# export https_proxy="http://..."
# export ftp_proxy="http://..."

MY_DIRNAME=$(dirname $0)
cd $MY_DIRNAME

cd ./[Your GitHub Directory]

# If you use MacOSX
#du -a | grep .DS_Store | xargs rm -rf

git add -A

git commit -m "Your submit comment"
 
git push origin main

git config pull.rebase false

git pull

open "[Your GitHub URL]"

exit
```

- .gitignore

```
.Rproj.user
.Rhistory
.RData
.Ruserdata
.DS_Store

```

## Run the R-devel of Docker / Bioconductor on R

```r
system("docker run \\
        -d \\
        --name bioc \\
        -v $(pwd)/[Your package]://home/rstudio/[Your package] \\
        -e PASSWORD=bioc \\
        -p 8787:8787 \\
        bioconductor/bioconductor_docker:devel")
#check CONTAINER
system("docker ps")

#Open the RStudio-server
system("open http://localhost:8787")
#ID: rstudio, PW: bioc

#Stop the container
system("docker stop [CONTAINER ID]")
```

## Run within Bioc Docker (ex. AHBioImageDbs_0.99.1)

- install the packages in Docker/Bioconductor

```r
BiocManager::install("AnnotationHub")
BiocManager::install("ExperimentHub")
BiocManager::install("BiocStyle")
BiocManager::install("BiocCheck")

#check their loads
library(AnnotationHub)
library(ExperimentHub)
```

- (If you need) install tex/pdfLatex on Terminal window by Root User

```sh
sudo apt-get update
sudo apt-get install texlive-latex-base texlive-latex-extra texinfo
sudo apt-get install texlive-fonts-recommended texlive-fonts-extra
```

- install test for BioImageDbs package

```r
system("R CMD INSTALL AHBioImageDbs")
system("R CMD build --keep-empty-dirs --no-resave-data AHBioImageDbs")
system("R CMD check --no-vignettes --timings --no-multiarch AHBioImageDbs_0.99.1.tar.gz")
BiocCheck::BiocCheck("./AHBioImageDbs_0.99.1.tar.gz")
```

## For Submit




