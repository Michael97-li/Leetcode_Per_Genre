# Arrays

## Introduce

*Arrays are a simple data structure for storing lots of similar items. They exist in all programming languages, and are used as the basis for most other data structures. On their own, Arrays can be used to solve many interesting problems. Arrays come up very often in interview problems, and so being a guru with them is a must!*

*In this Explore Card, we'll introduce Arrays and solve some cool problems with them. This Card is  **beginner friendly** , and we've provided lots of code snippets in **Java** to help you understand. Each topic begins with informative articles, followed by real interview problems for you to practice on.*

*In addition, the problems have hints which will help you in building up ideas on how to solve them. These hints will be subtle enough to set you on a particular path for reaching the optimized solution, without giving away the answer too easily.*

---

## Code Example

### 977. Squares of a Sorted Array | Easy

Given an integer array `nums` sorted in **non-decreasing** order, return  *an array of **the squares of each number** sorted in non-decreasing order* .

**Example 1:**

<pre><strong>Input:</strong> nums = [-4,-1,0,3,10]
<strong>Output:</strong> [0,1,9,16,100]
<strong>Explanation:</strong> After squaring, the array becomes [16,1,0,9,100].
After sorting, it becomes [0,1,9,16,100].
</pre>

**Example 2:**

<pre><strong>Input:</strong> nums = [-7,-3,2,3,11]
<strong>Output:</strong> [4,9,9,49,121]</pre>

#### 分析

简单的逐个平方+排序 -> O(NlogN), 尝试实现O(N)

#### 基本思路

使用Two Pointer, 一头一尾比较绝对值 

TC: O(N)

SC: O(N)

#### JAVA

```java
class Solution {
	public int[] sortedSquares(int[] nums) {
		int n = nums.length;
		int[] result = new int[n];
		int left = 0;
		int right = n - 1;

		for (int i = n - 1; i >= 0; i--){
			int square;
			if (Math.abs(nums[left]) < Math.abs(nums[right])) {
				square = nums[right];
				right --;
			} else {
				square = nums[left];
				left ++;
			}
			result[i] = square * square;
		}
		return result;
	}
}
```
