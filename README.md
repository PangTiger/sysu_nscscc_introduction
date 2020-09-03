# sysu_nscscc_introduction
龙芯杯大赛的简单介绍

## 参赛经验分享
为了让师弟师妹们少走一些坑，我们STPU队的几位兄弟各自完成了一份比赛的经验与总结，希望能够帮助到后面参赛的师弟师妹们。

### 设计目标
一个优秀的工程项目首先应当具备合理的设计目标，设计目标是整个SoC的总体架构，也是进行细节设计时最基本的宏观约束条件。对于刚刚入门的新手而言，一个比较合适的设计目标是“略大于或等于目前队伍的实际硬件设计能力”。例如，如果队伍已经按照教科书的样例实现过一个简单的无Cache的五级流水线CPU，那么该队伍完全可以将“实现一个带有多路组相联的Cache的流水线CPU”定为自己的设计目标。如果认为多路组相联不容易实现，可以先实现一个直接映射的Cache，结合AXI Burst传输之后，直接映射的Cache性能依然不俗。

稍微偏一下题，按照2017-2020共四届龙芯杯比赛的经验，如果能够实现直接映射的ICache和DCache，主频不太低的情况下，性能分应该能够达到30分以上的水平，这已经可以稳进龙芯杯决赛了。

### 项目分工

### 进度管理

### 工作效率：线上/线下

### 硬件Debug经验

### 软件Debug经验