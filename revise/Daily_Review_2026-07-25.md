# 2026-07-25 每日复习汇总与考点整理

> 生成时间：2026-07-25  
> 包含模块：数据结构与算法 (真题算法与DP模板)、计算机网络 (TCP/IP与核心简答)、408考研错题与待办

---

## 一、今日错题与待办事项 (Daily Error & TODO)

1. **概率论**：
   - 课本 p221 第 6.10 题
   - 课本 p220 第 6.9 题
   - 课本 p208 第 5.4 题
   - 课本 p109 画圈那题
   - 课本 p119 最后一题

2. **高等数学**：
   - 课本 p264 第 1 题
   - 课本 p264 第 2 题（用柱坐标书写）
   - 课本 p265 第 3 题

3. **线性代数**：
   - 线性代数 880 p67 第 1 题
   - **(重点)** 左乘矩阵 $A$ 决定了“行”的命运

---

## 二、数据结构与算法 核心考点 (C语言/算法)

### 1. 2023年统考真题：有向图 K 顶点判定 (邻接矩阵)
**题目描述**：有向图 G 采用邻接矩阵存储，将图中出度大于入度的顶点称为 K 顶点。要求输出 G 中所有的 K 顶点，并返回 K 顶点的个数。

```c
int func(MGraph G) {
    int count = 0;
    for (int i = 0; i < G.numver; i++) {
        int indegree = 0, outdegree = 0;
        for (int j = 0; j < G.numver; j++) {
            outdegree += G.Edge[i][j];
            indegree += G.Edge[j][i];
        }
        if (outdegree > indegree) {
            printf("%c", G.verticle[i]);
            count++;
        }
    }
    return count;
} 
// 时间复杂度: O(V^2)，空间复杂度: O(1)
```

### 2. 有向图入度与出度统计 (邻接表)
**题目描述**：有向图采用邻接表存储，计算每个顶点的入度和出度，将其存储到两个数组中。

```c
void func(Graph G, int inres[], int outres[]) {
    for (int i = 0; i < G.numver; i++) {
        inres[i] = 0;
        outres[i] = 0;
    }
    for (int i = 0; i < G.numver; i++) {
        Node p = G.adjlist[i].firstarc;
        for (; p != NULL; p = p->nextarc) {
            outres[i]++;
            inres[p->adjvex]++;
        }
    }
} 
// 时间复杂度: O(V+E)，空间复杂度: O(V)
```

### 3. 动态规划 (Dynamic Programming) 经典问题与算法模板

#### (1) 0-1 背包问题
在不超过背包承重 $W$ 的前提下，选择价值最大的物品组合。每个物品只能选 0 次或 1 次。  
`dp[i][j]` 表示面对前 $i$ 个物品，容量为 $j$ 时的最大总价值。

```c
Algorithm ZeroOneKnapsack(w, v, N, W):
    定义二维数组 dp[N+1][W+1] = {0}
    FOR i = 1 TO N DO:
        FOR j = 1 TO W DO:
            IF w[i] > j THEN
                dp[i][j] = dp[i-1][j]
            ELSE
                dp[i][j] = max(dp[i-1][j], v[i] + dp[i-1][j - w[i]])
            END IF
        END FOR
    END FOR
    RETURN dp[N][W]
```

#### (2) 分割等和子集
判断正整数数组 nums 能否分割成两个元素和相等的子集（转化为背包容量为 `Sum/2` 的 0-1 背包可行性问题）。

```c
Algorithm CanPartition(nums):
    N = length(nums)
    sum = 0
    FOR i = 1 TO N DO: sum = sum + nums[i]
    IF sum % 2 != 0 THEN RETURN false
    Target = sum / 2
    
    定义二维布尔数组 dp[N+1][Target+1] = {false}
    FOR i = 0 TO N DO: dp[i][0] = true
    
    FOR i = 1 TO N DO:
        FOR j = 1 TO Target DO:
            IF nums[i] > j THEN
                dp[i][j] = dp[i-1][j]
            ELSE
                dp[i][j] = dp[i-1][j] OR dp[i-1][j - nums[i]]
            END IF
        END FOR
    END FOR
    RETURN dp[N][Target]
```

