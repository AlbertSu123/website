# Array

TLDR: put everything into a map, then solve

## [Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)

Easy - use a map

## [Valid Anagram](https://leetcode.com/problems/valid-anagram/)

Easy, use a map

## [Two sum](https://leetcode.com/problems/two-sum/)

Easy, use a map

## [Group Anagrams](https://leetcode.com/problems/group-anagrams/)

Easy, just use a nested map

## [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)

Not super easy, but you need to use a nested map and to iterate backwards through the map

## [Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

Needed to submit this a few times, ran into issues with array indexes, which were solved by drawing things out on a piece of paper.
Also, the optimal strategy for me on this problem was to solve it first with brute force(use two hash maps), then it was obvious what work was repeated such that we could do it without any extra space.

## [Valid Sudoku](https://leetcode.com/problems/valid-sudoku/)

Easy, but requires a bit of coding and thinking through how to iterate through 3x3 squares in sudoku.

## [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)

Easy, just make sure to think about how can I iterate through less stuff(use a set), and make sure to iterate through the less stuff.

# Two Pointer
## When should you use two pointers?
Intuition: Use two pointers when the problem needs two or more pieces of information in the answer. 
Two pointer also is commonly used when the array is sorted/when order matters, as is the case with trapping rain water.
Can you evaluate the correctness of the current location of the two pointers?

### Ok, you've decided that two pointers is the best approach, what next?
- Check if sorting the array would make things easier
- How fast should the pointers move? Should they move together? Could we make them move faster than one index at a time? (ie trapping rain water)

## [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)

Easy, but need to be more careful with what variables I'm using. TLDR: need to do leetcode when I'm more awake

## [Two Sum II](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

This is just binary search but not really, since we are only going one element at a time.

Other common two pointer problems:
- Merge two sorted lists: [1, 3, 4, 6] and [2, 4, 7, 7] -> [1, 2, 3, 4, 6, 7, 7].
- Remove duplicates from an array: [1, 1, 1, 4, 4, 5, 5, 5] -> [1, 4, 5].

## [Three Sum](https://leetcode.com/problems/3sum/submissions/884601048/)
Use the two pointer approach after sorting input array and add results to a set
There is an edge case if the entire array is all zeroes, make sure to catch that edge case.
TLDR: Two pointer op

## [Container with the most water](https://leetcode.com/problems/container-with-most-water/submissions/884724541/)
Use the two pointer approach, one pointer at each of the array
If the Area is bigger than maxArea, set maxArea
Move the shorter pointer towards the middle until you hit a taller pointer.

## [Longest Substring Without Repeating Characters]()
Problem: Given a string, find the length of the longest substring without repeating pointers
Solution: Create a map (character -> index of character within string)
1. Use two pointers for the current substring
2. If the right character is not in the map, update the longest substring length
3a. If the right character is in the map and the most recent location of the character is within the window, move the window's left to the most recent location of the character + 1
3b. If the right character is in the map and the most recent location of the character is within the window, update the longest substring length
4. Update the map with the rightmost character and its location

## [Longest Repeating Character Replacement]
Problem: Given a string s and integer k. Return the length of the longest substring containing the same letter you can get if you can replace at most k characters within the substring with any character
Solution:
```
def characterReplacement(self, s: str, k: int) -> int:
    freq = defaultdict(lambda: 0) # character -> number of appearances in window
    most_freq_letter, max_len, l = 0,0,0
    for r in range(len(s)):
        freq[s[r]] += 1
        # clever part
        most_freq_letter = max(most_freq_letter, freq[s[r]])
        letters_to_change = r - l + 1 - most_freq_letter
        if letters_to_change > k:
            # move the left end of the window right 1
            freq[s[l]] -= 1
            l += 1
        max_len = max(max_len, r - l + 1)
    return max_len
```

# Trees

## [Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)
Need to figure out how pointers work here lol
Do multi line assignments not work in c++?

## [Max Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)
For easy tree problems that only require recursion, anyone at berkeley who has taken 61a will demolish these since we literally had to cram this stuff within 5 weeks into learning how to program.

## [Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)
Just think about base case, recursive case, and the problem is pretty easy
The max diameter of a binary tree how long the longest line you could draw on a binary tree without picking up your pencil or retracing your steps.
To calculate this, get the max of {longest left branch + longest right branch, max diameter of the left subtree, max diameter of the right subtree} 

Also, C++ max functions take in max 2 function parameters, so just pass a list in.

## [Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/)
Problem: See if any of left and right subtree heights differ by more than one.
Solution: Create a helper function that calculates subtree height.
if the abs(left subtree height - right subtree height) > 1 or if the left or right subtrees are imbalanced, return false, else return true.

Helper functions are pretty useful in recursive functions since it makes the recursive function's code shorter, making it easier to reason about.

## [Same Tree](https://leetcode.com/problems/same-tree/)
Problem: Check if two binary trees have the same values
Solution:
Check the current roots, if they are the same, recursively check the left and right subtrees

