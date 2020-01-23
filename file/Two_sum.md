## 1. Two Sum

![two_sum](../image/two_sum.png)

### 暴力解

```
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int [] ans= new int [2];
        for(int i=0;i<nums.length;i++){
            for(int j=i+1;j<nums.length;j++){
                if (nums[i]+nums[j] == target){
                    ans[0]=i;
                    ans[1]=j;
                    return ans;
                }
            }
        }
        return ans;
    }
}
```

### HashMap存值

暴力解直接两层循环进行查找，但是相应的，第二层循环总是被重复，我们可以使用HashMap对直接遍历过的值进行存储，后续我们只需要判断目标值与当前值的差值是否在HashMap中即可。

```
class Solution {
    public int[] twoSum(int[] nums, int target) {
    Map<Integer,Integer> map=new HashMap<>();
    for(int i=0;i<nums.length;i++){
        int sub=target-nums[i];
        if(map.containsKey(sub)){
            return new int[]{i,map.get(sub)};
        }
        map.put(nums[i], i);
    }
    throw new IllegalArgumentException("No two sum solution");
    }
}
```



