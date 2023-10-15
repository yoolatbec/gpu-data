# tianshu-a100-data
## MLPerf
### training
#### image\_segmentation
| | tianshu | a100 |
|-------|-----|-----|
| 单卡 | 大约34个epoch一天 | 4000 epoches, time: 1514.2363 |

在使用天数GPU进行训练的时候，如果不指定使用特定的GPU，那么进程会访问所有的GPU，即使只使用一张GPU。如果指定了使用的GPU，那么进程就只使用指定的GPU。但是训练速度没有变快。

## ecp proxy app suite
### ExaMiniMD

编译报错：
```
error: couldn't allocate input reg for constraint 'l' at line 2156113530
clang-13: error: fatbinary command failed with exit code 1 (use -v to see invocation)
```

### QuickSilver

天数编译器在汇编阶段执行了链接操作？
```
lld: error: undefined symbol: getGlobalThreadID()
>>> referenced by /tmp/main-b5df91-7e8b75.o:(CycleTrackingKernel(MonteCarlo*, int, ParticleVault*, ParticleVault*))
>>> referenced by /tmp/main-b5df91-7e8b75.o:(CycleTrackingKernel(MonteCarlo*, int, ParticleVault*, ParticleVault*))

lld: error: undefined symbol: CycleTrackingGuts(MonteCarlo*, int, ParticleVault*, ParticleVault*)
>>> referenced by /tmp/main-b5df91-7e8b75.o:(CycleTrackingKernel(MonteCarlo*, int, ParticleVault*, ParticleVault*))
>>> referenced by /tmp/main-b5df91-7e8b75.o:(CycleTrackingKernel(MonteCarlo*, int, ParticleVault*, ParticleVault*))
```

-----------2023.10.8------------

需要手动编译main.o，并手动链接成可执行文件

无法运行。

```
...
CrossSection:
   name: flat
   A: 0
   B: 0
   C: 0
   D: 0
   E: 1
   nuBar: 2.4
Segmentation fault (core dumped)
```

a100+nvcc:
```
main.cc: In function ‘void cycleTracking(MonteCarlo*)’:
main.cc:199:29: error: expected primary-expression before ‘<’ token
  199 |        CycleTrackingKernel<<<1, 1>>>(monteCarlo, numParticles, processingVault, processedVault);
      |                             ^
main.cc:199:36: error: expected primary-expression before ‘>’ token
  199 |        CycleTrackingKernel<<<1, 1>>>(monteCarlo, numParticles, processingVault, processedVault);
      |                                    ^
make: *** [Makefile:324: main.o] Error 1
```

a100+clang:
```
ptxas fatal   : Unresolved extern function '_ZN11NuclearData18getNumberReactionsEj'
clang-14: error: ptxas command failed with exit code 255 (use -v to see invocation)
```

### ExaMPM

依赖于Cabana

--------------------2023.10.15-------------------------

```
...
error: couldn't allocate input reg for constraint 'l' at line 2155701072
error: couldn't allocate input reg for constraint 'l' at line 2155701072
error: couldn't allocate input reg for constraint 'l' at line 2155701072
error: couldn't allocate input reg for constraint 'l' at line 2155701072
error: couldn't allocate input reg for constraint 'l' at line 2155701072
error: couldn't allocate input reg for constraint 'l' at line 2155701072
error: couldn't allocate input reg for constraint 'l' at line 2155701072
error: couldn't allocate input reg for constraint 'l' at line 2155701072
error: couldn't allocate input reg for constraint 'l' at line 2155701072
error: couldn't allocate input reg for constraint 'l' at line 2155701072
error: couldn't allocate input reg for constraint 'l' at line 2155701072
error: couldn't allocate input reg for constraint 'l' at line 2155701072
error: couldn't allocate input reg for constraint 'l' at line 2155701072
error: couldn't allocate input reg for constraint 'l' at line 2155672682
error: couldn't allocate input reg for constraint 'l' at line 2155680660
error: couldn't allocate input reg for constraint 'l' at line 2155680660
error: couldn't allocate input reg for constraint 'l' at line 2155680660
clang-13: error: fatbinary command failed with exit code 1 (use -v to see invocation)
```

### SNAP

### CabanaPIC

编译出错：
```
error: couldn't allocate input reg for constraint 'l' at line 2155763452
error: couldn't allocate input reg for constraint 'l' at line 2155763452
error: couldn't allocate input reg for constraint 'l' at line 2155763452
error: couldn't allocate input reg for constraint 'l' at line 2155794489
error: couldn't allocate input reg for constraint 'l' at line 2155783864
clang-13: error: fatbinary command failed with exit code 1 (use -v to see invocation)
```

单独编译仍然报错

a100:

编译kokkos时报错：
```
CMake Error at /mnt/sda/2022-0526/home/lhz/applications/spack/opt/spack/linux-debian11-zen2/gcc-10.2.1/cmake-3.23.2-ey7elcxx7apq23c3kxcsemxuuivmyaf4/share/cmake-3.23/Modules/FindPackageHandleStandardArgs.cmake:230 (message):
  Could NOT find TPLLIBDL (missing: LIBDL_FOUND_LIBRARIES)
Call Stack (most recent call first):
  /mnt/sda/2022-0526/home/lhz/applications/spack/opt/spack/linux-debian11-zen2/gcc-10.2.1/cmake-3.23.2-ey7elcxx7apq23c3kxcsemxuuivmyaf4/share/cmake-3.23/Modules/FindPackageHandleStandardArgs.cmake:594 (_FPHSA_FAILURE_MESSAGE)
  cmake/kokkos_functions.cmake:757 (FIND_PACKAGE_HANDLE_STANDARD_ARGS)
  cmake/Modules/FindTPLLIBDL.cmake:1 (KOKKOS_FIND_IMPORTED)
  cmake/kokkos_functions.cmake:288 (FIND_PACKAGE)
  cmake/kokkos_tpls.cmake:81 (KOKKOS_IMPORT_TPL)
  cmake/kokkos_tribits.cmake:248 (INCLUDE)
  CMakeLists.txt:218 (KOKKOS_SETUP_BUILD_ENVIRONMENT)
```

但是具体缺少哪个库不是很清楚

---------------2023.10.10-------------------

缺少dl库，安装完整的c工具链即可

### E3SM-kernels

### RIOPA

### GAMESS_RI-MP2_MiniApp

### HyPar

可以编译，可以正常执行

#### 编译

在编译时需要修改Makefile，原Makefile文件预设使用nvcc进行编译，并且将host端代码和device端代码分开编译，并将其分别链接成两个静态库。

如果使用clang进行编译时，直接将host端代码和device端代码链接到同一个静态库中，因此需要在Makefile中修改目标文件的依赖。

在链接为静态库时可以直接使用ar。

#### 3D Navier-Stokes Equations - Rising Thermal Bubble 

运行结果：
```
HyPar - Parallel (MPI) version with 1 processes
Allocated simulation object(s).
Reading solver inputs from file "solver.inp".
  No. of dimensions                          : 3
  No. of variables                           : 5
  Domain size                                : 64 64 64 
  Processes along each dimension             : 1 1 1 
  Exact solution domain size                 : 64 64 64 
  No. of ghosts pts                          : 3
  No. of iter.                               : 8000
  Restart iteration                          : 0
  Time integration scheme                    : rk (44)
  Spatial discretization scheme (hyperbolic) : weno5
  Split hyperbolic flux term?                : no
  Interpolation type for hyperbolic term     : components
  Spatial discretization type   (parabolic ) : nonconservative-1stage
  Spatial discretization scheme (parabolic ) : 2
  Time Step                                  : 2.500000E-02
  Check for conservation                     : no
  Screen output iterations                   : 100
  File output iterations                     : 500
  Initial solution file type                 : binary
  Initial solution read mode                 : parallel  [1 file IO rank(s)]
  Solution file write mode                   : serial
  Solution file format                       : binary
  Overwrite solution file                    : no
  Use GPU                                    : yes
  GPU device no                              : -1
  Physical model                             : navierstokes3d
Partitioning domain and allocating data arrays.
Reading from binary file initial_par.inp.xxx (parallel mode).
Volume integral of the initial solution:
 0:  1.1686650873137891E+09
 1:  0.0000000000000000E+00
 2:  0.0000000000000000E+00
 3:  0.0000000000000000E+00
 4:  2.4758406164999706E+14
Reading boundary conditions from boundary.inp.
  Boundary                      slip-wall:  Along dimension  0 and face +1
  Boundary                      slip-wall:  Along dimension  0 and face -1
  Boundary                      slip-wall:  Along dimension  1 and face +1
  Boundary                      slip-wall:  Along dimension  1 and face -1
  Boundary                      slip-wall:  Along dimension  2 and face +1
  Boundary                      slip-wall:  Along dimension  2 and face -1
6 boundary condition(s) read.
Initializing solvers.
Reading WENO parameters from weno.inp.
Initializing physics. Model = "navierstokes3d"
Reading physical model inputs from file "physics.inp".
Setting up time integration.
Solving in time (from 0 to 8000 iterations)
Writing solution file op_00000.bin.
iter=    100  t=2.500E+00  wctime: 3.2E-02 (s)  
iter=    200  t=5.000E+00  wctime: 3.2E-02 (s)  
iter=    300  t=7.500E+00  wctime: 3.2E-02 (s)  
iter=    400  t=1.000E+01  wctime: 3.2E-02 (s)  
iter=    500  t=1.250E+01  wctime: 3.2E-02 (s)  
... 
Writing solution file op_00015.bin.
iter=   7600  t=1.900E+02  wctime: 3.2E-02 (s)  
iter=   7700  t=1.925E+02  wctime: 3.2E-02 (s)  
iter=   7800  t=1.950E+02  wctime: 3.2E-02 (s)  
iter=   7900  t=1.975E+02  wctime: 3.2E-02 (s)  
iter=   8000  t=2.000E+02  wctime: 3.2E-02 (s)  
Completed time integration (Final time: 200.000000), total wctime: 253.470560 (seconds).
Writing solution file op_00016.bin.
Solver runtime (in seconds): 2.5370770700000000E+02
Total  runtime (in seconds): 2.5394644400000001E+02
Deallocating arrays.
Finished.
```

