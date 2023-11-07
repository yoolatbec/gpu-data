# A100 SYCL

## 配置1（build-cuda）

--------------------------2023.11.2-----------------------

在编译acpp-clang库时链接了libclang.so, libclang-cpp.so, libLLVM.so

编译参数：
```
clang++ -fsycl test.cpp -L${HOME}/share/lib/hipSYCL -L${HOME}/share -L${CUDA_ROOT}/lib64 -I${HOME}/share/include/AdaptiveCpp -lacpp-rt -lacpp-common -lrt-backend-cuda -lcuda -lcudart 
```

报错信息：
```
launcher count: 0
[AdaptiveCpp Warning] dag_direct_scheduler: Detected a requirement that is neither of discard access mode (SYCL 1.2.1) nor no_init property (SYCL 2020) that accesses uninitialized data. Consider changing to discard/no_init. Optimizing potential data transfers away.
[AdaptiveCpp Error] from /mnt/sda/2022-0526/home/lhz/gpu-sycl/AdaptiveCpp/include/hipSYCL/runtime/kernel_launcher.hpp:139 @ find_launcher(): No kernel launcher is present for requested backend
[AdaptiveCpp Error] from /mnt/sda/2022-0526/home/lhz/gpu-sycl/AdaptiveCpp/src/runtime/cuda/cuda_queue.cpp:354 @ submit_kernel(): Could not obtain backend kernel launcher
[AdaptiveCpp Error] accessor [host]: Aborting synchronization, runtime error list is non-empty
============== hipSYCL error report ============== 
hipSYCL has caught the following undhandled asynchronous errors: 

terminate called after throwing an instance of 'hipsycl::sycl::exception'
  what():  from /mnt/sda/2022-0526/home/lhz/gpu-sycl/AdaptiveCpp/include/hipSYCL/runtime/kernel_launcher.hpp:139 @ find_launcher(): No kernel launcher is present for requested backend
Aborted
```

编译参数：
```
clang++ -fsycl test.cpp -L${HOME}/share/lib/hipSYCL -L${HOME}/share -L${CUDA_ROOT}/lib64 -I${HOME}/share/include/AdaptiveCpp -lacpp-clang -lacpp-rt -lacpp-common -lrt-backend-cuda -lcuda -lcudart
```

报错信息：
```
: CommandLine Error: Option 'openmp-ir-builder-optimistic-attributes' registered more than once!
LLVM ERROR: inconsistency in registered CommandLine options
Aborted
```

