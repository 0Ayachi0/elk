# ELK Layout Library for Moonbit

[![Build Status](https://img.shields.io/github/actions/workflow/status/0Ayachi0/elk/ci.yml)](https://github.com/0Ayachi0/elk/actions) [![codecov](https://codecov.io/gh/0Ayachi0/elk/branch/main/graph/badge.svg)](https://codecov.io/gh/0Ayachi0/elk)

English | [简体中文](README_zh_CN.md)

A comprehensive ELK (Eclipse Layout Kernel) layout algorithm library implemented in MoonBit that provides multiple graph layout algorithms for complex graph structure layout requirements.

## 🚀 Key Features

• 🎯 **Core Algorithms** – Layered, Force-directed, and Fixed layout algorithms  
• 🔧 **Layout Options** – Configurable spacing, direction, sorting, and alignment  
• 🚀 **Advanced Features** – Edge crossing minimization and incremental layout  
• 📊 **Performance Optimized** – Efficient hashmap data structures  
• 🔄 **Incremental Layout** – Dynamic node and edge addition support  
• 🎨 **Edge Routing** – Multiple routing strategies (straight, orthogonal, polyline, splines)  
• 📈 **Statistics & Analysis** – Comprehensive layout statistics and analysis  
• 🔍 **Type Safety** – Generic implementation with robust type system  
• 📦 **Batch Operations** – Efficiently handle complex graph structures  
• 🧪 **Test Coverage** – Comprehensive test suite for all features  
• 📚 **Documentation** – Detailed usage examples and API reference  

## 📥 Installation

```bash
moon add 0Ayachi0/elk@0.1.0
```

## 🚀 Usage Guide

### 🔨 Create Graph Structure
Create nodes and edges to form your graph structure.

```moonbit
// Create simple graph structure
let nodeA: ElkNode = ("A", None, @immut/list.Nil, @immut/list.Nil, 0.0, 0.0, 10.0, 10.0);
let nodeB: ElkNode = ("B", None, @immut/list.Nil, @immut/list.Nil, 0.0, 0.0, 10.0, 10.0);
let edgeAB: ElkEdge = ("e1", "A", "B", @immut/list.Nil);
let root: ElkNode = ("A", None, @immut/list.Nil, @immut/list.Cons(edgeAB, @immut/list.Nil), 0.0, 0.0, 10.0, 10.0);
```

### 🎯 Basic Layout
Apply layout algorithms with default options.

```moonbit
// Use default layered layout options
let options: LayeredOptions = default_layered_options();
let result: ElkNode = layout_layered(root, options);
```

### 🔧 Custom Layout Options
Configure layout parameters for specific requirements.

```moonbit
// Create custom layout options
let custom_options: LayeredOptions = (
    50.0,           // layer_spacing: Layer spacing
    30.0,           // node_spacing: Node spacing
    "DOWN",         // direction: Layout direction
    "DEGREE",       // node_sorting: Node sorting strategy
    "ORTHOGONAL",   // edge_routing: Edge routing strategy
    "CENTER",       // alignment: Node alignment
    true,           // consider_node_labels: Consider node labels
    true,           // consider_port_positions: Consider port positions
    1.0,            // aspect_ratio: Aspect ratio
    15              // iterations: Algorithm iterations
);

let result: ElkNode = layout_layered(root, custom_options);
```

### 🔄 Universal Layout API
Use the universal API to switch between different layout algorithms.

```moonbit
// Use universal layout API for different algorithms
let layered_options: LayoutOptions = ("layered", options);
let layered_result: ElkNode = elk_layout(root, layered_options);

let force_options: LayoutOptions = ("force", options);
let force_result: ElkNode = elk_layout(root, force_options);

let fixed_options: LayoutOptions = ("fixed", options);
let fixed_result: ElkNode = elk_layout(root, fixed_options);
```

### 📈 Incremental Layout
Dynamically add nodes and edges with incremental layout support.

```moonbit
// Create initial layout state
let initial_state = (
    @hashmap.new(),  // Node mapping
    @hashmap.new(),  // Layer mapping
    @hashmap.new(),  // Layer to node list mapping
    @hashmap.new()   // Node position mapping
);

// Add nodes incrementally
let nodeA: ElkNode = ("A", None, @immut/list.Nil, @immut/list.Nil, 0.0, 0.0, 10.0, 10.0);
let state1 = incremental_layout_add_node(initial_state, nodeA, 0);

let nodeB: ElkNode = ("B", None, @immut/list.Nil, @immut/list.Nil, 0.0, 0.0, 10.0, 10.0);
let state2 = incremental_layout_add_node(state1, nodeB, 1);

// Add edges incrementally
let edgeAB: ElkEdge = ("e1", "A", "B", @immut/list.Nil);
let state3 = incremental_layout_add_edge(state2, edgeAB);

// Update layout
let options: LayeredOptions = default_layered_options();
let final_state = incremental_layout_update(state3, options);

// Get position information
let positions = incremental_layout_get_positions(final_state);
```

### 🎨 Edge Routing
Apply different edge routing strategies for visual appeal.

```moonbit
let source_pos: Position = (0.0, 0.0);
let target_pos: Position = (100.0, 100.0);

// Straight routing
let straight_route = route_edge("A", "B", source_pos, target_pos, "STRAIGHT");

// Orthogonal routing
let orthogonal_route = route_edge("A", "B", source_pos, target_pos, "ORTHOGONAL");

// Polyline routing
let polyline_route = route_edge("A", "B", source_pos, target_pos, "POLYLINE");

// Spline routing
let spline_route = route_edge("A", "B", source_pos, target_pos, "SPLINES");
```

### 🏗️ Complex Graph Structures
Handle complex multi-layer graphs with advanced features.

```moonbit
// Create complex multi-layer graph: A -> B,C -> D,E,F -> G
let nodeA: ElkNode = ("A", Some("Start"), @immut/list.Nil, @immut/list.Nil, 0.0, 0.0, 10.0, 10.0);
let nodeB: ElkNode = ("B", Some("Process1"), @immut/list.Nil, @immut/list.Nil, 0.0, 0.0, 10.0, 10.0);
let nodeC: ElkNode = ("C", Some("Process2"), @immut/list.Nil, @immut/list.Nil, 0.0, 0.0, 10.0, 10.0);
let nodeD: ElkNode = ("D", Some("Sub1"), @immut/list.Nil, @immut/list.Nil, 0.0, 0.0, 10.0, 10.0);
let nodeE: ElkNode = ("E", Some("Sub2"), @immut/list.Nil, @immut/list.Nil, 0.0, 0.0, 10.0, 10.0);
let nodeF: ElkNode = ("F", Some("Sub3"), @immut/list.Nil, @immut/list.Nil, 0.0, 0.0, 10.0, 10.0);
let nodeG: ElkNode = ("G", Some("End"), @immut/list.Nil, @immut/list.Nil, 0.0, 0.0, 10.0, 10.0);

let edgeAB: ElkEdge = ("e1", "A", "B", @immut/list.Nil);
let edgeAC: ElkEdge = ("e2", "A", "C", @immut/list.Nil);
let edgeBD: ElkEdge = ("e3", "B", "D", @immut/list.Nil);
let edgeBE: ElkEdge = ("e4", "B", "E", @immut/list.Nil);
let edgeCD: ElkEdge = ("e5", "C", "D", @immut/list.Nil);
let edgeCF: ElkEdge = ("e6", "C", "F", @immut/list.Nil);
let edgeDG: ElkEdge = ("e7", "D", "G", @immut/list.Nil);
let edgeEG: ElkEdge = ("e8", "E", "G", @immut/list.Nil);
let edgeFG: ElkEdge = ("e9", "F", "G", @immut/list.Nil);

let nodeA_with_edges: ElkNode = ("A", Some("Start"), @immut/list.Nil, 
    @immut/list.Cons(edgeAB, @immut/list.Cons(edgeAC, @immut/list.Nil)), 0.0, 0.0, 10.0, 10.0);
let root: ElkNode = nodeA_with_edges;

let options: LayeredOptions = default_layered_options();
let result: ElkNode = layout_layered(root, options);
```

### 🏷️ Labeled Nodes and Edges
Work with labeled nodes and edges for rich graph representation.

```moonbit
// Create nodes with labels
let nodeA: ElkNode = ("A", Some("Input"), @immut/list.Nil, @immut/list.Nil, 0.0, 0.0, 10.0, 10.0);
let nodeB: ElkNode = ("B", Some("Process"), @immut/list.Nil, @immut/list.Nil, 0.0, 0.0, 10.0, 10.0);
let nodeC: ElkNode = ("C", Some("Output"), @immut/list.Nil, @immut/list.Nil, 0.0, 0.0, 10.0, 10.0);

// Create edges with labels
let label1: ElkLabel = ("data", 0.0, 0.0);
let label2: ElkLabel = ("result", 0.0, 0.0);
let edgeAB: ElkEdge = ("e1", "A", "B", @immut/list.Cons(label1, @immut/list.Nil));
let edgeBC: ElkEdge = ("e2", "B", "C", @immut/list.Cons(label2, @immut/list.Nil));

let nodeA_with_edges: ElkNode = ("A", Some("Input"), @immut/list.Nil, 
    @immut/list.Cons(edgeAB, @immut/list.Nil), 0.0, 0.0, 10.0, 10.0);
let root: ElkNode = nodeA_with_edges;

let options: LayeredOptions = default_layered_options();
let result: ElkNode = layout_layered(root, options);
```

## 📋 Data Structures

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

## 🎛️ Layout Options

### LayeredOptions
```moonbit
typealias (
    Float,                // layer_spacing: Layer spacing
    Float,                // node_spacing: Node spacing
    LayoutDirection,      // direction: Layout direction
    NodeSortingStrategy,  // node_sorting: Node sorting strategy
    EdgeRoutingStrategy,  // edge_routing: Edge routing strategy
    Alignment,            // alignment: Node alignment
    Bool,                 // consider_node_labels: Consider node labels
    Bool,                 // consider_port_positions: Consider port positions
    Float,                // aspect_ratio: Layout aspect ratio
    Int                   // iterations: Algorithm iterations
) as LayeredOptions
```

### Supported Enum Values

**Layout Direction (LayoutDirection)**:
- `"UP"`: Upward layout
- `"DOWN"`: Downward layout
- `"LEFT"`: Leftward layout
- `"RIGHT"`: Rightward layout

**Node Sorting Strategy (NodeSortingStrategy)**:
- `"NONE"`: No sorting
- `"INPUT_ORDER"`: Sort by input order
- `"DEGREE"`: Sort by degree
- `"NAME"`: Sort by name

**Edge Routing Strategy (EdgeRoutingStrategy)**:
- `"STRAIGHT"`: Straight routing
- `"ORTHOGONAL"`: Orthogonal routing
- `"POLYLINE"`: Polyline routing
- `"SPLINES"`: Spline routing

**Alignment (Alignment)**:
- `"TOP"`: Top alignment
- `"BOTTOM"`: Bottom alignment
- `"LEFT"`: Left alignment
- `"RIGHT"`: Right alignment
- `"CENTER"`: Center alignment
- `"NONE"`: No alignment

## 📊 Complete API Reference

### Core Layout Algorithms
- `layout_layered(root, options)` - Apply layered layout algorithm
- `layout_force(root, options)` - Apply force-directed layout algorithm
- `layout_fixed(root, options)` - Apply fixed layout algorithm
- `elk_layout(root, options)` - Universal layout API

### Incremental Layout
- `incremental_layout_add_node(state, node, layer)` - Add node to layout
- `incremental_layout_add_edge(state, edge)` - Add edge to layout
- `incremental_layout_update(state, options)` - Update layout positions
- `incremental_layout_get_positions(state)` - Get node positions

### Edge Routing
- `route_edge(source, target, source_pos, target_pos, strategy)` - Route edge with strategy

### Utility Functions
- `default_layered_options()` - Get default layout options
- `list_length(lst)` - Get list length
- `reverse_list(lst)` - Reverse list

## 📈 Performance Characteristics

| Operation | Time Complexity |
|-----------|----------------|
| `layout_layered()` | O(V + E) basic, O(V²) crossing minimization |
| `layout_force()` | O(V² × iterations) |
| `layout_fixed()` | O(1) |
| `incremental_layout_add_node()` | O(1) |
| `incremental_layout_add_edge()` | O(V) |
| `incremental_layout_update()` | O(V + E) |
| `route_edge()` | O(1) |
| `list_length()` | O(n) |
| `reverse_list()` | O(n) |

## 🏗️ Algorithm Details

### Layered Layout Algorithm
1. **Layering Phase**: Use BFS algorithm to assign nodes to different layers
2. **Sorting Phase**: Sort nodes in each layer according to specified strategy
3. **Crossing Minimization**: Use greedy algorithm to reduce edge crossings
4. **Coordinate Assignment**: Assign final coordinates based on spacing and alignment

### Force-directed Layout Algorithm
1. **Initialization**: Assign initial positions to all nodes
2. **Force Calculation**: Calculate repulsive and attractive forces
3. **Position Update**: Update node positions based on combined forces
4. **Iterative Optimization**: Repeat process until convergence

### Incremental Layout
- Support dynamic addition/removal of nodes and edges
- Maintain layout stability for existing elements
- Efficient state management

## 🧪 Test Coverage

The project includes comprehensive test cases:
- Basic layout functionality tests
- Complex graph structure tests
- Edge crossing minimization tests
- Incremental layout tests
- Edge routing tests
- Performance tests
- Different layout direction tests

## 🚀 Build and Run

```bash
# Build project
moon build

# Run tests
moon test
```

## 📦 Dependencies

- `moonbitlang/core`: Provides basic data structure support
- `moonbitlang/core/hashmap`: Efficient hash map implementation
- `moonbitlang/core/immut/list`: Immutable list implementation

## 📜 License

This project is licensed under the MIT License. See LICENSE for details.

## 📢 Contact & Support

• MoonBit Community: moonbit-community  
• GitHub Issues: Report an Issue  

## 📝 Changelog

### v1.0.0
- ✅ Implemented basic Layered, Fixed, Force layout algorithms
- ✅ Support for multiple layout options and configurations
- ✅ Implemented edge crossing minimization algorithm
- ✅ Enhanced edge routing algorithms
- ✅ Implemented incremental layout functionality
- ✅ Added comprehensive test coverage
- ✅ Complete documentation and usage examples

👋 If you like this project, give it a ⭐! Happy coding! 🚀