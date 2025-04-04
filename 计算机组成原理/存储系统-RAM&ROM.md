## 3.2 静态随机存取存储器
静态随机存取存储器(SRAM)的优点是==存取速度快==，但存储密度和容量不如 DRAM 大。
### 3.2.1 基本的静态存储元阵列
![[Pasted image 20250318141406.png|500]]
1. 存储位元
2. 三组信号线：
	1. 地址线
	2. 数据线
	3. 控制线

### 3.2.2 基本的 SRAM 逻辑结构

地址线共 15 条，其中 x 方向 8 条(A0～A7)，经行译码输出 256 行，y 方向 7 条(A8～A14)
双向数据线有 8 条，即 I/O_1～I/O_7。
（256 行×128 列×8位）
![[Pasted image 20250318141706.png]]
- **$\overline{CS}$（片选信号）**：当 CS 为低电平（有效）时，表明该 SRAM 芯片被选中，内部的门 G1 和 G2 都被打开，此时芯片能够参与操作。
- **$\overline{OE}$（输出使能信号）**：当 OE 为低电平时，开启门 G2，允许数据输出，实现读操作。
- **$\overline{WE}$（写使能信号）**：在写操作时，WE 为低电平，门 G1 被开启；而在读操作时，WE 为高电平，门 G1 关闭。这样通过门 G1 和 G2 的互锁控制，确保同一时间只能进行读操作或写操作，避免数据混乱。
-

### 3.2.3 SRAM 读/写时序
![[Pasted image 20250318142644.png|500]]


### 3.2.4 存储器容量的扩充      ==重点==
1. 位扩展：地址线和控制线公用而数 据线单独分开连接。
	- 例：4位到8位，由一片变两片
2. 字扩展：给定芯片的地址总线和数据总线公用，读写控制信号线公用，由地址总线的高位译码产生片选信号，让各个芯片分时工作。
3. 字位扩展 若给定的芯片的字数和位数均不符合要求，则需要先进行位扩展，再进行字扩展。



## 3.3 动态随机存取存储器(DRAM)
### 3.3.1 DRAM 存储元的工作原理
SRAM具有两个稳定的状态。而动态随机存取存储器 (DRAM)简化了每个存储元的结构，因而 DRAM 的存储密度很高，通常用作计算机的主存储器。
### 3.3.2 DRAM芯片的逻辑结构
![[Pasted image 20250324084702.png]]
由于DRAM容量大，为了减少芯片的引脚数量，采用行列分时传送。例如上芯片本来需要20位引脚用于地址编码，现在只需10位，先$\overline{RAS}$,打入行地址锁存器，再$\overline{CAS}$打入列地址锁存器。

### 3.3.3 DRAM 读/写时序
![[Pasted image 20250324085153.png]]
读写周期为$\overline{RAS}$的下降沿到下一个$\overline{RAS}$的下降沿之间的时间。通常为控制方便，读周期和写周期时间相等。
这里$\overline{RAS}$ 和$\overline{CAS}$ 之间的时间间隔用于让电路恢复稳定
### 3.3.4 DRAM 的刷新操作
DRAM 存储位元是基于电容器上的电荷量存储信息的，读操作是破坏性的，而且未读写的存储元电荷量也会逐渐流失，需要定时刷新。
由于自动刷新不需要给出列地址，而行地址由片内刷新计数器自动生成，故 3.11 例：DRAM 存储容量 扩展 72 计算机组成原理 可利用CAS 信号先于 RAS 信号有效来启动一次刷新操作，地址信号无效。
#### 刷新策列： 
1. 集中式刷新策略：
	   64ms 的刷新周期时间：进行一段时间的读写操作后，集中刷新所有8192行，此时读写操作被暂停，这段集中刷新操作时间为“死时间”
2. 分散式刷新策略：
		每一行的刷新操作被均匀地分配到刷新周期时间内。由于 64ms 除以 8192 约等于 7.8μs，所以 DRAM 每隔 7.8μs 刷新一行。
3. 异步刷新（集中分散组合）
		

### 3.3.5 突发传输模式
DRAM价格便宜，存储量大，适合作为主存，为了提升访问速度，采用突发访问技术：
	是在存储器同一行中对相邻的存储单元进行连续访问的方式(突发长度几字节—数千字)

### 3.3.6 同步 DRAM(SDRAM)
传统的DRAM在控制信号、地址信号传输后，cpu需要等待一段时间（需要额外多等待一会确保数据传输可靠）后才能读取，而通过将DRAM 接上 时信号 与CPU同步运行，优化DRAM 和 CPU 间的接口。
![[Pasted image 20250331001557.png|500]]
SRAM 特征：
1. 同步操作：所有的输入输出信号都在时钟信号CLK的控制下，严格与CPU同步进行
2. 多存储体配置：将SDRAN拆分为多个独立的存储体（BANK），支持流水线并行操作。各bank 间可同时或独立操作，也可以顺序和交替工作（例： 一个bank 在刷新不影响另一个bank 读写操作）
3. 命令控制：用$\overline{RAS},\overline{CAS},\overline{WE},\overline{CS}$ 与固定地址线（选择banks）的组合表示命令（激活、读写、预充）
	- 激活就是对应后面的CDRAM的加载到cache中
	- 预充：关闭已激活的行，重写回SRAM中