使用acpp进行编译报错：
```
: CommandLine Error: Option 'disable-auto-upgrade-debug-info' registered more than once!
LLVM ERROR: inconsistency in registered CommandLine options
PLEASE submit a bug report to https://github.com/llvm/llvm-project/issues/ and include the crash backtrace, preprocessed source, and associated run script.
Stack dump:
0.	Program arguments: /mnt/sda/2022-0526/home/lhz/share/bin/clang-17 -cc1 -triple nvptx64-nvidia-cuda -aux-triple x86_64-unknown-linux-gnu -S -dumpdir a- -disable-free -clear-ast-before-backend -disable-llvm-verifier -discard-value-names -main-file-name test.cpp -mrelocation-model static -mframe-pointer=all -fno-rounding-math -no-integrated-as -aux-target-cpu x86-64 -fcuda-is-device -mllvm -enable-memcpyopt-without-libcalls -fcuda-allow-variadic-functions -mlink-builtin-bitcode /mnt/sda/2022-0526/home/lhz/applications/spack/opt/spack/linux-debian11-zen2/gcc-10.2.1/cuda-11.7.0-pi2jeygiwu3745m6urydwcm4er2nkye7/nvvm/libdevice/libdevice.10.bc -target-sdk-version=11.7 -target-cpu sm_80 -target-feature +ptx77 -debugger-tuning=gdb -fno-dwarf-directory-asm -resource-dir /mnt/sda/2022-0526/home/lhz/share/lib/clang/17 -internal-isystem /mnt/sda/2022-0526/home/lhz/share/lib/clang/17/include/cuda_wrappers -include __clang_cuda_runtime_wrapper.h -isystem /mnt/sda/2022-0526/home/lhz/share/bin/../include/AdaptiveCpp -isystem /mnt/sda/2022-0526/home/lhz/share/bin/../include/AdaptiveCpp/hipSYCL/std/hiplike -D __OPENSYCL__ -D __HIPSYCL__ -D __ADAPTIVECPP__ -D __ACPP__ -D __HIPSYCL_ENABLE_CUDA_TARGET__ -D __HIPSYCL_CLANG__ -U __FLOAT128__ -U __SIZEOF_FLOAT128__ -D __HIPSYCL_ENABLE_OMPHOST_TARGET__ -I /mnt/sda/2022-0526/home/lhz/share/include -D _ENABLE_EXTENDED_ALIGNED_STORAGE -c-isystem /mnt/sda/2022-0526/home/lhz/share/include/ncurses -c-isystem /mnt/sda/2022-0526/home/lhz/workspace/include -c-isystem /mnt/sda/2022-0526/home/lhz/share/include -c-isystem /mnt/sda/2022-0526/home/lhz/applications/spack/opt/spack/linux-debian11-zen2/gcc-10.2.1/cuda-11.7.0-pi2jeygiwu3745m6urydwcm4er2nkye7/include -internal-isystem /usr/lib/gcc/x86_64-linux-gnu/10/../../../../include/c++/10 -internal-isystem /usr/lib/gcc/x86_64-linux-gnu/10/../../../../include/x86_64-linux-gnu/c++/10 -internal-isystem /usr/lib/gcc/x86_64-linux-gnu/10/../../../../include/c++/10/backward -internal-isystem /usr/lib/gcc/x86_64-linux-gnu/10/../../../../include/c++/10 -internal-isystem /usr/lib/gcc/x86_64-linux-gnu/10/../../../../include/x86_64-linux-gnu/c++/10 -internal-isystem /usr/lib/gcc/x86_64-linux-gnu/10/../../../../include/c++/10/backward -internal-isystem /mnt/sda/2022-0526/home/lhz/share/lib/clang/17/include -internal-isystem /usr/local/include -internal-isystem /usr/lib/gcc/x86_64-linux-gnu/10/../../../../x86_64-linux-gnu/include -internal-externc-isystem /usr/include/x86_64-linux-gnu -internal-externc-isystem /include -internal-externc-isystem /usr/include -internal-isystem /mnt/sda/2022-0526/home/lhz/applications/spack/opt/spack/linux-debian11-zen2/gcc-10.2.1/cuda-11.7.0-pi2jeygiwu3745m6urydwcm4er2nkye7/include -internal-isystem /mnt/sda/2022-0526/home/lhz/share/lib/clang/17/include -internal-isystem /usr/local/include -internal-isystem /usr/lib/gcc/x86_64-linux-gnu/10/../../../../x86_64-linux-gnu/include -internal-externc-isystem /usr/include/x86_64-linux-gnu -internal-externc-isystem /include -internal-externc-isystem /usr/include -std=c++17 -fdeprecated-macro -fno-autolink -fdebug-compilation-dir=/mnt/sda/2022-0526/home/lhz/gpu-sycl -ferror-limit 19 -fgnuc-version=4.2.1 -fcxx-exceptions -fexceptions -fcolor-diagnostics -load /mnt/sda/2022-0526/home/lhz/share/bin/../lib/libacpp-clang.so -fpass-plugin=/mnt/sda/2022-0526/home/lhz/share/bin/../lib/libacpp-clang.so -cuid=a31d30b8a4a10878 -D__GCC_HAVE_DWARF2_CFI_ASM=1 -o /tmp/test-sm_80-e75ce4.s -x cuda test.cpp
 #0 0x00000000029121f7 llvm::sys::PrintStackTrace(llvm::raw_ostream&, int) (/mnt/sda/2022-0526/home/lhz/share/bin/clang-17+0x29121f7)
 #1 0x000000000291016e llvm::sys::RunSignalHandlers() (/mnt/sda/2022-0526/home/lhz/share/bin/clang-17+0x291016e)
 #2 0x000000000291289f SignalHandler(int) Signals.cpp:0:0
 #3 0x00007f0d2522c140 __restore_rt (/lib/x86_64-linux-gnu/libpthread.so.0+0x13140)
 #4 0x00007f0d24bedce1 raise (/lib/x86_64-linux-gnu/libc.so.6+0x38ce1)
 #5 0x00007f0d24bd7537 abort (/lib/x86_64-linux-gnu/libc.so.6+0x22537)
 #6 0x00007f0d0d272d50 llvm::report_fatal_error(llvm::Twine const&, bool) (/mnt/sda/2022-0526/home/lhz/share/lib/libLLVM-17.so+0x83fd50)
 #7 0x00007f0d0d272b66 (/mnt/sda/2022-0526/home/lhz/share/lib/libLLVM-17.so+0x83fb66)
 #8 0x00007f0d0d25e9c0 (anonymous namespace)::CommandLineParser::removeOption(llvm::cl::Option*, llvm::cl::SubCommand*) CommandLine.cpp:0:0
 #9 0x00007f0d0d25040b llvm::cl::Option::addArgument() (/mnt/sda/2022-0526/home/lhz/share/lib/libLLVM-17.so+0x81d40b)
#10 0x00007f0d0d19a8e2 _GLOBAL__sub_I_AutoUpgrade.cpp AutoUpgrade.cpp:0:0
#11 0x00007f0d25257fe2 (/lib64/ld-linux-x86-64.so.2+0xffe2)
#12 0x00007f0d252580e9 (/lib64/ld-linux-x86-64.so.2+0x100e9)
#13 0x00007f0d24ceaaed _dl_catch_exception (/lib/x86_64-linux-gnu/libc.so.6+0x135aed)
#14 0x00007f0d2525c364 (/lib64/ld-linux-x86-64.so.2+0x14364)
#15 0x00007f0d24ceaa90 _dl_catch_exception (/lib/x86_64-linux-gnu/libc.so.6+0x135a90)
#16 0x00007f0d2525b8fa (/lib64/ld-linux-x86-64.so.2+0x138fa)
#17 0x00007f0d2520a258 (/lib/x86_64-linux-gnu/libdl.so.2+0x1258)
#18 0x00007f0d24ceaa90 _dl_catch_exception (/lib/x86_64-linux-gnu/libc.so.6+0x135a90)
#19 0x00007f0d24ceab4f _dl_catch_error (/lib/x86_64-linux-gnu/libc.so.6+0x135b4f)
#20 0x00007f0d2520aa65 (/lib/x86_64-linux-gnu/libdl.so.2+0x1a65)
#21 0x00007f0d2520a2e4 dlopen (/lib/x86_64-linux-gnu/libdl.so.2+0x12e4)
#22 0x0000000002902289 llvm::sys::DynamicLibrary::getPermanentLibrary(char const*, std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char>>*) (/mnt/sda/2022-0526/home/lhz/share/bin/clang-17+0x2902289)
#23 0x00000000030f6b8b clang::CompilerInstance::LoadRequestedPlugins() (/mnt/sda/2022-0526/home/lhz/share/bin/clang-17+0x30f6b8b)
#24 0x000000000323f8bf clang::ExecuteCompilerInvocation(clang::CompilerInstance*) (/mnt/sda/2022-0526/home/lhz/share/bin/clang-17+0x323f8bf)
#25 0x0000000000aba2d7 cc1_main(llvm::ArrayRef<char const*>, char const*, void*) (/mnt/sda/2022-0526/home/lhz/share/bin/clang-17+0xaba2d7)
#26 0x0000000000ab7eb5 ExecuteCC1Tool(llvm::SmallVectorImpl<char const*>&, llvm::ToolContext const&) driver.cpp:0:0
#27 0x0000000000ab6e74 clang_main(int, char**, llvm::ToolContext const&) (/mnt/sda/2022-0526/home/lhz/share/bin/clang-17+0xab6e74)
#28 0x0000000000ac4dd1 main (/mnt/sda/2022-0526/home/lhz/share/bin/clang-17+0xac4dd1)
#29 0x00007f0d24bd8d0a __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x23d0a)
#30 0x0000000000ab447a _start (/mnt/sda/2022-0526/home/lhz/share/bin/clang-17+0xab447a)
clang++: error: unable to execute command: Aborted
clang++: error: clang frontend command failed due to signal (use -v to see invocation)
clang version 17.0.1
Target: x86_64-unknown-linux-gnu
Thread model: posix
InstalledDir: /mnt/sda/2022-0526/home/lhz/share/bin
clang++: error: unable to execute command: Aborted
clang++: note: diagnostic msg: Error generating preprocessed source(s).
```

