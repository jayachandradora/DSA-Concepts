# Stacks/Queues: Monotonic stacks, bracket problems

Stacks and Queues are fundamental data structures widely used in algorithm design and coding problems. Monotonic stacks and bracket problems are common subtopics within this category. Let's dive into the core problems related to **monotonic stacks**, **bracket problems**, and their use cases along with solutions.

### 1. **Monotonic Stack Problems**

A **monotonic stack** is a stack that is either **monotonically increasing** or **monotonically decreasing**. It's often used to solve problems where you need to efficiently find the next or previous element with certain properties (e.g., the next greater element).

#### Core Problems and Solutions

---

#### **Problem 1: Next Greater Element (NGE)**

**Problem:**
Given an array of integers, find the next greater element for each element in the array. The next greater element is the first element on the right that is greater than the current element. If no such element exists, output -1.

**Example:**
Input: `[4, 5, 2, 10]`
Output: `[5, 10, 10, -1]`

**Solution:**
We can use a **monotonic decreasing stack** to efficiently find the next greater element:
- Traverse the array from right to left.
- Maintain a stack that stores elements in decreasing order.
- For each element, pop elements from the stack that are smaller than or equal to the current element, and the top of the stack (if it exists) will be the next greater element.

```python
def nextGreaterElement(nums):
    stack = []
    result = [-1] * len(nums)
    
    for i in range(len(nums) - 1, -1, -1):
        while stack and stack[-1] <= nums[i]:
            stack.pop()
        if stack:
            result[i] = stack[-1]
        stack.append(nums[i])
    
    return result

# Example usage:
print(nextGreaterElement([4, 5, 2, 10]))  # Output: [5, 10, 10, -1]
```

---

#### **Problem 2: Largest Rectangle in Histogram**

**Problem:**
Given an array of integers representing the heights of bars in a histogram, find the area of the largest rectangle that can be formed using consecutive bars.

**Solution:**
This problem can be solved using a **monotonic increasing stack**:
- Traverse through the bars and maintain a stack of indices where the heights are in increasing order.
- When encountering a smaller height than the bar at the stack's top, calculate areas for rectangles formed with bars from the stack.

```python
def largestRectangleArea(heights):
    stack = []
    max_area = 0
    heights.append(0)  # Add a 0 to the end to flush out remaining bars in stack
    
    for i in range(len(heights)):
        while stack and heights[i] < heights[stack[-1]]:
            h = heights[stack.pop()]
            w = i if not stack else i - stack[-1] - 1
            max_area = max(max_area, h * w)
        stack.append(i)
    
    return max_area

# Example usage:
print(largestRectangleArea([2, 1, 5, 6, 2, 3]))  # Output: 10
```

---

### 2. **Bracket Problems**

Bracket problems often involve validating the balance of parentheses, square brackets, and curly braces. These problems can typically be solved using a stack.

#### Core Problems and Solutions

---

#### **Problem 3: Valid Parentheses**

**Problem:**
Given a string containing just the characters `'(', ')', '{', '}', '[', ']'`, determine if the input string is valid. An input string is valid if:
- Open brackets must be closed by the same type of bracket.
- Open brackets must be closed in the correct order.

**Example:**
Input: `"{[()]}"`  
Output: `True`

**Solution:**
We can use a stack to track opening brackets and ensure they match the closing ones.

```python
def isValid(s):
    stack = []
    mapping = {')': '(', '}': '{', ']': '['}
    
    for char in s:
        if char in mapping:  # If it's a closing bracket
            top_element = stack.pop() if stack else '#'
            if mapping[char] != top_element:
                return False
        else:
            stack.append(char)
    
    return not stack  # Stack should be empty if all open brackets are matched

# Example usage:
print(isValid("{[()]}"))  # Output: True
print(isValid("{[(])}"))  # Output: False
```

---

#### **Problem 4: Longest Valid Parentheses**

**Problem:**
Given a string containing just the characters `'('` and `')'`, find the length of the longest valid (well-formed) parentheses substring.

**Example:**
Input: `"(()())"`  
Output: `6`

**Solution:**
We can use a **stack** to track indices and calculate the length of valid parentheses substrings.

```python
def longestValidParentheses(s):
    stack = [-1]  # Initialize stack with -1 to handle edge case of valid substring starting from index 0
    max_length = 0
    
    for i, char in enumerate(s):
        if char == '(':  # Push the index of opening bracket
            stack.append(i)
        else:  # Closing bracket
            stack.pop()  # Pop the last opening bracket index
            if stack:
                max_length = max(max_length, i - stack[-1])  # Calculate the valid length
            else:
                stack.append(i)  # Push the current index as a base for future valid substrings
    
    return max_length

# Example usage:
print(longestValidParentheses("(()())"))  # Output: 6
```

---

### Use Cases of Monotonic Stacks and Bracket Problems:

1. **Monotonic Stack Use Cases:**
   - **Stock Span Problem**: For each day, calculate the number of consecutive days before the current day with a lower stock price.
   - **Temperature Problem**: For each day, find the number of days until a warmer temperature.
   - **Histogram/Rectangle problems**: The largest rectangle in a histogram problem can be solved efficiently using a monotonic stack.

2. **Bracket Problems Use Cases:**
   - **Syntax Validation**: Ensuring that the syntax in code or mathematical expressions is correct (balanced parentheses, brackets, and braces).
   - **Expression Parsing**: Many compilers and interpreters use stacks to parse and evaluate mathematical or logical expressions.
   - **XML/HTML Tag Parsing**: Checking if HTML tags are correctly nested and balanced.

These problems highlight the usefulness of stacks in scenarios involving nested structures, comparisons, and range-based queries, making them a key tool in problem-solving for coding interviews and competitive programming.