显存占用约1G。

V100对比：
```
Completed time integration (Final time: 200.000000), total wctime: 169.173154 (seconds).
```

a100+nvcc:
默认指定了-arch=sm_70，需要手动修改Makefile

```
HyPar - Parallel (MPI) version with 1 processes
Allocated simulation object(s).
Reading solver inputs from file "solver.inp".
  No. of dimensions                          : 3
  No. of variables                           : 5
  Domain size                                : 64 64 64 
  Processes along each dimension             : 1 1 1 
  Exact solution domain size                 : 64 64 64 
  No. of ghosts pts                          : 3
  No. of iter.                               : 8000
  Restart iteration                          : 0
  Time integration scheme                    : rk (44)
  Spatial discretization scheme (hyperbolic) : weno5
  Split hyperbolic flux term?                : no
  Interpolation type for hyperbolic term     : components
  Spatial discretization type   (parabolic ) : nonconservative-1stage
  Spatial discretization scheme (parabolic ) : 2
  Time Step                                  : 2.500000E-02
  Check for conservation                     : no
  Screen output iterations                   : 100
  File output iterations                     : 500
  Initial solution file type                 : binary
  Initial solution read mode                 : parallel  [1 file IO rank(s)]
  Solution file write mode                   : serial
  Solution file format                       : binary
  Overwrite solution file                    : no
  Use GPU                                    : yes
  GPU device no                              : -1
  Physical model                             : navierstokes3d
Partitioning domain and allocating data arrays.
Reading from binary file initial_par.inp.xxx (parallel mode).
Volume integral of the initial solution:
 0:  1.1686650873137891E+09
 1:  0.0000000000000000E+00
 2:  0.0000000000000000E+00
 3:  0.0000000000000000E+00
 4:  2.4758406164999706E+14
Reading boundary conditions from boundary.inp.
  Boundary                      slip-wall:  Along dimension  0 and face +1
  Boundary                      slip-wall:  Along dimension  0 and face -1
  Boundary                      slip-wall:  Along dimension  1 and face +1
  Boundary                      slip-wall:  Along dimension  1 and face -1
  Boundary                      slip-wall:  Along dimension  2 and face +1
  Boundary                      slip-wall:  Along dimension  2 and face -1
6 boundary condition(s) read.
Initializing solvers.
Reading WENO parameters from weno.inp.
Initializing physics. Model = "navierstokes3d"
Reading physical model inputs from file "physics.inp".
Setting up time integration.
Solving in time (from 0 to 8000 iterations)
Writing solution file op_00000.bin.
iter=    100  t=2.500E+00  wctime: 1.1E-02 (s)  
iter=    200  t=5.000E+00  wctime: 1.1E-02 (s)  
iter=    300  t=7.500E+00  wctime: 1.1E-02 (s)  
iter=    400  t=1.000E+01  wctime: 1.1E-02 (s)  
iter=    500  t=1.250E+01  wctime: 1.1E-02 (s)  
Writing solution file op_00001.bin.
...
iter=   7600  t=1.900E+02  wctime: 1.1E-02 (s)  
iter=   7700  t=1.925E+02  wctime: 1.1E-02 (s)  
iter=   7800  t=1.950E+02  wctime: 1.1E-02 (s)  
iter=   7900  t=1.975E+02  wctime: 1.1E-02 (s)  
iter=   8000  t=2.000E+02  wctime: 1.1E-02 (s)  
Completed time integration (Final time: 200.000000), total wctime: 91.371791 (seconds).
Writing solution file op_00016.bin.
Solver runtime (in seconds): 9.1693696000000003E+01
Total  runtime (in seconds): 9.2397503999999998E+01
Deallocating arrays.
Finished.

```

显存占用约1.3G。

结果的sha256值不同。

#### 3D Navier-Stokes Equations - Isotropic Turbulence Decay 

单卡显存不足

### FFTX

#### spiral-software

依赖于spiral-software库，编译该库(checkforGpu可执行文件)时出错：
```
/usr/bin/ld: cannot find -lculibos: No such file or directory
```

手动链接时不要使用
```
-lcudart_static
```

---------------2023.10.8-----------------

```
CMake Error at /usr/local/corex-3.0.0/share/cmake-3.21/Modules/CMakeDetermineCUDACompiler.cmake:293 (message):
  CMAKE_CUDA_ARCHITECTURES must be set to ivcore10 or ivcore11.
Call Stack (most recent call first):
  src/library/CMakeLists.txt:14 (project)
```

将CMAKE_CUDA_ARCHITECTURES设置为ivcore10后报错：
```
clang: error: unsupported CUDA gpu architecture: ivcore10
```

### Goulash

#### rush_larsen_gpu_cuda

可以编译，可以运行，但计算结果有问题：
```
  0.000 (0.000s): --------------- Begin rush_larsen_gpu_cuda [clang++] (timer zeroed) ---------------
  0.000 (0.000s): START Rush Larsen 100 10.00000000  cells 671088640  gpu_cuda [clang++]
  0.000 (0.000s): Version 2.0 RC1 (7/21/21)
  0.000 (0.000s): Selecting GPU 0 as default device
  0.009 (0.009s): Launching CUDA GPU cudaMalloc test
  0.009 (0.000s): Verified cudaMalloc worked on GPU
  0.009 (0.000s): Allocating and initializing kernel arrays
  0.009 (0.000s): Starting cudaMalloc of GPU arrays
  0.009 (0.000s): Finished cudaMalloc of GPU arrays
  0.009 (0.000s): Starting cudaMemcpy of CPU arrays to GPU arrays
  0.843 (0.833s): Finished cudaMemcpy of CPU arrays to GPU arrays
  0.843 (0.000s): Launching warmup iteration (not included in kernel timings)
  0.871 (0.028s): Starting kernel timings for Rush Larsen 100 10.00000000
  0.871 (0.000s): Starting iteration      1
  1.020 (0.149s): Starting iteration     11
  1.169 (0.149s): Starting iteration     21
  1.317 (0.149s): Starting iteration     31
  1.466 (0.149s): Starting iteration     41
  1.615 (0.149s): Starting iteration     51
  1.764 (0.149s): Starting iteration     61
  1.913 (0.149s): Starting iteration     71
  2.062 (0.149s): Starting iteration     81
  2.211 (0.149s): Starting iteration     91
  2.360 (0.149s): Finished kernel timings for Rush Larsen 100 10.00000000
  2.360 (0.000s): RUSHSTATS  Rush Larsen 100 10.00000000  1.4886 s  14886.19 us/iter  0.833 s datatrans gpu_cuda [clang++]
  2.582 (0.222s): Starting data check for sanity and consistency
  2.582 (0.000s): ERROR Data sanity check m_gate[0]=0.007582572281482 < 0.506796 (0.506796353074569 min expected value) gpu_cuda [clang++]
  2.824 (0.242s): ERROR Data consistency check m_gate[335544320]=0.000000000000000 != m_gate[0]=0.007582572281482 gpu_cuda [clang++]
  2.824 (0.000s): ERROR Data consistency check m_gate[335544321]=0.000000000000000 != m_gate[0]=0.007582572281482 gpu_cuda [clang++]
  2.824 (0.000s): ERROR Data consistency check m_gate[335544322]=0.000000000000000 != m_gate[0]=0.007582572281482 gpu_cuda [clang++]
  2.824 (0.000s): ERROR Data consistency check REMAINING ERROR MESSAGES SUPPRESSED! gpu_cuda [clang++]
  3.421 (0.598s): FAILED Data check 100 10.00000000  with 335544321 DATA CHECK ERRORS m_gate[0]=0.007582572281482 gpu_cuda [clang++]
  3.536 (0.115s): DONE Freed memory gpu_cuda [clang++]
  3.536 (0.000s): ----------------- End rush_larsen_gpu_cuda [clang++] ---------------
```

#### rush_larsen_gpu_lambda_cuda

