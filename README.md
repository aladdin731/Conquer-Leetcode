# Conquer-Leetcode

## Binary Search  二分法

- [x] 前提：有序！！！ 【Arrays.sort(nums)--O(nlogn)】
- [x] 时间复杂度：O(logn) 空间复杂度：O(1) 可以从时间复杂度倒推出二分法(增删改查的查！）
- [x] 递归和迭代：能用迭代不要用递归，当递归深度到达10^5时会出现stackoverflow
- [x] 万能迭代模版
- [x] 题型分类：
            1. 排序数据集上进行二分
            2. 答案集上进行二分
- [x] 习题：   

Source | Header | Keywords
------------ | ---------------------- | ------------------
[leetcode704](https://leetcode.com/problems/binary-search/) |  Binary Search | 找target本尊 target若有也只有一个 
[leetcode374](https://leetcode.com/problems/guess-number-higher-or-lower/) |  Guess Number Higher or Lower |  找target本尊 target若有也只有一个 多个api接口纸老虎
[leetcode278](https://leetcode.com/problems/first-bad-version/) |  First Bad Version |  找target本尊 target若有也只有一个 但这道题api接口只返回两种情况 
[leetcode35](https://leetcode.com/problems/search-insert-position/) |  Search Insert Position |  找target能插入的第一个位置 或 比target小的值有几个
[leetcode275](https://leetcode.com/problems/h-index-ii/) |  H-Index II |  注意是找后面的target
[leetcode162](https://leetcode.com/problems/find-peak-element/) |  Find Peak Element |  找波峰 二分变形 最后单独判断部分变为找更高的值而不是target 上下坡来分左右
[leetcode167](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/) |  Two Sum II - Input array is sorted |  更多的是双指针
[leetcode349](https://leetcode.com/problems/intersection-of-two-arrays/) |  Intersection of Two Arrays |  更多的是双指针
[leetcode350](https://leetcode.com/problems/intersection-of-two-arrays-ii/) |  Intersection of Two Arrays II |  更多的是双指针



### 1. 递归写法(不推荐）
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


    