4. 模式寄存器： SRAM在加电使用前先需要设置模式（模式寄存器）：
	1. $\overline{CAS}$ 延迟
	2. 突发类型，突发长度
	3. 测试模式
时序图
![[Pasted image 20250331001620.png|500]]

##### 读写命令
![[Pasted image 20250331001706.png|500]]

在 T1 时钟的上升沿，控制器发出存储体 A 的激活命令。存储体激活命令通过在时钟上 升沿发出下列信号组合发出： 
	CS=0、 RAS =0、CAS =1、 WE =1，地址线 A11=0 选择存储 体 A。
在 T3 时钟的上升沿，控制器发出存储体 A 的读命令。读命令通过在时钟上升沿发出下 列信号组合发出： 
	CS=0、 RAS =1、CAS =0、 WE =1。
经过 2 个时钟周期的内部操作，数据在 T5 时钟的上升沿开始送出。此例中，突发长度 BL=4，故在随后的四个时钟周期内分别送出一个数据字。 
在 T9 时钟的上升沿，DQ 输出被设置为高阻状态。
在 T10 时钟的上升沿，控制器发出 存储体 A 的写命令。写命令通过在时钟上升沿发出下列信号组合发出： 
	CS=0、 RAS =1、CAS =0、 WE =0。 在 T14 时钟的上升沿开始下一次读操作。


### 3.3.7 双倍数据率 SDRAM(DDR SDRAM)

单速率SDRAM(SDR SDRAM) 只能在时钟上升沿读写操作，而双数据率的 DDR SDRAM可以在上升沿和下降沿都可以传输数据。
![[Pasted image 20250331005248.png]]

具体实现：
在一次读取时，读取数据总线n比特的2倍的数据先存在锁存和缓存中，之后每一个上升或下降沿都读取一个n比特的数据。

而差分时钟，用相反的$\overline{CK}$ 校准 CK 上下沿间距，保证传输周期的稳定，以确保数据的正确传输。

DDR1：2n
DDR2：4n
DDR3：8n
DDR4：$8n \times m(banks)$
	每个bank 都有独立的激活、读取、写入和刷新操作，等价于16n或32n


### 3.3.9 CDRAM
将[cache](存储系统-cache) 及其思想 加入到DRAM中作为缓存器加速操作。
在常规的 DRAM 芯片封装内又集成了一个小容量 SRAM 作为高速缓冲存储器
##### 1. CDRAM 芯片的结构
图 3.17 为 1M×4 位 CDRAM 芯片的结构框图。一片 512×4 位的 SRAM 构成 cache，保 存最近访问的一行数据。

在第一次访问时：
	行选通信号，输入行地址，存入行地址锁存器 和 最后读出行地址锁存器
	行地址给DRAM,DRAM将行全部传给SRAM
	列选通信号，列地址给SRAM cache , 最后访问数据

后续访问时：
	行地址 先与 最后读出行地址锁存器 比较：
	- 相同，直接从cache访问数据
	- 不同，从DRAM 中写入 SRAM 更新最后读出行地址锁存器，访问
![[Pasted image 20250331010741.png]]
优势：
1. 突发操作的速度高，如果连续访问的地址的高 11 位相同(属于同一行地址)，那么只需连续变动9 位列地址就能从 SRAM 中快速读出数据。
2. 是在 SRAM 读出期间可同时对 DRAM 阵列进行刷新。
3. 允许在写操作完成的同时启动同一行的读操作。（数据输出路径(由 SRAM 到 I/O)与数据输入路径(由 I/O 到读出放大和列写选择)是分开的）

##### 2. CDRAM 存储模块
![[Pasted image 20250325133247.png|550]]
==必考==
 8个芯片共用片选信号Sel 、行选通信号 RAS 、刷新信号 Ref 和地址输入信号 A0～A10。

用$\overline{BE_0} \sim \overline{BE_3}$  选择实现字存取(32 位)、半字存取(高 16 位或低 16 位)或字节存取(任意 8 位)。

系统存储地址的最高两位 A23、A22作为模块选择地址，译码输出可以分别驱动 4 个这
样的 4MB 模块的Sel 信号。

优势：
	存储模块具有高速的突发存取能力
		例：如果连续访问的数据块的高13位地址相同，只有第一个存储字需要完整的存储周期（如6T），后面的因为已经存在SRAM中，故存储周期大幅缩短（如2T）。

## 3.4 只读存储器(ROM)
1. 掩膜ROM（MROM）
	只能读不能擦除
2. PROM（一次编程）
3. EPROM（多次性编程）：紫外线擦除
4. EEPROM（多次性编程）： 电可擦写，局部擦写

