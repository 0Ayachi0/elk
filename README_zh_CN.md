# ELK 布局算法库 for Moonbit

[![Build Status](https://img.shields.io/github/actions/workflow/status/0Ayachi0/elk/ci.yml)](https://github.com/0Ayachi0/elk/actions) [![codecov](https://codecov.io/gh/0Ayachi0/elk/branch/main/graph/badge.svg)](https://codecov.io/gh/0Ayachi0/elk)

[English](README.md) | 简体中文

一个用 Moonbit 语言实现的综合 ELK (Eclipse Layout Kernel) 布局算法库，提供多种图布局算法，支持复杂的图结构布局需求。

## 🚀 核心特性

• 🎯 **核心算法** – 分层、力导向和固定布局算法  
• 🔧 **布局选项** – 可配置的间距、方向、排序和对齐  
• 🚀 **高级功能** – 边缘交叉最小化和增量式布局  
• 📊 **性能优化** – 高效的 hashmap 数据结构  
• 🔄 **增量式布局** – 动态节点和边添加支持  
• 🎨 **边缘路由** – 多种路由策略（直线、正交、折线、样条）  
• 📈 **统计与分析** – 全面的布局统计和分析  
• 🔍 **类型安全** – 具有健壮类型系统的泛型实现  
• 📦 **批量操作** – 高效处理复杂图结构  
• 🧪 **测试覆盖** – 所有功能的综合测试套件  
• 📚 **文档完善** – 详细的使用示例和 API 参考  

## 📥 安装

```bash
moon add 0Ayachi0/elk@0.1.0
```

## 🚀 使用指南

### 🔨 创建图结构
创建节点和边来形成您的图结构。

```moonbit
// 创建简单的图结构
let nodeA: ElkNode = ("A", None, @immut/list.Nil, @immut/list.Nil, 0.0, 0.0, 10.0, 10.0);
let nodeB: ElkNode = ("B", None, @immut/list.Nil, @immut/list.Nil, 0.0, 0.0, 10.0, 10.0);
let edgeAB: ElkEdge = ("e1", "A", "B", @immut/list.Nil);
let root: ElkNode = ("A", None, @immut/list.Nil, @immut/list.Cons(edgeAB, @immut/list.Nil), 0.0, 0.0, 10.0, 10.0);
```

### 🎯 基本布局
使用默认选项应用布局算法。

```moonbit
// 使用默认分层布局选项
let options: LayeredOptions = default_layered_options();
let result: ElkNode = layout_layered(root, options);
```

### 🔧 自定义布局选项
为特定需求配置布局参数。

```moonbit
// 创建自定义布局选项
let custom_options: LayeredOptions = (
    50.0,           // layer_spacing: 层间距
    30.0,           // node_spacing: 节点间距
    "DOWN",         // direction: 布局方向
    "DEGREE",       // node_sorting: 节点排序策略
    "ORTHOGONAL",   // edge_routing: 边缘路由策略
    "CENTER",       // alignment: 节点对齐
    true,           // consider_node_labels: 考虑节点标签
    true,           // consider_port_positions: 考虑端口位置
    1.0,            // aspect_ratio: 宽高比
    15              // iterations: 算法迭代次数
);

let result: ElkNode = layout_layered(root, custom_options);
```

### 🔄 通用布局 API
使用通用 API 在不同布局算法之间切换。

```moonbit
// 使用通用布局 API 进行不同算法
let layered_options: LayoutOptions = ("layered", options);
let layered_result: ElkNode = elk_layout(root, layered_options);

let force_options: LayoutOptions = ("force", options);
let force_result: ElkNode = elk_layout(root, force_options);

let fixed_options: LayoutOptions = ("fixed", options);
let fixed_result: ElkNode = elk_layout(root, fixed_options);
```

### 📈 增量式布局
使用增量式布局支持动态添加节点和边。

```moonbit
// 创建初始布局状态
let initial_state = (
    @hashmap.new(),  // 节点映射
    @hashmap.new(),  // 层映射
    @hashmap.new(),  // 层到节点列表映射
    @hashmap.new()   // 节点位置映射
);

// 增量添加节点
let nodeA: ElkNode = ("A", None, @immut/list.Nil, @immut/list.Nil, 0.0, 0.0, 10.0, 10.0);
let state1 = incremental_layout_add_node(initial_state, nodeA, 0);

let nodeB: ElkNode = ("B", None, @immut/list.Nil, @immut/list.Nil, 0.0, 0.0, 10.0, 10.0);
let state2 = incremental_layout_add_node(state1, nodeB, 1);

// 增量添加边
let edgeAB: ElkEdge = ("e1", "A", "B", @immut/list.Nil);
let state3 = incremental_layout_add_edge(state2, edgeAB);

// 更新布局
let options: LayeredOptions = default_layered_options();
let final_state = incremental_layout_update(state3, options);

// 获取位置信息
let positions = incremental_layout_get_positions(final_state);
```

### 🎨 边缘路由
应用不同的边缘路由策略以获得视觉吸引力。

```moonbit
let source_pos: Position = (0.0, 0.0);
let target_pos: Position = (100.0, 100.0);

// 直线路由
let straight_route = route_edge("A", "B", source_pos, target_pos, "STRAIGHT");

// 正交路由
let orthogonal_route = route_edge("A", "B", source_pos, target_pos, "ORTHOGONAL");

// 折线路由
let polyline_route = route_edge("A", "B", source_pos, target_pos, "POLYLINE");

// 样条路由
let spline_route = route_edge("A", "B", source_pos, target_pos, "SPLINES");
```

### 🏗️ 复杂图结构
使用高级功能处理复杂的多层图。

```moonbit
// 创建复杂多层图：A -> B,C -> D,E,F -> G
let nodeA: ElkNode = ("A", Some("开始"), @immut/list.Nil, @immut/list.Nil, 0.0, 0.0, 10.0, 10.0);
let nodeB: ElkNode = ("B", Some("处理1"), @immut/list.Nil, @immut/list.Nil, 0.0, 0.0, 10.0, 10.0);
let nodeC: ElkNode = ("C", Some("处理2"), @immut/list.Nil, @immut/list.Nil, 0.0, 0.0, 10.0, 10.0);
let nodeD: ElkNode = ("D", Some("子1"), @immut/list.Nil, @immut/list.Nil, 0.0, 0.0, 10.0, 10.0);
let nodeE: ElkNode = ("E", Some("子2"), @immut/list.Nil, @immut/list.Nil, 0.0, 0.0, 10.0, 10.0);
let nodeF: ElkNode = ("F", Some("子3"), @immut/list.Nil, @immut/list.Nil, 0.0, 0.0, 10.0, 10.0);
let nodeG: ElkNode = ("G", Some("结束"), @immut/list.Nil, @immut/list.Nil, 0.0, 0.0, 10.0, 10.0);

let edgeAB: ElkEdge = ("e1", "A", "B", @immut/list.Nil);
let edgeAC: ElkEdge = ("e2", "A", "C", @immut/list.Nil);
let edgeBD: ElkEdge = ("e3", "B", "D", @immut/list.Nil);
let edgeBE: ElkEdge = ("e4", "B", "E", @immut/list.Nil);
let edgeCD: ElkEdge = ("e5", "C", "D", @immut/list.Nil);
let edgeCF: ElkEdge = ("e6", "C", "F", @immut/list.Nil);
let edgeDG: ElkEdge = ("e7", "D", "G", @immut/list.Nil);
let edgeEG: ElkEdge = ("e8", "E", "G", @immut/list.Nil);
let edgeFG: ElkEdge = ("e9", "F", "G", @immut/list.Nil);

let nodeA_with_edges: ElkNode = ("A", Some("开始"), @immut/list.Nil, 
    @immut/list.Cons(edgeAB, @immut/list.Cons(edgeAC, @immut/list.Nil)), 0.0, 0.0, 10.0, 10.0);
let root: ElkNode = nodeA_with_edges;

let options: LayeredOptions = default_layered_options();
let result: ElkNode = layout_layered(root, options);
```

### 🏷️ 带标签的节点和边
使用带标签的节点和边进行丰富的图表示。

```moonbit
// 创建带标签的节点
let nodeA: ElkNode = ("A", Some("输入"), @immut/list.Nil, @immut/list.Nil, 0.0, 0.0, 10.0, 10.0);
let nodeB: ElkNode = ("B", Some("处理"), @immut/list.Nil, @immut/list.Nil, 0.0, 0.0, 10.0, 10.0);
let nodeC: ElkNode = ("C", Some("输出"), @immut/list.Nil, @immut/list.Nil, 0.0, 0.0, 10.0, 10.0);

// 创建带标签的边
let label1: ElkLabel = ("数据", 0.0, 0.0);
let label2: ElkLabel = ("结果", 0.0, 0.0);
let edgeAB: ElkEdge = ("e1", "A", "B", @immut/list.Cons(label1, @immut/list.Nil));
let edgeBC: ElkEdge = ("e2", "B", "C", @immut/list.Cons(label2, @immut/list.Nil));

let nodeA_with_edges: ElkNode = ("A", Some("输入"), @immut/list.Nil, 
    @immut/list.Cons(edgeAB, @immut/list.Nil), 0.0, 0.0, 10.0, 10.0);
let root: ElkNode = nodeA_with_edges;

let options: LayeredOptions = default_layered_options();
let result: ElkNode = layout_layered(root, options);
```

## 📋 数据结构

### ElkNode
```moonbit
typealias (String, Option[String], @immut/list.T[ElkPort], @immut/list.T[ElkEdge], Float, Float, Float, Float) as ElkNode
// (id, label, ports, edges, x, y, width, height)
```

### ElkEdge
```moonbit
typealias (String, String, String, @immut/list.T[ElkLabel]) as ElkEdge
// (id, source, target, labels)
```

### ElkPort
```moonbit
typealias (String, Option[String], Float, Float, Float, Float) as ElkPort
// (id, label, x, y, width, height)
```

### ElkLabel
```moonbit
typealias (String, Float, Float) as ElkLabel
// (text, x, y)
```

## 🎛️ 布局选项

### LayeredOptions
```moonbit
typealias (
    Float,                // layer_spacing: 层间距
    Float,                // node_spacing: 节点间距
    LayoutDirection,      // direction: 布局方向
    NodeSortingStrategy,  // node_sorting: 节点排序策略
    EdgeRoutingStrategy,  // edge_routing: 边缘路由策略
    Alignment,            // alignment: 节点对齐
    Bool,                 // consider_node_labels: 考虑节点标签
    Bool,                 // consider_port_positions: 考虑端口位置
    Float,                // aspect_ratio: 布局宽高比
    Int                   // iterations: 算法迭代次数
) as LayeredOptions
```

### 支持的枚举值

**布局方向 (LayoutDirection)**:
- `"UP"`: 向上布局
- `"DOWN"`: 向下布局
- `"LEFT"`: 向左布局
- `"RIGHT"`: 向右布局

**节点排序策略 (NodeSortingStrategy)**:
- `"NONE"`: 不排序
- `"INPUT_ORDER"`: 按输入顺序
- `"DEGREE"`: 按度数排序
- `"NAME"`: 按名称排序

**边缘路由策略 (EdgeRoutingStrategy)**:
- `"STRAIGHT"`: 直线路由
- `"ORTHOGONAL"`: 正交路由
- `"POLYLINE"`: 折线路由
- `"SPLINES"`: 样条路由

**对齐方式 (Alignment)**:
- `"TOP"`: 顶部对齐
- `"BOTTOM"`: 底部对齐
- `"LEFT"`: 左对齐
- `"RIGHT"`: 右对齐
- `"CENTER"`: 居中对齐
- `"NONE"`: 不对齐

## 📊 完整 API 参考

### 核心布局算法
- `layout_layered(root, options)` - 应用分层布局算法
- `layout_force(root, options)` - 应用力导向布局算法
- `layout_fixed(root, options)` - 应用固定布局算法
- `elk_layout(root, options)` - 通用布局 API

### 增量式布局
- `incremental_layout_add_node(state, node, layer)` - 向布局添加节点
- `incremental_layout_add_edge(state, edge)` - 向布局添加边
- `incremental_layout_update(state, options)` - 更新布局位置
- `incremental_layout_get_positions(state)` - 获取节点位置

### 边缘路由
- `route_edge(source, target, source_pos, target_pos, strategy)` - 使用策略路由边

### 工具函数
- `default_layered_options()` - 获取默认布局选项
- `list_length(lst)` - 获取列表长度
- `reverse_list(lst)` - 反转列表

## 📈 性能特征

| 操作 | 时间复杂度 |
|------|------------|
| `layout_layered()` | O(V + E) 基本，O(V²) 交叉最小化 |
| `layout_force()` | O(V² × iterations) |
| `layout_fixed()` | O(1) |
| `incremental_layout_add_node()` | O(1) |
| `incremental_layout_add_edge()` | O(V) |
| `incremental_layout_update()` | O(V + E) |
| `route_edge()` | O(1) |
| `list_length()` | O(n) |
| `reverse_list()` | O(n) |

## 🏗️ 算法详情

### 分层布局算法
1. **分层阶段**: 使用 BFS 算法将节点分配到不同层
2. **排序阶段**: 根据指定策略对每层节点进行排序
3. **交叉最小化**: 使用贪心算法减少边缘交叉
4. **坐标分配**: 根据间距和对齐方式分配最终坐标

### 力导向布局算法
1. **初始化**: 为所有节点分配初始位置
2. **力计算**: 计算排斥力和吸引力
3. **位置更新**: 根据合力更新节点位置
4. **迭代优化**: 重复上述过程直到收敛

### 增量式布局
- 支持动态添加/删除节点和边
- 保持现有布局的稳定性
- 高效的状态管理

## 🧪 测试覆盖

项目包含全面的测试用例：
- 基本布局功能测试
- 复杂图结构测试
- 边缘交叉最小化测试
- 增量式布局测试
- 边缘路由测试
- 性能测试
- 不同布局方向测试

## 🚀 构建和运行

```bash
# 构建项目
moon build

# 运行测试
moon test
```

## 📦 依赖

- `moonbitlang/core`: 提供基础数据结构支持
- `moonbitlang/core/hashmap`: 高效的哈希映射实现
- `moonbitlang/core/immut/list`: 不可变列表实现

## 📜 许可证

本项目采用 MIT 许可证。详情请参阅 LICENSE 文件。

## 📢 联系与支持

• MoonBit 社区: moonbit-community  
• GitHub Issues: 报告问题  

## 📝 更新日志

### v1.0.0
- ✅ 实现基础 Layered、Fixed、Force 布局算法
- ✅ 支持多种布局选项和配置
- ✅ 实现边缘交叉最小化算法
- ✅ 完善边缘路由算法
- ✅ 实现增量式布局功能
- ✅ 添加全面的测试覆盖
- ✅ 完善文档和使用示例

👋 如果您喜欢这个项目，请给它一个 ⭐！祝编码愉快！🚀 