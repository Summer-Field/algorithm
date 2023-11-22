# 对称二叉树

> [Symmetic Tree](https://leetcode.cn/problems/symmetric-tree/)

## 基本思路

比较左右子树是否时对称反转的，因此我们需要遍历两棵树（根节点的左右子树），所以在遍历的过程中，我们就需要遍历两棵树。

那么，接下来的问题时，如何比较呢？对于左右两边的树，我们需要比较里侧和外侧是否相等。

本题只能通过**后序遍历**，因为我们需要通过递归函数的返回值来判断两个子树的值是否相等。

正是因为要比较两棵树的内侧和外侧，所以需要左子树的**左右中**，需要右子树的**右左中**。

## 解法

### 递归法

1. input/output:
  a. input: 因为我们要判断的是跟节点的两个子树是否相等，进而继续判断他们的子树的相等行，所以很自然，我们递归函数的输入输出是左右两个子树的节点指针。
  b. output: 我们需要输出两个子树是否相等，所以返回值为`bool`类型

2. 终止条件：要比较两个数值是否相等，很自然我们需要首先判断节点为空时的情况弄清楚，这样就不会操作空指针了。
  a. 左/右为空，右/左不为空： false，不对称
  b. 都为空: ture 对称

3. 单层递归逻辑：单层递归逻辑实际上就是处理：单层不为空，且数值相同的时候
  a. 比较外侧是否相等
  b. 比较内侧是否相等
  c. 返回最终值

```cpp
class Solution {
private:
  bool compare(TreeNode* left, TreeNode* right) {
    if(left == nullptr && right == nullptr) return true;
    else if(left != nullptr && right == nullptr) return false;
    else if(left == nullptr && right != nullptr) return false;
    else if(left->val != right->val) return false;

    return compare(left->left, right->right) && compare(left->right, right->left);
  }

public:
  bool isSymmetric(TreeNode* root) {
    if (root == nullptr) return true;
    return compare(root->left, root->right);
  }
}
```

### 迭代法（队列）

```cpp
  bool isSymmetric(TreeNode* root) {
    if(root == nullptr) return true;
    std::queue<TreeNode*> que;
    que.push(root->left);
    que.push(root->right);

    while(!que.empty()) {
      TreeNode* left_node = que.front();
      que.pop();
      TreeNode* right_node = que.front();
      que.pop();

      if (left_node == nullptr && right_node == nullptr) continue;
      else if (left_node == nullptr && right_node != nullptr) return false;
      else if (right_node == nullptr && left_node != nullptr) return false;
      else if (right_node->val != left_node->val ) return false;

    que.push(left_node->left);
    que.push(right_node->right);
    que.push(left_node->right);
    que.push(right_node->left);
    }

    return true;
  }
```

### 迭代法（栈）

```cpp
    bool isSymmetric(TreeNode* root) {
        std::stack<TreeNode*> st;
        st.push(root->right);
        st.push(root->left);

        while(!st.empty()) {
            TreeNode* lnode = st.top(); st.pop();
            TreeNode* rnode = st.top(); st.pop();

            if(lnode == nullptr && rnode == nullptr) continue;
            else if(lnode == nullptr && rnode != nullptr) return false;
            else if(rnode == nullptr && lnode != nullptr) return false;
            else if(lnode->val != rnode->val) return false;

            st.push(lnode->left);
            st.push(rnode->right);
            st.push(lnode->right);
            st.push(rnode->left);
        }

        return true;
    }
```
