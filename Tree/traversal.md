# 二叉树遍历

## 深度优先遍历

### 二叉树的递归遍历

#### **递归算法三要素**

1. 确定递归函数的参数和返回值

2. 确定终止条件

3. 确定单层递归的逻辑

#### 前/中/后序遍历

```c++
class Solution {
  vector<int> traversal(struct TreeNode* root){
    vector<int> ans;
    preorderTraversal(root, ans);
    midorderTraversal(root, ans);
    postoderTraversal(root, ans);
    return ans;
  }
}
```

##### 前序遍历

```c++
/* First, we need to confirm the input and return value of the function.
Here we need to traversal the bi-tree and store the output result.
So the input value should be the node of the tree and the result vector */
void preorderTraversal(struct TreeNode* node, vector<int>& vec) {
/* Second, we need to confirm the terminating condition
We kown that if the node is null we need to return */
    if (node == NULL) return;
/* Last, we need to confirm the single layer traversal logic.
For each node, the value is the middle part of the node.
Here we are going to pretraversal, so we need to push the middle
first and traversal the left and right in order. */
    vec.push_back(node->val);    // middle
    preorderTraversal(node->left, vec);  // left
    preorderTraversal(node->right, vec); // right
}
```

##### 中序遍历

```cpp
void midorderTraversal(struct TreeNode* node, vector<int>& vec){
  if(node == NULL)
    return;
  midorderTraversal(node->left, vec);
  vector.push_back(node->val);
  midorderTraversal(node->right, vec);
}
```

##### 后序遍历

```cpp
void postoderTraversal(struct TreeNode* node, vector<int>& vec){
  if(node == NULL)
    return;
  postoderTraversal(node->left, vec);
  postoderTraversal(node->right, vec);
  vector.push_back(node->val);
}
```

### 二叉树迭代遍历

**为什么我们可以使用栈来实现递归调用？**

> 递归的实现就是：\
> 每一次递归调用都会把函数的 **局部变量、参数值和返回地址** 等压入调用栈中然后递归返回的时候，从栈顶弹出上一次递归的各项参数，所以这就是递归为什么可以返回上一层位置的原因。

#### 前序遍历（迭代）

这里我们需要注意我们需要先压入右节点，再压入左节点\
因为栈是**先进后出**

```cpp
vector<int> preorderTraversal(TreeNode* root){
  std::vector<int> ans;
  std::stack<TreeNode*> st;

  if(root == nullptr)
    return ans;

  st.push(root);
  while(!st.empty()){
    TreeNode* node = st.top();
    st.pop();
    ans.push_back(node.val);
    // if node is Null we can not push it
    // Here the partial var is node->right/left
    if(node->right)
      st.push_back(node->right);
    if(node->left)
      st.push_back(node->left);
  }

  return ans;
}
```

#### 中序遍历（迭代）

注意，这里中序遍历是不能直接套用前序遍历的思路的。\
主要原因是**访问顺序和处理顺序不一致**。

因此在中序遍历中，我们需要使用

1. 指针的遍历来访问节点。
2. 使用栈来处理节点。

```cpp
vector<int> midorderTraversal(TreeNode* root) {
  vector<int> ans;
  std::stack<TreeNode*> st;
  TreeNode *cur = root;

  while(!st.empty() && cur != nullptr) {
    if(cur != nullptr) { // Use pointer to traversal the entire tree
      st.push(cur);
      cur = cur->left; // left node
    } else {
      cur = st.top(); // use stack to process the data
      st.pop();
      ans.push_back(cur->val); // mid node
      cur = cur->right; // right node
    }
  }

  return ans;
}
```

#### 后序遍历（迭代）

后序遍历的输出为左右中，先序遍历为中左右。所以我们要做的只是

1. 调整先序遍历代码输出为中右左
2. 将调整过的代码反转即可。

```cpp
vector<int> postorderTraversal(TreeNode* root) {
  std::vector<int> ans;
  std::stack<TreeNode*> st;
  TreeNode* node;

  if(root == nullptr)
    return ans;
  
  while(!st.empty()) {
    node = st.top();
    st.pop();
    ans.push_back(node->val);
    // Compared to preorderTraversal we need to make left fist
    if(node->left)
      st.push(node->left);     
    if(node->right)
      st.push(node->right);
  }

  reverse(ans.begin(), ans.end());

  return ans;
}
```

### 统一风格迭代法

