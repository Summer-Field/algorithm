# 反转二叉树

> [反转二叉树](https://leetcode.cn/problems/invert-binary-tree/)

## 基本思路

反转二叉树的思路特别简单，在遍历的过程中将每个节点的左右两个子节点反转即可。

## 注意事项

但是，**这里需要注意**。

并不是所有遍历的方法都是可行的，其中，中序遍历就不行。因为中序遍历的过程中，可能会导致某一些节点的左右孩子反转两次。

这里使用递归来理解这个问题，该题解大致的递归思路为

```cpp
traversal(root->left);
swap(root->left, root->right)
traversal(root->left);
```

如果我们使用中序遍历，考虑最后一层。我们首先调用`traversal(node->left)`遍历左子树，当左子树遍历完成后，立马调用`swap()`函数将`root->left` ,`root->right`交换了指针所指的值。那么我们在调用`traversal(node->right)`遍历右子树的时候，又回在调用一次实际上左子树的指针，那么就会再反转一次。

不过在调用前/后序遍历就不会有这种问题。

## 解法

以下是我的四种解法:

### 递归

```cpp
    TreeNode* invertTree(TreeNode* root) {
        if (root == nullptr)
            return root;
        swap(root->left, root->right);
        invertTree(root->left);
        invertTree(root->right);
        return root;
 
```

### 迭代

```cpp
    TreeNode* invertTree(TreeNode* root) {
        std::stack<TreeNode*> st;
        if (root)
            st.push(root);
        else
            return root;
        while(!st.empty()) {
            TreeNode* node = st.top();
            st.pop();
            swap(node->left, node->right);
            if(node->left) {
                st.push(node->left);
            }
            if(node->right) {
                st.push(node->right);
            }
        }
        return root;
    
```

### 迭代（统一）

```cpp
    TreeNode* invertTree(TreeNode* root) {
        std::stack<TreeNode*> st;
        if(root)
            st.push(root);
        while(!st.empty()) {
            TreeNode* node = st.top();
            st.pop();
            if(node == nullptr){
                node = st.top();
                st.pop();
                swap(node->left, node->right);
            } else {
                st.push(node);
                st.push(nullptr);
                if(node->left)
                    st.push(node->left);
                if(node->right)
                    st.push(node->right);
            }
        }
        return root;

```

### 层序遍历

```cpp
    TreeNode* invertTree(TreeNode* root) {
        std::queue<TreeNode*> que;
        if (root)
            que.push(root);
        while(!que.empty()) {
            int size = que.size();
            for(int i = 0; i < size; i++ ){
                TreeNode* node = que.front();
                que.pop();
                swap(node->left, node->right);
                if(node->left)
                    que.push(node->left);
                if(node->right)
                    que.push(node->right);
            }
        }
        return root;

```
