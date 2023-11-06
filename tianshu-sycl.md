# tianshu-sycl

----------------2023.10.25---------------------

使用AdaptiveCPP编译测试程序报错：
```
//acpp test.cpp --acpp-targets=cuda.explicit-multipass:ivcore10
error: unable to load plugin '/mnt/nvme0/home/lianghz/share/bin/../lib/libacpp-clang.so': '/mnt/nvme0/home/lianghz/share/bin/../lib/libacpp-clang.so: undefined symbol: _ZNK4llvm10ModulePass17createPrinterPassERNS_11raw_ostreamERKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEE'
```

可以直接使用天数编译器编译sycl代码。

-------------------2023.10.26-------------------

可以使用acpp-info命令获取设备信息。

尝试直接使用clang编译测试程序，程序运行时报错（在编译测试程序时没有链接acpp-clang）：
```
[AdaptiveCpp Error] from /mnt/nvme0/home/lianghz/workspace/gpu-sycl/AdaptiveCpp/include/hipSYCL/runtime/kernel_launcher.hpp:137 @ find_launcher(): No kernel launcher is present for requested backend
[AdaptiveCpp Error] from /mnt/nvme0/home/lianghz/workspace/gpu-sycl/AdaptiveCpp/src/runtime/cuda/cuda_queue.cpp:354 @ submit_kernel(): Could not obtain backend kernel launcher
============== hipSYCL error report ============== 
hipSYCL has caught the following undhandled asynchronous errors: 

   0. from /mnt/nvme0/home/lianghz/workspace/gpu-sycl/AdaptiveCpp/include/hipSYCL/runtime/kernel_launcher.hpp:137 @ find_launcher(): No kernel launcher is present for requested backend
   1. from /mnt/nvme0/home/lianghz/workspace/gpu-sycl/AdaptiveCpp/src/runtime/cuda/cuda_queue.cpp:354 @ submit_kernel(): Could not obtain backend kernel launcher
The application will now be terminated.
terminate called without an active exception
Aborted (core dumped)
```

在编译acpp-clang.so时链接libLLVM后尝试使用acpp编译测试程序：
```
acpp warning: No optimization flag was given, optimizations are disabled by default. Performance may be degraded. Compile with e.g. -O2/-O3 to enable optimizations.
: CommandLine Error: Option 'use-dbg-addr' registered more than once!
LLVM ERROR: inconsistency in registered CommandLine options
PLEASE submit a bug report to https://bugs.llvm.org/ and include the crash backtrace, preprocessed source, and associated run script.
Stack dump:
0.	Program arguments: /usr/local/corex-3.0.0/bin/clang-13 -cc1 -triple bi-iluvatar-ilurt -aux-triple x86_64-unknown-linux-gnu -S -disable-free -disable-llvm-verifier -discard-value-names -main-file-name test.cpp -mrelocation-model static -mframe-pointer=all -fno-rounding-math -fno-verbose-asm -no-integrated-as -aux-target-cpu x86-64 -fcuda-is-device -cl-single-precision-constant -cl-single-precision-printf -mlink-builtin-bitcode /usr/local/corex/nvvm/libdevice/libdevice.compute_bi.10.bc -emit-llvm-bc -O3 -target-sdk-version=10.2 -DCUDA_BI=1 -fnative-half-type -fnative-half-arguments-and-returns -target-cpu ivcore10 -mllvm -treat-scalable-fixed-error-as-warning -debugger-tuning=gdb -fno-dwarf-directory-asm -resource-dir /usr/local/corex-3.0.0/lib/clang/13.0.1 -internal-isystem /usr/local/corex-3.0.0/lib/clang/13.0.1/include/cuda_wrappers -internal-isystem /usr/local/corex/include -include __clang_cuda_runtime_wrapper.h -isystem /mnt/nvme0/home/lianghz/share/bin/../include/AdaptiveCpp -isystem /mnt/nvme0/home/lianghz/share/bin/../include/AdaptiveCpp/hipSYCL/std/hiplike -D __OPENSYCL__ -D __HIPSYCL__ -D __ADAPTIVECPP__ -D __ACPP__ -D __HIPSYCL_ENABLE_CUDA_TARGET__ -D __HIPSYCL_CLANG__ -U __FLOAT128__ -U __SIZEOF_FLOAT128__ -D __HIPSYCL_ENABLE_OMPHOST_TARGET__ -I /mnt/nvme0/home/lianghz/share/include -D _ENABLE_EXTENDED_ALIGNED_STORAGE -I/mnt/nvme0/home/lianghz/share/mpich/include -I/usr/local/corex/include -I. -internal-isystem /usr/lib/gcc/x86_64-linux-gnu/12/../../../../include/c++/12 -internal-isystem /usr/lib/gcc/x86_64-linux-gnu/12/../../../../include/x86_64-linux-gnu/c++/12 -internal-isystem /usr/lib/gcc/x86_64-linux-gnu/12/../../../../include/c++/12/backward -internal-isystem /usr/lib/gcc/x86_64-linux-gnu/12/../../../../include/c++/12 -internal-isystem /usr/lib/gcc/x86_64-linux-gnu/12/../../../../include/x86_64-linux-gnu/c++/12 -internal-isystem /usr/lib/gcc/x86_64-linux-gnu/12/../../../../include/c++/12/backward -internal-isystem /usr/local/corex-3.0.0/lib/clang/13.0.1/include -internal-isystem /usr/local/include -internal-isystem /usr/lib/gcc/x86_64-linux-gnu/12/../../../../x86_64-linux-gnu/include -internal-externc-isystem /usr/include/x86_64-linux-gnu -internal-externc-isystem /include -internal-externc-isystem /usr/include -internal-isystem /usr/local/corex-3.0.0/lib/clang/13.0.1/include -internal-isystem /usr/local/include -internal-isystem /usr/lib/gcc/x86_64-linux-gnu/12/../../../../x86_64-linux-gnu/include -internal-externc-isystem /usr/include/x86_64-linux-gnu -internal-externc-isystem /include -internal-externc-isystem /usr/include -std=c++17 -fdeprecated-macro -fno-autolink -fdebug-compilation-dir=/mnt/nvme0/home/lianghz/workspace/gpu-sycl -ferror-limit 19 -fgnuc-version=4.2.1 -fcxx-exceptions -fexceptions -fcolor-diagnostics -load /mnt/nvme0/home/lianghz/share/bin/../lib/libacpp-clang.so -fpass-plugin=/mnt/nvme0/home/lianghz/share/bin/../lib/libacpp-clang.so -cuid=69c187166d0822ae -D__GCC_HAVE_DWARF2_CFI_ASM=1 -o /tmp/test-186375.s -x cuda test.cpp
 #0 0x0000000001e46f5f PrintStackTraceSignalHandler(void*) Signals.cpp:0:0
 #1 0x0000000001e44ad9 SignalHandler(int) Signals.cpp:0:0
 #2 0x00007f37bf4e3520 (/lib/x86_64-linux-gnu/libc.so.6+0x42520)
 #3 0x00007f37bf537a7c __pthread_kill_implementation ./nptl/./nptl/pthread_kill.c:44:76
 #4 0x00007f37bf537a7c __pthread_kill_internal ./nptl/./nptl/pthread_kill.c:78:10
 #5 0x00007f37bf537a7c pthread_kill ./nptl/./nptl/pthread_kill.c:89:10
 #6 0x00007f37bf4e3476 gsignal ./signal/../sysdeps/posix/raise.c:27:6
 #7 0x00007f37bf4c97f3 abort ./stdlib/./stdlib/abort.c:81:7
 #8 0x00007f37b9829723 (/lib/x86_64-linux-gnu/libLLVM-14.so.1+0xd7a723)
 #9 0x00007f37b9829556 (/lib/x86_64-linux-gnu/libLLVM-14.so.1+0xd7a556)
#10 0x00007f37b9814990 (/lib/x86_64-linux-gnu/libLLVM-14.so.1+0xd65990)
#11 0x00007f37b980626b llvm::cl::Option::addArgument() (/lib/x86_64-linux-gnu/libLLVM-14.so.1+0xd5726b)
#12 0x00007f37b9724634 (/lib/x86_64-linux-gnu/libLLVM-14.so.1+0xc75634)
#13 0x00007f37bfa4247e call_init ./elf/./elf/dl-init.c:69:21
#14 0x00007f37bfa42568 _dl_init ./elf/./elf/dl-init.c:116:14
#15 0x00007f37bf615c85 _dl_catch_exception ./elf/./elf/dl-error-skeleton.c:184:18
#16 0x00007f37bfa49ff6 dl_open_worker ./elf/./elf/dl-open.c:812:6
#17 0x00007f37bfa49ff6 dl_open_worker ./elf/./elf/dl-open.c:771:1
#18 0x00007f37bf615c28 _dl_catch_exception ./elf/./elf/dl-error-skeleton.c:209:18
#19 0x00007f37bfa4a34e _dl_open ./elf/./elf/dl-open.c:883:17
#20 0x00007f37bf5316bc dlopen_doit ./dlfcn/./dlfcn/dlopen.c:56:13
#21 0x00007f37bf615c28 _dl_catch_exception ./elf/./elf/dl-error-skeleton.c:209:18
#22 0x00007f37bf615cf3 _dl_catch_error ./elf/./elf/dl-error-skeleton.c:228:12
#23 0x00007f37bf5311ae _dlerror_run ./dlfcn/./dlfcn/dlerror.c:145:17
#24 0x00007f37bf531748 dlopen_implementation ./dlfcn/./dlfcn/dlopen.c:71:51
#25 0x00007f37bf531748 dlopen ./dlfcn/./dlfcn/dlopen.c:81:12
#26 0x0000000001e2dc0a llvm::sys::DynamicLibrary::getPermanentLibrary(char const*, std::string*) (/usr/local/corex-3.0.0/bin/clang-13+0x1e2dc0a)
#27 0x00000000027cc70c clang::ExecuteCompilerInvocation(clang::CompilerInstance*) (/usr/local/corex-3.0.0/bin/clang-13+0x27cc70c)
#28 0x00000000009e94a4 cc1_main(llvm::ArrayRef<char const*>, char const*, void*) (/usr/local/corex-3.0.0/bin/clang-13+0x9e94a4)
#29 0x00000000009e3b95 ExecuteCC1Tool(llvm::SmallVectorImpl<char const*>&) driver.cpp:0:0
#30 0x000000000095b235 main (/usr/local/corex-3.0.0/bin/clang-13+0x95b235)
#31 0x00007f37bf4cad90 __libc_start_call_main ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
#32 0x00007f37bf4cae40 call_init ./csu/../csu/libc-start.c:128:20
#33 0x00007f37bf4cae40 __libc_start_main ./csu/../csu/libc-start.c:379:5
#34 0x00000000009e3475 _start (/usr/local/corex-3.0.0/bin/clang-13+0x9e3475)
clang-13: error: unable to execute command: Aborted (core dumped)
clang-13: error: clang frontend command failed due to signal (use -v to see invocation)
clang version 13.0.1
Target: x86_64-unknown-linux-gnu
Thread model: posix
InstalledDir: /usr/local/corex/bin
clang-13: error: unable to execute command: Aborted (core dumped)
clang-13: note: diagnostic msg: Error generating preprocessed source(s).
```

