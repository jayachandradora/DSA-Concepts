# Contiguous Elements

The **Sliding Window** technique is a powerful approach used in solving problems that involve subarrays or substrings, especially when dealing with **contiguous elements**. The main idea is to maintain a window of elements (usually between two pointers) and adjust the window size to achieve the desired result, while **scanning through the array or string** in a linear fashion.

### Common Problems for Sliding Window Technique

Sliding Window is frequently used in problems involving:
- Finding subarrays with a specific sum.
- Maximum or minimum in a subarray.
- Longest or shortest subarrays or substrings satisfying some condition.

I'll share **use cases**, **examples**, and **Java solutions** for some of the common sliding window problems.

---

### **1. Maximum Sum Subarray of Size K**

#### **Problem:**
Given an array of integers, find the maximum sum of a subarray of size `k`.

#### **Use Case:**
This problem appears frequently in scenarios like:
- **Sliding window average**: Tracking moving averages over time.
- **Performance tracking**: Analyzing the sum of elements in time windows (e.g., summing daily revenue for a given week in a sales analysis system).

#### **Example Problem:**
Given an array `arr = [2, 1, 5, 1, 3, 2]` and `k = 3`, find the maximum sum of any subarray of size 3.

#### **Solution:**

```java
public class MaxSumSubarray {
    public static int maxSumSubarray(int[] arr, int k) {
        int windowSum = 0;
        int maxSum = 0;
        
        // Compute the sum of the first window
        for (int i = 0; i < k; i++) {
            windowSum += arr[i];
        }
        
        maxSum = windowSum;
        
        // Slide the window and calculate the sum of remaining windows
        for (int i = k; i < arr.length; i++) {
            windowSum += arr[i] - arr[i - k];  // Slide the window
            maxSum = Math.max(maxSum, windowSum);
        }
        
        return maxSum;
    }

    public static void main(String[] args) {
        int[] arr = {2, 1, 5, 1, 3, 2};
        int k = 3;
        System.out.println("Maximum sum of subarray of size " + k + ": " + maxSumSubarray(arr, k)); // Output: 9
    }
}
```

#### **Explanation:**
- We compute the sum of the first `k` elements.
- Then, we slide the window by subtracting the element that goes out of the window (at index `i - k`) and adding the new element that comes into the window (at index `i`).
- **Time complexity**: O(n) where n is the length of the array.
- **Space complexity**: O(1) because we're using only a few extra variables.

---

### **2. Longest Substring Without Repeating Characters**

#### **Problem:**
Given a string, find the length of the longest substring without repeating characters.

#### **Use Case:**
- **Text processing**: Analyzing text or logs where duplicate characters or patterns need to be avoided.
- **Data cleaning**: Removing redundant or duplicate information from large datasets.

#### **Example Problem:**
Given the string `"abcabcbb"`, the length of the longest substring without repeating characters is 3 ("abc").

#### **Solution:**

```java
import java.util.HashSet;

public class LongestSubstringWithoutRepeating {
    public static int lengthOfLongestSubstring(String s) {
        int left = 0;
        int maxLength = 0;
        HashSet<Character> set = new HashSet<>();
        
        for (int right = 0; right < s.length(); right++) {
            // If the character at right is already in the set, shrink the window from the left
            while (set.contains(s.charAt(right))) {
                set.remove(s.charAt(left));
                left++;
            }
            
            set.add(s.charAt(right));  // Add the new character to the set
            maxLength = Math.max(maxLength, right - left + 1);  // Update maxLength
        }
        
        return maxLength;
    }

    public static void main(String[] args) {
        String s = "abcabcbb";
        System.out.println("Longest substring without repeating characters: " + lengthOfLongestSubstring(s)); // Output: 3
    }
}
```

#### **Explanation:**
- We maintain a sliding window with two pointers (`left` and `right`) and a set to track unique characters.
- If a duplicate character is encountered, we shrink the window by moving the `left` pointer until the duplicate is removed.
- **Time complexity**: O(n), where n is the length of the string (since we traverse the string once).
- **Space complexity**: O(min(n, m)), where n is the length of the string and m is the size of the character set (e.g., 128 for ASCII).

---

### **3. Minimum Size Subarray Sum**

#### **Problem:**
Given an array of positive integers and a target sum, find the minimal length of a subarray whose sum is greater than or equal to the target sum.

