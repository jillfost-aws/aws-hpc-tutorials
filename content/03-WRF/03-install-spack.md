---
title: "c. Install Spack"
weight: 23
tags: ["tutorial", "pcluster-manager", "ParallelCluster", "Spack"]
---

![Spack Logo](/images/wrf/spack.svg)

[Spack](https://spack.io/) is a package manager for supercomputers, Linux, and macOS. It makes installing scientific software easy. Spack isn’t tied to a particular language; you can build a software stack in Python or R, link to libraries written in C, C++, or Fortran, and easily swap compilers or target specific microarchitectures. In this tutorial we'll use Spack to compile and install weather codes.

First, on the head node - which we [connected to via SSM or DCV](02-connect-cluster.html) we'll run:

```bash
sudo su
export SPACK_ROOT=/shared/spack
mkdir -p $SPACK_ROOT
git clone -c feature.manyFiles=true https://github.com/spack/spack $SPACK_ROOT
cd $SPACK_ROOT
exit
echo "export SPACK_ROOT=/shared/spack" >> $HOME/.bashrc
echo "source $SPACK_ROOT/share/spack/setup-env.sh" >> $HOME/.bashrc
source $HOME/.bashrc
sudo chown -R $USER:$USER $SPACK_ROOT
```