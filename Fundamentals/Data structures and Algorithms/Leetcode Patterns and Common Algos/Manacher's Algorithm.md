## Problems
- Finds Longest Palindromic Substring in $O(n)$ time

## Key idea
- Can expand from center to check for palindrome since they are symmetric
- **Transforming string**
	- To account for even and odd length strings, we convert an input string e.g. `abbabc` to `#a#b#b#a#b#c`
	- Palindromes in transformed string guaranteed to have odd length (`a#b#b#a` has 7 chars)
- **Mirror Property**
	- If palindrome symmetric about $c$, left half mirrors right half
	- For palindrome with center at position $i$, there exists a mirrored position $i_{mirror} = 2C-i$ 
- **Iteration**
	- Check if current index is within radius
		- If it is, initialize palindrome radius at index to be minimum of distance from radius and palindrome radius at mirror
	- Expand palindrome radius if conditions are met
	- if index + palindrome radius is greater than current radius, update current radius and center
## Code
```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        string t = "#";
        for (char c: s){
            t += c;
            t += "#";
        }

        int n = t.size();
        vector<int> palindrome_radius (n, 0);
        int center = 0;
        int radius = 0;

        for (int i=0;i<n;i++){
            int mirror = 2 * center - i;
            if (i < radius){
                palindrome_radius[i] = min(radius - i, palindrome_radius[mirror]);
            }

            while (i + 1 + palindrome_radius[i] < n
                    && i - 1 - palindrome_radius[i] >= 0
                    && t[i+1+palindrome_radius[i]] == t[i-1-palindrome_radius[i]]){
                palindrome_radius[i]++;
            }

            if (i + palindrome_radius[i] > radius){
                center = i;
                radius = i + palindrome_radius[i];
            }
        }

        int max_length = 0;
        int idx = 0;
        for (int i=0;i<n;i++){
            if (palindrome_radius[i] > max_length){
                max_length = palindrome_radius[i];
                idx = i;
            }
        }

        int start_index = (idx - max_length) / 2;
        string longest_palindrome = s.substr(start_index, max_length);
        return longest_palindrome;
    }
};
```

