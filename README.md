# sysu_nscscc_introduction
龙芯杯大赛的简单介绍

## 参赛经验分享
为了让师弟师妹们少走一些坑，我们STPU队的几位兄弟各自完成了一份比赛的经验与总结，希望能够帮助到后面参赛的师弟师妹们。

### 设计目标
一个优秀的工程项目首先应当具备合理的设计目标，设计目标是整个SoC的总体架构，也是进行细节设计时最基本的宏观约束条件。对于刚刚入门的新手而言，一个比较合适的设计目标是“略大于或等于目前队伍的实际硬件设计能力”。例如，如果队伍已经按照教科书的样例实现过一个简单的无Cache的五级流水线CPU，那么该队伍完全可以将“实现一个带有多路组相联的Cache的流水线CPU”定为自己的设计目标。如果认为多路组相联不容易实现，可以先实现一个直接映射的Cache，结合AXI Burst传输之后，直接映射的Cache性能依然不俗。

稍微偏一下题，按照2017-2020共四届龙芯杯比赛的经验，如果能够实现直接映射的ICache和DCache，主频不太低的情况下，性能分应该能够达到30分以上的水平，这已经可以稳进龙芯杯决赛了。

### 项目分工
我们的项目分工为：刘嘉琦负责流水线的设计，林金煌负责ICache和DCache设计，何伟亮负责TLB以及用于双发射的分支预测模块，马艺文负责操作系统的编译与移植。这种分工大体上是由三个同学负责硬件设计，一个同学负责软件移植。

由于我们原本的设计目标是：完成一个较为完备的单发射CPU之后，进行尝试双发射顺序结构的CPU。但是由于时间的原因，单发射的设计耗费了我们大量的时间进行Debug，最终分支预测模块已经完成了，但是流水线和Cache没有完成双发射的设计。

经过本次比赛的磨练，我们发现这种分工总体上是比较合理的。CPU流水线设计完全交给一个同学设计，可以保证他在Debug时对流水线的了解比较充分，不会出现两个同学分别负责流水线的不同阶段，最终导致流水线不同阶段之间Debug比较困难的情况。我们发现Cache的设计难度和工作量与流水线设计大体相当，这就要求负责这两个模块的同学在比赛中投入更多的时间，同时也需要有基本的硬件设计经验。

另外在软件方面，至少需要一位同学负责操作系统的编译与移植。比较理想的情况下他的科技树应该为：
1. 硬件底层的知识（TLB 中断 串口）
2. 操作系统知识（TLB 内存管理 操作系统启动过程）
3. 单片机编程知识

对于电信院的同学而言，上述1和3应当是比较容易实现的，但是同学们在2上的知识基本是一片空白。负责软件移植的同学可以先在MooC平台学习一些操作系统的基本知识，然后尝试搭建Qemu模拟器平台，这样可以在硬件SoC设计还没有完成时先尝试移植操作系统。

### 进度管理
本次比赛中，非常值得一提的是我们的进度管理与DDL驱动。

我们的时间轴大致是这样的：
2019年5-7月：正在学习数字电路课程，参加粟涛老师培训，学习计算机组成
2019年8-9月：照着《自己动手写CPU》完成了一个非常基本的CPU，能够运行十几条指令。
2019年9-12月：继续参考《自己动手写CPU》，提高CPU的完成度，运行龙芯杯的功能测试，卡在大约第64个功能点（中断与异常）
2020年1-4月：躺平，在家摸鱼
2020年5月-7月：开始准备参赛，开始参考2019年国科大的代码，增加自己的流水线握手协议。7月中旬，完成了所有功能点测试（无Cache情况下）。
2020年8月：
1. 初赛DDL前2天，使用LUT搭建的Cache 功能测试、性能测试Debug通过，性能分29分@59MHz。系统测试由于非常弱智的Bug没有跑通，但最终还是进了决赛。
2. 初赛后，完成TLB测试，调大Dcache，改善访存部分的时序。最终优化后性能分增加到43分，主频来到了65MHz。在这期间，我们成功跑通了PMON，但UCore和Linux移植失败。

### 工作效率：线上/线下
事实证明，在家远程合作的工作效率远低于线下面对面合作。但是今年情况特殊，疫情之下，大多数情况只能靠队友们各自为战。推荐大家在完成项目时尽量抽出共同的时间，在线下合作。当然，对于一个四人的队伍，抽出这样的公共时间肯定有难度，但是最起码的要求是：Cache和流水线设计的两位同学还是应该保持紧密合作，在自己的模块完成度较高了之后，模块整合时一定要保证线下一起Debug。对于负责软件的同学，操作系统移植的任务相对而言更为独立一些，更多的时间会消耗在硬件完成之后。

### 硬件部分Debug经验
硬件Debug是一件十分费力的事情。

首先最常用的工具是Vivado的波形图。比较好用的设置包括：
1. 改变波形颜色、分组
2. 添加Marker用于定位
3. 改变波形的进制显示：2/10/16进制、ASIIC码显示
4. 在Vivado仿真设置中，每次仿真全部信号的波形，这样添加新的信号线时不需要重新跑仿真。

另外附上一些常见的硬件设计Bug:
1. 例化模块后忘记连线
2. 模块端口、例化连线的位宽不一致
3. 设计的控制信号考虑的情况不足：没有考虑到所有的输入情况
4. 写出组合逻辑Loop：此问题常见于Mealy型状态机，建议所有状态机使用Moore型

除了这些常见的硬件设计Bug外，更常见的Bug是时序错拍：早一拍或晚一拍。这种问题如果出现在流水线中，大概率是数据信号和控制信号的同步没有做好。

功能测试、性能测试的Bug可以在仿真波形图中发现，但是系统测试、操作系统移植的Bug只能通过硬件ILA发现。硬件ILA的使用需要参考龙芯官方给出的调试手册。
