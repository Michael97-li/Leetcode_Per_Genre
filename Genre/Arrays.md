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

### 1089. Duplicate Zeros

Given a fixed-length integer array `arr`, duplicate each occurrence of zero, shifting the remaining elements to the right.

**Note** that elements beyond the length of the original array are not written. Do the above modifications to the input array in place and do not return anything.

**Example 1:**

<pre><strong>Input:</strong> arr = [1,0,2,3,0,4,5,0]
<strong>Output:</strong> [1,0,0,2,3,0,0,4]
<strong>Explanation:</strong> After calling your function, the input array is modified to: [1,0,0,2,3,0,0,4]
</pre>

**Example 2:**

<pre><strong>Input:</strong> arr = [1,2,3]
<strong>Output:</strong> [1,2,3]
<strong>Explanation:</strong> After calling your function, the input array is modified to: [1,2,3]</pre>

#### 分析

If we know the number of elements which would be discarded from the end of the array, we can copy the rest. How do we find out how many elements would be discarded in the end? The number would be equal to the number of extra zeros which would be added to the array. The extra zero would create space for itself by pushing out an element from the end of the array.

Once we know how many elements from the original array would be part of the final array, we can just start copying from the end. Copying from the end ensures we don't lose any element since, the last few extraneous elements can be overwritten

#### 基本思路

**Algorithm**

1. Find the number of zeros which would be duplicated. Let's call it `possible_dups`. We do need to make sure we are not counting the zeros which would be trimmed off. Since, the discarded zeros won't be part of the final array. The count of `possible_dups` would give us the number of elements to be trimmed off the original array. Hence at any point, `length_ - possible_dups` is the number of elements which would be included in the final array.
   ![img](https://leetcode.com/problems/duplicate-zeros/Figures/1089/1089_Duplicate_Zeros_4.png)
   Note: In the diagram above we just show source and destination array for understanding purpose. We will be doing these operations only on one array.
2. Handle the edge case for a zero present on the boundary of the leftover elements.
   Let's talk about the edge case of this problem. We need to be extra careful when we are duplicating the zeros in the leftover array. This care should be taken for the `zero` which is lying on the boundary. Since, this zero might be counted as with possible duplicates, or may be just got included in the left over when there was no space left to accommodate its duplicate. If it is part of the `possible_dups` we would want to duplicate it otherwise we don't.
   > An example of the edge case is - [8,4,5,0,0,0,0,7]. In this array there is space to accommodate the duplicates of first and second occurrences of zero. But we don't have enough space for the duplicate of the third occurrence of zero. Hence when we are copying we need to make sure for the third occurrence we don't copy twice. Result = [8,4,5,0,`0`,0,`0`,0]
   >
3. Iterate the array from the end and copy a non-zero element once and zero element twice. When we say we discard the extraneous elements, it simply means we start from the left of the extraneous elements and start overwriting them with new values, eventually right shifting the left over elements and creating space for all the duplicated elements in the array.

Time Complexity: **O**(**N**), where **N** is the number of elements in the array. We do two passes through the array, one to find the number of `possible_dups` and the other to copy the elements. In the worst case we might be iterating the entire array, when there are less or no zeros in the array.

Space Complexity: **O**(**1**). We do not use any extra space

#### JAVA

```java
class Solution {
    public void duplicateZeros(int[] arr) {
        int possibleDups = 0;
        int length_ = arr.length - 1;

        // Find the number of zeros to be duplicated
        // Stopping when left points beyond the last element in the original array
        // which would be part of the modified array
        for (int left = 0; left <= length_ - possibleDups; left++) {

            // Count the zeros
            if (arr[left] == 0) {

                // Edge case: This zero can't be duplicated. We have no more space,
                // as left is pointing to the last element which could be included  
                if (left == length_ - possibleDups) {
                    // For this zero we just copy it without duplication.
                    arr[length_] = 0;
                    length_ -= 1;
                    break;
                }
                possibleDups++;
            }
        }

        // Start backwards from the last element which would be part of new array.
        int last = length_ - possibleDups;

        // Copy zero twice, and non zero once.
        for (int i = last; i >= 0; i--) {
            if (arr[i] == 0) {
                arr[i + possibleDups] = 0;
                possibleDups--;
                arr[i + possibleDups] = 0;
            } else {
                arr[i + possibleDups] = arr[i];
            }
        }
    }
}
```

### 88. Merge Sorted Array

You are given two integer arrays `nums1` and `nums2`, sorted in  **non-decreasing order** , and two integers `m` and `n`, representing the number of elements in `nums1` and `nums2` respectively.

**Merge** `nums1` and `nums2` into a single array sorted in  **non-decreasing order** .

The final sorted array should not be returned by the function, but instead be *stored inside the array *`nums1`. To accommodate this, `nums1` has a length of `m + n`, where the first `m` elements denote the elements that should be merged, and the last `n` elements are set to `0` and should be ignored. `nums2` has a length of `n`.

**Example 1:**

<pre><strong>Input:</strong> nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
<strong>Output:</strong> [1,2,2,3,5,6]
<strong>Explanation:</strong> The arrays we are merging are [1,2,3] and [2,5,6].
The result of the merge is [<u>1</u>,<u>2</u>,2,<u>3</u>,5,6] with the underlined elements coming from nums1.</pre>

#### 分析

Can you come up with an algorithm that runs in `O(m + n)` time

#### 基本思路

从后往前比

TC: O(m+n)

SC: O(1)

#### JAVA

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        // total length
        int len = m + n;
    
        // two index
        int index1 = m-1;
        int index2 = n-1;
    
        // compare from back
        while(index1>=0 && index2>=0){
            if (nums2[index2]>=nums1[index1]){
                nums1[len-1]=nums2[index2];
                index2--;
                len--;
            } else{
                nums1[len-1]=nums1[index1];
                nums1[index1] = 0;
                index1--;
                len--;
            }
        }
    
        if (index2 < 0) return;
        for (int i = index2;i>=0;i--){
            nums1[i]=nums2[index2];
            index2--;
        } 
    }
}
```
