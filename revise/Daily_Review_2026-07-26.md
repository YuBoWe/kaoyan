# 2026-07-26 每日复习汇总与考点整理

> 生成时间：2026-07-26  
> 包含模块：408考研错题与待办、数据结构与算法 (邻接表/矩阵转换)、动态规划 (目标和模板)、计算机网络 (分层架构、虚拟地址、尽最大努力交付、TCP连接管理)

---

## 一、今日错题与待办事项 (Daily Error & TODO)

1. **概率论**：
   - 课本 p223 第 6.13 题
   - 课本 p229 第 7.3 题（第一问和第三问）
   - 课本 p232 第 7.6 题（第二问）

2. **高等数学**：
   - 课本 p265 第 4 题
   - 课本 p268 第 2 题
   - 课本 p269 第 1、2 题

3. **线性代数**：
   - **向量组等价**：定义（$A$ 与 $B$ 中的向量可互相线性表示），充要条件是：
     $$\text{r}(A) = \text{r}(B) = \text{r}(A, B)$$
   - **规范正交基与施密特正交化**：
     - 规范正交基：相互垂直且长度都为 1 的一组基底。
     - 施密特正交化公式：
       $$\boldsymbol{\beta}_1 = \boldsymbol{\alpha}_1$$
       $$\boldsymbol{\beta}_2 = \boldsymbol{\alpha}_2 - \frac{(\boldsymbol{\alpha}_2, \boldsymbol{\beta}_1)}{(\boldsymbol{\beta}_1, \boldsymbol{\beta}_1)} \boldsymbol{\beta}_1$$
       $$\boldsymbol{\beta}_3 = \boldsymbol{\alpha}_3 - \frac{(\boldsymbol{\alpha}_3, \boldsymbol{\beta}_1)}{(\boldsymbol{\beta}_1, \boldsymbol{\beta}_1)} \boldsymbol{\beta}_1 - \frac{(\boldsymbol{\alpha}_3, \boldsymbol{\beta}_2)}{(\boldsymbol{\beta}_2, \boldsymbol{\beta}_2)} \boldsymbol{\beta}_2$$

4. **计算机组成原理与操作系统**：
   - **寻址方式考点**：
     - 基址寻址：面向操作系统，用于解决多道程序的“动态重定位”。
     - 变址寻址：面向程序员，用于解决数据的“循环与数组遍历”。
     - 堆栈寻址、跳跃寻址。
     - 缩短指令中某个地址的位数：寄存器寻址。
     - 简化地址结构的基本方法：隐含寻址。
     - 寻址方式算出的有效地址 $\text{EA}$：永远给出的都是数据的“首地址”（连续内存块中最小的地址）。
   - **教材习题**：计组教材 p170 第 13、22、29、32、33 题。
   - **文件系统核心原理**：
     - 磁盘索引节点 vs 内存索引节点。
     - 文件打开 (`open`)、删除 (`unlink`)、关闭 (`close`) 内核全流程（含权限检查、Dentry 查找、`nlink` / `i_count` / `f_count` 引用计数维护、系统打开文件表 `struct file` 及进程 `FD` 表分配）。

5. **计算机网络**：
   - 不携带数据的纯 ACK 报文段**不消耗序号**。

---

## 二、数据结构与算法 核心考点 (C语言/算法)

### 1. 邻接表转化成邻接矩阵
**算法思想**：遍历邻接表中每个顶点的边链表，将对应边的邻接矩阵元素 `Edge[i][p->adjvex]` 置为 1。

```c
void invert(mGraph *G1, Graph G2) {
    G1->numver = G2.numver;
    G1->numedg = G2.numedg;
    Node p;
    for (int i = 0; i < G2.numver; i++) {
        G1->verticle[i] = G2.adjlist[i].data;
        p = G2.adjlist[i].firstarc;
        for (; p != NULL; p = p->nextarc)
            G1->Edge[i][p->adjvex] = 1;
    }
} 
// 时间复杂度: O(V+E)，空间复杂度: O(V^2)
```

### 2. 邻接矩阵转化成邻接表
**算法思想**：遍历邻接矩阵，若 `Edge[i][j] != 0`，则动态分配边结点，通过头插法插入到顶点 `i` 的邻接表边链表中。