## 配置2（build-default）

cmake参数：

```
cmake -DCMAKE_INSTALL_PREFIX=~/share -DCUDA_TOOLKIT_ROOT_DIR=${CUDA_ROOT} -DWITH_CUDA_BACKEND=ON -DWITH_OPENCL_BACKEND=OFF ..
```

在编译libacpp-clang.so时不链接libclang.so libclang-cpp.so libLLVM.so。

使用acpp可以正常编译，可以正常运行。

编译参数：
```
clang++ -fsycl test.cpp -L${HOME}/share/lib -L${HOME}/share/lib/hipSYCL -I${HOME}/share/include/AdaptiveCpp -L${CUDA_ROOT}/lib64 -fpass-plugin=${HOME}/share/lib/libacpp-clang.so -lacpp-common -lacpp-rt -lrt-backend-cuda -lcuda -lcudart -lclang -lLLVM
```

报错信息：
```
launcher count: 0
[AdaptiveCpp Warning] dag_direct_scheduler: Detected a requirement that is neither of discard access mode (SYCL 1.2.1) nor no_init property (SYCL 2020) that accesses uninitialized data. Consider changing to discard/no_init. Optimizing potential data transfers away.
[AdaptiveCpp Error] from /mnt/sda/2022-0526/home/lhz/gpu-sycl/AdaptiveCpp/include/hipSYCL/runtime/kernel_launcher.hpp:139 @ find_launcher(): No kernel launcher is present for requested backend
[AdaptiveCpp Error] from /mnt/sda/2022-0526/home/lhz/gpu-sycl/AdaptiveCpp/src/runtime/cuda/cuda_queue.cpp:354 @ submit_kernel(): Could not obtain backend kernel launcher
[AdaptiveCpp Error] accessor [host]: Aborting synchronization, runtime error list is non-empty
============== hipSYCL error report ============== 
hipSYCL has caught the following undhandled asynchronous errors: 

terminate called after throwing an instance of 'hipsycl::sycl::exception'
  what():  from /mnt/sda/2022-0526/home/lhz/gpu-sycl/AdaptiveCpp/include/hipSYCL/runtime/kernel_launcher.hpp:139 @ find_launcher(): No kernel launcher is present for requested backend
Aborted
```