可以编译运行，但计算结果有问题：
```
  0.000 (0.000s): --------------- Begin rush_larsen_gpu_lambda_cuda [clang] (timer zeroed) ---------------
  0.000 (0.000s): START Rush Larsen 100 10.00000000  cells 671088640  gpu_lambda_cuda [clang]
  0.000 (0.000s): Version 2.0 RC1 (7/21/21)
  0.000 (0.000s): Selecting GPU 0 as default device
  0.006 (0.006s): Launching CUDA GPU cudaMalloc test
  0.006 (0.000s): Verified cudaMalloc worked on GPU
  0.006 (0.000s): Allocating and initializing kernel arrays
  0.006 (0.000s): Starting cudaMalloc of GPU arrays
  0.007 (0.001s): Finished cudaMalloc of GPU arrays
  0.007 (0.000s): Starting cudaMemcpy of CPU arrays to GPU arrays
  1.681 (1.675s): Finished cudaMemcpy of CPU arrays to GPU arrays
  1.681 (0.000s): Launching warmup iteration (not included in kernel timings)
  1.737 (0.055s): Starting kernel timings for Rush Larsen 100 10.00000000
  1.737 (0.000s): Starting iteration      1
  1.824 (0.088s): Starting iteration     11
  1.912 (0.088s): Starting iteration     21
  2.000 (0.088s): Starting iteration     31
  2.088 (0.088s): Starting iteration     41
  2.176 (0.088s): Starting iteration     51
  2.265 (0.089s): Starting iteration     61
  2.353 (0.088s): Starting iteration     71
  2.441 (0.088s): Starting iteration     81
  2.529 (0.088s): Starting iteration     91
  2.617 (0.088s): Finished kernel timings for Rush Larsen 100 10.00000000
  2.617 (0.000s): RUSHSTATS  Rush Larsen 100 10.00000000  0.8803 s  8802.86 us/iter  1.675 s datatrans gpu_lambda_cuda [clang]
  3.047 (0.430s): Starting data check for sanity and consistency
  3.047 (0.000s): ERROR Data sanity check m_gate[0]=0.000000000000000 < 0.506796 (0.506796353074569 min expected value) gpu_lambda_cuda [clang]
  3.447 (0.400s): FAILED Data check 100 10.00000000  with 1 DATA CHECK ERRORS m_gate[0]=0.000000000000000 gpu_lambda_cuda [clang]
  3.666 (0.219s): DONE Freed memory gpu_lambda_cuda [clang]
  3.666 (0.000s): ----------------- End rush_larsen_gpu_lambda_cuda [clang] ---------------
```

### IAMR

cpp文件中使用了cmath头文件中的max函数，但天数编译器将它识别成了cuda库中的函数，导致编译出错，需要手动修改为fmax函数。

源文件中CUDA_VERSION被定义为一个const char[]变量，但在天数的cuda库中CUDA_VERSION被定义为一个宏。

链接阶段会调用nvcc，但nvcc不能识别makefile给的flag，因此会失败，但实际上nvcc调用了/usr/bin/ld，因此可以使用gcc来链接：
```
gcc path/to/*.o -L/usr/local/corex/lib -lm -lcudart -lixlogger -lixthunk -lstdc++ -lgfortran -no-pie
```

#### cudaMallocManaged

天数GPU不支持统一内存，调用cudaMallocManaged会出错
```
Initializing CUDA...
CUDA initialized with 1 device.
amrex::Abort::0::CUDA error 31 in file /mnt/nvme0/home/lianghz/workspace/gpu_test/ecp-proxy/amrex/Src/Base/AMReX_Arena.cpp line 178: feature not yet implemented !!!

```

这个项目默认使用统一内存，需要手动取消，取消后程序可以正常运行

#### Bubble

配置文件有问题？
```
NavierStokesBase::estTimeStep() failed to provide a good timestep (probably because initial velocity field is zero with no external forcing).
Use ns.init_dt to provide a reasonable timestep on coarsest level.
Note that ns.init_shrink will be applied to init_dt.
amrex::Abort::0::
```

a100＋nvcc＋float－managed memory：无法运行
```
Initializing CUDA...
CUDA initialized with 1 device.
AMReX (23.05-31-g2e1106e246c4) initialized
Successfully read inputs file ... 
Successfully read inputs file ... 
NavierStokesBase::init_additional_state_types()::have_divu = 0
NavierStokesBase::init_additional_state_types()::have_dsdt = 0
NavierStokesBase::init_additional_state_types: num_state_type = 3
estTimeStep :: 
LEV = 0 UMAX = 0  0  
estimated timestep: dt = 0.1016446352
Multiplying dt by init_shrink: dt = 0.03049339168
grid_places() time: 0.001385071 new finest: 1
grid_places() time: 0.00079982 new finest: 2
Now regridding at level lbase = 0
grid_places() time: 0.001890361 new finest: 2
STEP = 0 TIME = 0 : REGRID  with lbase = 0
  Level 1   4 grids  1024 cells  25 % of domain
            smallest grid: 16 x 16  biggest grid: 16 x 16
  Level 2   4 grids  1024 cells  6.25 % of domain
            smallest grid: 16 x 16  biggest grid: 16 x 16

Now regridding at level lbase = 0
grid_places() time: 0.002568831 new finest: 2
STEP = 0 TIME = 0 : REGRID  with lbase = 0
  Level 1   4 grids  1024 cells  25 % of domain
            smallest grid: 16 x 16  biggest grid: 16 x 16
  Level 2   4 grids  1024 cells  6.25 % of domain
            smallest grid: 16 x 16  biggest grid: 16 x 16

calling initialVelocityProject
done calling initialVelocityProject
calling initialPressureProject
amrex::Abort::0::MLMG failed !!!
```

a100＋nvcc＋double＋managed memory：可以运行

a100＋nvcc＋float＋managed memory：无法运行
```
...
calling initialVelocityProject
done calling initialVelocityProject
calling initialPressureProject
amrex::Abort::0::MLMG failed !!!
```

#### ConvectedVortex

函数GPU::htod_memcpy_async报错：
```
ctedVortex inputs.2d.convectedvortex 
Initializing CUDA...
CUDA initialized with 1 device.
AMReX (23.05-29-g76d6d344e12d-dirty) initialized
Successfully read inputs file ... 
Successfully read inputs file ... 
NavierStokesBase::init_additional_state_types()::have_divu = 0
NavierStokesBase::init_additional_state_types()::have_dsdt = 0
NavierStokesBase::init_additional_state_types: num_state_type = 3
Creating projector
Installing projector level 0
amrex::Abort::0::CUDA error 1017 in file ../../../amrex/Src/Base/AMReX_GpuDevice.H line 245: internal error !!!
SIGABRT
See Backtrace.0 file for details
```

使用cudaMemcpy替换cudaMemcpyAsyn报错信息：
```
ctedVortex inputs.2d.convectedvortex 
Initializing CUDA...
CUDA initialized with 1 device.
AMReX (23.05-29-g76d6d344e12d-dirty) initialized
Successfully read inputs file ... 
Successfully read inputs file ... 
NavierStokesBase::init_additional_state_types()::have_divu = 0
NavierStokesBase::init_additional_state_types()::have_dsdt = 0
NavierStokesBase::init_additional_state_types: num_state_type = 3
Creating projector
Installing projector level 0
amrex::Abort::0::CUDA error 1017 in file ../../../amrex/Src/Base/AMReX_GpuDevice.H line 246: internal error !!!
SIGABRT
See Backtrace.0 file for details
```

去掉AMREX_CUDA_SAFE_CALL之后就好了？

-------------2023.10.3------------

不要使用统一内存。

在amrex/Src/Base/AMrex_Arena.H中将ArenaInfo中的device_use_managed_memory设置成false以禁用统一内存。

需要在源文件IAMR/Source/NavierStokesBase.cpp中修改init_dt=1.0？内存占用过高？

可能有部分cuda特性没有实现。

程序堵塞在将数据传回到host的过程中

--------------2023.10.3-----------------

运行报错：
```
Initializing CUDA...
CUDA initialized with 1 device.
AMReX (23.05-29-g76d6d344e12d-dirty) initialized
Successfully read inputs file ... 
Successfully read inputs file ... 
NavierStokesBase::init_additional_state_types()::have_divu = 0
NavierStokesBase::init_additional_state_types()::have_dsdt = 0
NavierStokesBase::init_additional_state_types: num_state_type = 3
Creating projector
Installing projector level 0

NavierStokesBase::estTimeStep() failed to provide a good timestep (probably because initial velocity field is zero with no external forcing).
Use ns.init_dt to provide a reasonable timestep on coarsest level.
Note that ns.init_shrink will be applied to init_dt.
amrex::Abort::0::
 !!!
SIGABRT
See Backtrace.0 file for details
```

可能是精度问题？
* 将init_dt改为1.0，可以运行，但只跑了一个step；在a100上可以跑363个step；
* 在makefile中将precision改回double，仍然报错。
* 在a100上将makefile改为float报错：../../Source/Projection.cpp(58): error: floating-point value does not fit in required floating-point type

在a100上不需要修改就能运行，跑了大概363个step。

#### amrex依赖

noinline宏定义问题

## ecp proxy app suite: machine learning

### FlexFlow

使用pip安装的情况下会重新安装torch

在编译时报错：
```
error: couldn't allocate output register for constraint 'h' at line 9720047
error: couldn't allocate input reg for constraint 'h' at line 9719696
error: couldn't allocate input reg for constraint 'h' at line 9719696
error: couldn't allocate output register for constraint 'h' at line 9720047
error: couldn't allocate input reg for constraint 'h' at line 9719696
error: couldn't allocate input reg for constraint 'h' at line 9719696
error: couldn't allocate output register for constraint 'h' at line 9720047
error: couldn't allocate input reg for constraint 'h' at line 9719696
error: couldn't allocate input reg for constraint 'h' at line 9719696
error: couldn't allocate output register for constraint 'h' at line 9720047
clang-13: error: fatbinary command failed with exit code 1 (use -v to see invocation)
```

