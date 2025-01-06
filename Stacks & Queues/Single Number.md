# 136. Single Number


### Question

Given a non-empty array of integers nums, every element appears twice except for one. Find that single one.
You must implement a solution with a linear runtime complexity and use only constant extra space.

### Example 1
Input: nums = [2,2,1]
Output: 1

### Example 2

Input: nums = [4,1,2,1,2]
Output: 4

### Example 2

Input: nums = [1, 1, 2, 3, 3, 4, 4, 8, 8]
Output: 2

### Soln 1

```ruby
public int singleNumber(int[] nums) {
        int lo = 0;
		int hi = nums.length - 1;
		while (lo < hi) {
			int mid = lo + (hi - lo) / 2;
			if (mid % 2 == 1)
				mid--;
			if (nums[mid] == nums[mid + 1])
				lo = mid + 2;
			else
				hi = mid;
		}
		return nums[lo];
    }
}

```
### Soln 2
```ruby

 public int singleNumber(int[] nums) {
        
       Map<Integer, Integer> map = new HashMap<Integer, Integer>();
		for (int num : nums) {
			if (map.containsKey(num)) {
				map.put(num, 2);
			} else
				map.put(num, 1);
		}
		for (Integer key : map.keySet()) {
			if (map.get(key) == 1) {
				return key;
			}
		}
		return 0;
    }
    
```

### Soln 3

```ruby
public int singleNumber(int[] nums) {
        int res = 0;
        
        for(int in : nums){
            res ^= in;
        }
        
        return res;
    }
```ruby


