# Leetcode Trick Notes

[#1](https://leetcode.com/problems/two-sum/) Two Sum

* iterate through input, adding num and its idx into a hashmap
* iterate again, this time calculate the complement. check if the complement exists in the map, and return the idx pair

[#3](https://leetcode.com/problems/longest-substring-without-repeating-characters/) Longest SubString Without Repeating Characters

  * use sliding window
  * maintain map of chars and a counter - if added char count is greater than 1, means there are dupes
  * inner while loop condition maintains counter > 1
  * curr substring is endPtr - startPtr - 1

[#5](https://leetcode.com/problems/longest-palindromic-substring/) Longest Palindromic Substring

* dynamic programming
* 
  ```
  state can be defined like so:

  boolean[][] dp = new boolean[s.length()][s.length()]
  dp[i][j] is true if s.substring(i, j) is a palindrome
  ```
* all single chars are palindromes 
  `dp[i][i] = true`
* all strings length 2 are palindromes: 
  
  `dp[i][[i + 1] = true iff dp[i][i + 1] && s.charAt(i) == s.charAt(i + 1)`

* all strings length 3 and greater are palindromes iff:

  `dp[i][j] = dp[i + 1][j - 1] && s.charAt(i) == s.charAt(j)`

[#7](https://leetcode.com/problems/reverse-integer/) Reverse Integer

  * simple math based strategy. get last digit with:
    `lastDigit = num % 10`
  * build num with:
    `reversed = reversed * 10 + lastDigit`
  * before we build reversed, we need to check bounds
    ```
    if (reversed > Integer.MAX_VALUE / 10 || reversed < Integer.MIN_VALUE / 10) {         
      return 0; 
    }
    ```
  * If the reversed val is greater than `Integer.MAX_VALUE / 10` or less than `Integer.MIN_VALUE / 10` then next digit added will case reversed to overflow. We need to return 0 here.

[#11](https://leetcode.com/problems/container-with-most-water/) Container with Most Water

  * two pointers - left starts at beginning, right starts at end
  * pointer movement strategy is as follows:
    
    ```
    int currArea = Math.min(heights[left], heights[right]) * right - left;
    maxArea = Math.max(maxArea, currArea);
    if (heights[left] <= heights[right]) {
      left++;
    }
    else {
      right--;
    }
    ```

[#12](https://leetcode.com/problems/integer-to-roman/) Integer to Roman

  * create a mapping of single roman numerals (I, V, X, L, C, D, M) and two letter roman numerals (IV, IX, XL, XC, CD, CM) that map to their numerical value
  * trick - greedily subtract the largest roman numeral possible, and build the roman numeral as you go along

[#14](https://leetcode.com/problems/longest-common-prefix/) Longest Common Prefix

  * sort array
  * if there is a common prefix, first and last will have the same.
  * compare each char and if there is a match add it to the output stringbuilder
  * stringbuilder.toString() output is your prefix

[#15](https://leetcode.com/problems/3sum/) Three Sum

  * sort the array
  * if value greater than 0, we can break because its impossible for the remaining values (since its sorted) to sum to 0
  * if curr num and next num not equal to each other, we can binary search for pair that sums to zero with the current index
  * remember that in the binary search helper you need to iterate until we don't see any duplicate numbers

[#19](https://leetcode.com/problems/remove-nth-node-from-end-of-list/) Delete Nth Node from End of Linked List

  * Nth node from end - can calculate index to delete using len(list) - n
  * maintain a sentinel node and ptr to sentinel node and build new list
  * if currIndex == len(list) - n, skip node.
  * one pass solution: maintain two ptrs. ptr1 is n + 1 (1 indexed) nodes ahead of ptr2. advance ptr1 and ptr2 until ptr1 is null
  * ptr2 now points to node that needs to be deleted.

[#20](https://leetcode.com/problems/valid-parentheses/) Valid Parentheses

  * use a stack - when we see any type of open paren/bracket/brace, we push the closed version on to the stack. if we see a closed paren/bracket/brace:
    
    * if stack is empty -> return false
    * if peek is not equal to current paren/bracket/brace then we return false
  
[#21](https://leetcode.com/problems/merge-two-sorted-lists/) Merged Two Sorted Lists

  * recursive function

    * if both lists null --> return null
    * if one list null --> return other
    * if list1.val < list2.val 
      
      * create dummy node with list1.val
      * dummy.next = recursiveMerge(list1.next, list2)
    * else

      * create dummy node with list2.val
      * dummy.next = recursiveMerge(list1, list2.next)

[#23](https://leetcode.com/problems/merge-k-sorted-lists/) Merge K Sorted Lists
  
  * leverage PriorityQueue
  * iterate through each LinkedList then add value to PriorityQueue Library
  * iterate through priorityqueue and build new LinkedList

[#25](https://leetcode.com/problems/reverse-nodes-in-k-group/) Reverse nodes in K group

  * figure out length of list - if length > k return head, if k == length return reversed of head
  * using dummy ptr build the first k nodes, maintain pointer to the rest of the nodes.
  * recursive call on the rest of the nodes, and connect the result to the end of the reversed k nodes

[#34](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/) Find First and Last Position of Element in Sorted Array
  
  * use binary search - two functions, one for start idx and one for end idx
  * in each binary search maintain an int mapped to largest or smallest int - when you find occurence of target, compare the mid idx to the curr smallest idx.
  * Could also be asked like so - Find all counts of target in sorted Array with Duplicates (simply sub the end and start idx)


[#198](https://leetcode.com/problems/house-robber/) House Robber

  * dynamic programming
  * edge cases: if len houses is 1, return first house. if len houses is 2, return max of first and second house
  * init a int[] as dp. dp[0] = house[0], dp[1] = house[1]
  * from house idx = 2 to the end
    ```
    dp[i] = Math.max(house[i] + dp[i-2], dp[i-1])
    ```

[#204](https://leetcode.com/problems/count-primes/) Count Primes

  * sieve of eratosthenes - init an array of length n. for each prime number, we mark the multiples of this prime number as not prime in the array
  * ```
    for (int i = 2; i * i < n; i++) {
      for (int j = i * i; j < n; j += i) {
        isPrime[j] = false;
      }
    }
    ```
  * outer loop termination condition: we only need to check up to sqrt(n) or i * i < n.
  * inner loop starts at i * i because previous multiples would have been marked as not prime by a smaller number. 

    * when you process i = 2, all multiples of 2 are not prime (2, 4, 6, ...)
    * when you process i = 3, all multiples of 3 are not prime (3, 9, 15, ...) (notice that 2 has marked some already as not prime (12, 18, 24) that are also multiples of 3)
    * all the unmarked numbers are non-prime 

[#337](https://leetcode.com/problems/house-robber-iii/) House Robber III
  
  * create a helper function that outputs int[], output[0] = max if rob curr node, output[1] = max if skip curr node.
  * helper function calls children first - use left[] and right[] to build curr[]
  * 
    ```
    curr[0] = root.val + left[1] + right[1]
    curr[1] = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);
    ```

[#702](https://leetcode.com/problems/search-in-a-sorted-array-of-unknown-size/) Search in sorted array of unknown size

  * first find the range of the search. Strategy is set the left to old right, and right is multiplied by 2. complexity is O(log n)
    ```
    left = 0, right = 1;
    while (reader.get(right) < target) {
      left = right;
      right *= 2;
    }
    ```
  * standard binary search after that

[#1314](https://leetcode.com/problems/matrix-block-sum/) Matrix Block Sum
  
  * create a buffer matrix m x n
  * create a prefix sum of rows and cols
  * ```
    int[][] dp = new int[m][n];
    for (int i = 0; i < m; i++) {
        dp[i][0] = mat[i][0];
        for (int j = 1; j < n; j++) {
            dp[i][j] = dp[i][j-1] + mat[i][j];
        }
    }
    
    for (int i = 0; i < m; i++) {
        for (int j = 1; j < n; j++) {
            dp[j][i] = dp[j-1][i] + dp[j][i];
        }
    }
    ```
  * create range of r and c
    ```
    r1 = Math.max(r - k, 0), c1 = Math.max(c - k, 0);
    r2 = Math.max(r + k, m-1), c2 = math.max(c + k, n-1);
    ```
  * calculate the ans at index pair (i, j) like so
    ```
    ans[i][j] = dp[r2][c2];
    if r1 == 0 && r2 == 0
      continue
    if r1 - 1 >= 0:
      ans[i][j] -= dp[r1-1][c2] <-- prefix sum of row above our rectangle
    if c1 - 1 >= 0:
      ans[i][j] -= dp[r2][c1-1] <-- prefix sum of col before our rectangle
    if (r1 - 1 >= 0 && c1 - 1 >= 0):
      ans[i][j] += dp[r1-1][c1-1] <-- we double subtracted the value thats the diagonal to the left of our starting bound of our rectangle. we need to add this back
    ```

[#1586](https://leetcode.com/problems/binary-search-tree-iterator-ii/) Binary Search Tree Iterator II

  * perform a inorder traversal with stack and build an arraylist of tree values (sorted)
  * init a pointer to -1
  * increment pointer when next() is called, decrement pointer when prev() is called

