
### 001

**Two Sum** https://leetcode.com/problems/two-sum/

Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

Example:

```
Given nums = [2, 7, 11, 15], target = 9,
Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

**分析**

要求：给出一个数字数组和一个目的值，返回数组中两个相加为目的值的下标。这里假设有且仅有一个解，不能用相同的元素两次。

1. 首先我们把num数组中的数据以哈希表的形式存放起来，哈希表中的key为num值，value是num下标，即`hash_table[num[index]] = index;`
2. 我们在哈希表中查找`y=target-x`是否存在，如果存在则取出x和y的下标即可。

**解决**：时间复杂度O(N) 、空间复杂度O(N)

注意：虽然题目要求不能用相同元素，但是submit时提交失败，没办法只能在添加的时候，处理了一下相同元素的情况。

```
public class Solution_001 {
    public static int[] twoSum(int[] arrays, int target) {
        HashMap<Integer, Integer> hashMap = new HashMap<Integer, Integer>();
        for (int i = 0; i < arrays.length; i++) {
            int temp = target - arrays[i];
            if (hashMap.containsKey(temp)) {
                int index = hashMap.get(temp);
                if (arrays[i] + arrays[index] == target) {
                    return new int[]{index, i};
                }
            } else {
                hashMap.put(arrays[i], i);
            }
        }
        for (int i = 0; i < arrays.length; i++) {
            int temp = target - arrays[i];
            if (hashMap.containsKey(temp)) {
                int index1 = hashMap.get(temp);
                int index2 = hashMap.get(arrays[i]);
                if (index1 != index2) {
                    if (index1 > index2) {
                        return new int[]{index2, index1};
                    } else {
                        return new int[]{index1, index2};
                    }
                }
            }
        }
        return null;
    }

    public static void main(String[] args) {
//        int[] arrays = {3, 2, 4};
        int[] arrays = {3, 3};
        int target = 6;
        int[] result = twoSum(arrays, target);
        System.out.println(result[0] + "," + result[1]);
    }
}
```

### 002