前面讲的迭代法除了前序和后续两者的代码风格一致。中序由于访问顺序和处理节点顺序不同，因此出现了代码风格不一致的情况。这里介绍统一二叉树遍历方法：**标记法**

具体处理方法为：把访问节点放入栈中，把需要处理的节点也放入栈中，只是在他后面加一个空指针标记一下。

这种处理方法，我们就可以需要处理的节点后面全部标记一个`nullptr`，然后遇到`nullptr`就处理它

我们需要注意，在遍历过程中遇到非空节点时，放入栈中的顺序和输出顺序是需要相反的。

- 前序遍历（中左右）,放入顺序（右左中）

#### 前序遍历（统一）

```cpp
vector<int> midorderTraversal(TreeNode* root) {
  std::vector<int> ans;
  std::stack<TreeNode*> st;

  st.push(root);

  while(!st.empty()) {
    TreeNode* node = st.top();
    st.pop();
    if(node != nullptr) {
      if(node->right)
        st.push(node->right);
      if(node->left)
        st.push(node->left);
      st.push(node);
      st.push(nullptr);
    } else {
      node = st.top();
      st.pop();
      ans.push_back(node->val);
    }
  }

  return ans;
}

```

#### 中序遍历（统一）

```cpp
vector<int> midorderTraversal(TreeNode* root) {
  std::vector<int> ans;
  std::stack<TreeNode*> st;
  st.push(root);
  while(!st.empty()) {
    TreeNode* node = st.top();
    st.pop();
    if(node != nullptr) {
      if(node->right)
        st.push(node->right);
      st.push(node);
      st.push(nullptr);
      if(node->left)
        st.push(node->left);
    } else {
      node = st.top();
      st.pop();
      ans.push_back(node->val);
    }
  }

  return ans;
}
```

#### 后序遍历（统一）

```cpp
vector<int> midorderTraversal(TreeNode* root) {
  std::vector<int> ans;
  std::stack<TreeNode*> st;
  st.push(root);
  while(!st.empty()) {
    TreeNode* node = st.top();
    st.pop();
    if(node != nullptr) {
      st.push(node);
      st.push(nullptr);
      if(node->right)
        st.push(node->right);
      if(node->left)
        st.push(node->left);
    } else {
      node = st.top();
      st.pop();
      ans.push_back(node->val);
    }
  }

  return ans;
}
```

## 层序遍历（广度优先遍历）

前面，我们使用三种方法介绍了深度优先的二叉树遍历方法。

1. 递归
2. 迭代（栈）
3. 标记法（栈，迭代，统一）

接下来我们介绍广度优先算法，这里我们需要使用另一种数据结构**队列**来实现。

- 队列先进先出，符合层序遍历
- 栈先进后出，符合深度遍历

以下是层序遍历的模版代码：

### 迭代法

```cpp
vector<vector<int>> levelOrder(TreeNode* root) {
  vector<vector<int>> ans;
  queue<TreeNode*> que;

  if (root)
    que.push(root);

  while(!que.empty()) {
    int size = que.size();
    vector<int> vec;

    for(int i = 0, i < size, i++) {
      TreeNode* node = que.front();
      que.pop();
      vec.push_back(node->val);
      if(node->left)
        que.push(node->left);
      if(node->right)
        que.push(node->right);
    }

    ans.push_back(vec);
  }

  return ans;
}
```

### 递归法

本质：使用先序遍历的方法，新增一个记录当前遍历层数的变量，然后记录答

1. 输入参数：（需要遍历的树，返回值，当前遍历所在的层数）输出参数：void
2. 终止条件：节点为空就返回
3. 单层逻辑：b
  a. 如果答案的层数（答案能覆盖的层数）等于当前遍历的层数，也就是没有更多的层数装接下来的答案是，新增一层。
  b. 将某层的答案中加入对应的答案中
  c. 遍历左右节点。

```cpp
void order(TreeNode* node, vector<vector<int>>& ans, int depth) {
  if (node == nullptr)
    return;
  if (ans.size() == depth) ans.push_back(vector<int>());
  ans[depth].push_back(node->val);
  order(node->left, ans, depth + 1);
  order(node->right, ans, depth + 1);
}

vector<vector<int>> levelOrder(TreeNode* root) {
  vector<vector<int>> ans;
  int depth = 0;
  order(root, ans, depth);
  return ans;
}
```
