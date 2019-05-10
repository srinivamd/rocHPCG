# rocHPCG
rocHPCG is a benchmark based on the [HPCG][] benchmark application, implemented on top of AMD's Radeon Open Compute [ROCm][] runtime and toolchains. rocHPCG is created using the [HIP][] programming language and optimized for AMD's latest discrete GPUs.

## Requirements
* Git
* CMake (3.5 or later)
* MPI
* NUMA library
* AMD [ROCm] 2.2 platform
* rocRAND

## Quickstart rocHPCG build and install

#### Install script
You can build rocHPCG using the *install.sh* script
```
# Clone rocHPCG using git
git clone https://github.com/ROCmSoftwarePlatform/rocHPCG.git

# Go to rocHPCG directory
cd rocHPCG

# Run install.sh script
# Command line options:
#    -h|--help         - prints this help message
#    -i|--install      - install after build
#    -d|--dependencies - install dependencies
#    -r|--reference    - reference mode
#    -g|--debug        - -DCMAKE_BUILD_TYPE=Debug (default: Release)
#    -t|--test         - build HPCG test application for single GPU
#    --with-mpi        - compile with MPI support (default: enabled)
#    --with-openmp     - compile with OpenMP support (default: enabled)
#    --with-memmgmt    - compile with smart memory management (default: enabled)
#    --with-memdefrag  - compile with memory defragmentation (defaut: enabled)
./install.sh -di
```

## Running rocHPCG benchmark application
You can run the rocHPCG benchmark application by either using command line parameters or the `hpcg.dat` input file
```
rochpcg <nx> <ny> <nz> <runtime>
# where
# nx      - is the global problem size in x dimension
# ny      - is the global problem size in y dimension
# nz      - is the global problem size in z dimension
# runtime - is the desired benchmarking time in seconds (> 1800s for official runs)
```

Similarly, these parameters can be entered into an input file `hpcg.dat` in the working directory, e.g. `nx = ny = nz = 280` and `runtime = 1800`.
```
HPCG benchmark input file
Sandia National Laboratories; University of Tennessee, Knoxville
280 280 280
1800
```

## Performance evaluation
For performance evaluation purposes, the number of iterations should be as low as possible (e.g. convergence rate as high as possible), since the final HPCG score is scaled to 50 iterations.
Furthermore, it is observed that high memory occupancy performs better on our devices. My problem size suggestion for devices with 16GB is `nx = ny = nz = 280`, runtime for official runs have to be at least 1800 seconds, e.g.
```
./rochpcg 280 280 280 1860
```
Please note that convergence rate behaviour might change in a multi-GPU environment and need to be adjusted accordingly.

## Support
Please use [the issue tracker][] for bugs and feature requests.

[HPCG]: https://www.hpcg-benchmark.org/
[ROCm]: https://github.com/RadeonOpenCompute/ROCm
[HIP]: https://github.com/GPUOpen-ProfessionalCompute-Tools/HIP/
[the issue tracker]: https://github.com/ROCmSoftwarePlatform/rocHPCG/issues
