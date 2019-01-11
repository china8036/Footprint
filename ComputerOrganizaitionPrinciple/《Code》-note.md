- [《编码：隐匿在计算机软硬件背后的语言》 读后笔记](#编码隐匿在计算机软硬件背后的语言-读后笔记)
  - [1. Refer Links](#1-refer-links)

# 《编码：隐匿在计算机软硬件背后的语言》 读后笔记

- P135

  加法是算术运算中最基本的运算。实际上，加法计算就是计算机要做的唯一工作。

- P163

  反向器（输出接输入） → 振荡器（在不需要人工干涉的情况下，可以完全自发地工作；在所有自动控制系统中，有着重要作用；为使不同组件同步工作，所有计算机都配备着某种振荡器） → 时钟

- P167, P206, P214

  触发器（可以记住电路的先前状态） → 计数器 → 锁存器 + 译码器 → 存储器 (RAM) → RAM 阵列（容量 =2^ 地址输入端个数）（易失性）

- P244

  自动操作：指令 + RAM 电路 + 处理器电路 → 计算机

  条件跳转指令 -- 能否控制重复操作或循环是计算机与计算器的区别。

- P274, P276

  继电器 → 真空管 → 晶体管（半导体硅）

  微处理器 (microprocessor):
  - 世界上第一个可商用的微处理器: (1971.11) **Intel 4004**: 4-bit
  - 改变世界的 2 款 microprocessor:
    - 世界上第一个 8 位微处理器 (1974.4) **Intel 8008**: 8-bit, 2 MHz, 14-bit address bus that could address 16 KB of memory
    - (1974.8) **Motorola 6800**: 8-bit, 1 MHz, 14-bit address bus that could address 16 KB of memory
  - Other
    - (1974) **Intel 8080**: 8-bit
    - (1976) **Intel 8085**: 8-bit
    - (1978) **Intel 8086**: 16-bit. The 8086 gave rise to the x86 architecture, which eventually became Intel's most successful line of processors.
    - (1979) **Intel 8088**: 16-bit
    - (1979) **Motorola 68000**: 16/32-bit

  字节序
  - little-endian: Intel
  - big-endian: Motorola


## 1. Refer Links

《编码：隐匿在计算机软硬件背后的语言》 2012 年 10 月第一次印刷