acpp提供的默认参数：
```
/mnt/sda/2022-0526/home/lhz/share/bin/clang++ -isystem /mnt/sda/2022-0526/home/lhz/share/bin/../include/AdaptiveCpp -D__OPENSYCL__ -D__HIPSYCL__ -D__ADAPTIVECPP__ -D__ACPP__ -std=c++17 -x cuda --cuda-path=/mnt/sda/2022-0526/home/lhz/applications/spack/opt/spack/linux-debian11-zen2/gcc-10.2.1/cuda-11.7.0-pi2jeygiwu3745m6urydwcm4er2nkye7 -D__HIPSYCL_ENABLE_CUDA_TARGET__ -fplugin=/mnt/sda/2022-0526/home/lhz/share/bin/../lib/libacpp-clang.so -D__HIPSYCL_CLANG__ -U__FLOAT128__ -U__SIZEOF_FLOAT128__ -isystem /mnt/sda/2022-0526/home/lhz/share/bin/../include/AdaptiveCpp/hipSYCL/std/hiplike --cuda-gpu-arch=sm_80 -fpass-plugin=/mnt/sda/2022-0526/home/lhz/share/bin/../lib/libacpp-clang.so -D__HIPSYCL_ENABLE_OMPHOST_TARGET__ -I/mnt/sda/2022-0526/home/lhz/share/include -D_ENABLE_EXTENDED_ALIGNED_STORAGE test.cpp -o test -L/mnt/sda/2022-0526/home/lhz/share/bin/../lib/ -lacpp-rt -Wl,-rpath=/mnt/sda/2022-0526/home/lhz/share/bin/../lib/ -Wl,-rpath=/mnt/sda/2022-0526/home/lhz/applications/spack/opt/spack/linux-debian11-zen2/gcc-10.2.1/cuda-11.7.0-pi2jeygiwu3745m6urydwcm4er2nkye7/lib64 -L/mnt/sda/2022-0526/home/lhz/applications/spack/opt/spack/linux-debian11-zen2/gcc-10.2.1/cuda-11.7.0-pi2jeygiwu3745m6urydwcm4er2nkye7/lib64 -lcudart -L/mnt/sda/2022-0526/home/lhz/share/lib -lboost_context -lboost_fiber -Wl,-rpath=/mnt/sda/2022-0526/home/lhz/share/lib
```

编译参数：
```
clang++ -L${HOME}/share/lib -L${HOME}/share/lib/hipSYCL -I${HOME}/share/include/AdaptiveCpp -L${CUDA_ROOT}/lib64 -D__OPENSYCL__ -D__HIPSYCL__ -D__ADAPTIVECPP__ -D__ACPP__ -D__HIPSYCL_ENABLE_CUDA_TARGET__ -std=c++20 -x cuda  -fplugin=${HOME}/share/lib/libacpp-clang.so --cuda-gpu-arch=sm_80 -fpass-plugin=${HOME}/share/lib/libacpp-clang.so -lacpp-rt -lacpp-common -lcudart -lboost_fiber -lboost_context test.cpp -o test
```

可以成功编译，可以正常运行。

## SYCL-bench

### 2DConvolution

