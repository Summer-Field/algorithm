# 移除元素

> [移除元素](https://leetcode.cn/problems/remove-element/)

## 基本思路

这道题要求我们就地(**in-place**)移除某个元素。就是不能有额外的空间使用。

当然可以直接使用暴力法移除，暴力法太过于简单粗暴。这里不介绍。

我们这里介绍**双指针法**:

我们同时移动两个快慢指针: `fast`, `slow`

如果快指针遇到了需要跳过的则只移动`fast`，否则一起移动并将`s[fast]`赋值给`s[slow]`

## 代码实现

```cpp
int removeElement(vector<int>& nums, int val ){
    int size = nums.size();
    int slow = 0, fast = 0;

    while(fast < size) {
        if(nums[fast] != val)
            nums[slow++] = nums[fast];
        fast++;
    }

    return slow;
}
```
