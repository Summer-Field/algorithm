# 二叉树最大深度

> [二叉树最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)

## 基本思路

层序遍历：使用层序遍历，没遍历一层加一。

递归法：如果使用深度优先遍历的话，答案就是二叉树子叶节点的深度（前序遍历）/高度（后序遍历）

- 子叶节点深度：从根节点到该叶节点最长简单路径边的条数/节点数（取决于深度从0/1开始）
- 子叶节点高度：从该节点点到子叶节点最长简单路径边的条数/节点数（取决于深度从0/1开始）

所以，根节点的高度就是二叉树的最大深度，所以本题中我们使用**后序遍历**求根节点的高度来求二叉树最大深度。

## 解法

### 层序遍历

```cpp
  int maxDepth(TreeNode* root) {
      int depth = 0;
      std::queue<TreeNode*> que;
      if(root != nullptr) que.push(root);
      while(!que.empty()) {
          int size = que.size();
          for(int i = 0; i < size; i++) {
              TreeNode* node = que.front();
              que.pop();
              if(node->left) que.push(node->left);
              if(node->right) que.push(node->right);
          }
          depth++;
      }
      return depth;
```

### 递归

```cpp
class solution {
public: 
  int maxDepth(TreeNode* root) {
    if (root == nullptr) return 0;
    return 1 + max (maxDepth(root->left), maxDepth(root->right));
  }
}
```