#### **Use Case:**
- **Financial analysis**: Finding the smallest window (time period) where a stock or portfolio achieves a specific target return.
- **Threshold calculations**: In situations where you need to find the minimum number of elements to exceed a threshold (e.g., in machine learning or data filtering).

#### **Example Problem:**
Given the array `[2, 3, 1, 2, 4, 3]` and target sum `7`, the minimal length of the subarray whose sum is greater than or equal to 7 is `2` (subarray `[4, 3]`).

#### **Solution:**

```java
public class MinSubarraySum {
    public static int minSubArrayLen(int target, int[] nums) {
        int n = nums.length;
        int minLength = Integer.MAX_VALUE;
        int windowSum = 0;
        int left = 0;

        for (int right = 0; right < n; right++) {
            windowSum += nums[right];  // Expand the window

            // Shrink the window from the left as long as the sum is greater than or equal to the target
            while (windowSum >= target) {
                minLength = Math.min(minLength, right - left + 1);
                windowSum -= nums[left];
                left++;
            }
        }
        
        return minLength == Integer.MAX_VALUE ? 0 : minLength;  // If no valid subarray is found
    }

    public static void main(String[] args) {
        int[] arr = {2, 3, 1, 2, 4, 3};
        int target = 7;
        System.out.println("Minimum length of subarray: " + minSubArrayLen(target, arr)); // Output: 2
    }
}
```

#### **Explanation:**
- We slide the window from the left and right, expanding it until the sum is greater than or equal to the target.
- Once we have a valid window, we try to shrink it from the left to find the minimum length.
- **Time complexity**: O(n), where n is the number of elements in the array.
- **Space complexity**: O(1), since we're using only a few variables.

---

### **4. Longest Substring with At Most K Distinct Characters**

#### **Problem:**
Given a string, find the length of the longest substring that contains at most `k` distinct characters.

#### **Use Case:**
- **Text analysis**: In text processing, it's often required to extract substrings or sections that meet certain diversity criteria, such as extracting a substring that only contains a limited set of characters.
- **Pattern recognition**: Finding the longest substring in a sequence where only a limited number of unique symbols or elements are allowed.

#### **Example Problem:**
Given the string `"eceba"` and `k = 2`, the longest substring with at most 2 distinct characters is `"ece"`, which has a length of `3`.

#### **Solution:**

```java
import java.util.HashMap;

public class LongestSubstringKDistinct {
    public static int lengthOfLongestSubstringKDistinct(String s, int k) {
        int left = 0;
        int maxLength = 0;
        HashMap<Character, Integer> map = new HashMap<>();
        
        for (int right = 0; right < s.length(); right++) {
            map.put(s.charAt(right), map.getOrDefault(s.charAt(right), 0) + 1);

            // If the map contains more than k distinct characters, shrink the window from the left
            while (map.size() > k) {
                map.put(s.charAt(left), map.get(s.charAt(left)) - 1);
                if (map.get(s.charAt(left)) == 0) {
                    map.remove(s.charAt(left));
                }
                left++;
            }
            
            maxLength = Math.max(maxLength, right - left + 1);
        }
        
        return maxLength;
    }

    public static void main(String[] args) {
        String s = "eceba";
        int k = 2;
        System.out.println("Longest substring with at most " + k + " distinct characters: " + lengthOfLongestSubstringKDistinct(s, k)); // Output: 3
    }
}
```

#### **Explanation:**
- We maintain a sliding

 window and a hashmap that stores the frequency of characters within the window.
- If the number of distinct characters exceeds `k`, we shrink the window from the left.
- **Time complexity**: O(n), where n is the length of the string.
- **Space complexity**: O(k), where k is the number of distinct characters in the substring.

---

### Summary of Use Cases and Examples:
The **Sliding Window** technique is widely used in problems involving contiguous subarrays or substrings, such as:
- **Maximum sum subarray** with fixed or sliding window sizes.
- **Longest substrings without repeating characters**.
- **Minimum subarray length with sum constraints**.
- **Longest substring with at most `k` distinct characters**.

By efficiently maintaining a "window" and adjusting its size, we can solve these problems in linear time (O(n)) with constant space (O(1) or O(k)). The sliding window approach helps avoid brute-force solutions that might require nested loops and would otherwise have higher time complexity.