```
********** Results for Polybench_2DConvolution**********
problem-size: 3072
local-size: 256
device-name: NVIDIA A100-PCIE-40GB
sycl-implementation: hipSYCL
launcher count: 2
...
launcher count: 2
throughput-metric: N/A
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 0.066464 [s]
run-time-stddev: 0.017659 [s]
run-time-median: 0.075405 [s]
run-time-min: 0.046605 [s]
run-time-samples: "0.046605 0.048016 0.075405 0.080271 0.082023"
run-time-throughput: N/A
Verification: PASS
```

### 2mm

```
********** Results for Polybench_2mm**********
problem-size: 3072
local-size: 256
device-name: NVIDIA A100-PCIE-40GB
sycl-implementation: hipSYCL
launcher count: 2
...
launcher count: 2
throughput-metric: N/A
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 77.276196 [s]
run-time-stddev: 0.026628 [s]
run-time-median: 77.289057 [s]
run-time-min: 77.233161 [s]
run-time-samples: "77.233161 77.267725 77.289057 77.295194 77.295842"
run-time-throughput: N/A
Verification: PASS

real	31m21.971s
user	31m5.613s
sys	0m1.988s
```

### 3DConvolution

```
********** Results for Polybench_3DConvolution**********
problem-size: 3072
local-size: 256
device-name: NVIDIA A100-PCIE-40GB
sycl-implementation: hipSYCL
launcher count: 2
[AdaptiveCpp Error] from /mnt/sda/2022-0526/home/lhz/gpu-sycl/AdaptiveCpp/src/runtime/cuda/cuda_allocator.cpp:48 @ allocate(): cuda_allocator: cudaMalloc() failed (error code = CUDA:2)
[AdaptiveCpp Error] from /mnt/sda/2022-0526/home/lhz/gpu-sycl/AdaptiveCpp/src/runtime/dag_direct_scheduler.cpp:113 @ ensure_allocation_exists(): dag_direct_scheduler: Lazy memory allocation has failed.
[AdaptiveCpp Error] from /mnt/sda/2022-0526/home/lhz/gpu-sycl/AdaptiveCpp/src/runtime/dag_direct_scheduler.cpp:113 @ ensure_allocation_exists(): dag_direct_scheduler: Lazy memory allocation has failed.
============== hipSYCL error report ============== 
hipSYCL has caught the following undhandled asynchronous errors: 

   0. from /mnt/sda/2022-0526/home/lhz/gpu-sycl/AdaptiveCpp/src/runtime/cuda/cuda_allocator.cpp:48 @ allocate(): cuda_allocator: cudaMalloc() failed (error code = CUDA:2)
   1. from /mnt/sda/2022-0526/home/lhz/gpu-sycl/AdaptiveCpp/src/runtime/dag_direct_scheduler.cpp:113 @ ensure_allocation_exists(): dag_direct_scheduler: Lazy memory allocation has failed.
   2. from /mnt/sda/2022-0526/home/lhz/gpu-sycl/AdaptiveCpp/src/runtime/dag_direct_scheduler.cpp:113 @ ensure_allocation_exists(): dag_direct_scheduler: Lazy memory allocation has failed.
The application will now be terminated.
terminate called without an active exception
```

### 3mm

```
********** Results for Polybench_3mm**********
problem-size: 3072
local-size: 256
device-name: NVIDIA A100 80GB PCIe
sycl-implementation: hipSYCL
launcher count: 2
...
launcher count: 2
throughput-metric: N/A
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 107.720678 [s]
run-time-stddev: 0.548102 [s]
run-time-median: 107.891123 [s]
run-time-min: 106.750496 [s]
run-time-samples: "106.750496 107.883307 107.891123 108.007950 108.070516"
run-time-throughput: N/A
Verification: PASS

real	46m14.344s
user	46m2.401s
sys	0m2.016s
```

### arith

```
********** Results for MicroBench_Arith_int32_512**********
problem-size: 3072
local-size: 256
device-name: NVIDIA A100 80GB PCIe
sycl-implementation: hipSYCL
launcher count: 2
...
launcher count: 2
throughput-metric: 0.005859 [GOP]
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 0.000762 [s]
run-time-stddev: 0.000069 [s]
run-time-median: 0.000732 [s]
run-time-min: 0.000728 [s]
run-time-samples: "0.000728 0.000731 0.000732 0.000736 0.000885"
run-time-throughput: 8.046039 [GOP/s]
Verification: PASS
********** Results for MicroBench_Arith_fp32_512**********
problem-size: 3072
local-size: 256
device-name: NVIDIA A100 80GB PCIe
sycl-implementation: hipSYCL
launcher count: 2
...
launcher count: 2
throughput-metric: 0.005859 [SP GFLOP]
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 0.000753 [s]
run-time-stddev: 0.000060 [s]
run-time-median: 0.000727 [s]
run-time-min: 0.000723 [s]
run-time-samples: "0.000723 0.000724 0.000727 0.000733 0.000861"
run-time-throughput: 8.108627 [SP GFLOP/s]
Verification: PASS
********** Results for MicroBench_Arith_fp64_512**********
problem-size: 3072
local-size: 256
device-name: NVIDIA A100 80GB PCIe
sycl-implementation: hipSYCL
launcher count: 2
...
launcher count: 2
throughput-metric: 0.005859 [DP GFLOP]
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 0.000765 [s]
run-time-stddev: 0.000060 [s]
run-time-median: 0.000739 [s]
run-time-min: 0.000732 [s]
run-time-samples: "0.000732 0.000739 0.000739 0.000743 0.000872"
run-time-throughput: 8.000446 [DP GFLOP/s]
Verification: PASS
```