可以调用sycl API，但无法提交内核：缺少kernel_launcher

----------------------2023.10.30--------------------

A100上可以正常运行sycl测试程序。

cmake配置：
```
cmake -DCMAKE_INSTALL_PREFIX=~/share -DCUDA_TOOLKIT_ROOT_DIR=${CUDA_ROOT} -DWITH_CUDA_BACKEND=ON -DWITH_OPENCL_BACKEND=OFF ..
```

天数上acpp编译测试程序的错误可能是因为编译使用的工具链不同：
```
https://www.leadroyal.cn/p/1014/
```

使用clang编译acpp需要使用openmp，但天数编译器可能缺少默认的openmp。

--------------------2023.10.31----------------------

在编译libacpp-clang.so时需要链接使用clang编译的LLVM库？

```
/usr/bin/ld: /mnt/nvme0/home/lianghz/share/lib/libacpp-clang.so: undefined reference to `clang::NamedDecl::getQualifiedNameAsString[abi:cxx11]() const'
/usr/bin/ld: /mnt/nvme0/home/lianghz/share/lib/libacpp-clang.so: undefined reference to `clang::DeclarationName::getAsString[abi:cxx11]() const'
```

C++ ABI问题

## 配置1

-----------------2023.11.1------------------------

ACPP编译参数(build-cxx20)：

```
cmake -DCMAKE_INSTALL_PREFIX=~/share -DCMAKE_CXX_FLAGS="-D_GLIBCXX_USE_CXX11_ABI=0 -std=c++20" -DCMAKE_CXX_COMPILER=g++ -DCUDA_TOOLKIT_ROOT_DIR=/usr/local/corex -DWITH_CUDA_BACKEND=ON -DWITH_OPENCL_BACKEND=OFF ..
```

在编译libacpp-clang.so时链接libclang.so libclang-cpp.so libLLVM.so。

在编译测试程序时需要指定CXX ABI：

```
clang++ -fsycl test.cpp -L${HOME}/share/lib -L${HOME}/share/lib/hipSYCL -I${HOME}/share/include/AdaptiveCpp -lacpp-clang -lacpp-rt -lacpp-common -lrt-backend-cuda -std=c++20 -lcudart -lixthunk -lixlogger -D_GLIBCXX_USE_CXX11_ABI=0
```

