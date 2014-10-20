# Docker Image for phylogeo 
This Dockerfile uses parts of several of the [rocker](https://registry.hub.docker.com/repos/rocker/) builds and adds
the neccesary [Bioconductor](http://www.bioconductor.org/) packages to build [phyloseq](https://joey711.github.io/phyloseq/) and [phylogeo](http://zachcp.github.io/phylogeo/). 

The docker build provides an RStudio interface where the phylogeo package is ready to be used. As per the [rocker/rstudio instructions](https://github.com/rocker-org/rocker/wiki/Using-the-RStudio-Image), you will need to start the docker image, get the ipaddress and then login:

```
# get the iP address
boot2docker ip
# download and run the phylogeo image
# (takes awhile the first time)
sudo docker run -d -p 8787:8787 zachcp/phylogeo
# visit the ipaddress at the default port
http://[ipaddress]:8787
# login with username/password
username: rstudio
password: rstudio
```