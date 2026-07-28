# 2026-07-28 每日复习汇总与考点整理

> 生成时间：2026-07-28  
> 包含模块：408考研错题与待办、数据结构与算法 (图算法与动态规划)、计算机网络 (核心简答、IP/ARP/ICMP协议、以太网与拥塞控制)、运维集群架构 (Cilium与Envoy Gateway)、英语真题 (2012 Text 1 & Text 2)

---

## 一、今日错题与待办事项 (Daily Error & TODO)

1. **数学与考研错题 (来自 daily-error.md)**：
   - **概率论**：习题册 p44 选择第 1、3、4 大题第 2、3、4 题
   - **线性代数**：880 p73 第 7 题

2. **计算机网络与数据结构待办/重点知识点 (来自 daily-error.md & TODO标记)**：
   - **以太网帧与载荷限制**：
     - 以太网帧头（14字节）+ 帧尾（4字节）= 18字节。最小帧长 64 字节 $\rightarrow$ 数据载荷（IP分组）最小不低于 $64 - 18 = 46$ 字节。
   - **TCP 慢开始定义**：
     - 每收到 1 个确认新数据的 ACK，$\text{cwnd} = \text{cwnd} + 1\text{ MSS}$。
   - **数据结构与网络 归档待办**：
     - 邻接表转化为邻接矩阵
     - 设计算法判断无向图是否是一棵树
     - 描述五层协议的网络体系结构要点及各层功能
     - 说明 IP、ARP、ICMP 的作用
     - IP 地址的主要特点
     - IP 地址与 MAC 地址的区别及为何使用两种不同地址

3. **动态规划算法待办/重点题目 (来自 408/dp.md TODO标记)**：
   - 目标和问题 (Target Sum)
   - 一和零 / 0和1问题 (Ones and Zeroes)

---

## 二、计算机网络与图算法 核心考点 (C语言/算法仓库)

### 1. 计算机网络 核心概念与协议 (简答整理)
- **五层协议体系结构**：物理层(bit/比特流)、数据链路层(frame/组装成帧)、网络层(packet/路由选择/IP)、运输层(segment/进程通信/TCP/UDP)、应用层(为用户应用服务)。
- **IP、ARP、ICMP 的作用**：
  - **IP**：屏蔽异构网络细节，逻辑上构成统一的虚拟互联网。
  - **ARP**：实现 IP 地址到 MAC 地址的动态映射与转换。
  - **ICMP**：报告差错和异常情况，提高 IP 数据报交付成功率。
- **IP 地址的主要特点**：分等级结构（网络前缀+主机号）、标识设备接口、相同前缀构成同一逻辑网络、各网络地位平等。
- **IP 地址与 MAC 地址的区别与联系**：
  - MAC 地址属于数据链路层/物理层（硬件物理地址）；IP 地址属于网络层（逻辑地址）。
  - 使用两种地址能屏蔽异构网络硬件差异，配合 ARP 自动映射实现全网统一寻址与通信。

### 2. 数据结构 图算法
- **最小生成树 (MST)**：
  - **Kruskal 算法**：贪心策略按边权从小到大选择，并查集防环，适合稀疏图 $O(E \log E)$。
  - **Prim 算法**：从顶点出发扩展生成树，适合稠密图 $O(V^2)$ / 堆优化 $O(E \log V)$。
- **关键算法设计与应用**：
  - 邻接表与邻接矩阵的相互转换算法。
  - 基于 DFS 算法判断无向图连通性与边数 ($V - E = 1$) 以验证是否为一棵树。

---

## 三、动态规划 核心考点 (408考研主仓库)

### 1. 目标和问题 (`[TODO标记已归档]`)
- **题目分析**：在数组 `nums` 中加 `+`/`-` 凑成 `target`，等价于选一个正数子集 $P$，使得 $P = (\text{sum} + \text{target}) / 2$。
- **状态转移方程**：
  $$\text{dp}[i][j] = \text{dp}[i-1][j] + \text{dp}[i-1][j - \text{nums}[i]]$$
  - 边界：$\text{dp}[0][0] = 1$。`j` 需从 0 开始遍历以处理包含 0 的元素。

### 2. 一和零 (0和1) 背包问题 (`[TODO标记已归档]`)
- **题目分析**：二维背包问题，背包有两个限制维度（最多 $m$ 个 0 和 $n$ 个 1）。
- **状态转移方程**：
  $$\text{dp}[i][j][k] = \max(\text{dp}[i-1][j][k], \text{dp}[i-1][j - \text{zeros}][k - \text{ones}] + 1)$$

---

## 四、运维环境与系统架构 (408考研主仓库)

### 1. Cilium 网络与 eBPF
- **核心组件**：
  - **Cilium Agent**：运行于每个节点（DaemonSet），监听 K8s API，将网络/安全策略编译为 eBPF 字节码并注入内核 Hooks。
  - **Cilium Operator**：单例运行，负责集群级 IPAM、垃圾回收和 Cluster Mesh 同步。
  - **Hubble**：基于 eBPF 的网络可观测性与 L7 指标拓扑平台。

### 2. Envoy Gateway
- **部署与架构**：结合 Gateway API (v1) 与 Envoy Proxy 实现云原生入口网关，基于 CRD (`GatewayClass`, `EnvoyProxy`, `Gateway`, `HTTPRoute`) 管理集群统一入口与跨节点路由。

---

## 五、英语真题复习 (2012 Text 1 & Text 2)

### 1. Text 1: 同伴压力与社会疗法 (Peer Pressure & Social Cure)
- **核心观点**：Tina Rosenberg 提出的“Social Cure”（利用群体动力引导正面行为），公共卫生倡导者应向广告商学习应用 peer pressure。
- **题目要点**：
  - *peer pressure* 传统上常带来 *undesirable behaviors*。
  - *imitation of behaviors* 往往在无意识中发生 (*occurs without our realizing it*)。

### 2. Text 2: Entergy 公司与 Vermont 州核电站争议
- **核心观点**：Entergy 公司违背长期承诺（*reneging on commitments* = *dishonoring*），引发关于州政府对核能监管权力边界的法律诉讼 (*limits of states' power over nuclear issues*)。
- **题目要点**：
  - 词汇：*reneging on* $\rightarrow$ *dishonoring*。
  - 推理：Entergy 公司的失信行为可能影响其在其他地区的核电站申请 (*business elsewhere might be affected*)。