测试程序运行报错：

```
./a.out: /mnt/nvme0/home/lianghz/share/cuda-10.2/lib64/libcudart.so.10.2: version `CUDART' not found (required by /mnt/nvme0/home/lianghz/share/lib/hipSYCL/librt-backend-cuda.so)
```

-------------------2023.11.2-----------------------

不要在LD_LIBRARY_PATH中提供cuda库的路径，或者使用corex cuda库的路径覆盖cuda库的路径。

缺少backend_launcher：

```
launcher count: 0
[AdaptiveCpp Warning] dag_direct_scheduler: Detected a requirement that is neither of discard access mode (SYCL 1.2.1) nor no_init property (SYCL 2020) that accesses uninitialized data. Consider changing to discard/no_init. Optimizing potential data transfers away.
[AdaptiveCpp Error] from /mnt/nvme0/home/lianghz/workspace/gpu-sycl/AdaptiveCpp/include/hipSYCL/runtime/kernel_launcher.hpp:139 @ find_launcher(): No kernel launcher is present for requested backend
[AdaptiveCpp Error] from /mnt/nvme0/home/lianghz/workspace/gpu-sycl/AdaptiveCpp/src/runtime/cuda/cuda_queue.cpp:354 @ submit_kernel(): Could not obtain backend kernel launcher
============== hipSYCL error report ============== 
hipSYCL has caught the following undhandled asynchronous errors: 

   0. from /mnt/nvme0/home/lianghz/workspace/gpu-sycl/AdaptiveCpp/include/hipSYCL/runtime/kernel_launcher.hpp:139 @ find_launcher(): No kernel launcher is present for requested backend
   1. from /mnt/nvme0/home/lianghz/workspace/gpu-sycl/AdaptiveCpp/src/runtime/cuda/cuda_queue.cpp:354 @ submit_kernel(): Could not obtain backend kernel launcher
The application will now be terminated.
terminate called without an active exception
Aborted (core dumped)
```

## 配置2（build--default）

cmake参数：
```
cmake -DCMAKE_INSTALL_PREFIX=~/share -DCMAKE_CXX_FLAGS="-D_GLIBCXX_USE_CXX11_ABI=0 -std=c++20" -DCMAKE_CXX_COMPILER=g++ -DCUDA_TOOLKIT_ROOT_DIR=/usr/local/corex -DWITH_CUDA_BACKEND=ON -DWITH_OPENCL_BACKEND=OFF .. 
```

在编译libacpp-clang.so时不链接libclang.so libclang-cpp.so libLLVM.so。

编译参数：
```
clang++ -fsycl -L${HOME}/share/lib -L${HOME}/share/lib/hipSYCL -I${HOME}/share/include/AdaptiveCpp -D__OPENSYCL__ -D__HIPSYCL__ -D__ADAPTIVECPP__ -D__ACPP__ -D__HIPSYCL_ENABLE_CUDA_TARGET__ -std=c++20 -x cuda -fplugin=${HOME}/share/lib/libacpp-clang.so --cuda-gpu-arch=ivcore10 -fpass-plugin=${HOME}/share/lib/libacpp-clang.so -lacpp-rt -lacpp-common -lcudart -lboost_fiber -lboost_context test.cpp -o test -D_GLIBCXX_USE_CXX11_ABI=0
```

\_\_noinline\_\_问题。

可以编译。

测试程序可以正常运行。

现在可以使用acpp进行编译。

## SYCL-bench

### 2DConvolution

```
********** Results for Polybench_2DConvolution**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.001059 [s]
run-time-stddev: 0.000050 [s]
run-time-median: 0.001049 [s]
run-time-min: 0.001001 [s]
run-time-samples: "0.001001 0.001032 0.001049 0.001078 0.001133"
run-time-throughput: N/A
Verification: PASS
```

```
real	0m0.758s
user	0m0.582s
sys	0m0.177s
```

### 2mm

```
********** Results for Polybench_2mm**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 1.206332 [s]
run-time-stddev: 0.000645 [s]
run-time-median: 1.206494 [s]
run-time-min: 1.205315 [s]
run-time-samples: "1.205315 1.206131 1.206494 1.206802 1.206918"
run-time-throughput: N/A
Verification: PASS

real	41m3.093s
user	40m55.809s
sys	0m0.980s

```

### 3DConvolution

```
********** Results for Polybench_3DConvolution**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
sycl-implementation: hipSYCL
launcher count: 2
[AdaptiveCpp Error] from /mnt/nvme0/home/lianghz/workspace/gpu-sycl/AdaptiveCpp/src/runtime/cuda/cuda_allocator.cpp:48 @ allocate(): cuda_allocator: cudaMalloc() failed (error code = CUDA:2)
[AdaptiveCpp Error] from /mnt/nvme0/home/lianghz/workspace/gpu-sycl/AdaptiveCpp/src/runtime/dag_direct_scheduler.cpp:113 @ ensure_allocation_exists(): dag_direct_scheduler: Lazy memory allocation has failed.
[AdaptiveCpp Error] from /mnt/nvme0/home/lianghz/workspace/gpu-sycl/AdaptiveCpp/src/runtime/dag_direct_scheduler.cpp:113 @ ensure_allocation_exists(): dag_direct_scheduler: Lazy memory allocation has failed.
============== hipSYCL error report ============== 
hipSYCL has caught the following undhandled asynchronous errors: 

   0. from /mnt/nvme0/home/lianghz/workspace/gpu-sycl/AdaptiveCpp/src/runtime/cuda/cuda_allocator.cpp:48 @ allocate(): cuda_allocator: cudaMalloc() failed (error code = CUDA:2)
   1. from /mnt/nvme0/home/lianghz/workspace/gpu-sycl/AdaptiveCpp/src/runtime/dag_direct_scheduler.cpp:113 @ ensure_allocation_exists(): dag_direct_scheduler: Lazy memory allocation has failed.
   2. from /mnt/nvme0/home/lianghz/workspace/gpu-sycl/AdaptiveCpp/src/runtime/dag_direct_scheduler.cpp:113 @ ensure_allocation_exists(): dag_direct_scheduler: Lazy memory allocation has failed.
The application will now be terminated.
terminate called without an active exception
```

### 3mm

在天数上运行若干轮后出现类似休眠的现象：显卡占用率0％？

```
********** Results for Polybench_3mm**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 1.807627 [s]
run-time-stddev: 0.000125 [s]
run-time-median: 1.807622 [s]
run-time-min: 1.807443 [s]
run-time-samples: "1.807443 1.807583 1.807622 1.807730 1.807756"
run-time-throughput: N/A
Verification: PASS