#### (3) 最后一块石头的重量 II
两两碰撞粉碎石头，求最后剩下一块石头的最小重量（转化为将石头分为两堆，使两堆重量尽可能接近 `TotalWeight/2`）。

---

## 三、计算机网络 核心考点 (408考研主仓库)

### 1. TCP 首部结构与字段
- **首部长度**：标准固定 20 字节，含选项最多 60 字节（`Data Offset` 字段 4 位，单位 4 字节）。
- **序号 (seq)**：占 32 位，本报文段所发送数据的首字节序号，解决乱序。
- **确认号 (ack)**：占 32 位，期望收到对方下一个报文段的首字节序号，累加确认解决丢包。
- **窗口大小 (Window Size)**：占 16 位，用于流量控制。
- **6大控制位**：`URG` (紧急), `ACK` (确认号有效), `PSH` (推送上交), `RST` (重置连接), `SYN` (同步建连), `FIN` (终止释放)。

### 2. TCP 三次握手
- **过程**：
  1. 客户端 -> 服务端：`SYN=1, seq=x` (状态: `SYN-SENT`)
  2. 服务端 -> 客户端：`SYN=1, ACK=1, seq=y, ack=x+1` (状态: `SYN-RCVD`)
  3. 客户端 -> 服务端：`ACK=1, seq=x+1, ack=y+1` (状态: `ESTABLISHED`)
- **核心考点：为什么必须三次握手而不能是两次？**  
  防止因**滞留的历史连接请求 (旧 SYN 报文)** 延迟到达服务端而建立无效的“半连接/幽灵连接”，白白浪费服务端资源。

### 3. TCP 四次挥手
- **过程**：
  1. 客户端 -> 服务端：`FIN=1, seq=u` (状态: `FIN-WAIT-1`)
  2. 服务端 -> 客户端：`ACK=1, seq=v, ack=u+1` (状态: `CLOSE-WAIT`；客户端进入 `FIN-WAIT-2`)
  3. 服务端 -> 客户端：`FIN=1, ACK=1, seq=w, ack=u+1` (状态: `LAST-ACK`)
  4. 客户端 -> 服务端：`ACK=1, seq=u+1, ack=w+1` (服务端进入 `CLOSED`；客户端进入 `TIME-WAIT` 等待 2MSL)
- **核心考点：为什么 TIME-WAIT 状态必须等待 2MSL？**  
  1. **保证最后一个 ACK 报文能够可靠到达服务端**（防 ACK 丢失致服务端重传 FIN）。  
  2. **防止“已失效的报文段”出现在下一个新连接中**（使旧连接的所有报文段自然消失）。

### 4. 计算机网络经典简答题
1. **能否用一个大交换机代替全部路由器？**  
   - 无法代替。交换机工作在数据链路层（依 MAC 地址转发），路由器工作在网络层（依 IP 地址转发）。
   - 交换机无法隔离广播域，替换会导致严重的广播风暴。
   - 交换机只能连接同构网络，无法实现异构网络互连。

2. **为什么 IP 地址又称为虚拟地址？**  
   - IP 地址由网络层软件维护和分配，是逻辑标识而非固化在硬件中的物理地址。
   - IP 协议在异构物理网络之上抽象出统一的逻辑 (虚拟) 网络。
   - IP 地址解耦了逻辑寻址与物理交付，最终需转化为 MAC 地址交付。

3. **虚拟分组的含义**  
   - 网络层通过定义统一格式的 IP 数据报，抽象出统一数据传输单元，屏蔽底层差异。
   - 对底层物理网络透明，二层节点（如交换机）转发时只关注 MAC 地址，对包内封装的 IP 数据报不可见。
