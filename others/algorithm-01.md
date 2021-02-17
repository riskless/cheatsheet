### Prefix Sum Algorithm
- It is a simple yet powerful technique that allows us to perform fast calculation on the sume of elements in a given range (called contiguous segments of array).
- A normal algorithm takes O(m*n) time to perform m number of queries to fine range sum on n size array
- Prefix sum algorith takes O(n) time to perform m number of queries to find range sum on n size array.
- Range sum query formula: A[i,j] = A[j] - A[i-1]
- It takes O(n) time to calculate prefix sum array of n size array.
- It takes O(1) time to perform range sum query on n size array.

- [6,3,-2,4,-1,0,-5] -> [6,9,7,11,10,10,5]
```java
int[] A = {6,3,-2,4,-1,0,-5};
for (int i=1;i<n;i++) {
	A[I] = A[i] + A[i-1];
}

// Calculate the sum between range [0,4] -> A[4]
// Calculate the sum between range [0,6] -> A[6]

/* Calculate the sum between range [2,6] */
// sum between range [0,6] = sum between range [0,1] + sum between range [2,6]
// A[6] = A[1] + sum between range [2,6]
// A[6] - A[1] = sum between range [2,6]

/* To calculate the sum between range [i,j] */
// A[i,j] = A[j] - A[i-1]
```