real	57m31.938s
user	57m21.842s
sys	0m0.948s
```

这数据认真的？

### DRAM

```
********** Results for MicroBench_DRAM_fp32_1**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
sycl-implementation: hipSYCL
launcher count: 2
launcher count: 2
[AdaptiveCpp Error] from /mnt/nvme0/home/lianghz/workspace/gpu-sycl/AdaptiveCpp/src/runtime/cuda/cuda_allocator.cpp:48 @ allocate(): cuda_allocator: cudaMalloc() failed (error code = CUDA:2)
[AdaptiveCpp Error] from /mnt/nvme0/home/lianghz/workspace/gpu-sycl/AdaptiveCpp/src/runtime/dag_direct_scheduler.cpp:113 @ ensure_allocation_exists(): dag_direct_scheduler: Lazy memory allocation has failed.
[AdaptiveCpp Error] from /mnt/nvme0/home/lianghz/workspace/gpu-sycl/AdaptiveCpp/src/runtime/dag_direct_scheduler.cpp:113 @ ensure_allocation_exists(): dag_direct_scheduler: Lazy memory allocation has failed.
============== hipSYCL error report ============== 
hipSYCL has caught the following undhandled asynchronous errors: 

   0. from /mnt/nvme0/home/lianghz/workspace/gpu-sycl/AdaptiveCpp/src/runtime/cuda/cuda_allocator.cpp:48 @ allocate(): cuda_allocator: cudaMalloc() failed (error code = CUDA:2)
   1. from /mnt/nvme0/home/lianghz/workspace/gpu-sycl/AdaptiveCpp/src/runtime/dag_direct_scheduler.cpp:113 @ ensure_allocation_exists(): dag_direct_scheduler: Lazy memory allocation has failed.
   2. from /mnt/nvme0/home/lianghz/workspace/gpu-sycl/AdaptiveCpp/src/runtime/dag_direct_scheduler.cpp:113 @ ensure_allocation_exists(): dag_direct_scheduler: Lazy memory allocation has failed.
The application will now be terminated.
terminate called without an active exception
```

### arith

```
********** Results for MicroBench_Arith_int32_512**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000072 [s]
run-time-stddev: 0.000004 [s]
run-time-median: 0.000071 [s]
run-time-min: 0.000068 [s]
run-time-samples: "0.000068 0.000071 0.000071 0.000072 0.000079"
run-time-throughput: 85.589550 [GOP/s]
Verification: PASS
********** Results for MicroBench_Arith_fp32_512**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000053 [s]
run-time-stddev: 0.000001 [s]
run-time-median: 0.000054 [s]
run-time-min: 0.000051 [s]
run-time-samples: "0.000051 0.000052 0.000054 0.000054 0.000054"
run-time-throughput: 114.217836 [SP GFLOP/s]
Verification: PASS
********** Results for MicroBench_Arith_fp64_512**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
sycl-implementation: hipSYCL
launcher count: 2
launcher count: 2
launcher count: 2
throughput-metric: 0.005859 [DP GFLOP]
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 0.000050 [s]
run-time-stddev: 0.000000 [s]
run-time-median: 0.000050 [s]
run-time-min: 0.000050 [s]
run-time-samples: "0.000050"
run-time-throughput: 117.942331 [DP GFLOP/s]
Verification: FAIL

real	0m0.012s
user	0m0.005s
sys	0m0.009s
```

### atax

```
********** Results for Polybench_Atax**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.004150 [s]
run-time-stddev: 0.000031 [s]
run-time-median: 0.004139 [s]
run-time-min: 0.004125 [s]
run-time-samples: "0.004125 0.004136 0.004139 0.004146 0.004204"
run-time-throughput: N/A
Verification: PASS

real	0m0.287s
user	0m0.200s
sys	0m0.080s
```

### bicg

```
********** Results for Polybench_Bicg**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.004416 [s]
run-time-stddev: 0.000495 [s]
run-time-median: 0.004201 [s]
run-time-min: 0.004175 [s]
run-time-samples: "0.004175 0.004196 0.004201 0.004208 0.005301"
run-time-throughput: N/A
Verification: PASS

real	0m0.326s
user	0m0.218s
sys	0m0.105s
```

### blocked-transform

```
********** Results for Runtime_BlockedTransform_iter_64_blocksize_256**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000776 [s]
run-time-stddev: 0.000709 [s]
run-time-median: 0.000456 [s]
run-time-min: 0.000449 [s]
run-time-samples: "0.000449 0.000454 0.000456 0.000476 0.002043"
run-time-throughput: N/A
Verification: PASS
********** Results for Runtime_BlockedTransform_iter_128_blocksize_256**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000461 [s]
run-time-stddev: 0.000014 [s]
run-time-median: 0.000466 [s]
run-time-min: 0.000445 [s]
run-time-samples: "0.000445 0.000447 0.000466 0.000472 0.000477"
run-time-throughput: N/A
Verification: PASS
********** Results for Runtime_BlockedTransform_iter_256_blocksize_256**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000491 [s]
run-time-stddev: 0.000006 [s]
run-time-median: 0.000491 [s]
run-time-min: 0.000483 [s]
run-time-samples: "0.000483 0.000488 0.000491 0.000491 0.000500"
run-time-throughput: N/A
Verification: PASS
********** Results for Runtime_BlockedTransform_iter_512_blocksize_256**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000575 [s]
run-time-stddev: 0.000008 [s]
run-time-median: 0.000574 [s]
run-time-min: 0.000567 [s]
run-time-samples: "0.000567 0.000569 0.000574 0.000579 0.000588"
run-time-throughput: N/A
Verification: PASS
********** Results for Runtime_BlockedTransform_iter_64_blocksize_512**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000267 [s]
run-time-stddev: 0.000014 [s]
run-time-median: 0.000259 [s]
run-time-min: 0.000256 [s]
run-time-samples: "0.000256 0.000258 0.000259 0.000276 0.000288"
run-time-throughput: N/A
Verification: PASS
********** Results for Runtime_BlockedTransform_iter_128_blocksize_512**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000280 [s]
run-time-stddev: 0.000007 [s]
run-time-median: 0.000280 [s]
run-time-min: 0.000269 [s]
run-time-samples: "0.000269 0.000279 0.000280 0.000286 0.000287"
run-time-throughput: N/A
Verification: PASS
********** Results for Runtime_BlockedTransform_iter_256_blocksize_512**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000302 [s]
run-time-stddev: 0.000008 [s]
run-time-median: 0.000304 [s]
run-time-min: 0.000289 [s]
run-time-samples: "0.000289 0.000301 0.000304 0.000305 0.000309"
run-time-throughput: N/A
Verification: PASS
********** Results for Runtime_BlockedTransform_iter_512_blocksize_512**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
sycl-implementation: hipSYCL
launcher count: 2
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
run-time-mean: 0.000342 [s]
run-time-stddev: 0.000006 [s]
run-time-median: 0.000346 [s]
run-time-min: 0.000335 [s]
run-time-samples: "0.000335 0.000337 0.000346 0.000346 0.000348"
run-time-throughput: N/A
Verification: PASS
********** Results for Runtime_BlockedTransform_iter_64_blocksize_1024**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000189 [s]
run-time-stddev: 0.000019 [s]
run-time-median: 0.000181 [s]
run-time-min: 0.000177 [s]
run-time-samples: "0.000177 0.000178 0.000181 0.000185 0.000222"
run-time-throughput: N/A
Verification: PASS
********** Results for Runtime_BlockedTransform_iter_128_blocksize_1024**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000206 [s]
run-time-stddev: 0.000015 [s]
run-time-median: 0.000211 [s]
run-time-min: 0.000182 [s]
run-time-samples: "0.000182 0.000203 0.000211 0.000215 0.000221"
run-time-throughput: N/A
Verification: PASS
********** Results for Runtime_BlockedTransform_iter_256_blocksize_1024**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000224 [s]
run-time-stddev: 0.000008 [s]
run-time-median: 0.000227 [s]
run-time-min: 0.000214 [s]
run-time-samples: "0.000214 0.000217 0.000227 0.000228 0.000235"
run-time-throughput: N/A
Verification: PASS
********** Results for Runtime_BlockedTransform_iter_512_blocksize_1024**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000242 [s]
run-time-stddev: 0.000014 [s]
run-time-median: 0.000245 [s]
run-time-min: 0.000224 [s]
run-time-samples: "0.000224 0.000232 0.000245 0.000248 0.000259"
run-time-throughput: N/A
Verification: PASS
********** Results for Runtime_BlockedTransform_iter_64_blocksize_2048**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000167 [s]
run-time-stddev: 0.000029 [s]
run-time-median: 0.000153 [s]
run-time-min: 0.000151 [s]
run-time-samples: "0.000151 0.000152 0.000153 0.000159 0.000218"
run-time-throughput: N/A
Verification: PASS
********** Results for Runtime_BlockedTransform_iter_128_blocksize_2048**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000161 [s]
run-time-stddev: 0.000009 [s]
run-time-median: 0.000160 [s]
run-time-min: 0.000152 [s]
run-time-samples: "0.000152 0.000155 0.000160 0.000164 0.000174"
run-time-throughput: N/A
Verification: PASS
********** Results for Runtime_BlockedTransform_iter_256_blocksize_2048**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000182 [s]
run-time-stddev: 0.000009 [s]
run-time-median: 0.000181 [s]
run-time-min: 0.000174 [s]
run-time-samples: "0.000174 0.000178 0.000181 0.000182 0.000196"
run-time-throughput: N/A
Verification: PASS
********** Results for Runtime_BlockedTransform_iter_512_blocksize_2048**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000202 [s]
run-time-stddev: 0.000012 [s]
run-time-median: 0.000196 [s]
run-time-min: 0.000191 [s]
run-time-samples: "0.000191 0.000195 0.000196 0.000204 0.000222"
run-time-throughput: N/A
Verification: PASS