### atax

```
********** Results for Polybench_Atax**********
problem-size: 3072
local-size: 256
device-name: NVIDIA A100 80GB PCIe
sycl-implementation: hipSYCL
launcher count: 2
...
launcher count: 2
throughput-metric: N/A
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 0.408849 [s]
run-time-stddev: 0.000096 [s]
run-time-median: 0.408826 [s]
run-time-min: 0.408777 [s]
run-time-samples: "0.408777 0.408781 0.408826 0.408848 0.409011"
run-time-throughput: N/A
Verification: PASS
```

### bicg

```
********** Results for Polybench_Bicg**********
problem-size: 3072
local-size: 256
device-name: NVIDIA A100 80GB PCIe
sycl-implementation: hipSYCL
launcher count: 2
...
launcher count: 2
throughput-metric: N/A
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 0.246500 [s]
run-time-stddev: 0.090837 [s]
run-time-median: 0.205860 [s]
run-time-min: 0.205841 [s]
run-time-samples: "0.205841 0.205855 0.205860 0.205948 0.408995"
run-time-throughput: N/A
Verification: PASS
```

### blocked_transform

```
********** Results for Runtime_BlockedTransform_iter_64_blocksize_256**********
problem-size: 3072
local-size: 256
device-name: NVIDIA A100-PCIE-40GB
sycl-implementation: hipSYCL
launcher count: 2
...
launcher count: 2
throughput-metric: N/A
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 0.016639 [s]
run-time-stddev: 0.000306 [s]
run-time-median: 0.016512 [s]
run-time-min: 0.016482 [s]
run-time-samples: "0.016482 0.016490 0.016512 0.016524 0.017187"
run-time-throughput: N/A
Verification: PASS
********** Results for Runtime_BlockedTransform_iter_128_blocksize_256**********
problem-size: 3072
local-size: 256
device-name: NVIDIA A100-PCIE-40GB
sycl-implementation: hipSYCL
launcher count: 2
...
launcher count: 2
throughput-metric: N/A
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 0.027533 [s]
run-time-stddev: 0.003581 [s]
run-time-median: 0.026493 [s]
run-time-min: 0.024260 [s]
run-time-samples: "0.024260 0.024261 0.026493 0.031290 0.031363"
run-time-throughput: N/A
Verification: PASS
********** Results for Runtime_BlockedTransform_iter_256_blocksize_256**********
problem-size: 3072
local-size: 256
device-name: NVIDIA A100-PCIE-40GB
sycl-implementation: hipSYCL
launcher count: 2
...
launcher count: 2
throughput-metric: N/A
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 0.388662 [s]
run-time-stddev: 0.488962 [s]
run-time-median: 0.062886 [s]
run-time-min: 0.046309 [s]
run-time-samples: "0.046309 0.046481 0.062886 0.664093 1.123543"
run-time-throughput: N/A
Verification: PASS
********** Results for Runtime_BlockedTransform_iter_512_blocksize_256**********
problem-size: 3072
local-size: 256
device-name: NVIDIA A100-PCIE-40GB
sycl-implementation: hipSYCL
launcher count: 2
...
launcher count: 2
throughput-metric: N/A
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 0.090686 [s]
run-time-stddev: 0.000218 [s]
run-time-median: 0.090558 [s]
run-time-min: 0.090533 [s]
run-time-samples: "0.090533 0.090551 0.090558 0.090744 0.091044"
run-time-throughput: N/A
Verification: PASS
********** Results for Runtime_BlockedTransform_iter_64_blocksize_512**********
problem-size: 3072
local-size: 256
device-name: NVIDIA A100-PCIE-40GB
sycl-implementation: hipSYCL
launcher count: 2
...
launcher count: 2
throughput-metric: N/A
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 0.357548 [s]
run-time-stddev: 0.498170 [s]
run-time-median: 0.008705 [s]
run-time-min: 0.008592 [s]
run-time-samples: "0.008592 0.008606 0.008705 0.681374 1.080463"
run-time-throughput: N/A
Verification: PASS
********** Results for Runtime_BlockedTransform_iter_128_blocksize_512**********
problem-size: 3072
local-size: 256
device-name: NVIDIA A100-PCIE-40GB
sycl-implementation: hipSYCL
launcher count: 2
...
launcher count: 2
throughput-metric: N/A
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 0.016081 [s]
run-time-stddev: 0.000058 [s]
run-time-median: 0.016102 [s]
run-time-min: 0.015978 [s]
run-time-samples: "0.015978 0.016098 0.016102 0.016106 0.016119"
run-time-throughput: N/A
Verification: PASS
********** Results for Runtime_BlockedTransform_iter_256_blocksize_512**********
problem-size: 3072
local-size: 256
device-name: NVIDIA A100-PCIE-40GB
sycl-implementation: hipSYCL
launcher count: 2
...
launcher count: 2
throughput-metric: N/A
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 0.030983 [s]
run-time-stddev: 0.000597 [s]
run-time-median: 0.030916 [s]
run-time-min: 0.030068 [s]
run-time-samples: "0.030068 0.030902 0.030916 0.031444 0.031587"
run-time-throughput: N/A
Verification: PASS
********** Results for Runtime_BlockedTransform_iter_512_blocksize_512**********
problem-size: 3072
local-size: 256
device-name: NVIDIA A100-PCIE-40GB
sycl-implementation: hipSYCL
launcher count: 2
...
launcher count: 2
throughput-metric: N/A
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 0.046749 [s]
run-time-stddev: 0.001195 [s]
run-time-median: 0.046175 [s]
run-time-min: 0.045520 [s]
run-time-samples: "0.045520 0.045987 0.046175 0.048031 0.048031"
run-time-throughput: N/A
Verification: PASS
********** Results for Runtime_BlockedTransform_iter_64_blocksize_1024**********
problem-size: 3072
local-size: 256
device-name: NVIDIA A100-PCIE-40GB
sycl-implementation: hipSYCL
launcher count: 2
...
launcher count: 2
throughput-metric: N/A
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 0.338937 [s]
run-time-stddev: 0.747412 [s]
run-time-median: 0.004658 [s]
run-time-min: 0.004550 [s]
run-time-samples: "0.004550 0.004558 0.004658 0.004968 1.675948"
run-time-throughput: N/A
Verification: PASS
********** Results for Runtime_BlockedTransform_iter_128_blocksize_1024**********
problem-size: 3072
local-size: 256
device-name: NVIDIA A100-PCIE-40GB
sycl-implementation: hipSYCL
...
launcher count: 2
throughput-metric: N/A
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 0.008429 [s]
run-time-stddev: 0.000302 [s]
run-time-median: 0.008301 [s]
run-time-min: 0.008268 [s]
run-time-samples: "0.008268 0.008275 0.008301 0.008333 0.008967"
run-time-throughput: N/A
Verification: PASS
********** Results for Runtime_BlockedTransform_iter_256_blocksize_1024**********
problem-size: 3072
local-size: 256
device-name: NVIDIA A100-PCIE-40GB
sycl-implementation: hipSYCL
launcher count: 2
...
launcher count: 2
throughput-metric: N/A
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 0.017111 [s]
run-time-stddev: 0.000084 [s]
run-time-median: 0.017144 [s]
run-time-min: 0.016965 [s]
run-time-samples: "0.016965 0.017127 0.017144 0.017147 0.017174"
run-time-throughput: N/A
Verification: PASS
********** Results for Runtime_BlockedTransform_iter_512_blocksize_1024**********
problem-size: 3072
local-size: 256
device-name: NVIDIA A100-PCIE-40GB
sycl-implementation: hipSYCL
launcher count: 2
...
launcher count: 2
throughput-metric: N/A
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 0.030987 [s]
run-time-stddev: 0.000471 [s]
run-time-median: 0.030653 [s]
run-time-min: 0.030633 [s]
run-time-samples: "0.030633 0.030647 0.030653 0.031474 0.031531"
run-time-throughput: N/A
Verification: PASS
********** Results for Runtime_BlockedTransform_iter_64_blocksize_2048**********
problem-size: 3072
local-size: 256
device-name: NVIDIA A100-PCIE-40GB
sycl-implementation: hipSYCL
launcher count: 2
...
launcher count: 2
throughput-metric: N/A
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 0.002817 [s]
run-time-stddev: 0.000008 [s]
run-time-median: 0.002814 [s]
run-time-min: 0.002809 [s]
run-time-samples: "0.002809 0.002813 0.002814 0.002818 0.002831"
run-time-throughput: N/A
Verification: PASS
********** Results for Runtime_BlockedTransform_iter_128_blocksize_2048**********
problem-size: 3072
local-size: 256
device-name: NVIDIA A100-PCIE-40GB
sycl-implementation: hipSYCL
launcher count: 2
...
launcher count: 2
throughput-metric: N/A
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 0.005060 [s]
run-time-stddev: 0.000011 [s]
run-time-median: 0.005063 [s]
run-time-min: 0.005048 [s]
run-time-samples: "0.005048 0.005049 0.005063 0.005068 0.005073"
run-time-throughput: N/A
Verification: PASS
********** Results for Runtime_BlockedTransform_iter_256_blocksize_2048**********
problem-size: 3072
local-size: 256
device-name: NVIDIA A100-PCIE-40GB
sycl-implementation: hipSYCL
launcher count: 2
...
launcher count: 2
throughput-metric: N/A
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 0.361270 [s]
run-time-stddev: 0.510679 [s]
run-time-median: 0.010067 [s]
run-time-min: 0.008186 [s]
run-time-samples: "0.008186 0.009541 0.010067 0.650672 1.127883"
run-time-throughput: N/A
Verification: PASS
********** Results for Runtime_BlockedTransform_iter_512_blocksize_2048**********
problem-size: 3072
local-size: 256
device-name: NVIDIA A100-PCIE-40GB
sycl-implementation: hipSYCL
launcher count: 2
...
launcher count: 2
throughput-metric: N/A
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 0.015453 [s]
run-time-stddev: 0.000038 [s]
run-time-median: 0.015442 [s]
run-time-min: 0.015406 [s]
run-time-samples: "0.015406 0.015441 0.015442 0.015466 0.015509"
run-time-throughput: N/A
Verification: PASS
```

