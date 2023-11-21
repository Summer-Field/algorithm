# 二叉树理论基础

## 二叉树种类

### 满二叉树

定义：如果一棵树节点的度只有0/2，那么这棵树就是满二叉树

### 完全二叉树

定义：除了最底层节点没有填满，其余节点都已经填满，且最后一层节点都集中在最左边

### 二叉搜索树

定义：前面介绍的树都是没有数值的，但是二叉搜索树是有数值的。二叉搜索树是一棵有序树。

- 若它的左子树不空，则左子树上所有结点的值均小于它的根结点的值；
- 若它的右子树不空，则右子树上所有结点的值均大于它的根结点的值；
- 它的左、右子树也分别为二叉排序树

### 平衡二叉搜索树

定义：又被称为AVL（Adelson-Velsky and Landis）树，且具有以下性质：它是一棵空树或它的左右两\
个子树的高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树。

> C++中map、set、multimap，multiset的底层实现都是平衡二叉搜索树，所以map、set的增删操作时间时\
> 间复杂度是logn，注意我这里没有说unordered_map、unordered_set，unordered_map、unordered_set底层实现是哈希表。

## 二叉树遍历方式

- 深度优先遍历
  - 前序遍历（递归法，迭代法）
  - 中序遍历（递归法，迭代法）
  - 后序遍历（递归法，迭代法）
- 广度优先遍历
  - 层次遍历（迭代法）

## 二叉树定义

```c++
struct TreeNode {
  int val;
  struct TreeNode *left;
  struct TreeNode *right;
  TreeNode(int x) : val(x), left(NULL), right(NULL) {}
}
```