real	0m0.318s
user	0m0.318s
sys	0m0.004s
```

### correlation

```
********** Results for Polybench_Correlation**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 3.063683 [s]
run-time-stddev: 0.018995 [s]
run-time-median: 3.060577 [s]
run-time-min: 3.035200 [s]
run-time-samples: "3.035200 3.060326 3.060577 3.080665 3.081646"
run-time-throughput: N/A
Verification: PASS

real	4m27.944s
user	4m12.424s
sys	0m0.320s
```

### covariance

```
********** Results for Polybench_Covariance**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 3.040296 [s]
run-time-stddev: 0.020544 [s]
run-time-median: 3.046844 [s]
run-time-min: 3.018157 [s]
run-time-samples: "3.018157 3.019321 3.046844 3.053738 3.063418"
run-time-throughput: N/A
Verification: PASS

real	4m12.413s
user	3m56.799s
sys	0m0.332s
```

### fdtd2d

```
0 0: 34347012028044448.000000 24019198012642644.000000 30.069029
throughput-metric: N/A
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 0.209884 [s]
run-time-stddev: 0.000000 [s]
run-time-median: 0.209884 [s]
run-time-min: 0.209884 [s]
run-time-samples: "0.209884"
run-time-throughput: N/A
Verification: FAIL

real	0m9.736s
user	0m9.388s
sys	0m0.400s
```

### gemm

```
********** Results for Polybench_Gemm**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.634163 [s]
run-time-stddev: 0.000423 [s]
run-time-median: 0.634059 [s]
run-time-min: 0.633828 [s]
run-time-samples: "0.633828 0.633856 0.634059 0.634201 0.634868"
run-time-throughput: N/A
Verification: PASS

real	17m50.668s
user	17m47.125s
sys	0m0.360s
```

### gesummv

```
********** Results for Polybench_Gesummv**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.006256 [s]
run-time-stddev: 0.000035 [s]
run-time-median: 0.006239 [s]
run-time-min: 0.006237 [s]
run-time-samples: "0.006237 0.006237 0.006239 0.006249 0.006317"
run-time-throughput: N/A
Verification: PASS

real	0m0.390s
user	0m0.230s
sys	0m0.141s
```

### gramschmidt

```
********** Results for Polybench_Gramschmidt**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
sycl-implementation: hipSYCL
...
throughput-metric: N/A
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 11.385905 [s]
run-time-stddev: 0.000000 [s]
run-time-median: 11.385905 [s]
run-time-min: 11.385905 [s]
run-time-samples: "11.385905"
run-time-throughput: N/A
Verification: FAIL

real	6m7.735s
user	6m7.471s
sys	0m0.780s
```

### host_device_bandwidth

```
********** Results for MicroBench_HostDeviceBandwidth_1D_H2D_Contiguous**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
sycl-implementation: hipSYCL
launcher count: 2
[AdaptiveCpp Warning] kernel_operation: Could not find embedded pointer in kernel blob for this requirement; do you have unnecessary accessors that are unused in your kernel?
launcher count: 2
[AdaptiveCpp Warning] kernel_operation: Could not find embedded pointer in kernel blob for this requirement; do you have unnecessary accessors that are unused in your kernel?
launcher count: 2
[AdaptiveCpp Error] from /mnt/nvme0/home/lianghz/workspace/gpu-sycl/AdaptiveCpp/src/runtime/cuda/cuda_allocator.cpp:48 @ allocate(): cuda_allocator: cudaMalloc() failed (error code = CUDA:2)
[AdaptiveCpp Error] from /mnt/nvme0/home/lianghz/workspace/gpu-sycl/AdaptiveCpp/src/runtime/dag_direct_scheduler.cpp:113 @ ensure_allocation_exists(): dag_direct_scheduler: Lazy memory allocation has failed.
[AdaptiveCpp Error] from /mnt/nvme0/home/lianghz/workspace/gpu-sycl/AdaptiveCpp/src/runtime/dag_direct_scheduler.cpp:113 @ ensure_allocation_exists(): dag_direct_scheduler: Lazy memory allocation has failed.
============== hipSYCL error report ============== 
hipSYCL has caught the following undhandled asynchronous errors: 

   0. from /mnt/nvme0/home/lianghz/workspace/gpu-sycl/AdaptiveCpp/src/runtime/cuda/cuda_allocator.cpp:48 @ allocate(): cuda_allocator: cudaMalloc() failed (error code = CUDA:2)
   1. from /mnt/nvme0/home/lianghz/workspace/gpu-sycl/AdaptiveCpp/src/runtime/dag_direct_scheduler.cpp:113 @ ensure_allocation_exists(): dag_direct_scheduler: Lazy memory allocation has failed.
   2. from /mnt/nvme0/home/lianghz/workspace/gpu-sycl/AdaptiveCpp/src/runtime/dag_direct_scheduler.cpp:113 @ ensure_allocation_exists(): dag_direct_scheduler: Lazy memory allocation has failed.