### MLPerf-DimeNet++

### miniGAN

### CRADL

```
real    7m40.473s
user    6m24.409s
sys 1m38.960s
```
显存占用约175M

### MLPerf-Cosmoflow

用tensorflow写的

### MLPerf-DeepCam

## infinity hub

### amber

clang在编译时出错：
```
clang-13: warning: argument unused during compilation: '-u se_fast_math' [-Wunused-command-line-argument]
clang-13: warning: argument unused during compilation: '-Xptxas' [-Wunused-command-line-argument]
error: couldn't allocate output register for constraint 'f'
LLVM ERROR: Cannot select: intrinsic %llvm.nvvm.d2i.lo
PLEASE submit a bug report to https://bugs.llvm.org/ and include the crash backtrace.
Stack dump:
0.  Program arguments: /usr/local/corex-3.0.0/bin/llc -mcpu=ivcore10 -mtriple=bi-iluvatar-ilurt -filetype=obj /tmp/lda_xc_zlp-175ca1.s -o /tmp/lda_xc_zlp-175ca1-da36b4.o
1.  Running pass 'CallGraph Pass Manager' on module '/tmp/lda_xc_zlp-175ca1.s'.
2.  Running pass 'Iluvatar DAG->DAG Pattern Instruction Selection' on function '@_Z25xc_lda_xc_zlp_func_kernelPKvP13xc_lda_work_t'
 #0 0x0000000001af537f PrintStackTraceSignalHandler(void*) Signals.cpp:0:0
 #1 0x0000000001af2ef9 SignalHandler(int) Signals.cpp:0:0
 #2 0x00007f6526a68520 (/lib/x86_64-linux-gnu/libc.so.6+0x42520)
 #3 0x00007f6526abca7c __pthread_kill_implementation ./nptl/./nptl/pthread_kill.c:44:76
 #4 0x00007f6526abca7c __pthread_kill_internal ./nptl/./nptl/pthread_kill.c:78:10
 #5 0x00007f6526abca7c pthread_kill ./nptl/./nptl/pthread_kill.c:89:10
 #6 0x00007f6526a68476 gsignal ./signal/../sysdeps/posix/raise.c:27:6
 #7 0x00007f6526a4e7f3 abort ./stdlib/./stdlib/abort.c:81:7
 #8 0x0000000001a5c1a4 llvm::report_fatal_error(llvm::Twine const&, bool) (/usr/local/corex-3.0.0/bin/llc+0x1a5c1a4)
 #9 0x0000000001a5c2fe (/usr/local/corex-3.0.0/bin/llc+0x1a5c2fe)
#10 0x0000000001941ef1 llvm::SelectionDAGISel::CannotYetSelect(llvm::SDNode*) (/usr/local/corex-3.0.0/bin/llc+0x1941ef1)
#11 0x0000000001944fe1 llvm::SelectionDAGISel::SelectCodeCommon(llvm::SDNode*, unsigned char const*, unsigned int) (/usr/local/corex-3.0.0/bin/llc+0x1944fe1)
#12 0x0000000000852857 (anonymous namespace)::IluvatarDAGToDAGISel::Select(llvm::SDNode*) IluvatarISelDAGToDAG.cpp:0:0
#13 0x0000000001940ba4 llvm::SelectionDAGISel::DoInstructionSelection() (/usr/local/corex-3.0.0/bin/llc+0x1940ba4)
#14 0x00000000019483f4 llvm::SelectionDAGISel::CodeGenAndEmitDAG() (/usr/local/corex-3.0.0/bin/llc+0x19483f4)
#15 0x000000000194b7a9 llvm::SelectionDAGISel::SelectAllBasicBlocks(llvm::Function const&) (/usr/local/corex-3.0.0/bin/llc+0x194b7a9)
#16 0x000000000194da3a llvm::SelectionDAGISel::runOnMachineFunction(llvm::MachineFunction&) (.part.873) SelectionDAGISel.cpp:0:0
#17 0x0000000000fb345f llvm::MachineFunctionPass::runOnFunction(llvm::Function&) (.part.35) MachineFunctionPass.cpp:0:0
#18 0x00000000013c83d1 llvm::FPPassManager::runOnFunction(llvm::Function&) (/usr/local/corex-3.0.0/bin/llc+0x13c83d1)
#19 0x0000000000bc8be9 (anonymous namespace)::CGPassManager::runOnModule(llvm::Module&) CallGraphSCCPass.cpp:0:0
#20 0x00000000013c9887 llvm::legacy::PassManagerImpl::run(llvm::Module&) (/usr/local/corex-3.0.0/bin/llc+0x13c9887)
#21 0x000000000073c000 compileModule(char**, llvm::LLVMContext&) llc.cpp:0:0
#22 0x00000000006bc382 main (/usr/local/corex-3.0.0/bin/llc+0x6bc382)
#23 0x00007f6526a4fd90 __libc_start_call_main ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
#24 0x00007f6526a4fe40 call_init ./csu/../csu/libc-start.c:128:20
#25 0x00007f6526a4fe40 __libc_start_main ./csu/../csu/libc-start.c:379:5
#26 0x00000000007348d5 _start (/usr/local/corex-3.0.0/bin/llc+0x7348d5)
clang-13: error: unable to execute command: Aborted (core dumped)
clang-13: error: fatbinary command failed due to signal (use -v to see invocation)
clang version 13.0.1
Target: x86_64-unknown-linux-gnu
Thread model: posix
InstalledDir: /usr/local/corex/bin
clang-13: note: diagnostic msg: 
********************

PLEASE ATTACH THE FOLLOWING FILES TO THE BUG REPORT:
Preprocessed source(s) and associated run script(s) are located at:
clang-13: note: diagnostic msg: /tmp/lda_xc_zlp-19f659.cu
clang-13: note: diagnostic msg: /tmp/lda_xc_zlp-7393a3.cu
clang-13: note: diagnostic msg: /tmp/lda_xc_zlp-19f659.sh
clang-13: note: diagnostic msg: 

********************

```

a100+nvcc:
可以正常编译，已安装

### BabelStream

链接有问题，需要手动链接

天数GPU运行结果：
```
BabelStream
Version: 4.0
Implementation: CUDA
Running kernels 100 times
Precision: float
Array size: 134.2 MB (=0.1 GB)
Total size: 402.7 MB (=0.4 GB)
Using CUDA device Iluvatar BI-V100
Driver: 10020
Validation failed on sum. Error 3.54711e-06
Sum was 39.7911071777344 but should be 39.7912483215332
Function    MBytes/sec  Min (sec)   Max         Average     
Copy        545900.100  0.00049     0.00053     0.00049     
Mul         580878.248  0.00046     0.00050     0.00046     
Add         480544.669  0.00084     0.00087     0.00085     
Triad       487349.686  0.00083     0.00086     0.00083     
Dot         374485.682  0.00072     0.00074     0.00072  
```
titanxp对照：
```
BabelStream
Version: 3.3
Implementation: CUDA
Running kernels 100 times
Precision: double
Array size: 268.4 MB (=0.3 GB)
Total size: 805.3 MB (=0.8 GB)
Using CUDA device TITAN Xp
Driver: 9010
Function    MBytes/sec  Min (sec)   Max         Average
Copy        433182.244  0.00124     0.00128     0.00125
Mul         431744.565  0.00124     0.00125     0.00125
Add         436451.083  0.00185     0.00186     0.00185
Triad       436491.299  0.00184     0.00186     0.00185
Dot         435969.304  0.00123     0.00125     0.00124

```
a100+nvcc：
```
BabelStream
Version: 4.0
Implementation: CUDA
Running kernels 100 times
Precision: float
Array size: 134.2 MB (=0.1 GB)
Total size: 402.7 MB (=0.4 GB)
Using CUDA device NVIDIA A100-PCIE-40GB
Driver: 11040
Validation failed on sum. Error 2.49256e-06
Sum was 39.7911071777344 but should be 39.7912063598633
Function    MBytes/sec  Min (sec)   Max         Average     
Copy        1272688.489 0.00021     0.00027     0.00026     
Mul         1244139.118 0.00022     0.00027     0.00026     
Add         1327924.227 0.00030     0.00033     0.00032     
Triad       1312086.757 0.00031     0.00032     0.00032     
Dot         964970.365  0.00028     0.00032     0.00031  
```
a100+clang-14:
```
BabelStream
Version: 4.0
Implementation: CUDA
Running kernels 100 times
Precision: float
Array size: 134.2 MB (=0.1 GB)
Total size: 402.7 MB (=0.4 GB)
Using CUDA device NVIDIA A100-PCIE-40GB
Driver: 11040
Validation failed on sum. Error 2.49256e-06
Sum was 39.7911071777344 but should be 39.7912063598633
Function    MBytes/sec  Min (sec)   Max         Average     
Copy        1022455.458 0.00026     0.00045     0.00027     
Mul         1001886.523 0.00027     0.00027     0.00027     
Add         1247995.239 0.00032     0.00033     0.00032     
Triad       1252693.227 0.00032     0.00040     0.00032     
Dot         876521.076  0.00031     0.00032     0.00031   
```

