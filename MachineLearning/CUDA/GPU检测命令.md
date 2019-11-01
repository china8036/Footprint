
# GPU 监测命令

## nvidia-smi (NVIDIA System Management Interface)

https://developer.nvidia.com/nvidia-system-management-interface

https://developer.download.nvidia.cn/compute/DCGM/docs/nvidia-smi-367.38.pdf

https://blog.csdn.net/Bruce_0712/article/details/63683787

https://blog.csdn.net/csdnofzyk/article/details/80187919

https://stackoverflow.com/questions/40937894/nvidia-smi-volatile-gpu-utilization-explanation

https://cloud.tencent.com/developer/article/1063821

https://blog.csdn.net/U201017971/article/details/78922274

https://yiyibooks.cn/yiyi/tensorflow_13/performance/performance_guide.html

https://www.jianshu.com/p/3fe480c5bb12

https://zhuanlan.zhihu.com/p/31558973

> The NVIDIA System Management Interface (nvidia-smi) is a command line utility, based on top of the NVIDIA Management Library (NVML), intended to aid in the management and monitoring of NVIDIA GPU devices.
>
> This utility allows administrators to query GPU device state and with the appropriate privileges, permits administrators to modify GPU device state.  It is targeted at the TeslaTM, GRIDTM, QuadroTM and Titan X product, though limited support is also available on other NVIDIA GPUs.

-h, –help	Print usage information and exit.

-L, –list-gpus	Display a list of GPUs connected to the system.

-i,–id=	Target a specific GPU.
-f,–filename=	Log to a specified file, rather than to stdout.
-l,–loop=	Probe until Ctrl+C at specified second interval.