The application will now be terminated.
terminate called without an active exception
```

### kmeans

```
********** Results for Kmeans_fp32**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000052 [s]
run-time-stddev: 0.000005 [s]
run-time-median: 0.000049 [s]
run-time-min: 0.000048 [s]
run-time-samples: "0.000048 0.000048 0.000049 0.000053 0.000060"
run-time-throughput: N/A
Verification: PASS
********** Results for Kmeans_fp64**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
sycl-implementation: hipSYCL
launcher count: 2
...
launcher count: 2
Fail at = 1Expected = 1Actual =0
throughput-metric: N/A
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 0.000055 [s]
run-time-stddev: 0.000000 [s]
run-time-median: 0.000055 [s]
run-time-min: 0.000055 [s]
run-time-samples: "0.000055"
run-time-throughput: N/A
Verification: FAIL

real	0m0.012s
user	0m0.004s
sys	0m0.010s
```

### lin_reg_coeff

```
********** Results for LinearRegressionCoeff_fp32**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000533 [s]
run-time-stddev: 0.000059 [s]
run-time-median: 0.000501 [s]
run-time-min: 0.000486 [s]
run-time-samples: "0.000486 0.000489 0.000501 0.000568 0.000620"
run-time-throughput: N/A
Verification: PASS
********** Results for LinearRegressionCoeff_fp64**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000483 [s]
run-time-stddev: 0.000013 [s]
run-time-median: 0.000483 [s]
run-time-min: 0.000470 [s]
run-time-samples: "0.000470 0.000470 0.000483 0.000493 0.000498"
run-time-throughput: N/A
Verification: PASS

real	0m0.017s
user	0m0.013s
sys	0m0.007s
```

### lin_reg_error

```
********** Results for LinearRegression_fp32**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000580 [s]
run-time-stddev: 0.000003 [s]
run-time-median: 0.000580 [s]
run-time-min: 0.000575 [s]
run-time-samples: "0.000575 0.000577 0.000580 0.000583 0.000583"
run-time-throughput: N/A
Verification: PASS
********** Results for LinearRegression_fp64**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000174 [s]
run-time-stddev: 0.000000 [s]
run-time-median: 0.000174 [s]
run-time-min: 0.000174 [s]
run-time-samples: "0.000174"
run-time-throughput: N/A
Verification: FAIL

real	0m0.064s
user	0m0.058s
sys	0m0.008s
```

### local_mem

```
********** Results for MicroBench_LocalMem_int32_4096**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
sycl-implementation: hipSYCL
launcher count: 2
...
launcher count: 2
throughput-metric: 0.000000 [GiB]
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 0.000269 [s]
run-time-stddev: 0.000007 [s]
run-time-median: 0.000269 [s]
run-time-min: 0.000262 [s]
run-time-samples: "0.000262 0.000264 0.000269 0.000272 0.000280"
run-time-throughput: N/A
Verification: PASS
********** Results for MicroBench_LocalMem_fp32_4096**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
sycl-implementation: hipSYCL
launcher count: 2
...
launcher count: 2
throughput-metric: 0.000000 [GiB]
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 0.000267 [s]
run-time-stddev: 0.000005 [s]
run-time-median: 0.000265 [s]
run-time-min: 0.000260 [s]
run-time-samples: "0.000260 0.000263 0.000265 0.000272 0.000272"
run-time-throughput: N/A
Verification: PASS
********** Results for MicroBench_LocalMem_fp64_4096**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
sycl-implementation: hipSYCL
launcher count: 2
...
launcher count: 2
throughput-metric: 0.000000 [GiB]
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 0.000390 [s]
run-time-stddev: 0.000001 [s]
run-time-median: 0.000390 [s]
run-time-min: 0.000388 [s]
run-time-samples: "0.000388 0.000389 0.000390 0.000391 0.000391"
run-time-throughput: N/A
Verification: PASS

real	0m0.018s
user	0m0.015s
sys	0m0.005s
```

### matmulchain

```
********** Results for MatmulChain**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.375442 [s]
run-time-stddev: 0.000553 [s]
run-time-median: 0.375812 [s]
run-time-min: 0.374697 [s]
run-time-samples: "0.374697 0.375000 0.375812 0.375840 0.375862"
run-time-throughput: N/A
Verification: PASS

real	0m2.286s
user	0m0.219s
sys	0m0.207s
```

### median

```
********** Results for MedianFilter**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.005185 [s]
run-time-stddev: 0.000002 [s]
run-time-median: 0.005184 [s]
run-time-min: 0.005182 [s]
run-time-samples: "0.005182 0.005183 0.005184 0.005186 0.005188"
run-time-throughput: N/A
Verification: PASS

real	0m3.956s
user	0m2.822s
sys	0m1.051s
```

### mol_dyn

```
********** Results for MolecularDynamics**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000055 [s]
run-time-stddev: 0.000012 [s]
run-time-median: 0.000048 [s]
run-time-min: 0.000045 [s]
run-time-samples: "0.000045 0.000047 0.000048 0.000064 0.000072"
run-time-throughput: N/A
Verification: PASS

real	0m0.016s
user	0m0.017s
sys	0m0.000s
```

### mvt

```
********** Results for Polybench_Mvt**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.006123 [s]
run-time-stddev: 0.000008 [s]
run-time-median: 0.006128 [s]
run-time-min: 0.006114 [s]
run-time-samples: "0.006114 0.006115 0.006128 0.006129 0.006130"
run-time-throughput: N/A
Verification: PASS