### chroma

quda编译报错：
```
[ 31%] Building CUDA object lib/CMakeFiles/quda.dir/gauge_phase.cu.o
error: couldn't allocate input reg for constraint 'l' at line 6713540
error: couldn't allocate input reg for constraint 'l' at line 6713540
error: couldn't allocate input reg for constraint 'l' at line 6713540
...
```

尝试手动编译报错的文件，仍然报错：
```
//gauge_plaq.cu
...
note: !srcloc = 7949146
<inline asm>:1:2: error: invalid instruction
        shfl.sync.down.b32 v9, v6, v0, v1, v7;
        ^
note: !srcloc = 7949146
<inline asm>:1:2: error: invalid instruction
        shfl.sync.down.b32 v9, v5, v0, v1, v7;
        ^
note: !srcloc = 7949146
<inline asm>:1:2: error: invalid instruction
        shfl.sync.down.b32 v0, v8, v0, v1, v7;
        ^
note: !srcloc = 7949146
clang-13: error: fatbinary command failed with exit code 1 (use -v to see invocation)

```

clang在链接时默认不会链接math库和stdc++库

### cp2k

可以正常编译，但在运行时报错：
```
Inconsistency in warp sizes: Cuda/Hip indicates warp size = 32, while the gpu_properties files indicates warp_size = 64.
Please check whether src/acc/libsmm_acc/kernels/gpu_properties.json contains the correct data about the GPU you are using. ABORT in dbcsr_lib.F:236 DBCSR compiled w/ threading support while libsmm_acc compiled w/o threading support.
```

暂时还没有找到配置文件

----------------2023.10.4---------------

配置文件在ext目录下。

应该不是配置的问题，可能是编译的问题。

需要手动编译libcp2kdbm.a文件

---------------2023.10.9----------------

配置命令：
```
./install_cp2k_toolchain.sh --mpi-mode=no --enable-cuda=yes --with-cmake=system 
```

a100上缺少相关的fortran库，编译失败

### gromacs

cuda math库和c++ math库中函数声明冲突
```
In file included from /mnt/nvme0/home/lianghz/workspace/gpu_test/infinityhub/gromacs-2023.2/src/gromacs/domdec/gpuhaloexchange_impl_gpu.cpp:46:
In file included from /mnt/nvme0/home/lianghz/workspace/gpu_test/infinityhub/gromacs-2023.2/src/gromacs/domdec/gpuhaloexchange_impl_gpu.h:51:
In file included from /mnt/nvme0/home/lianghz/workspace/gpu_test/infinityhub/gromacs-2023.2/src/gromacs/gpu_utils/gpueventsynchronizer.h:51:
In file included from /mnt/nvme0/home/lianghz/workspace/gpu_test/infinityhub/gromacs-2023.2/src/gromacs/gpu_utils/device_event.h:115:
In file included from /mnt/nvme0/home/lianghz/workspace/gpu_test/infinityhub/gromacs-2023.2/src/gromacs/gpu_utils/device_event.cuh:44:
In file included from /mnt/nvme0/home/lianghz/workspace/gpu_test/infinityhub/gromacs-2023.2/src/gromacs/gpu_utils/cudautils.cuh:43:
In file included from /mnt/nvme0/home/lianghz/workspace/gpu_test/infinityhub/gromacs-2023.2/src/gromacs/gpu_utils/device_stream.h:49:
In file included from /usr/local/corex/include/cuda_runtime.h:115:
In file included from /usr/local/corex/include/crt/common_functions.h:267:
/usr/local/corex/include/crt/math_functions.h:9523:70: error: non-constexpr declaration of 'cos' follows constexpr declaration
extern __DEVICE_FUNCTIONS_DECL__ __cudart_builtin__ float    __cdecl cos(float);
                                                                     ^
/usr/lib/gcc/x86_64-linux-gnu/12/../../../../include/c++/12/cmath:184:3: note: previous declaration is here
  cos(float __x)
```

a100:
```
CMake Error at cmake/FindFFTW.cmake:104 (message):
  Could not find fftwf_plan_many_[r2c|c2r] in
  /mnt/sda/2022-0526/home/lhz/share/lib, take a look at the error message in
  /mnt/sda/2022-0526/home/lhz/workspace/gpu_test/infinityhub/gromacs-2023.2/build/CMakeFiles/CMakeError.log
  to find out what went wrong.  If you are using a static lib (.a) make sure
  you have specified all dependencies of fftw3f in FFTWF_LIBRARY by hand
  (e.g.  -DFFTWF_LIBRARY='/path/to/libfftw3f.so;/path/to/libm.so') !
Call Stack (most recent call first):
  cmake/gmxManageFFTLibraries.cmake:64 (find_package)
  CMakeLists.txt:842 (include)
```

在cmake时加上-DGMX_BUILD_OWN_FFTW=ON

a100+nvcc: 
```
/usr/bin/ld: /mnt/sda/2022-0526/home/lhz/workspace/gpu_test/infinityhub/gromacs-2023.2/build/lib/libgromacs.so.8: undefined reference to `srot_'
/usr/bin/ld: /mnt/sda/2022-0526/home/lhz/workspace/gpu_test/infinityhub/gromacs-2023.2/build/lib/libgromacs.so.8: undefined reference to `strsm_'
```

### hpcg

缺少cuda版本

### hpl

### hpl-mxp

### lammps

需要找到Makefile文件改flags

---------------2023.10.7--------------

```
/mnt/nvme0/home/lianghz/workspace/gpu_test/infinityhub/lammps/lib/gpu/lal_zbl.cu:115:17: error: no matching function for call to 'tex1Dfetch'
    numtyp4 ix; fetch4(ix,i,pos_tex); //x_[i];
                ^~~~~~~~~~~~~~~~~~~~
/mnt/nvme0/home/lianghz/workspace/gpu_test/infinityhub/lammps/lib/gpu/lal_pre_cuda_hip.h:142:39: note: expanded from macro 'fetch4'
    #define fetch4(ans,i,pos_tex) ans=tex1Dfetch(pos_tex, i);
                                      ^~~~~~~~~~
/usr/local/corex/include/texture_indirect_functions.h:112:21: note: candidate template ignored: couldn't infer template argument 'T'
static __device__ T tex1Dfetch(cudaTextureObject_t texObject, int x)
                    ^
/usr/local/corex/include/texture_indirect_functions.h:104:53: note: candidate function template not viable: requires 3 arguments, but 2 were provided
static __device__ typename __nv_itex_trait<T>::type tex1Dfetch(T *ptr, cudaTextureObject_t obj, int x)
                                                    ^
/mnt/nvme0/home/lianghz/workspace/gpu_test/infinityhub/lammps/lib/gpu/lal_zbl.cu:124:19: error: no matching function for call to 'tex1Dfetch'
      numtyp4 jx; fetch4(jx,j,pos_tex); //x_[j];
                  ^~~~~~~~~~~~~~~~~~~~
/mnt/nvme0/home/lianghz/workspace/gpu_test/infinityhub/lammps/lib/gpu/lal_pre_cuda_hip.h:142:39: note: expanded from macro 'fetch4'
    #define fetch4(ans,i,pos_tex) ans=tex1Dfetch(pos_tex, i);
                                      ^~~~~~~~~~
/usr/local/corex/include/texture_indirect_functions.h:112:21: note: candidate template ignored: couldn't infer template argument 'T'
static __device__ T tex1Dfetch(cudaTextureObject_t texObject, int x)
                    ^
/usr/local/corex/include/texture_indirect_functions.h:104:53: note: candidate function template not viable: requires 3 arguments, but 2 were provided
static __device__ typename __nv_itex_trait<T>::type tex1Dfetch(T *ptr, cudaTextureObject_t obj, int x)
                                                    ^
/mnt/nvme0/home/lianghz/workspace/gpu_test/infinityhub/lammps/lib/gpu/lal_zbl.cu:218:17: error: no matching function for call to 'tex1Dfetch'
    numtyp4 ix; fetch4(ix,i,pos_tex); //x_[i];
                ^~~~~~~~~~~~~~~~~~~~
/mnt/nvme0/home/lianghz/workspace/gpu_test/infinityhub/lammps/lib/gpu/lal_pre_cuda_hip.h:142:39: note: expanded from macro 'fetch4'
    #define fetch4(ans,i,pos_tex) ans=tex1Dfetch(pos_tex, i);
                                      ^~~~~~~~~~
/usr/local/corex/include/texture_indirect_functions.h:112:21: note: candidate template ignored: couldn't infer template argument 'T'
static __device__ T tex1Dfetch(cudaTextureObject_t texObject, int x)
                    ^
/usr/local/corex/include/texture_indirect_functions.h:104:53: note: candidate function template not viable: requires 3 arguments, but 2 were provided
static __device__ typename __nv_itex_trait<T>::type tex1Dfetch(T *ptr, cudaTextureObject_t obj, int x)
                                                    ^
/mnt/nvme0/home/lianghz/workspace/gpu_test/infinityhub/lammps/lib/gpu/lal_zbl.cu:228:19: error: no matching function for call to 'tex1Dfetch'
      numtyp4 jx; fetch4(jx,j,pos_tex); //x_[j];
                  ^~~~~~~~~~~~~~~~~~~~
/mnt/nvme0/home/lianghz/workspace/gpu_test/infinityhub/lammps/lib/gpu/lal_pre_cuda_hip.h:142:39: note: expanded from macro 'fetch4'
    #define fetch4(ans,i,pos_tex) ans=tex1Dfetch(pos_tex, i);
                                      ^~~~~~~~~~
/usr/local/corex/include/texture_indirect_functions.h:112:21: note: candidate template ignored: couldn't infer template argument 'T'
static __device__ T tex1Dfetch(cudaTextureObject_t texObject, int x)
                    ^
/usr/local/corex/include/texture_indirect_functions.h:104:53: note: candidate function template not viable: requires 3 arguments, but 2 were provided
static __device__ typename __nv_itex_trait<T>::type tex1Dfetch(T *ptr, cudaTextureObject_t obj, int x)
```

