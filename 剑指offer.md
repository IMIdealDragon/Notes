# 剑指offer之滚瓜烂熟

[TOC]

### 找出数组中重复的数字(DFS)

给定一个长度为 n 的整数数组 `nums`，数组中所有的数字都在 0∼n−1 的范围内。

数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。

请找出数组中任意一个重复的数字。

**注意**：如果某些数字不在 0∼n−1 的范围内，或数组中不包含重复数字，则返回 -1；

**样例**

```
给定 nums = [2, 3, 5, 4, 3, 2, 6, 7]。

返回 2 或 3
```

```cpp
//记住是遍历数组，把数组去放到槽里才对。如果判断当前是否在当前槽不对，因为当前槽不一定有自己的数，容易陷入死循环，还有就是要先找一下，是否有超过n-1的数的
class Solution {
public:
    int duplicateInArray(vector<int>& nums) {
        for(auto x : nums)
        {
            if(x > nums.size() - 1 || x < 0)
                return -1;
        }
        for(int i = 0; i < nums.size(); i++)
        {
            while(nums[i] != nums[nums[i]]) swap(nums[i], nums[nums[i]]);
            if(nums[i] != i)
            return nums[i];
        }

        return -1;
    }
};
```

### topk问题，最小的k个数(堆排序，小根堆)

输入n个整数，找出其中最小的k个数。

**注意：**

- 数据保证k一定小于等于输入数组的长度;
- 输出数组内元素请按从小到大顺序排序;

样例

```
输入：[1,2,3,4,5,6,7,8] , k=4

输出：[1,2,3,4]
```

```cpp
class Solution {
public:
    vector<int> getLeastNumbers_Solution(vector<int> input, int k) {
        priority_queue<int> q;//大顶堆,不包含重复元素的，去重的
        for(auto x : input)
        {
            q.push(x);
            if(q.size() > k )
            {
                q.pop();
            }
        }
       vector<int> ans;
       while(q.size())
       {
           ans.push_back(q.top());
           q.pop();
       }
        
        reverse(ans.begin(), ans.end());
        
        return ans;
    }
};
```