### Array Manipulation
- [Problem statement](https://www.hackerrank.com/challenges/crush/problem)

### Sliding Window Technique
- Problem Statement
	- Given an array of integers n and a positive number k, find the maximum sum of any contiguous subarray of size k.
- Example 1
	- Input: [2,1,5,1,3,2], k=3
	- Output: 9
	- Subarray with maximum sum is [5,1,3]

### Climbing Stairs
- Problem Statement
	- A child is climbing up a staircase with n steps and can hop either 1 step or 2 steps at a time. Implement a method to count how many possible ways the child can clib up the stairs.
	- T(n) = T(n-1) + T(n-2)

- Problem Statement
	- A child is climbing up a staircase with n steps and can hop either 1 step 2 steps, or 3 steps at a time. Implement a method to count how many possible ways the child can clib up the stairs.
	- T(n) = T(n-1) + T(n-2) + T(n-3)

```java
// staris(n)	: 1	2	3	4	5	6		7
// no of ways	: 1	2	3	5	8	13	21
```
### Two Pointer Algorithm 
- Two pointer technique is normally used for searching and it uses two pointer in one loop over the given data structure. This is quite common approach which is used to solve coding problems, mostly related to strings, arrrays and linked lists.
- In order to use two pointers, most of the times the data structure needs to be ordered in some way, which helps us to reduce the time complexity from O(n^2) or O(n^3) to O(n) of jusst one loop with two pointers and search each time just one time. So depending on whether the input string/array is sorted or not, the two pointer method can take O(n logn) time complexity or even better which is O(n).
- Variants of two pointer
	- Opposite directional: One pointer starts from the beginning while the other pointer starts from the end. They move toward each other until they both meet or some condition satify.
		- Two Sum - Input array is sorted
		- Valid Palindrome
		- Move Zeroes
		- Reverse String
		- Remove Element
	- Equi-directional: Both start from the beginning, One slow-runner and the other fast-runner.
		- Find the maximum sum of any contiguous subarray of size k.
		- Find middle node of a linked list
		- Linked List Cycle
		- Longest Substring Without Repeating Characters
		- Remove Duplicates from Sorted Array

### Two Sum - Input array is sorted (Opposite directional)
- Problem Statement
	- Given an array of integers that is already sorted in ascending order, find two numbers such that they add up to a specific target number
- The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2.

- Example 1
	- Input: nums = [2,7,11,15], target = 9
	- Output: [1,2]
	- The sum of 2 and 7 is 9. Therefore index1 = 1, index2 = 2

- Example 2
	- Input: nums = [-3,2,3,3,6,8,15], target = 6
	- Output: [3,4]
	- The sum of 3 and 3 is 6. Therefore index1 = 3, index2 = 4

### Find the maximum sum of any contiguous subarray of size k (Equi-directional)
- Problem Statement
	- Given an array of integers n and a positive number k, find the maximum sum of any contiguous subarray of size k.

- Example 1
	- Input: nums = [2,1,5,1,3,2], k = 3
	- Output: 9
	- Subarray with maximum sum is [5,1,3]

- Example 2
	- Input: nums = [1,9,-1,-2,7,3,-1,2], k = 4
	- Output: 13
	- Subarray with maximum sum is [9,-1,-2,7]

### Length Of The Longest Consecutive 1s In Binary Representation Of A Number
- Count the maximum number of consecutive 1s in binary number, without traversing the complete binary string?
- number & (number<<1)

### Merge Sort
- Merge sort is one of the most efficient sorting algorithm.
- It works on the principle of Divide and Conquer.
- It works recursively breaking down a problem into two or more sub-problems until these become simple enought to be solved directly.
- There are lot of algrithms available for sorting but most of the algorith will take O(n^2) time to accomplish the task which is not optimal at all. But merge sort can achieve this in O(nlogn) time with the help of some auxiliary space.
- Stability is also one of the factor to learn this algorithm.
- 3 Step Process
	- Divide the array recursively into almost two halves until it can not be further divided.
	- If there is only one element in the array it is already sorted, return.
	- Merge the smaller sorted arrays into new array in sorted order.
- mid is the point where the array is divided into two subarrays
	- mid = low + (high - low)/2
	- mid = 0 + (6 - 0)/2 = 3 // low=0, high=6


### Recursion
- When a function calls itsef directly or indirectly it is called recusive function and the process is called recursion.
- It is a powerful problem solving technique where solution of a larger problem defined in terms of smaller instances of itself.
- Recursion always have terminating condition also known as base case [to avoid infinite loop].
- Recusive function performs some part of task and delegate rest of it to a recursive call for further processing.

- Applications of recursion
	- Fibonacci Series, Factorial Finding
	- Merge Sort, Quick Sort
	- Binary Search
	- Towers of Hanoi
	- Tree Traversals and Problems based on it: InOrder, PreOrder, PostOrder
	- Graph Traversals: Depth First Search and Breadth First Search
	- Divde and Conquer Algoriths
	- Backtracking Algoriths
	- Dynamic Programming

### Head Recursion
- A function is called head recursive if recursive call is made before it performs its own task and this process is known as head recursion.
- Example: Given an integer n, print the sequence of integers.
```java
// Here recursive call is happening before the print(n) task
headRec(int n) {
	if(n==0) return;
	else headRec(n-1);

	print(n);
}

```

### Tail Recursion
- A function is called tail recursive if recursive call is made at the end of the function after it performs its own task and this process is known as tail recursion.
- Example: Given an integer n, print the sequence of integers.
```java
// Here task print(n) is getting executed first before recursive call is happening in the last
tailRec(int n) {
	if(n==0) return;
	else print(n);
	
	tailRec(n-1);
}

```

### Dynamic Programming
- Remembering stuff to save time later
- Dynamic Programming is a powerful technique that allows one to solve different types of problems, in Polynomial Time for which a naive approach would take exponential time.
- Components of DP
	- Recursion
	- Memoization
- Characteristics of DP
	- Optimal Substructure Property
		- A given problem has Optimal Substructure Propery, if an optimal solution of the given problem can be obtained by using optimal solutions of its subproblems.
		- fib(n) = fib(n-1) + fib(n-2) if n>1
	- Overlapping Subproblems
		- Subproblems are a subset of the original problem.
		- Any problem has overlapping sub-problems, if finding its solution requires solving the same subproblem again and again.
- Dynamic Programming Methods
	- Top-down (Memoization)
		- It's a similar approach to recursion as it looks for value in the table first, which acts as a cache, before computing solutions. 
		- If the problem solution is found then the result will be taken otherwise the problem will be solved and the results are likely to be stored in the table for future use. 
		- Here the table is filled on demand of the problems that may be an array, map or 2D matrix, which totally depends on the number of the state which you have to store to solve the problem.
		- If you use the top-down approach you are not going to fill all the entries.
	- Bottom-up (Tablulation)
		- Tabulation is the opposite of the top-down approach and avoids recursion. 
		- In this approach, we solve the problem in bottom-up manner (by solving all the related sub-problems first).
		- This is typically done by filling up all entries in an n-dimensional table. Based on the results in the table, the solution to the original problem is computed.


```java
// Recursive implementation
fib(int n) {
	if(n<2) return n;
	return fib(n-1) + fib(n-2);
}

// After applying memoization
fib(int n) {
	if(n<2) return n;
	if(cache[n] is defined) return cache[n];

	return cache[n] = fib(n-1) + fib(n-2);
}

// the code for our bottom-up dynamic programming approach
fib(int n) {
	int cache[] = new int[n+1];

	// base cases
	cache[0] = 0;
	cache[1] = 1;

	for(int i=2;i<=n; i++) {
		cache[i] = cache[i-1] + cache[i-2];
	}

	return cache[n];
}
```

### Technique to solve DP problmes
- To solve any dynamic programming problem, we can use the FAST method.
	- Find the recursive solution
	- Analyse the solution (look for overlapping problems)
	- Save the results for future (use n-dimensional array for caching purpose)
	- Tweak the solution to make it more powerful by eliminating recursion overhead (known as Bottom-up approach)

### House Robber
- Problem Statement
	- You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and it will automatically contact the police if two adjacent houses were broken into on the same night.
	- Give a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.
- Example
	- Input: [2,7,9,3,1]
	- Output: 12 -> [2,9,1]
- Algoriths
	- Case 1 (Rob the current house)
		- If current ith house is selected for robbery it means he can't rob previous (i-1)th or (i+1)th house but can safely proceed to the one before previous (i-2)th and gets all cumulative money that follows
		- robbery of current house + money from houses before th previous
		- ith_house_is_selected = currentHouseValue + rob(i-2)
	- Case 2 (Do not rob the current house)
		- If current ith house is not robbed it menas robber will gets all the possible money of (i-1)th or (i+1)th house and so on.
		- money from the previous house robbery and any money captured before that
		- ith_house_is_not_selected = rob(i-1)
	- Case 1 / Case 2
		- rob(i) = Math.max(ith_house_is_selected, ith_house_is_not_selected)
- Pattern analysis
	- Given an array of positive numbers, find the maximum sum of a subsequence with the constraint that no two numbers in the sequence should be adjacent in the array.
	- Find the maximum sum in a array such that no two elements are adjacent.

	### Knapsack Problem
	- Problem Statement (0-1 Knapsack)
		- Given a set S of n items, each with its own value Vi and weight Wi for all 1<=i<=n and a maximum knapsack capacity C, compute the maximum value of the items that you can carry. You cannot take fractions of items.
		- Suppose you're a thief. You're in a store with a knapsack, and there are all these items you can steal. But you can only take what you can fit in your knapsack. The knapsack can hold 4 lbs. You're trying to maxmize the value of the items you put in your knapsack.
	- 0/1 Knapsack
		- Items are indivisible
		- Either you have to take it or leave it
		- Require DP approach to solve the problem
	- Fractional Knapsack
		- Items are divisible
		- You can carry fraction of the item.
		- Require Greedy approach to solve the problem
- Example
	- input: val = {10,40,30,50}, wt = {5,4,6,3}, C = 10
	- Output: 90
	- (40 with weight 4 and 50 with weight 3)
		- Total wt = 7, Toal val = 90

- Algorithms
	- Case 1 [Choose the current item]
		- If current ith item is selected, so maximum profit value you can make
		- value of current item + maximum profit with remaining capacity
		- ith_item_is_selected = val[i-1] + maxV(i-1, C-wt[i-1])
	- Case 2 [Do not choose the current item]
		- If current ith item is not selected, maximum profit value you can make
		- maximum value from the remaining items except current item
		- ith_item_is_not_selected = maxV(i-1, C)
	- Case1 / Case2
		- maxV(i,C) = Math.max(ith_item_is_selected, ith_item_is_not_selected)
- Pattern Analysis
	- Given a set S of n items, each with its own value Vi and weight Wi for all 1<=i<=n and a maximum knapsack capacity C, compute the maximum value of the items that you can carry. You cannot take fractions of items.

	- Subset Sum: Give a set S of 1<=n<=20 integers and a positive integer x, is there a subset of S that sums to x?
		- S = {17,5,7,15,3,8}, X = 16

### Mindmaps for coding interviews
- Data Structures
	- Array is sorted -> Binary Search, Two pointer
	- Linked List -> Two pointer
	- Tree -> DFS, BFS
	- Graph -> DFS, BFS
- Coding Problems
	- Frequency/Counter/Duplicates/Common String -> Map
	- Top/Least K items -> Heap
	- Maximum/Minimum/Subarray/Subsets -> Dynamic Programming
	- Permutation/Subsets -> Back Tracking
	- Recursion is prohibited -> Stack
	- Common String -> Map, Trie