a100+nvcc:可以编译

### milc

需要quda

### mini-hacc

### minimod

### namd

```
//config
./config Linux-x86_64-g++ --charm-arch multicore-linux-x86_64 --cxx clang --cuda-prefix /usr/local/corex --with-cuda
```

make报错：
```
charmc> All libraries are: -L.rootdir/charm-6.10.2/multicore-linux-x86_64/bin/../lib -lmoduleCkLoop -lmoduleCkMulticast -lckmain -lck -lmemory-default -lthreads-default -lconv-machine -lconv-core -ltmgr -lconv-util -lconv-partition -lhwloc_embedded -lm -lmemory-default -lthreads-default -lldb-rand -lconv-ldb -lpthread -lckqt -ldl -lcufft_static -lculibos -lcudart_static -lrt -ltcl8.5 -ldl -lpthread -lsrfftw -lsfftw -lm -lmoduleCkMulticast -lmoduleCkLoop -lmoduleNDMeshStreamer -lmodulecompletion -lz -lm .rootdir/charm-6.10.2/multicore-linux-x86_64/bin/../lib/conv-static.o
Fatal Error by charmc in directory /mnt/nvme0/home/lianghz/workspace/gpu_test/infinityhub/NAMD_2.14_Source/Linux-x86_64-g++
   obj/ComputeBondedCUDAKernel.o: file not recognized: File truncated
charmc exiting...
rm -f moduleinit272024.C moduleinit272024.o
make: *** [Makefile:467: namd2] Error 1
```

手动使用gcc链接失败，tcl库可能没有成功链接上

-------------2023.10.6-------------

Makefile中使用了nvcc编译cuda代码，所以在链接时缺少部分.o文件。

使用天数编译器编译报错：
```
In file included from src/ComputeBondedCUDAKernel.cu:12:
In file included from src/ComputeBondedCUDAKernel.h:3:
src/CudaUtils.h:98:26: error: redefinition of 'atomicAdd'
static __device__ double atomicAdd(double* address, double val) {
                         ^
/usr/local/corex/include/sm_60_atomic_functions.hpp:77:40: note: previous definition is here
__SM_60_ATOMIC_FUNCTIONS_DECL__ double atomicAdd(double *address, double val)
                                       ^
src/ComputeBondedCUDAKernel.cu:1832:22: error: no matching function for call to 'min'
    int nblock_use = min(max_nblock, nleft);
                     ^~~
/usr/local/corex-3.0.0/lib/clang/13.0.1/include/__clang_cuda_math.h:197:16: note: candidate function not viable: call to __device__ function from __host__ function
__DEVICE__ int min(int __a, int __b) { return __nv_min(__a, __b); }
...
In file included from <built-in>:1:
In file included from /usr/local/corex-3.0.0/lib/clang/13.0.1/include/__clang_cuda_runtime_wrapper.h:342:
/usr/local/corex/include/texture_indirect_functions.h:107:4: error: use of undeclared identifier '__nv_tex_surf_handler'
   __nv_tex_surf_handler("__itex1Dfetch", ptr, obj, x);
   ^
/usr/local/corex/include/texture_indirect_functions.h:116:3: note: in instantiation of function template specialization 'tex1Dfetch<float>' requested here
  tex1Dfetch(&ret, texObject, x);
```


a100+nvcc:可以正常编译，可执行文件位于Linux-x86_64-g++文件夹下

### namd3.0

### nwchem

### relion

找不到omp.h?

---------2023.10.7-----------

```
/mnt/nvme0/home/lianghz/workspace/gpu_test/infinityhub/relion/src/acc/cuda/cuda_kernels/helper.cuh:945:19: error: redefinition of 'atomicAdd'
__device__ double atomicAdd(double* address, double val)
                  ^
/usr/local/corex/include/sm_60_atomic_functions.hpp:77:40: note: previous definition is here
__SM_60_ATOMIC_FUNCTIONS_DECL__ double atomicAdd(double *address, double val)
                                       ^
In file included from <built-in>:1:
In file included from /usr/local/corex-3.0.0/lib/clang/13.0.1/include/__clang_cuda_runtime_wrapper.h:342:
/usr/local/corex/include/texture_indirect_functions.h:163:4: error: use of undeclared identifier '__nv_tex_surf_handler'
   __nv_tex_surf_handler("__itex3D", ptr, obj, x, y, z);
   ^
/usr/local/corex/include/texture_indirect_functions.h:172:3: note: in instantiation of function template specialization 'tex3D<float>' requested here
  tex3D(&ret, texObject, x, y, z);
  ^
/mnt/nvme0/home/lianghz/workspace/gpu_test/infinityhub/relion/src/acc/acc_projectorkernel_impl.h:137:15: note: in instantiation of function template specialization 'tex3D<float>' requested here
                                real =    tex3D<XFLOAT>(mdlReal, xp + (XFLOAT)0.5, yp + (XFLOAT)0.5, zp + (XFLOAT)0.5);
                                          ^
In file included from <built-in>:1:
In file included from /usr/local/corex-3.0.0/lib/clang/13.0.1/include/__clang_cuda_runtime_wrapper.h:342:
/usr/local/corex/include/texture_indirect_functions.h:145:4: error: use of undeclared identifier '__nv_tex_surf_handler'
   __nv_tex_surf_handler("__itex2D", ptr, obj, x, y);
   ^
/usr/local/corex/include/texture_indirect_functions.h:154:3: note: in instantiation of function template specialization 'tex2D<float>' requested here
  tex2D(&ret, texObject, x, y);
  ^
/mnt/nvme0/home/lianghz/workspace/gpu_test/infinityhub/relion/src/acc/acc_projectorkernel_impl.h:286:14: note: in instantiation of function template specialization 'tex2D<float>' requested here
                                real =   tex2D<XFLOAT>(mdlReal, xp + (XFLOAT)0.5, yp + (XFLOAT)0.5);
                                         ^
```

### specfem3d cartesian

```
/usr/bin/ld: /lib/x86_64-linux-gnu/crt1.o: in function `_start':
(.text+0x1b): undefined reference to `main'
```

### specfem3d globe

### trilinos

cmake版本太低

### grid

### mpas

### openfoam

### petsc

## ecp proxy app: 4.0&5.0

### AMG

非cuda程序？

### Ember

非cuda程序？

### Laghos

### MACSio

非运算密集型程序？

### miniAMR

非cuda程序？

### miniQMC

### miniVite

### NEKbone

### PICSARlite

### SW4lite

### SWFFT

### Thornado-mini

### XSBench

头文件中出现__noinline__宏定义冲突

```
In file included from Main.cu:1:
In file included from ./XSbench_header.cuh:5:
In file included from /usr/local/corex/include/thrust/reduce.h:784:
In file included from /usr/local/corex/include/thrust/detail/reduce.inl:26:
In file included from /usr/local/corex/include/thrust/system/detail/generic/reduce_by_key.h:88:
In file included from /usr/local/corex/include/thrust/system/detail/generic/reduce_by_key.inl:29:
In file included from /usr/local/corex/include/thrust/transform.h:724:
In file included from /usr/local/corex/include/thrust/detail/transform.inl:25:
In file included from /usr/local/corex/include/thrust/system/detail/generic/transform.h:105:
In file included from /usr/local/corex/include/thrust/system/detail/generic/transform.inl:24:
In file included from /usr/local/corex/include/thrust/detail/internal_functional.h:29:
In file included from /usr/lib/gcc/x86_64-linux-gnu/12/../../../../include/c++/12/memory:76:
In file included from /usr/lib/gcc/x86_64-linux-gnu/12/../../../../include/c++/12/bits/shared_ptr.h:53:
/usr/lib/gcc/x86_64-linux-gnu/12/../../../../include/c++/12/bits/shared_ptr_base.h:196:22: error: expected expression
/usr/local/corex/include/crt/host_defines.h:83:33: note: expanded from macro '__noinline__'
        __attribute__((noinline))
```

## rodinia

### b+tree