real	0m0.390s
user	0m0.290s
sys	0m0.093s
```

### nbody

```
********** Results for NBody_Hierarchical_fp32**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.003080 [s]
run-time-stddev: 0.000006 [s]
run-time-median: 0.003078 [s]
run-time-min: 0.003075 [s]
run-time-samples: "0.003075 0.003075 0.003078 0.003080 0.003089"
run-time-throughput: N/A
Verification: PASS
********** Results for NBody_Hierarchical_fp64**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000469 [s]
run-time-stddev: 0.000000 [s]
run-time-median: 0.000469 [s]
run-time-min: 0.000469 [s]
run-time-samples: "0.000469"
run-time-throughput: N/A
Verification: FAIL
********** Results for NBody_NDRange_fp32**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.003074 [s]
run-time-stddev: 0.000002 [s]
run-time-median: 0.003074 [s]
run-time-min: 0.003072 [s]
run-time-samples: "0.003072 0.003072 0.003074 0.003076 0.003077"
run-time-throughput: N/A
Verification: PASS
********** Results for NBody_NDRange_fp64**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000399 [s]
run-time-stddev: 0.000000 [s]
run-time-median: 0.000399 [s]
run-time-min: 0.000399 [s]
run-time-samples: "0.000399"
run-time-throughput: N/A
Verification: FAIL

real	0m0.453s
user	0m0.416s
sys	0m0.032s
```

### pattern_L2

### reduction

```
********** Results for Pattern_Reduction_NDRange_int32**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000078 [s]
run-time-stddev: 0.000016 [s]
run-time-median: 0.000072 [s]
run-time-min: 0.000069 [s]
run-time-samples: "0.000069 0.000071 0.000072 0.000074 0.000106"
run-time-throughput: N/A
Verification: PASS
********** Results for Pattern_Reduction_NDRange_int64**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000071 [s]
run-time-stddev: 0.000002 [s]
run-time-median: 0.000071 [s]
run-time-min: 0.000069 [s]
run-time-samples: "0.000069 0.000070 0.000071 0.000073 0.000074"
run-time-throughput: N/A
Verification: PASS
********** Results for Pattern_Reduction_NDRange_fp32**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000069 [s]
run-time-stddev: 0.000003 [s]
run-time-median: 0.000068 [s]
run-time-min: 0.000066 [s]
run-time-samples: "0.000066 0.000068 0.000068 0.000069 0.000074"
run-time-throughput: N/A
Verification: PASS
********** Results for Pattern_Reduction_NDRange_fp64**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000068 [s]
run-time-stddev: 0.000000 [s]
run-time-median: 0.000068 [s]
run-time-min: 0.000068 [s]
run-time-samples: "0.000068"
run-time-throughput: N/A
Verification: FAIL
********** Results for Pattern_Reduction_Hierarchical_int32**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000069 [s]
run-time-stddev: 0.000002 [s]
run-time-median: 0.000070 [s]
run-time-min: 0.000068 [s]
run-time-samples: "0.000068 0.000068 0.000070 0.000070 0.000072"
run-time-throughput: N/A
Verification: PASS
********** Results for Pattern_Reduction_Hierarchical_int64**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000071 [s]
run-time-stddev: 0.000003 [s]
run-time-median: 0.000070 [s]
run-time-min: 0.000067 [s]
run-time-samples: "0.000067 0.000068 0.000070 0.000073 0.000074"
run-time-throughput: N/A
Verification: PASS
********** Results for Pattern_Reduction_Hierarchical_fp32**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000070 [s]
run-time-stddev: 0.000003 [s]
run-time-median: 0.000069 [s]
run-time-min: 0.000067 [s]
run-time-samples: "0.000067 0.000068 0.000069 0.000072 0.000075"
run-time-throughput: N/A
Verification: PASS
********** Results for Pattern_Reduction_Hierarchical_fp64**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000068 [s]
run-time-stddev: 0.000000 [s]
run-time-median: 0.000068 [s]
run-time-min: 0.000068 [s]
run-time-samples: "0.000068"
run-time-throughput: N/A
Verification: FAIL

real	0m0.022s
user	0m0.011s
sys	0m0.014s
```

### scalar_prod

```
********** Results for ScalarProduct_NDRange_int32**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000163 [s]
run-time-stddev: 0.000021 [s]
run-time-median: 0.000160 [s]
run-time-min: 0.000145 [s]
run-time-samples: "0.000145 0.000147 0.000160 0.000164 0.000197"
run-time-throughput: N/A
Verification: PASS
********** Results for ScalarProduct_NDRange_int64**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000153 [s]
run-time-stddev: 0.000005 [s]
run-time-median: 0.000152 [s]
run-time-min: 0.000148 [s]
run-time-samples: "0.000148 0.000150 0.000152 0.000155 0.000160"
run-time-throughput: N/A
Verification: PASS
********** Results for ScalarProduct_NDRange_fp32**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000156 [s]
run-time-stddev: 0.000008 [s]
run-time-median: 0.000152 [s]
run-time-min: 0.000149 [s]
run-time-samples: "0.000149 0.000150 0.000152 0.000162 0.000167"
run-time-throughput: N/A
Verification: PASS
********** Results for ScalarProduct_NDRange_fp64**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000160 [s]
run-time-stddev: 0.000000 [s]
run-time-median: 0.000160 [s]
run-time-min: 0.000160 [s]
run-time-samples: "0.000160"
run-time-throughput: N/A
Verification: FAIL
********** Results for ScalarProduct_Hierarchical_int32**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000153 [s]
run-time-stddev: 0.000007 [s]
run-time-median: 0.000151 [s]
run-time-min: 0.000147 [s]
run-time-samples: "0.000147 0.000147 0.000151 0.000158 0.000162"
run-time-throughput: N/A
Verification: PASS
********** Results for ScalarProduct_Hierarchical_int64**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000152 [s]
run-time-stddev: 0.000009 [s]
run-time-median: 0.000149 [s]
run-time-min: 0.000144 [s]
run-time-samples: "0.000144 0.000144 0.000149 0.000160 0.000163"
run-time-throughput: N/A
Verification: PASS
********** Results for ScalarProduct_Hierarchical_fp32**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000153 [s]
run-time-stddev: 0.000007 [s]
run-time-median: 0.000150 [s]
run-time-min: 0.000147 [s]
run-time-samples: "0.000147 0.000149 0.000150 0.000156 0.000164"
run-time-throughput: N/A
Verification: PASS
********** Results for ScalarProduct_Hierarchical_fp64**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000155 [s]
run-time-stddev: 0.000000 [s]
run-time-median: 0.000155 [s]
run-time-min: 0.000155 [s]
run-time-samples: "0.000155"
run-time-throughput: N/A
Verification: FAIL

