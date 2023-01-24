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
