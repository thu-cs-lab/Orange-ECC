# FPGA 相关

### ILA

Integrated logic analyzer

See: <https://www.xilinx.com/products/intellectual-property/ila.html>

### 时序

考虑语境：

1. ~逻辑。通常和组合逻辑对应。
1. 
    综合~。通常指”光跑得不够快“这个事实。由于杰哥没有时间优化物理常数，所以需要你优化你的代码。

    请参考人人爱的 Coursera: <https://www.coursera.org/lecture/intro-fpga-design-embedded-systems/4-improving-timing-with-pipelining-3AgRZ>

## HDL / RTL / HLS

这是一组相关的概念：

- HDL: Hardwire Design Language，指硬件描述语言。
- RTL: Register Transfer Language/Level，寄存器转移，是一种 HDL 语言对于硬件的抽象方式。常用的硬件描述语言暂时都使用这种抽象： VHDL, Verilog, SystemVerilog, Chisel/FIRRTL, etc.
- HLS: High Level Synthesis，一般和 RTL 对应，指将类似高级语言(C, etc.)实现的逻辑编译成 RTL 层的表述。