### correlation

### covariance

### DRAM

### fdtd2d

### gemm

```
********** Results for Polybench_Gemm**********
problem-size: 3072
local-size: 256
device-name: NVIDIA A100 80GB PCIe
sycl-implementation: hipSYCL
launcher count: 2
...
launcher count: 2
throughput-metric: N/A
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 40.037717 [s]
run-time-stddev: 0.006900 [s]
run-time-median: 40.035420 [s]
run-time-min: 40.030808 [s]
run-time-samples: "40.030808 40.032364 40.035420 40.043821 40.046174"
run-time-throughput: N/A
Verification: PASS

real	34m26.316s
user	34m41.106s
sys	0m1.540s
```

### gesummv

### gramschmidt

### host_device_bandwidth

### kmeans

### lin_reg_coeff

### lin_reg_error

### local_mem

### matmulchain

### median

### mol_dyn

### mvt

### nbody

### pattern_L2

### reduction

### scalar_prod

### segmentedreduction

### sf

### sobel

### sobe5

### sobel7

### syr2k

```
********** Results for Polybench_Syr2k**********
problem-size: 3072
local-size: 256
device-name: NVIDIA A100-PCIE-40GB
sycl-implementation: hipSYCL
launcher count: 2
...
launcher count: 2
throughput-metric: N/A
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 159.695254 [s]
run-time-stddev: 0.602716 [s]
run-time-median: 159.515994 [s]
run-time-min: 159.097913 [s]
run-time-samples: "159.097913 159.493377 159.515994 159.662645 160.706341"
run-time-throughput: N/A
Verification: PASS

real	29m13.546s
user	31m51.929s
sys	0m1.505s
```

### syrk

```
********** Results for Polybench_Syrk**********
problem-size: 3072
local-size: 256
device-name: NVIDIA A100 80GB PCIe
sycl-implementation: hipSYCL
launcher count: 2
launcher count: 2
launcher count: 2
launcher count: 2
launcher count: 2
launcher count: 2
launcher count: 2
launcher count: 2
launcher count: 2
launcher count: 2
launcher count: 2
launcher count: 2
launcher count: 2
launcher count: 2
launcher count: 2
throughput-metric: N/A
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 40.178531 [s]
run-time-stddev: 0.022819 [s]
run-time-median: 40.172018 [s]
run-time-min: 40.155796 [s]
run-time-samples: "40.155796 40.159565 40.172018 40.201800 40.203477"
run-time-throughput: N/A
Verification: PASS

real	11m15.986s
user	11m15.512s
sys	0m0.636s
```