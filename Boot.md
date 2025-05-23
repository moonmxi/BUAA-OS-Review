# 系统引导

## 计算机启动过程（MIPS）

### U-Boot启动流程

* **stage1** ：依赖于CPU体系结构的代码（如设备初始化代码等）通常都放在stage1且可以用汇编语言来实现
* **stage2** ：stage2则通常用C语言来实现，这样可以实现复杂的功能，而且有更好的可读性和可移植性。

### MIPS基本地址空间

* **kuseg** :这些地址是用户态可用的地址,在有MMU的机器里,这些地址将一概被MMU作转换,除非MMU的设置被建立好,否则这2G的地址是不可用的。
* **kseg0** ：地址处于 `0x80000000~0x9fffffff` ，将虚拟地址的最高位置0得到物理地址，通过cache访存。这一部分用于存放内核代码与数据。
* **kseg1** ：地址处于 `0xa0000000~0xbfffffff` ，将虚拟地址的最高3位置0得到物理地址，不通过cache访存。这一部分可以用于访问外设。是唯一在系统重启时能正常工作的地址空间。
* **kseg2** ：地址位于 `0xc0000000~0xffffffff`，这块区域只能在核心态下使用并且要经过MMU的转换.。在MMU设置好之前,不要存取该区域.。除非在写一个真正的操作系统，否则没有理由用kseg2。有时会看到该区域被分为kseg2和kseg3，意在强调低半部分(kseg2)可供运行在管理态的程序使用。

### MIPS ROM/Flash启动地址

MIPS的启动入口地址是 `0xBFC0 0000`，通过将最高3位清零（`&0x1FFF FFFF`）的方法，将ROM所在的地址区映射到物理内存的低端512M(`0x0000 0000 - 0x1FFF FFFF`)空间，也是非翻译无需转换的地址区域。
