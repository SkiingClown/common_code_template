# 思来想去，不记录一些模板还是不行

<font size="3">




## 计算最长公共子序列



```cpp
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        vector<vector<int>> dp(text1.size()+1, vector<int>(text2.size()+1, 0));

        for (int i = 1; i <= text1.size(); i++) {
            for (int j = 1; j <= text2.size(); j++) {
                dp[i][j] = text1[i-1] == text2[j-1] ? dp[i-1][j-1] + 1 : dp[i-1][j-1];
                dp[i][j] = max({dp[i-1][j], dp[i][j-1], dp[i][j]});
            }
        }

        return dp[text1.size()][text2.size()];
    }
};
```


## 计算子数组的最大值

给你一个整数数组 nums ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。其中**子数组 是数组中的一个连续部分。**

```cpp
//注意该模板是依据转移方程简化而来，转移方程如下：
// f(i) 代表以第 i 个数结尾的「连续子数组的最大和」
//f(i)=max{f(i−1)+nums[i],nums[i]}
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int pre = 0, maxAns = nums[0];
        for (const auto &x: nums) {
            pre = max(pre + x, x);
            maxAns = max(maxAns, pre);
        }
        return maxAns;
    }
};

```

## 快速选择计算第K大的值 模板

``` cpp
class Solution {
public:
    int partition(vector<int>& nums, int left, int right) {
        //这里选择第一个为基准值
        int pivot = nums[left];
        swap(nums[left], nums[right]); //交换基准值到最后一位去

        int i = left, save = left;  //这里i表示游标，一直遍历向右，save表示满足大于基准值的值的起始位置，save左边表示小于基准值的值
        for (; i < right; ) {
            if (nums[i] <= pivot) { 
                swap(nums[i], nums[save]);
                i++;
                save++;
            }
            else if (nums[i] > pivot) {
                i++;
            }
        }

        swap(nums[right], nums[save]);
    
        return save;
    }

    int findKthLargest(vector<int>& nums, int k) {
        int n = nums.size();
        int left = 0;
        int right = n - 1;
        
        while (true) {
            int pos = partition(nums, left, right);

            //这里的返回值表示pos的位置前有pos个值小于它的，
            //满足第K大时，直接返回，
            if (n - pos == k) {     
                return nums[pos];
            }
            else if (n - pos > k) {   // 搜索右区间 
                left = pos + 1;
            }
            else {  // 搜索左区间
                right = pos - 1;
            }
        }

        return -1;
    }
};

```

## 前缀树模板

针对leetcode208. 实现 Trie (前缀树)，但一般来说，只有insert和search是必须的。

1. 这里先提供数组的实现方法。（N的长度为树中所有字符长的总和）
``` cpp
class Trie {
public:
    int N = 1e5;    
    vector<vector<int>> tire;
    vector<int> count;
    int index;
    //Trie() 初始化前缀树对象
    Trie() {
        index = 0;
        tire.assign(N, vector<int>(26));
        count.assign(N, 0);
    }
    
    //向前缀树中插入字符串 word 
    void insert(string word) {
        int p = 0;
        for (int i = 0; i < word.size(); i++) {
            int c = word[i] - 'a';
            if (tire[p][c] == 0) {
                tire[p][c] = ++index;
            }
            p = tire[p][c];
        }

        count[p]++;
    }
    
    //如果字符串 word 在前缀树中，返回 true（即，在检索之前已经插入）；否则，返回 false 
    bool search(string word) {
        int p = 0;
        for (int i = 0; i < word.size(); i++) {
            int c = word[i] -'a';
            if (tire[p][c] != 0) {
                p = tire[p][c];
            }
            else {
                return false;
            }
        }

        return count[p] > 0;
    }
    
    // 如果之前已经插入的字符串 word 的前缀之一为 prefix ，返回 true ；否则，返回 false 。
    bool startsWith(string prefix) {
        int p = 0;
        for (int i = 0; i < prefix.size(); i++) {
            int c = prefix[i] - 'a';
            if (tire[p][c] != 0) {
                p = tire[p][c];
            }
            else {
                return false;
            }
        }

        return true;
    }
};


```



</font>