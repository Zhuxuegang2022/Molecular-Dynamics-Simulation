# 《分子动力学模拟》第4章：一个简单的分子动力学模拟程序

## 本章程序

见 src/simpleMD.cpp


该 C++ 程序可以在 Linux 和 Windows 操作系统使用。我们推荐使用 GCC 工具。在命令行可以用如下方式编译本章的程序：
```shell
    $ g++ -O3 simpleMD.cpp -o simpleMD
```
其中，`-O3` 选项表示优化等级。编译完成后，将生成名为 `simpleMD` 的可执行文件（在 Windows 中为 `simpleMD.exe`)。

然后，就可以在命令行使用该程序：
```shell
    $ simpleMD numCells numSteps temperature timeStep
```
如果使用时忘了给命令行参数，程序会提示正确的用法。

具体地，笔者用如下命令运行程序：
```shell
    $ simpleMD 4 20000 60 5
```
也就是说，考虑原子数为 $4^3 \times 4 = 256$ 的固态氩体系，运行 $2\times 10^4$ 步，初始温度为 60 K，积分步长为 5 fs。该模拟在笔者的计算机中运行的时间约为 10 秒钟。

## 本章范例

### 能量守恒的测试

程序每隔100步输出系统的总动能 $K(t)$ 和总势能 $U(t)$，它们都是时间 $t$ 的函数。对于大小有限的体系，它们都是随时间 $t$ 涨落的。然而，根据能量守恒定律，系统动能和势能的和，即总能量 $E(t)=K(t)+V(t)$，应该是不随时间变化的。当然，我们的模拟中使用了具有一定误差的数值积分方法，故总能量也会有一定大小的涨落。这个涨落主要与积分的时间步长有关系。一般来说，积分的时间步长越大，总能量的涨落越大。

![energy](src/ex1/fig-chapter5-energy.png)

（a) 体系总动能随时间的变化；（b) 体系总势能随时间的变化；（c) 体系总能随时间的变化；（d) 相对总能量随时间的变化。


上图（a-c）给出了系统的总动能、总势能和总能量随时间变化的情况。可以看出动能是正的，势能是负的，涨落相对较大；总能是负的，涨落较小。细心的读者可以注意到，体系的总动能在很短的时间内突然降低了大约一半。这是因为，我们的模拟体系一开始是完美的面心立方晶格，每个原子都处于受力为零的平衡状态，没有振动产生的额外势能。我们用某个温度（在我们的例子中是 60 K）初始化了原子的速度，故该体系一开始是有一定的动能的。假设每个原子都会在其平衡位置做简谐振动（这就是所谓的简谐近似，它对很多问题的研究来说是一个很好的出发点），故根据能量均分定理，在达到热力学平衡后体系的动能会基本上等于体系的振动势能。也就是说，随着时间的推移，体系的动能平均值会减半，减少的部分变成了原子的振动势能。从图（a-b）可以看到，这个过程是很快的。该过程实际上就是一个从非平衡态跑向平衡态的过程。在这个例子中，这个过程只需要 1 ps 的量级。图 （d）给出了 $(E(t)-\langle E\rangle)/|\langle E\rangle|$，即总能的相对涨落值。因为总能量在模拟的初期也有一个突然的变化，我们在计算总能的相对涨落时去掉了第一组输出的能量值（这相当于去掉了一个远离平衡态的时刻）。总能量相对涨落值在 $10^{-4}$ 量级。对于很小的体系来说，这是一个合理的值。


