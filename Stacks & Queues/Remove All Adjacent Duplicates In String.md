# Remove All Adjacent Duplicates In String
# 1047. Remove All Adjacent Duplicates In String

### Question

You are given a string s consisting of lowercase English letters. A duplicate removal consists of choosing two adjacent and equal letters and removing them.

We repeatedly make duplicate removals on s until we no longer can.

Return the final string after all such duplicate removals have been made. It can be proven that the answer is unique

### Example 1:

Input: s = "abbaca"
Output: "ca"
Explanation: 
For example, in "abbaca" we could remove "bb" since the letters are adjacent and equal, and this is the only possible move.  
The result of this move is that the string is "aaca", of which only "aa" is possible, so the final string is "ca".

### Example 2:

Input: s = "azxxzy"
Output: "ay"

### Soln 1

```ruby
 public String removeDuplicates(String S) {
    
        StringBuilder sb = new StringBuilder(S);
        
        for(int i=0; i < S.length()-1; i++)
            if(S.charAt(i) == S.charAt(i+1))
                return removeDuplicates(S.substring(0, i) + S.substring(i + 2));
        
        return sb.toString();
    }
```
Soln 2

```ruby
public String removeDuplicates(String str) { // JD solution using stack
        
        if(str == null || str.length()<2)
            return str;
        
        Stack<Character> st = new Stack<>();
		int i = 0; 
		StringBuilder sb = new StringBuilder();
		while(i < str.length()) {
			if(st.isEmpty()) 
				st.add(str.charAt(i));
			else if(str.charAt(i) == st.peek()) 
				st.pop();
			else 
				st.add(str.charAt(i));
            i++;
		}
		
		while(!st.isEmpty()) 
			sb.append(st.pop());
		
		return sb.reverse().toString();
    }
```

### 2nd version
https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string-ii/
