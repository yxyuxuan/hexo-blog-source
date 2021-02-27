---
title: React中diff算法
date: 2020-08-17 10:34:47
tags: React
categories: React
---

> Virtual DOM是什么？diff算法是什么？diff算法是怎样让Virtual DOM快速实现页面渲染的?

### 一. Virtual DOM（虚拟DOM）

Virtual DOM（虚拟DOM）：是用JS对象来描述真实的DOM结构。当调用setState时会创建新的虚拟DOM树，新旧虚拟DOM树通过diff算法快速找出差异，将差异更新到真实的DOM树上。

### 二. diff算法

#### 1. 传统diff算法

​	传统diff算法通过循环递归对节点进行一次对比，效率很低，算法复杂度达到O(n^3)，n是树中节点的总数。

#### 2. React中的diff算法

​	React 通过制定策略，将 O(n^3) 复杂度的问题转换成 O(n) 复杂度的问题。

​	diff 策略：

​	(1). Web UI 中 DOM 节点跨层级的移动操作特别少，可以忽略不计。

​	(2). 拥有相同类的两个组件将会生成相似的树形结构，拥有不同类的两个组件将会生成不同的树形结构。

​	(3). 对于同一层级的一组子节点，它们可以通过唯一 id 进行区分。

   基于以上三个前提策略，React 分别对 tree diff、component diff 以及 element diff 进行算法优化，事实也证 明这三个前提策略是合理且准确的，它保证了整体界面构建的性能。

### 三. tree diff

React 对树的算法进行了简洁明了的优化，即对树进行分层比较，两棵树只会对同一层次的节点进行比较。

既然 DOM 节点跨层级的移动操作少到可以忽略不计，针对这一现象，React 通过 updateDepth 对 Virtual DOM 树进行层级控制，只会对相同颜色方框内的 DOM 节点进行比较，即同一个父节点下的所有子节点。当发现节点已经不存在，则该节点及其子节点会被完全删除掉，不会用于进一步的比较。这样只需要对树进行一次遍历，便能完成整个 DOM 树的比较。

由于 React 只会简单的考虑同层级节点的位置变换，而对于不同层级的节点，只有创建和删除操作。

### 四. component diff

因为react通过组件化开发，在对比组件差异上也采用上述算法。即，同一层只要出现不是同一类型的组件，就替换该组件的所有子节点。对于同一类型的组件，则通过`shouldComponentUpdate`去判断是否需要通过`diff`进行分析。`shouldComponentUpdate`默认为true。

### 五. element diff

element diff主要是根据`mountIndex`和`lastIndex`进行比较，在确定是否移动 ，`mountIndex`是A节点在旧节点结合中的位置，`lastIndex`指访问过的节点，在旧集合中最右的位置，每次遍历都有可能会更新。

#### 算法描述

1. 遍历新节点集合

2. 如果出现旧节点集合中有与当前指针所指新节点A相同的节点，则通过对比节点位置进行判断操作，对比`mountIndex`和`lastIndex`：

   如果`mountIndex>=lastIndex`：不做移动操作。并把`lastIndex`更新为`mountIndex`。

   如果`mountIndex<lastIndex`：移动。

3. 如果新节点集合中有旧节点集合中不存在的节点，添加，更新`lastIndex`。

4. 最后遍历旧节点集合，如果存在新节点集合上不存在的点，则将其删除。

至于为什么要比较`mountIndex`和`lastIndex`，是因为要保证当前要进行移动操作的节点一定要比`lastIndex`小，一是为了节约性能，二是为了使节点排序更有条理，如果不进行比较，看见有相同的节点就移动，整个队列就乱了套了。

## 六. 总结

- React 通过制定大胆的 diff 策略，将 O(n3) 复杂度的问题转换成 O(n) 复杂度的问题；
- React 通过**分层求异**的策略，对 tree diff 进行算法优化；
- React 通过**相同类生成相似树形结构，不同类生成不同树形结构**的策略，对 component diff 进行算法优化；
- React 通过**设置唯一 key**的策略，对 element diff 进行算法优化；
- 建议，在开发组件时，保持稳定的 DOM 结构会有助于性能的提升；
- 建议，在开发过程中，尽量减少类似将最后一个节点移动到列表首部的操作，当节点数量过大或更新操作过于频繁时，在一定程度上会影响 React 的渲染性能。