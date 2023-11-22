# 二叉树最小深度

> [二叉树最小深度](https://leetcode.cn/problems/minimum-depth-of-binary-tree/)

## 基本思路

### 递归法

三部曲：

1. 输入输出：

输入：输入就是某一个`TreeNode*`节点指针，来计算该节点的深度/高度
输出：本题需要求的是深度，因此输出的值应该是一个值为int的值来代表某一次所在的深度/高度
2. 终止条件：

本题需要求二叉树的最小深度，那么什么时候到达最小深度呢？数形结合，就是某个节点的的左右孩子都为空，那么该节点就是到达了我们的最小深度

因此，本题的终止条件是如果该节点是子叶节点(`node->left == nullptr && node->right == nullptr`)的话就返回1，记录该层需要记录表示一层
3. 单层递归逻辑：

如果没有到达终止条件，也就是说该节点一定不是最小深度所在的节点，\
那么我们就需要去遍历这个节点的左/右子树。

如果这个某子树不存在，那么我们就不遍历他，并将该节点的深度记为`INT_MAX`，因为不可能是最小深度的节点
如果某子树存在，那么我们就继续向深处遍历他，并记录该节点子树的深度。

### 层序遍历（队列）

思路很简单，层序遍历整个树，如果遇到满足条件(`node->left == nullptr && node->right == nullptr`)
那么就返回层数 + 1

## 代码实现

### 递归

```cpp
int traversal(TreeNode* node) {
    if(node->left == nullptr && node->right == nullptr) return 1;
    int left_min = INT_MAX;
    int right_min = INT_MAX;
    if(node->left) left_min = traversal(node->left);
    if(node->right) right_min = traversal(node->right);
    return 1 + min(left_min, right_min);
}

int minDepth(TreeNode* root) {
    if(root == nullptr) return 0;
    return traversal(root);
}
```

### 层序遍历

```cpp
int minDepth(TreeNode* root) {
    queue<TreeNode*> que;
    int depth = 0;
    if (root != nullptr) que.push(root);
    while(!que.empty()) {
        int size = que.size();
        for(int i = 0; i < size; i++) {
            TreeNode* node = que.front();
            que.pop();
            if(node->left == nullptr && node->right == nullptr) return depth + 1;
            if(node->left) que.push(node->left);
            if(node->right) que.push(node->right);
        }
        depth ++;
    }
    return depth;
}
```
