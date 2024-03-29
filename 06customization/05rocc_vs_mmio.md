# RoCC vs MMIO

可以通过多种方式将加速器或自定义 IO 设备添加到您的 SoC：

1. MMIO 外设（又名 TileLink 附加加速器）。
2. 紧耦合 RoCC 加速器。

这些方法的不同之处在于处理器和自定义块之间的通信方法。

通过 TileLink-Attached 方法，处理器通过内存映射寄存器与 MMIO 外设进行通信。

相比之下，处理器通过自定义协议和 RISC-V ISA 编码空间中保留的自定义非标准 ISA 指令与 RoCC 加速器进行通信。每个核最多可以有四个加速器，这些加速器由自定义指令控制并与 CPU 共享资源。 RoCC 协处理器指令具有以下形式。

```
customX rd, rs1, rs2, funct
```

X 是数字 0-3，决定指令的操作码，它控制指令将路由到哪个加速器。`rd`、`rs1` 和 `rs2` 字段是目标寄存器和两个源寄存器的寄存器号。 `funct` 字段是一个 7 位整数，加速器可以使用它来区分不同的指令。

请注意，通过 RoCC 接口进行通信需要自定义软件工具链，而 MMIO 外设可以使用该标准工具链以及适当的驱动程序支持。