## [Subtree of Another Tree](https://leetcode.com/problems/subtree-of-another-tree/)
Problem: Return true if there is a subtree that matches a subtree of the current tree
Solution: Do nullChecks, if the trees are the same return true, else recurse on left and right subtrees

Make sure to read the problem lol, I solved the wrong problem at first

## [Lowest Common Ancestor of Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)
Thought this was the lowest common ancestor of binary tree at first, started to learn dfs lol

## [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal)
Problem: Return an array of all the nodes in each level of a binary tree
Solution: Create a queue, get all the elements in the queue, pop that many elements and add that to a row array, then insert elements back into the queue.

## [Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)
Problem: Return all the nodes on a binary tree that you would see if viewing it from the right
Solution: Do level order traversal, but only save one element from each row, the last element.
Try to figure out which algorithm will work, instead of hoping some recursive bullshit will work.

## [Valid Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)
Problem: Check if a tree is a valid binary search tree
Solution: Create an array using in order traversal, then check if elements are ascending
Try to figure out how to do in order traversal first.

## [Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)
Problem: Return the Kth smallest element in a BST
Solution: Use in order traversal and return the kth element
Study up on in order traversal lol
```
stack<TreeNode*> s;
TreeNode* p = root;
while (p != NULL || !s.empty()){
    while (p != NULL){
        s.push(p);
        p = p->left;
    }
    p = s.top();
    s.pop();
    // Do something with p->val here
    p = p->right;
}
```

# Dynamic Programming

## When should you use dynamic programming?
When you see subproblems that would be helpful to know the answer to before the main problem

## [Climbing Stairs](https://leetcode.com/problems/climbing-stairs)
Problem: You can go up one stair or two stairs every step. How many ways can you go up n stairs?
Solution: Build a dict(# of stairs left, number of ways) bottom up

Lesson: If the immediate solution doesn't come to mind, solve it recursively first

## [Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/)
Problem: You are given an integer array cost where cost[i] is the cost of ith step on a staircase. Once you pay the cost, you can either climb one or two steps.
Solution: Build a dict(step, min cost to reach step)

Lesson: Recursive solution first

## [House Robber](https://leetcode.com/problems/house-robber/)
Problem: Given an array of house values, maximize the amount robbed. If you rob two consecutive houses, you go to jail.
Solution: Build a dict(index => max value possible at this index)

Lesson: Check for edge cases. In this case it would be if the array had less than 2 elements

## [House Robber II](https://leetcode.com/problems/house-robber-ii/)
Problem: Same as house robber one but the houses are in a circle, so if you rob house 1 you can't rob house n and vice versa

Solution: Same thing as house robber but call the house robber solver twice, one for [0:n-1], one for [1:n]

## [Longest Palindromic Substring]()
Problem: Given a string in s, return the longest palindromic substring in s
Solution: 

# Bitwise Operator

## [Reverse Integer]()
Problem: Given a signed 32 bit integer x, return x with the digits reversed. If reversing x causes the value to go outside [-2**31, 2**31 - 1], return 0.

Solution:
1. Find if x is negative or positive
2. 
```
While x:
    result = result * 10 + x % 10
    x //= 10
```
3. Add multiply result by symbol

## [Single Number](https://leetcode.com/problems/single-number/)
Problem: Given an array of numbers, find the single number that is not repeated twice
Solution: Xor everything together, the final number is the answer.

## [Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/)
Problem: Return the number of 1 bits in an integer
Solution: Iterate through the number and & it with 1

# Linked list

## [Add Two Numbers]()
Problem: Given two linked lists representing two integers. The digits are stored in reverse order and each of the nodes contain a single digit. Add the two numbers and return the sum as a linked list.

ie 2->4->3 + 5->6->4 = 7->0->8

Solution:
Create a helper function that recursively returns the solution.
helper(list1, list2, carry)
1. If both lists are null and carry is 0, return an empty list
2. If both lists are null and carry is non-zero, append the carry to the end of the list
3. If l1 is not null and l2 is null, append l1.val + carry % 10 and recursively call the function with carry = max(l1.val + carry - 10, 0)
4. Repeat but with l2
5. Otherwise, add the first elements of both lists and the carry, append this to the end of the list, and recursively call it with carry = (l1.val + l2.val + carry) // 10

## [Remove Nth Node from End of List]()
Problem: Given the head of a linked list, remove the nth node from the end of the list and return its head
ie 1->2->3->4->5 n=2 => 1->2->3->5
Solution:
1. Use two pointers that both start off at head
2. Advance the first pointer by n
3. Advance both pointers simultaneously until the first pointer reaches end.
4. Remove the node at the second pointer

## [Merge k sorted lists]()

# Stack

## [Min Stack]
Problem: Implement a stack that supports retrieving the minimum element in constant time and push, pop, top
Solution: Create a linked list with three fields in each node: min, val, next