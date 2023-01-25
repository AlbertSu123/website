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

I think I've cracked the two pointer style problems now!

# Trees

## [Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)
Need to figure out how pointers work her lol
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