```c
void invert(mGraph G1, Graph *G2) {
    G2->numver = G1.numver;
    G2->numedg = G1.numedg;

    for (int i = 0; i < G2->numver; i++) {
        G2->adjlist[i].firstarc = NULL;
        G2->adjlist[i].data = G1.verticle[i];
    }

    for (int i = 0; i < G1.numver; i++)
        for (int j = 0; j < G1.numver; j++)
            if (G1.Edge[i][j] != 0) {
                Node p = (Node)malloc(sizeof(ANode));
                p->adjvex = j;
                p->nextarc = G2->adjlist[i].firstarc;
                G2->adjlist[i].firstarc = p;
            }
} 
// 时间复杂度: O(V^2)，空间复杂度: O(V^2)
```

---

## 三、动态规划 核心考点 (408考研主仓库)

### 1. 目标和 (Target Sum)
**题目描述**：给定非负整数数组 `nums` 和目标整数 `target`，为每个数字前面添加 `+` 或 `-`，求能构造出运算结果等于 `target` 的不同表达式数目。

**状态定义与方程**：
- 设正数和为 $P$，总和为 $Sum$，推导得 $P = \frac{Sum + Target}{2}$。
- $dp[i][j]$ 表示前 $i$ 个数字凑出目标和为 $j$ 的方案数。
- 转移方程：当 $nums[i] \le j$ 时，$dp[i][j] = dp[i-1][j] + dp[i-1][j - nums[i]]$；否则 $dp[i][j] = dp[i-1][j]$。

```c
Algorithm TargetSum(nums, target):
    N = length(nums)
    sum = 0
    FOR i = 1 TO N DO:
        sum = sum + nums[i]
    END FOR
    
    IF abs(target) > sum OR (sum + target) % 2 != 0 THEN
        RETURN 0
    END IF
    
    P = (sum + target) / 2
    定义二维整数数组 dp[N+1][P+1] = {0}
    dp[0][0] = 1
    
    FOR i = 1 TO N DO:
        FOR j = 0 TO P DO:
            IF nums[i] > j THEN
                dp[i][j] = dp[i-1][j]
            ELSE
                dp[i][j] = dp[i-1][j] + dp[i-1][j - nums[i]]
            END IF
        END FOR
    END FOR
    
    RETURN dp[N][P]
```

---

## 四、计算机网络 核心考点 (408考研主仓库)

### 1. 网络体系架构采用分层结构的原因与生活实例
- **核心原因**：
  1. 化繁为简：将复杂通信问题分解为局部问题。
  2. 各层独立：低耦合与透明性，底层实现变更不影响上层。
  3. 易于实现与维护：方便开发、调试与定位故障。
  4. 促进标准化：有利于制定国际标准与跨团队协作。
- **生活实例**：现代快递物流系统（用户层、网点接口层、干线运输层）。

### 2. IP 地址被称为“虚拟地址”的原因
1. **软件维护与逻辑属性**：由网络层软件维护分配，非硬件固化。
2. **屏蔽异构网络差异**：在各种物理/数据链路层不同的网络之上抽象出统一的逻辑 IP 网络。
3. **逻辑寻址与物理交付解耦**：用于网络层逻辑路由，交付时通过 ARP 转换为 MAC 地址。

### 3. 尽最大努力交付 (Best-Effort Delivery) 的含义
1. **四不保证**：不保证无差错、不保证按序、不保证不重复、不保证不丢失。
2. **尽力传输**：路由器不无故丢弃数据包，仅在校验和出错或缓存溢出时被迫丢弃。
3. **责任分工**：IP 层不负责重传，可靠传输交给上层 TCP 协议处理。

### 4. TCP 核心机制与连接管理
- **纯 ACK 报文**：不携带数据的纯 ACK 报文段不消耗序号。
- **三次握手/四次挥手考点**：
  - `SYN` 和 `FIN` 报文即便不携带数据也**消耗一个序号**。
  - 为什么客户端 `TIME-WAIT` 须等待 2MSL：保证最后一个 ACK 能够到达服务端，并防止已失效报文段干扰新连接。
