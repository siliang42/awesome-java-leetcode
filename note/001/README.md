# [Two Sum][title]

## Description

Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.

You may assume that each input would have ***exactly*** one solution, and you may not use the same element twice.

**Example:**

```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

## 给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

## 你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

**示例:**
```
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```
**Tags:** Array, Hash Table


## 思路 0

题意是让你从给定的数组中找到两个元素的和为指定值的两个索引，最容易的当然是循环两次，复杂度为 `O(n^2)`，首次提交居然是 2ms，打败了 100% 的提交，谜一样的结果，之后再次提交就再也没跑到过 2ms 了。
暴力法很简单，遍历每个元素 xx，并查找是否存在一个值与 target - xtarget−x 相等的目标元素。
```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        for (int i = 0; i < nums.length; ++i) {
            for (int j = i + 1; j < nums.length; ++j) {
                if (nums[i] + nums[j] == target) {
                    return new int[]{i, j};
                }
            }
        }
        return null;
    }
}
```
复杂度分析：

时间复杂度：O(n^2)O(n2)，
对于每个元素，我们试图通过遍历数组的其余部分来寻找它所对应的目标元素，这将耗费 O(n)O(n) 的时间。因此时间复杂度为 O(n^2)O(n2)。

空间复杂度：O(1)O(1)。

## 思路 1

利用 HashMap 作为存储，键为目标值减去当前元素值，索引为值，比如 `i = 0` 时，此时首先要判断 `nums[0] = 2` 是否在 map 中，如果不存在，那么插入键值对 `key = 9 - 2 = 7, value = 0`，之后当 `i = 1` 时，此时判断 `nums[1] = 7` 已存在于 map 中，那么取出该 `value = 0` 作为第一个返回值，当前 `i` 作为第二个返回值，具体代码如下所示。

两遍哈希表
为了对运行时间复杂度进行优化，我们需要一种更有效的方法来检查数组中是否存在目标元素。如果存在，我们需要找出它的索引。保持数组中的每个元素与其索引相互对应的最好方法是什么？哈希表。
通过以空间换取速度的方式，我们可以将查找时间从 O(n)O(n) 降低到 O(1)O(1)。哈希表正是为此目的而构建的，它支持以 近似 恒定的时间进行快速查找。我用“近似”来描述，是因为一旦出现冲突，查找用时可能会退化到 O(n)O(n)。但只要你仔细地挑选哈希函数，在哈希表中进行查找的用时应当被摊销为 O(1)O(1)。
一个简单的实现使用了两次迭代。在第一次迭代中，我们将每个元素的值和它的索引添加到表中。然后，在第二次迭代中，我们将检查每个元素所对应的目标元素（target - nums[i]target−nums[i]）是否存在于表中。注意，该目标元素不能是 nums[i]nums[i] 本身！
复杂度分析：
```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int len = nums.length;
        HashMap<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < len; ++i) {
            final Integer value = map.get(nums[i]);
            if (value != null) {
                return new int[] { value, i };
            }
            map.put(target - nums[i], i);
        }
        return null;
    }
}
```

时间复杂度：O(n)O(n)，
我们把包含有 nn 个元素的列表遍历两次。由于哈希表将查找时间缩短到 O(1)O(1) ，所以时间复杂度为 O(n)O(n)。

空间复杂度：O(n)O(n)，
所需的额外空间取决于哈希表中存储的元素数量，该表中存储了 nn 个元素。

## 思路 2
一遍哈希表
事实证明，我们可以一次完成。在进行迭代并将元素插入到表中的同时，我们还会回过头来检查表中是否已经存在当前元素所对应的目标元素。如果它存在，那我们已经找到了对应解，并立即将其返回。

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            if (map.containsKey(complement)) {
                return new int[] { map.get(complement), i };
            }
            map.put(nums[i], i);
        }
        throw new IllegalArgumentException("No two sum solution");
    }
}
```
复杂度分析：

时间复杂度：O(n)O(n)，
我们只遍历了包含有 nn 个元素的列表一次。在表中进行的每次查找只花费 O(1)O(1) 的时间。

空间复杂度：O(n)O(n)，
所需的额外空间取决于哈希表中存储的元素数量，该表最多需要存储 nn 个元素。
## 结语

如果你同我一样热爱数据结构、算法、LeetCode，可以关注我 GitHub 上的 LeetCode 题解：[awesome-java-leetcode][ajl]



[title]: https://leetcode.com/problems/two-sum
[ajl]: https://github.com/Blankj/awesome-java-leetcode
