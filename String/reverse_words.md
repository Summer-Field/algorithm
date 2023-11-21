# 反转字符串中的单词

> [反转单词](https://leetcode.cn/problems/reverse-words-in-a-string)

## 基本思路

本题十分考察字符串的基本功，涉及了很多和字符串相关的操作。本题当然可以直接使用`split()`库函数，然后再将字符串倒叙相加。不过这样这道题就没有意义了。
所以本题的新要求是：**不使用额外的空间,空间复杂度要求N(1)** 来实现

那么我们来思考，如果我们将整个字符串反转后，然后再单独将每个单词单独反转一遍就可以得到反转单词后的字符串了。

不过本题还有一个难点就是：**将字符串开头、中间、结尾中的没有意义的字符串删除**

因此本题的思路为：

1. 删除掉整个字符串中没有意义的空格
2. 反转字符串
3. 单独反转每个单词

## 代码实现

### 去除多余空格`removeExtraSpaces()`

整体思路是使用**双指针**的技巧：

1. 首先，使用快指针`fast`来遍历前面的空字符，如果需要空字符直接跳过。遍历之后`fast`指向第一个非`' '`字符
2. 然后，我们同时移动`fast`和`slow`指针，实现去除中间字符串中多余的空格。如果遇到非空字符则将`fast`指向的字符传给`slow`指向的位置。
如果我们遇到`' '`空字符，我们选择保留第一个空字符，并判断后续的字符是否也是`' '` 空字符，如果是我们则跳过这些字符，不给`slow`指向的字符复制。
至此，我们的slow指针之前的字符已经包含了所有被去除空格的字符。这时，slow可能有两空可能:\
  a. 如果结尾有`' '`，slow指向`' '`\
  b. 如果结尾没有`' '`, slow指向最后一个字符+1的位置
3. 调用`resize()`函数去除最后中多余的空字符。\
  a. 如果结尾为`' '`, resize slow - 1\
  b. 如果结尾为字符, resize slow

```cpp
// remove the extra spaces in the head
void removeExtraSpaces(string& s) {
    int fast = 0, slow = 0;
    int size = s.size();
    // if s is a null string return
    if (size == 0)
        return;
    // remove the spaces at the head, now the s[fast] is not ' ' any more
    while(fast < size && s[fast] == ' ') fast++;
    // remove the spaces at in the middle
    for(;fast < size; fast++) {
        if(fast - 1 >= 0 && s[fast] == ' ' && s[fast] == s[fast - 1]) continue;
        else s[slow++] = s[fast];
    }
    // remove the spaces at the end
    if(slow - 1 > 0 && s[slow - 1] == ' ') s.resize(slow - 1); // end with ' '
    else s.resize(slow); // end with character
}
```

上面的代码是基本思路，然后这个函数其实可以写得更加精简一些，可以参考之前做过的去除[重复元素](../Array/remove_element.md)。
本质上就是将我们需要去除的目标元素定为`' '`。然后再每个单词后手动补上`' '`

因此，更加精简的代码实现为：

```cpp
void removeExtraSpaces(string& s) {
    int size = s.size();
    int fast = 0, slow = 0;
    for(; fast < size; fast++){                                       // 遍历整个数组
      if(s[fast] != ' ') {                                            // 跳过所有遇到的空格，只有遇到非空格的时候才处理
          if(slow != 0) s[slow++] = ' ';                              // 手动再非空字符后面加一个空格，注意⚠️，不能再第一个字符前面添加空格，所以我们需要做一个判断
          while(fast < size && s[fast] != ' ') s[slow++] = s[fast++]; // 填充我们需要的单词
      }
    }
    s.resize(slow);                                                   // 重构数组大小
}
```

### 反转区间字符串`reverse()`

```cpp
reverse(string& s, int start, int end) {
  for(int i = start, j = end; i < j; i++, j--)
    swap(s[i], s[j]);
}
```

### 反转字符串单词`reverseWords()`

```cpp
string reverseWords(string s) {
// 首先去除空格字符
removeExtraSpaces(s);
int size = s.size();
// 反转整个字符串
reverse(s, 0, size - 1);
// 反转区间字符
for(int i = 0, j = 0; i <= size; ++i) {
  if(i == size || s[i] == ' ') {
    reverse(s, j, i - 1);
    j = i + 1;
  }
}
return s;
```
