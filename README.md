# gem5

This is a fork of the official [gem5 repo](https://gem5.googlesource.com/public/gem5/) that implements the GPU architecture for [PWS](https://dl.acm.org/doi/10.1145/3609102).

## Docker Setup Guide

The instructions for setting up the docker container for running the GPU model are found [here](https://www.gem5.org/documentation/general_docs/gpu_models/GCN3).
This document assumes you already have docker installed on your machine.
If using Windows, WSL2 and Docker tend to consume all available resources, so it may be worthwhile to [limit the resources](https://dev.to/tallesl/vmmen-process-consuming-too-much-memory-docker-desktop-273p) available to WSL2.

### First Time Running Container

If it is the first time running the container, you need to first pull the image and create the container:

1. `docker pull gcr.io/gem5-test/gcn-gpu:v21-2`
2. `docker run -it gcr.io/gem5-test/gcn-gpu:v21-2`

The `docker run` command will attach the user to the container instance.
To exit the container, enter the command `exit`.

### Subsequent Accesses

If you have already created the container instance, there is no need to create another with `docker run`; instead, you can use `docker start`:

1. Find the container name using `docker container ls -a`. You can use either the value in the `CONTAINER ID` or the `NAMES` field.
2. `docker container start -ia <container_name>`.

To exit the container, enter the command `exit`.

## gem5 Guide

### Building

The guide to building gem5 is found [here](https://www.gem5.org/documentation/general_docs/building).
The basic command to build the AMD APU system is:

```
scons -j8 build/GCN3_X86/gem5.opt --ignore-style
```

Where we assume the host machine can support 8 parallel threads of build execution.
Use command `nproc` to see the number of available processors and for a good estimate of the number of threads to supply to the `-j` flag.

### Running the Simulator

The gem5 GPU model [documentation](https://www.gem5.org/documentation/general_docs/gpu_models/GCN3) gives the [square](https://gem5.googlesource.com/public/gem5-resources/+/refs/heads/stable/src/gpu/square/) benchmark as a good starting point.
From there, we can deduce that the general command to run the simulator is:

```
build/GCN3_X86/gem5.opt configs/example/apu_se.py -n 3 -c <benchmark_path> [-o "<options>"]
```

### Running Benchmarks

To run example applications on the simulator, we recommend the HIP examples found [here](https://github.com/ROCm-Developer-Tools/HIP-Examples).