real	0m0.037s
user	0m0.017s
sys	0m0.025s
```

### segmentedreduction

```
********** Results for Pattern_SegmentedReduction_NDRange_int16**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000066 [s]
run-time-stddev: 0.000040 [s]
run-time-median: 0.000048 [s]
run-time-min: 0.000045 [s]
run-time-samples: "0.000045 0.000046 0.000048 0.000052 0.000137"
run-time-throughput: N/A
Verification: PASS
********** Results for Pattern_SegmentedReduction_NDRange_int32**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000046 [s]
run-time-stddev: 0.000002 [s]
run-time-median: 0.000046 [s]
run-time-min: 0.000044 [s]
run-time-samples: "0.000044 0.000046 0.000046 0.000046 0.000050"
run-time-throughput: N/A
Verification: PASS
********** Results for Pattern_SegmentedReduction_NDRange_int64**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000045 [s]
run-time-stddev: 0.000002 [s]
run-time-median: 0.000044 [s]
run-time-min: 0.000044 [s]
run-time-samples: "0.000044 0.000044 0.000044 0.000045 0.000048"
run-time-throughput: N/A
Verification: PASS
********** Results for Pattern_SegmentedReduction_NDRange_fp32**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000045 [s]
run-time-stddev: 0.000002 [s]
run-time-median: 0.000045 [s]
run-time-min: 0.000042 [s]
run-time-samples: "0.000042 0.000045 0.000045 0.000046 0.000047"
run-time-throughput: N/A
Verification: PASS
********** Results for Pattern_SegmentedReduction_NDRange_fp64**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
sycl-implementation: hipSYCL
launcher count: 2
launcher count: 2
throughput-metric: N/A
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 0.000043 [s]
run-time-stddev: 0.000000 [s]
run-time-median: 0.000043 [s]
run-time-min: 0.000043 [s]
run-time-samples: "0.000043"
run-time-throughput: N/A
Verification: FAIL
********** Results for Pattern_SegmentedReduction_Hierarchical_int16**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000045 [s]
run-time-stddev: 0.000001 [s]
run-time-median: 0.000045 [s]
run-time-min: 0.000043 [s]
run-time-samples: "0.000043 0.000044 0.000045 0.000045 0.000047"
run-time-throughput: N/A
Verification: PASS
********** Results for Pattern_SegmentedReduction_Hierarchical_int32**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000045 [s]
run-time-stddev: 0.000001 [s]
run-time-median: 0.000044 [s]
run-time-min: 0.000044 [s]
run-time-samples: "0.000044 0.000044 0.000044 0.000046 0.000047"
run-time-throughput: N/A
Verification: PASS
********** Results for Pattern_SegmentedReduction_Hierarchical_int64**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000044 [s]
run-time-stddev: 0.000002 [s]
run-time-median: 0.000043 [s]
run-time-min: 0.000041 [s]
run-time-samples: "0.000041 0.000043 0.000043 0.000045 0.000046"
run-time-throughput: N/A
Verification: PASS
********** Results for Pattern_SegmentedReduction_Hierarchical_fp32**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.000044 [s]
run-time-stddev: 0.000001 [s]
run-time-median: 0.000044 [s]
run-time-min: 0.000042 [s]
run-time-samples: "0.000042 0.000043 0.000044 0.000045 0.000045"
run-time-throughput: N/A
Verification: PASS
********** Results for Pattern_SegmentedReduction_Hierarchical_fp64**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
sycl-implementation: hipSYCL
launcher count: 2
launcher count: 2
throughput-metric: N/A
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 0.000047 [s]
run-time-stddev: 0.000000 [s]
run-time-median: 0.000047 [s]
run-time-min: 0.000047 [s]
run-time-samples: "0.000047"
run-time-throughput: N/A
Verification: FAIL

real	0m0.022s
user	0m0.007s
sys	0m0.017s
```

### sf

```
********** Results for MicroBench_sf_fp32_16**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
sycl-implementation: hipSYCL
launcher count: 2
...
launcher count: 2
throughput-metric: 0.000137 [GOP]
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 0.000059 [s]
run-time-stddev: 0.000010 [s]
run-time-median: 0.000054 [s]
run-time-min: 0.000054 [s]
run-time-samples: "0.000054 0.000054 0.000054 0.000055 0.000077"
run-time-throughput: 2.554485 [GOP/s]
Verification: PASS
********** Results for MicroBench_sf_fp64_16**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
sycl-implementation: hipSYCL
launcher count: 2
launcher count: 2
launcher count: 2
throughput-metric: 0.000137 [GOP]
kernel-time-mean: N/A
kernel-time-stddev: N/A
kernel-time-median: N/A
kernel-time-min: N/A
kernel-time-samples: N/A
kernel-time-throughput: N/A
run-time-mean: 0.000055 [s]
run-time-stddev: 0.000000 [s]
run-time-median: 0.000055 [s]
run-time-min: 0.000055 [s]
run-time-samples: "0.000055"
run-time-throughput: 2.499165 [GOP/s]
Verification: FAIL

real	0m0.012s
user	0m0.013s
sys	0m0.000s
```

### sobel

```
********** Results for Sobel3**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.005178 [s]
run-time-stddev: 0.000009 [s]
run-time-median: 0.005177 [s]
run-time-min: 0.005169 [s]
run-time-samples: "0.005169 0.005175 0.005177 0.005179 0.005193"
run-time-throughput: N/A
Verification: PASS

real	0m3.962s
user	0m2.788s
sys	0m1.085s
```

### sobel5

```
********** Results for Sobel5**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.013620 [s]
run-time-stddev: 0.000034 [s]
run-time-median: 0.013611 [s]
run-time-min: 0.013589 [s]
run-time-samples: "0.013589 0.013608 0.013611 0.013613 0.013678"
run-time-throughput: N/A
Verification: PASS

real	0m4.004s
user	0m2.750s
sys	0m1.133s
```

### sobel7

```
********** Results for Sobel7**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.026268 [s]
run-time-stddev: 0.000033 [s]
run-time-median: 0.026254 [s]
run-time-min: 0.026252 [s]
run-time-samples: "0.026252 0.026253 0.026254 0.026255 0.026326"
run-time-throughput: N/A
Verification: PASS

real	0m4.104s
user	0m2.798s
sys	0m1.123s
```

### syr2k

```
********** Results for Polybench_Syr2k**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 1.534632 [s]
run-time-stddev: 0.000925 [s]
run-time-median: 1.534840 [s]
run-time-min: 1.533639 [s]
run-time-samples: "1.533639 1.533785 1.534840 1.535034 1.535862"
run-time-throughput: N/A
Verification: PASS

real	10m28.329s
user	10m20.316s
sys	0m0.352s
```

### syrk

```
********** Results for Polybench_Syrk**********
problem-size: 3072
local-size: 256
device-name: Iluvatar BI-V100
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
run-time-mean: 0.659142 [s]
run-time-stddev: 0.000172 [s]
run-time-median: 0.659081 [s]
run-time-min: 0.658976 [s]
run-time-samples: "0.658976 0.659042 0.659081 0.659196 0.659413"
run-time-throughput: N/A
Verification: PASS

real	5m11.822s
user	5m8.286s
sys	0m0.252s
```