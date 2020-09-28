# Conquer-Leetcode

## Binary Search  二分法

- [x] 前提：有序！！！ 【Arrays.sort(nums)--O(nlogn)】
- [x] 时间复杂度：从时间复杂度O(logn)倒推出二分法(增删改查的查）
- [x] 递归和迭代：能用迭代不要用递归
- [x] 万能迭代模版
- [x] 题型分类：
            1. 排序数据集上进行二分
            2. 答案集上进行二分
- [x] 习题：   

Source | Header | Keywords
------------ | ---------------------- | -----------------------------------------------
[leetcode704](https://leetcode.com/problems/binary-search/) |  Binary Search | 找target本尊 target若有也只有一个 
[leetcode374](https://leetcode.com/problems/guess-number-higher-or-lower/) |  Guess Number Higher or Lower |  找target本尊 target若有也只有一个 多个api接口纸老虎
[leetcode278](https://leetcode.com/problems/first-bad-version/) |  First Bad Version |  找target本尊 target若有也只有一个 但这道题api接口只返回两种情况 
[leetcode35](https://leetcode.com/problems/search-insert-position/) |  Search Insert Position |  找target能插入的第一个位置 或 比target小的值有几个
[leetcode275](https://leetcode.com/problems/h-index-ii/) |  H-Index II |  注意是找后面的target 而且是动态target
[leetcode69](https://leetcode.com/problems/sqrtx/) | sqrtx|  答案集进行二分 找 mid <= x/mid 的最后一个值
[leetcode162](https://leetcode.com/problems/find-peak-element/) |  Find Peak Element |  找波峰 二分变形 最后单独判断部分变为找更高的值而不是target 上下坡来分左右
[leetcode153](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/) |  Find Minimum in Rotated Sorted Array | 旋转分段 找最小
[leetcode154](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/) | Find Minimum in Rotated Sorted Array II | duplicate153 最坏O(n)
[leetcode33](https://leetcode.com/problems/search-in-rotated-sorted-array/) |   Search in Rotated Sorted Array |  旋转 分段 找target
[leetcode81](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/) |  Search in Rotated Sorted Array II | duplicate33 最坏O(n) 
[leetcode74](https://leetcode.com/problems/search-a-2d-matrix/) |Search a 2D Matrix| 二维数组化一维数组 mid/col mid%col  排序数集进行二分
[leetcode240](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/) | Search a 2D Matrix II | 非典型二分 右上角或左下角开始 O(max(m,n)) 排序数集进行二分
[lintcode540](https://leetcode.com/problems/single-element-in-a-sorted-array/) | Single Element in a Sorted Array| 非典型二分
[leetcode302](https://leetcode.com/problems/smallest-rectangle-enclosing-black-pixels/) | Smallest Rectangle Enclosing Black Pixels |未排序数集进行二分 O(mlongn + nlongm)
[leetcode658](https://leetcode.com/problems/find-k-closest-elements//) | Find K Closest Elements|排序数集进行二分 
[lintcode183](https://www.lintcode.com/problem/wood-cut/description) | Wood Cut|答案集进行二分 
[leetcode167](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/) |  Two Sum II - Input array is sorted |  更多的是双指针
[leetcode349](https://leetcode.com/problems/intersection-of-two-arrays/) |  Intersection of Two Arrays |  更多的是双指针
[leetcode350](https://leetcode.com/problems/intersection-of-two-arrays-ii/) |  Intersection of Two Arrays II |  更多的是双指针



### 1. 递归写法(不推荐，当递归深度到达10^5时会出现stackoverflow）
```java
    private int binarySearchRecursion(int[] nums, int low, int high, int target) {
        if (low > high) {
            return -1;
        }
        int mid = low + (high - low)/2;
        if (nums[mid] == target) {
            return mid;
        }
        if (nums[mid] > target) {
            return helper(nums, low, mid - 1, target);
        }
        if (nums[mid] < target) {
            return helper(nums, mid + 1, high, target);
        }
        return -1;
    }
```

### 2. 迭代万能模版——首选

##### target在数组中只有一个或没有 —— 找target那个值，返回index，没有就返回-1
```java
    public int binarySearch(int[] nums, int target) {
        // corner case
        if (nums == null || nums.length == 0) {
            return 0;
        }
        int left = 0;
        int right = nums.length-1; //决定了是 [left,right] 闭区间
        while(left + 1 < right){
            int mid = left + (right-left)/2; // prevent overflow when right = Integer.MAX_VALUE
            if(nums[mid] > target){ 
                right = mid;         
            }else if(nums[mid] < target){
                left = mid;
            }else {
                return mid;
            }
        }
        //  退出循环的时候 left right是相邻的 但可能是没有==target的情况的 
        if (nums[left] == target) {
            return left;
        }
        if (nums[right] == target) {
            return right;
        }
        return -1;
    }
```

##### target在数组中有多个，返回第1个target的位置
```java
    public int binarySearch(int[] nums, int target) {
        // corner case
        if (nums == null || nums.length == 0) {
            return 0;
        }
        int left = 0;
        int right = nums.length-1; //决定了是 [left,right] 闭区间
        while(left + 1 < right){
            int mid = left + (right-left)/2; // prevent overflow when right = Integer.MAX_VALUE
            if(nums[mid] >= target){ 
                right = mid;         
            }else {
                left = mid;
            }
        }
        //  退出循环的时候 left right是相邻的 但可能是没有==target的情况的 
        if (nums[left] == target) {
            return left;
        }
        if (nums[right] == target) {
            return right;
        }
        return -1;
    }
```

##### target在数组中有多个，返回最后1个target的位置
```java
    public int binarySearch(int[] nums, int target) {
        // corner case
        if (nums == null || nums.length == 0) {
            return 0;
        }
        int left = 0;
        int right = nums.length-1; //决定了是 [left,right] 闭区间
        while(left + 1 < right){
            int mid = left + (right-left)/2; // prevent overflow when right = Integer.MAX_VALUE
            if(nums[mid] <= target){ 
                left = mid;     
            }else {
                right = mid;
            }
        }
        //  退出循环的时候 left right是相邻的 但可能是没有==target的情况的 
        if (nums[right] == target) {
            return right;
        }
        if (nums[left] == target) {
            return left;
        }
        return -1;
    }
```
### 3. 两外两种 迭代模版

##### 找target那个值，返回index，没有就返回-1
```java
    public int binarySearch(int[] nums, int target) {
        // corner case
        if (nums == null || nums.length == 0) {
            return 0;
        }
        int left = 0;
        int right = nums.length-1; //决定了是 [left,right] 闭区间 也就决定了while中是 <=
        while(left <= right){
            int mid = left + (right-left)/2; // prevent overflow when right = Integer.MAX_VALUE
            if(nums[mid] > target){ 
                right = mid - 1;         
            }else if(nums[mid] < target){
                left = mid + 1;
            }else {
                return mid;
            }
        }
        return -1;
    }
```

##### 找target那个值，返回index，没有就返回-1
```java
    public int binarySearch(int[] nums, int target) {
        // corner case
        if (nums == null || nums.length == 0) {
            return 0;
        }
        int left = 0;
        int right = nums.length; //决定了是 [left,right) 左闭右开区间 也就决定了while中是 <
        while(left < right){
            int mid = left + (right-left)/2; // prevent overflow when right = Integer.MAX_VALUE
            if(nums[mid] > target){ 
                right = mid;         
            }else if(nums[mid] < target){
                left = mid + 1;
            }else {
                return mid;
            }
        }
        return -1;
    }
```

## bit  位运算

运算名称 | 运算符号 | 运算性质 
------------ | ---------------------- | -----------------------------------------------
与 | &  | 同为1， 其他为0 
或  | \| | 同为0， 其他为1 
取反  | ～  | 1变0 0变1 
异或  | ^  | 不同为1， 相同为0 
左移  | <<  | 高位丢弃 低位补0 
右移  | >>  | 低位丢弃 高位补0/1 




## Trie 字典树

- [x] 前缀树 字母树 哈希树的变种 用来查询前缀
- [x] 时间复杂度： O(word.length()) 空间复杂度: O(N * word.length * 26) 用空间换取时间
- [x] 各种函数的写法（插入/查询单词存在/查询前缀存在） array && hashmap实现Trie
- [x] 常见提示：不重复单词列表 （公共）前缀
- [x] 字典树可以维护： 目标前缀的单词数/单词集合
- [x] 习题：   

Source | Header | Keywords
------------ | ---------------------- | -----------------------------------------------
[lintcode 333](https://www.lintcode.com/problem/identifying-strings/description) |  identifying-strings | 找能识别出自己的最小前缀 
[leetcode 211](https://leetcode.com/problems/design-add-and-search-words-data-structure/) |  design-add-and-search-words-data-structure | "." any
[leetcode 212](https://leetcode.com/problems/word-search-ii/) |  word-search-ii | 回溯和字典树 
[lintcode 1848](https://www.lintcode.com/problem/word-search-iii/description) |  word-search-iii | 同时出现的最大数量的集合 单词不重复 回溯和字典树 难
[lintcode 635](https://www.lintcode.com/problem/boggle-game/description) | boggle-game | 同时出现的最大数量 但单词可以重复 回溯和字典树 难
[lintcode 775](https://www.lintcode.com/problem/palindrome-pairs/description) | palindrome-pairs | 回文 逆序插入 难
[lintcode 634](https://www.lintcode.com/problem/word-squares/description) | word-squares | 主对焦对称 难
[lintcode 1624](https://www.lintcode.com/problem/max-distance/description) | max-distance | 难
[lintcode 1221](https://www.lintcode.com/problem/concatenated-words/description) | concatenated-words | dp 难
[lintcode 623](https://www.lintcode.com/problem/k-edit-distance/description) | k-edit-distance | dp 难
[lintcode 1248](https://www.lintcode.com/problem/k-edit-distance/description) | k-edit-distance | 异或运算 中等
[lintcode 722](https://www.lintcode.com/problem/maximum-subarray-vi/description) | maximum-subarray-vi |  超难




### 1. Array实现Trie
```java
class Trie {
    private Trie[] sons;
    private boolean isWord;
    private String word;
    /** Initialize your data structure here. */
    public Trie() {
        // 0 - 26 对应 a 到 z
        sons = new Trie[26];
        for (Trie son : sons) {
            son = null;
        }
        isWord = false;
        word = null;
    }
    
    /** Inserts a word into the trie. */
    public void insert(String word) {
        int L = word.length();
        Trie node = this;
        for (int i = 0; i < L; i++) {
            char letter = word.charAt(i);
            if (node.sons[letter - 'a'] == null) {
                node.sons[letter - 'a'] = new Trie();
            }
            node = node.sons[letter - 'a'];
        }
        node.isWord = true;
        node.word = word;
    }
    
    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        int L = word.length();
        Trie node = this;
        for (int i = 0; i < L; i++) {
            char letter = word.charAt(i);
            if (node.sons[letter - 'a'] == null) {
                return false;
            }
            node = node.sons[letter - 'a'];
        }
        return node.isWord;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {
        int L = prefix.length();
        Trie node = this;
        for (int i = 0; i < L; i++) {
            char letter = prefix.charAt(i);
            if (node.sons[letter - 'a'] == null) {
                return false;
            }
            node = node.sons[letter - 'a'];
        }
        return true;
    }
}
```

### 2. HashMap实现Trie
```java
public class Trie {
    private Map<Character, Trie> sons;
    private boolean isWord;
    private String word;
    public Trie() {
        sons = new HashMap<>();
        isWord = false;
        word = null;
    }

    public void insert(String word) {
        int L = word.length();
        Trie node = this;
        for (int i = 0; i < L; i++) {
            char letter = word.charAt(i);
            if (!node.sons.containsKey(letter)) {
                node.sons.put(letter, new Trie());
            }
            node = node.sons.get(letter);
        }
        node.isWord = true;
        node.word = word;
    }

    public boolean search(String word) {
        int L = word.length();
        Trie node = this;
        for (int i = 0; i < L; i++) {
            char letter = word.charAt(i);
            if (!node.sons.containsKey(letter)) {
                return false;
            }
            node = node.sons.get(letter);
        }
        return node.isWord;
    }

    public boolean startsWith(String prefix) {
        int L = prefix.length();
        Trie node = this;
        for (int i = 0; i < L; i++) {
            char letter = prefix.charAt(i);
            if (!node.sons.containsKey(letter)) {
                return false;
            }
            node = node.sons.get(letter);
        }
        return true;
    }
}
```


## 栈与表达式

- [x] 语法树 前中后序表达式 中缀表达式--人类思维  后缀表达式--计算机思维
- [x] 中缀转后缀 -- 单调栈应用
- [x] 利用算符优先级 构造 单调递增栈
- [x] 从左至右扫描表达式 对于操作数字 放入结果
- [x] 对于算符 栈不空 且 栈顶优先级大 弹栈  栈空 且 栈顶优先级小 压栈 
- [x] 弹出的元素除了括号均放入结果

名称 | 符号 | 优先级
------------ | ---------------------- | -----------------
结束符 | # | min
右括号 |）| 1
加号| +| 2
减号 |- |2
乘号 |*| 3
除号 |/| 3
左括号 |（ |max

- [x] 习题

Source | Header | Keywords
------------ | ---------------------- | -----------------------------------------------
[lintcode 368](https://www.lintcode.com/problem/expression-evaluation/description) | expression-evaluation| 表达式求值
[leetcode 367](https://www.lintcode.com/problem/expression-tree-build/description) |  expression-tree-build | 表达树构造


    