天数GPU：
```
WG size of kernel 1 & 2  = 256 
Selecting device 0
Input File: ../../data/b+tree/mil.txt 
Command File: ../../data/b+tree/command.txt 
Command Buffer: 
j 6000 3000
k 10000


Getting input from file ../../data/b+tree/mil.txt...
Transforming data to a GPU suitable structure...
Tree transformation took 0.079346
Waiting for command
> 
******command: j count=6000, rSize=6000 
knodes_elem=7874, knodes_unit_mem=2068, knodes_mem=16283432
# of blocks = 6000, # of threads/block = 256 (ensure that device can handle)
Time spent in different stages of GPU_CUDA KERNEL:
 0.000011000000 s,  0.169648364186 % : GPU: SET DEVICE / DRIVER INIT
 0.000021000000 s,  0.323874145746 % : GPU MEM: ALO
 0.005171999801 s, 79.765579223633 % : GPU MEM: COPY IN
 0.001172999968 s, 18.090684890747 % : GPU: KERNEL
 0.000098999997 s,  1.526835322380 % : GPU MEM: COPY OUT
 0.000008000000 s,  0.123380623758 % : GPU MEM: FRE
Total time:
0.006484000012 s
> > > > > > > > > > > > 
 ******command: k count=10000 
records_elem=1000000, records_unit_mem=4, records_mem=4000000
knodes_elem=7874, knodes_unit_mem=2068, knodes_mem=16283432
# of blocks = 10000, # of threads/block = 256 (ensure that device can handle)
Time spent in different stages of GPU_CUDA KERNEL:
 0.000003000000 s,  0.140186905861 % : GPU: SET DEVICE / DRIVER INIT
 0.000011000000 s,  0.514018654823 % : GPU MEM: ALO
 0.001217999961 s, 56.915885925293 % : GPU MEM: COPY IN
 0.000843000016 s, 39.392520904541 % : GPU: KERNEL
 0.000059999998 s,  2.803738117218 % : GPU MEM: COPY OUT
 0.000005000000 s,  0.233644858003 % : GPU MEM: FRE
Total time:
0.002139999997 s
> > > > > > > > > > 
```

A100+nvcc：
```
WG size of kernel 1 & 2  = 256 
Selecting device 0
Input File: ../../data/b+tree/mil.txt 
Command File: ../../data/b+tree/command.txt 
Command Buffer: 
j 6000 3000
k 10000


Getting input from file ../../data/b+tree/mil.txt...
Transforming data to a GPU suitable structure...
Tree transformation took 0.170902
Waiting for command
> 
******command: j count=6000, rSize=6000 
knodes_elem=7874, knodes_unit_mem=2068, knodes_mem=16283432
# of blocks = 6000, # of threads/block = 256 (ensure that device can handle)
Time spent in different stages of GPU_CUDA KERNEL:
 0.105066999793 s, 97.217651367188 % : GPU: SET DEVICE / DRIVER INIT
 0.000554999977 s,  0.513537049294 % : GPU MEM: ALO
 0.001715000020 s,  1.586875677109 % : GPU MEM: COPY IN
 0.000079999998 s,  0.074023351073 % : GPU: KERNEL
 0.000038999999 s,  0.036086387932 % : GPU MEM: COPY OUT
 0.000617999991 s,  0.571830451488 % : GPU MEM: FRE
Total time:
0.108074001968 s
> > > > > > > > > > > > 
 ******command: k count=10000 
records_elem=1000000, records_unit_mem=4, records_mem=4000000
knodes_elem=7874, knodes_unit_mem=2068, knodes_mem=16283432
# of blocks = 10000, # of threads/block = 256 (ensure that device can handle)
Time spent in different stages of GPU_CUDA KERNEL:
 0.000007000000 s,  0.190943807364 % : GPU: SET DEVICE / DRIVER INIT
 0.000798999972 s, 21.794872283936 % : GPU MEM: ALO
 0.001950999955 s, 53.218769073486 % : GPU MEM: COPY IN
 0.000081999999 s,  2.236770391464 % : GPU: KERNEL
 0.000019999999 s,  0.545553743839 % : GPU MEM: COPY OUT
 0.000806999975 s, 22.013093948364 % : GPU MEM: FRE
Total time:
0.003665999975 s
> > > > > > > > > > 
```

### backprop

### bfs

天数GPU：
```
Reading File
Read File
Copied Everything to GPU memory
Start traversing the tree
Kernel Executed 12 times
Result stored in result.txt

real    0m0.763s
user    0m0.720s
sys 0m0.044s
```
A100+nvcc：
```
Reading File
Read File
Copied Everything to GPU memory
Start traversing the tree
Kernel Executed 12 times
Result stored in result.txt

real    0m1.573s
user    0m1.033s
sys 0m0.469s
```

### dwt2d

天数GPU：
```
real    0m0.047s
user    0m0.016s
sys 0m0.033s

```

A100+nvcc：
```
real    0m0.519s
user    0m0.056s
sys 0m0.438s
```

### gaussian

天数GPU：
```
Time total (including memory transfers) 0.003011 sec
Time for CUDA kernels:  0.001123 sec
```

A100+nvcc：
```
Time total (including memory transfers) 0.150715 sec
Time for CUDA kernels:  0.000259 sec
```

### heartwall

天数GPU：
```
real    0m0.501s
user    0m0.474s
sys 0m0.028s
```

A100+nvcc：
```
real    0m0.328s
user    0m0.092s
sys 0m0.215s
```

### hotspot

天数GPU：
```
real    0m0.172s
user    0m0.152s
sys 0m0.020s
```

A100+nvcc：
```
real    0m0.537s
user    0m0.279s
sys 0m0.230s
```

A100+clang14：
```
real    0m0.510s
user    0m0.270s
sys 0m0.212s
```

### hotspot3d

天数GPU：
```
Time: 0.006 (s)
Accuracy: 5.547907e-05
```

A100+nvcc：
```
Time: 0.007 (s)
Accuracy: 4.096975e-05
```

结果不一样

### huffman

### hybridsort

需要GL库

### kmeans

需要omp.h

----------------------2023.10.15-------------------------

```
In file included from kmeans_cuda.cu:15:
./kmeans_cuda_kernel.cu:89:19: error: no matching function for call to 'tex1Dfetch'
                                float diff = (tex1Dfetch(t_features,addr) -
                                              ^~~~~~~~~~
/usr/local/corex/include/texture_indirect_functions.h:112:21: note: candidate template ignored: couldn't infer template argument 'T'
static __device__ T tex1Dfetch(cudaTextureObject_t texObject, int x)
                    ^
/usr/local/corex/include/texture_indirect_functions.h:104:53: note: candidate function template not viable: requires 3 arguments, but 2 were provided
static __device__ typename __nv_itex_trait<T>::type tex1Dfetch(T *ptr, cudaTextureObject_t obj, int x)
```

模板推理问题？

### lavaMD

天数GPU：
```
thread block size of kernel = 128 
Configuration used: boxes1d = 10
Time spent in different stages of GPU_CUDA KERNEL:
 0.002329000039 s, 38.591548919678 % : GPU: SET DEVICE / DRIVER INIT
 0.000009000000 s,  0.149130076170 % : GPU MEM: ALO
 0.002114000032 s, 35.028995513916 % : GPU MEM: COPY IN
 0.001283999998 s, 21.275892257690 % : GPU: KERNEL
 0.000293999998 s,  4.871582508087 % : GPU MEM: COPY OUT
 0.000005000000 s,  0.082850046456 % : GPU MEM: FRE
Total time:
0.006035000086 s
```

A100+nvcc：
```
thread block size of kernel = 128 
Configuration used: boxes1d = 10
Time spent in different stages of GPU_CUDA KERNEL:
 0.198311001062 s, 97.553184509277 % : GPU: SET DEVICE / DRIVER INIT
 0.000707000028 s,  0.347787588835 % : GPU MEM: ALO
 0.000728000014 s,  0.358117908239 % : GPU MEM: COPY IN
 0.002480000025 s,  1.219962120056 % : GPU: KERNEL
 0.000384000014 s,  0.188897371292 % : GPU MEM: COPY OUT
 0.000675000018 s,  0.332046151161 % : GPU MEM: FRE
Total time:
0.203284993768 s
```

### leukocyte

tex1Dfetch函数匹配问题

### lud

可以运行，运行时间太短

### mummergpu

tex2D函数匹配问题

### myocyte

库内部符号链接问题

### nn

天数GPU：
```
real    0m0.029s
user    0m0.008s
sys 0m0.022s
```

A100+nvcc：
```
real    0m0.310s
user    0m0.062s
sys 0m0.220s
```

### nw

可以运行，运行时间过短，且无法验证正确性

### particlefilter

天数GPU：
```
VIDEO SEQUENCE TOOK 0.007175
TIME TO SEND TO GPU: 0.000179
GPU Execution: 0.003192
FREE TIME: 0.000007
TIME TO SEND BACK: 0.000101
SEND ARRAY X BACK: 0.000049
SEND ARRAY Y BACK: 0.000025
SEND WEIGHTS BACK: 0.000020
XE: 64.000000
YE: 64.000000
0.000000
PARTICLE FILTER TOOK 0.005918
ENTIRE PROGRAM TOOK 0.013093
```

A100+nvcc：
```
VIDEO SEQUENCE TOOK 0.017504
TIME TO SEND TO GPU: 0.000073
GPU Execution: 0.003231
FREE TIME: 0.000027
TIME TO SEND BACK: 0.000063
SEND ARRAY X BACK: 0.000017
SEND ARRAY Y BACK: 0.000010
SEND WEIGHTS BACK: 0.000009
XE: 48.520612
YE: 72.447167
17.634231
PARTICLE FILTER TOOK 0.201660
ENTIRE PROGRAM TOOK 0.219164
```

