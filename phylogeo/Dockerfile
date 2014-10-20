## Start with the rstudio image providing 'base R' as well as RStudio based on Debian testing
FROM rocker/rstudio

## This handle reaches Carl and Dirk
MAINTAINER "Carl Boettiger and Dirk Eddelbuettel" rocker-maintainers@eddelbuettel.com

###############################################################################
# update with rocker/r-devel packages for installing source

## From the Build-Depends of the Debian R package, plus subversion
RUN apt-get update -qq \
&& apt-get install -y --no-install-recommends \
   bash-completion \
   bison \
   debhelper \
   default-jdk \
   g++ \
   gcc \
   gfortran \
   groff-base \
   libblas-dev \
   libbz2-dev \
   libcairo2-dev \
   libjpeg-dev \
   liblapack-dev \
   liblzma-dev \
   libncurses5-dev \
   libpango1.0-dev \
   libpcre3-dev \
   libpng-dev \
   libreadline-dev \
   libtiff5-dev \
   libx11-dev \
   libxt-dev \
   mpack \
   tcl8.5-dev \
   tk8.5-dev \
   x11proto-core-dev \
   xauth \
   xdg-utils \
   xfonts-base \
   xvfb \
   zlib1g-dev 

## Add CRAN binaries and update
RUN echo 'deb http://debian-r.debian.net/debian-r/ unstable main' >> /etc/apt/sources.list \
&& gpg --keyserver keyserver.ubuntu.com --recv-keys AE05705B842492A68F75D64E01BF7284B26DD379 \ 
&& gpg --export AE05705B842492A68F75D64E01BF7284B26DD379  | apt-key add -
#RUN gpg --check-sigs AE05705B842492A68F75D64E01BF7284B26DD379 

## We need the deb-src repositories to use apt-get build-dep
RUN echo 'deb-src http://debian-r.debian.net/debian-r/ unstable main' >> /etc/apt/sources.list \
&& echo 'deb-src http://http.debian.net/debian testing main' >> /etc/apt/sources.list 


## Install build dependencies for R packages with apt-get build-dep 
RUN apt-get update && apt-get build-dep -y \
    r-cran-rgl \
    r-cran-rjava \
    r-cran-rmysql \
    r-cran-rpostgresql \ 
    r-cran-rsqlite \
    r-cran-rsqlite.extfuns \
    r-cran-xml 

## Add BiocInstaller and graph which is needed for igraph
RUN install2.r -r http://bioconductor.org/packages/3.0/bioc --deps TRUE \
    BiocInstaller \
    graph \
    && rm -rf /tmp/downloaded_packages/

# ## Finally ready to install the R packages.  NOTE: failure to install a package doesn't throw an image build error.
# ## Install devtools, ggplot2, dplyr, tidyr + full suggests lists
# ## also install dependencied for phyloseq, DESeq2 and gridExtra
RUN install2.r --error --deps TRUE \
    foreach \
    BatchJobs \
    BBmisc \
    devtools \
    XML \
    xtable \
    RcppArmadillo \
    locfit \
    ade4 \
    ape \
    biom \
    data.table \
    igraph \
    vegan \
    abind \
    tiff \
    jpeg \
    png \
    ggplot2 && \
    rm -rf /tmp/downloaded_packages/

## Add a few github repos where the CRAN version isn't sufficiently recent (e.g. has outstanding bugs) 
RUN installGithub.r hadley/reshape \
&& rm -rf /tmp/downloaded_packages/

# install bioconductor and then update a few other packages for R 3.1.1
RUN install2.r -r http://bioconductor.org/packages/3.0/bioc --deps TRUE \
    EBImage \
    phyloseq \
    && rm -rf /tmp/downloaded_packages/

RUN install2.r --error --deps TRUE \
    gridExtra && \
    rm -rf /tmp/downloaded_packages/

## Add a few github repos where the CRAN version isn't sufficiently recent (e.g. has outstanding bugs) 
RUN installGithub.r zachcp/phylogeo -d FALSE \
&& rm -rf /tmp/downloaded_packages/