无法验证正确性

### pathfinder

天数GPU：
```
real    0m0.559s
user    0m0.482s
sys 0m0.076s
```

A100+nvcc：
```
real    0m1.191s
user    0m0.794s
sys 0m0.285s
```

### srad

天数GPU：
```
Time spent in different stages of the application:
 0.000000000000 s,  0.000000000000 % : SETUP VARIABLES
 0.000006000000 s,  0.014902759343 % : READ COMMAND LINE PARAMETERS
 0.010185999796 s, 25.299919128418 % : READ IMAGE FROM FILE
 0.000402000005 s,  0.998484909534 % : RESIZE IMAGE
 0.002341999905 s,  5.817043781281 % : GPU DRIVER INIT, CPU/GPU SETUP, MEMORY ALLOCATION
 0.000456000009 s,  1.132609724998 % : COPY DATA TO CPU->GPU
 0.001552999951 s,  3.857331037521 % : EXTRACT IMAGE
 0.014936000109 s, 37.097938537598 % : COMPUTE
 0.000001000000 s,  0.002483793302 % : COMPRESS IMAGE
 0.000148000006 s,  0.367601394653 % : COPY DATA TO GPU->CPU
 0.010156000033 s, 25.225404739380 % : SAVE IMAGE INTO FILE
 0.000075000004 s,  0.186284497380 % : FREE MEMORY
Total time:
0.040261000395 s
```

A100+nvcc：
```
Time spent in different stages of the application:
 0.000004000000 s,  0.001502691652 % : SETUP VARIABLES
 0.000015000000 s,  0.005635093898 % : READ COMMAND LINE PARAMETERS
 0.031711999327 s, 11.913339614868 % : READ IMAGE FROM FILE
 0.001114999992 s,  0.418875306845 % : RESIZE IMAGE
 0.199701994658 s, 75.022628784180 % : GPU DRIVER INIT, CPU/GPU SETUP, MEMORY ALLOCATION
 0.000069000002 s,  0.025921430439 % : COPY DATA TO CPU->GPU
 0.000016000000 s,  0.006010766607 % : EXTRACT IMAGE
 0.005745999981 s,  2.158616781235 % : COMPUTE
 0.000003000000 s,  0.001127018710 % : COMPRESS IMAGE
 0.000155999995 s,  0.058604974300 % : COPY DATA TO GPU->CPU
 0.026575999334 s,  9.983883857727 % : SAVE IMAGE INTO FILE
 0.001075000037 s,  0.403848379850 % : FREE MEMORY
Total time:
0.266189008951 s
```

无法验证正确性

### streamcluster

库内存符号链接问题

## LULESH

thrust库的头文件中存在noinline宏定义冲突：
```
In file included from allocator.cu:14:
In file included from ./vector.h:3:
In file included from /usr/local/corex/include/thrust/host_vector.h:25:
In file included from /usr/lib/gcc/x86_64-linux-gnu/12/../../../../include/c++/12/memory:76:
In file included from /usr/lib/gcc/x86_64-linux-gnu/12/../../../../include/c++/12/bits/shared_ptr.h:53:
/usr/lib/gcc/x86_64-linux-gnu/12/../../../../include/c++/12/bits/shared_ptr_base.h:196:22: error: use of undeclared identifier 'noinline'; did you mean 'inline'?
      __attribute__((__noinline__))
                     ^
/usr/local/corex/include/crt/host_defines.h:83:24: note: expanded from macro '__noinline__'
        __attribute__((noinline))
```

## 问题

### noinline attribute问题
由于天数GPU编译器基于clang，而clang中无法处理cuda库文件的标准c++头文件中对noinline定义的冲突，因此如果cu文件中引入了c++定义的noinline编译就会报错。

在编译flag中加入--stdlib=libc++可以部分解决这个问题，在其它时候这样做会导致找不到标准头文件错误。

找到冲突的地方，直接undef这个宏定义。

### 文件名识别问题
天数编译器在编译cuda项目时根据文件后缀判断文件类型，只要后缀是.cpp就不会识别为cuda文件。

### device端代码和host端代码分开编译

部分项目将代码中的device代码和host代码分开编译，但使用天数编译器时一般只能合并在一起编译，因此需要在Makefile文件或cmake文件中将部分依赖项删掉。

### \_\_NVCC\_\_宏定义
天数编译器在编译cuda文件时没有预先定义__NVCC__宏，如果程序依赖于这个宏需要在makefile文件中手动定义。

### -fPIE问题
天数编译器编译出来的.o文件在链接时默认会链接成一个PIE文件，但在编译时没有加上-fPIE选项。

在链接时加入-no-pie选项。

### device-to-host data transport

部分程序在执行过程中会堵塞在同步或将数据传回到host的数据传输上，后续其它程序运行也会堵塞在同步和将数据传回到host的数据传输上，即使该程序已经终止。

```
#include <cuda.h>
#include <cuda_runtime.h>

#include <iostream>

using namespace std;

__global__ void vecadd(float* a, float* b, float* c, int max){
    int tid = threadIdx.x;
    if(tid < max){
    	   c[tid] = a[tid] + b[tid];
    }
}

int main(){
    int size = 2 << 20;
    float* a = new float[size];
    float* b = new float[size];
    float* c = new float[size];

    for(int i = 0; i < size; i++){
        a[i] = i;
	b[i] = i;
    }

    float* da;
    float* db;
    float* dc;

    cudaMalloc(&da, size * sizeof(float));
    cudaMalloc(&db, size * sizeof(float));
    cudaMalloc(&dc, size * sizeof(float));

    cudaMemcpyAsync(da, a, size * sizeof(float), cudaMemcpyHostToDevice);
    cout << "transporting array a" << endl;
    cudaStreamSynchronize(0);
    cout << "first synchronization" << endl;

    cudaMemcpyAsync(db, b, size * sizeof(float), cudaMemcpyHostToDevice);
    cout << "transporting array b" << endl;
    cudaStreamSynchronize(0);
    cout << "second synchronization" << endl;

    vecadd<<<size / 128, 128>>>(da, db, dc, size);

    //cudaMemcpy(c, dc, size * sizeof(float), cudaMemcpyDeviceToHost);
    cudaMemcpyAsync(c, dc, size * sizeof(float), cudaMemcpyDeviceToHost);  //程序卡死在这里
    cout << "transporting array dc" << endl;  //
    cudaStreamSynchronize(0);
    cout << "third synchronization" << endl;

    cudaFree(da);
    cudaFree(db);
    cudaFree(dc);

    delete [] a;
    delete [] b;
    delete [] c;

    return 0;
}
```
重启可以暂时解决问题？

如果多个程序在同一个设备上运行，并且这些程序需要进行同步，则有可能卡死？

-----------2023.10.3------------

原因是程序中试图调用cudaMallocManaged分配统一内存，但天数GPU不支持统一内存。

使用cudaMallocManaged分配内存将返回nullptr。

### math库

```
non-constexpr declaration of 'cos' follows constexpr declaration
extern __DEVICE_FUNCTIONS_DECL__ __cudart_builtin__ float    __cdecl cos(float);
                                                                     ^
/usr/lib/gcc/x86_64-linux-gnu/12/../../../../include/c++/12/cmath:184:3: note: previous declaration is here
  cos(float __x)
  ^
```

### openmp

可能缺少openmp支持

-------------------2023.10.8---------------

需要手动提供omp相关的头文件和库文件的路径

omp.h:
```
/usr/lib/gcc/x86_64-linux-gnu/11/include
```

### cudaTextureObject_t

在cuda中cudaTextureObject_t被定义为unsigned long long，但天数提供的cuda库没有定义这个类型

### 库内部符号链接
```
/usr/bin/ld: /usr/local/corex-3.0.0/bin/../lib/libcudart.so: undefined reference to `cuProfilerStart@CUDA'
/usr/bin/ld: /usr/local/corex-3.0.0/bin/../lib/libcudart.so: undefined reference to `cuProfilerStop@CUDA'
/usr/bin/ld: /usr/local/corex-3.0.0/bin/../lib/libcudart.so: undefined reference to `cuInit@CUDA'
/usr/bin/ld: /usr/local/corex-3.0.0/bin/../lib/libcudart.so: undefined reference to `cuModuleLoadData@CUDA'
/usr/bin/ld: /usr/local/corex-3.0.0/bin/../lib/libcudart.so: undefined reference to `cuDriverGetVersion@CUDA'
/usr/bin/ld: /usr/local/corex-3.0.0/bin/../lib/libcudart.so: undefined reference to `cuGetExportTable@CUDA'
/usr/bin/ld: /usr/local/corex-3.0.0/bin/../lib/libcudart.so: undefined reference to `cuDeviceGetCount@CUDA'
/usr/bin/ld: /usr/local/corex-3.0.0/bin/../lib/libcudart.so: undefined reference to `cuProfilerInitialize@CUDA'

```
### 特殊符号识别

天数编译器似乎不能正确识别<< <和>> >。

### 纹理相关函数

部分程序中似乎使用了与纹理有关的函数，天数提供的cuda库中不确定有没有实现。