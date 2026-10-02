---
comments: true
difficulty: Easy
tags:
    - Array
    - Hash Table
    - Pigeonhole Principle
---

<!-- problem:start -->

# [961. N-Repeated Element in Size 2N Array](https://leetcode.com/problems/n-repeated-element-in-size-2n-array)

[中文文档](/solution/0900-0999/0961.N-Repeated%20Element%20in%20Size%202N%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> có các tính chất sau:</p>

<ul>
	<li><code>nums.length == 2 * n</code>.</li>
	<li><code>nums</code> chứa <code>n + 1</code> giá trị <strong>khác nhau</strong>, trong đó có <code>n</code> giá trị xuất hiện <strong>đúng một lần</strong> trong mảng.</li>
	<li>Có đúng một phần tử trong <code>nums</code> xuất hiện <code>n</code> lần.</li>
</ul>

<p>Hãy trả về <em>phần tử xuất hiện </em><code>n</code><em> lần</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Input:</strong> nums = [1,2,3,3]
<strong>Output:</strong> 3
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Input:</strong> nums = [2,1,2,5,3,2]
<strong>Output:</strong> 2
</pre><p><strong class="example">Ví dụ 3:</strong></p>
<pre><strong>Input:</strong> nums = [5,1,5,2,5,3,5,4]
<strong>Output:</strong> 5
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 5000</code></li>
	<li><code>nums.length == 2 * n</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
	<li><code>nums</code> chứa <code>n + 1</code> phần tử <strong>khác nhau</strong>, trong đó một phần tử xuất hiện đúng <code>n</code> lần.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Mảng độ dài $2n$ có một giá trị xuất hiện $n$ lần và $n$ giá trị khác nhau còn lại. Giá trị đầu tiên đã có trong set chính là đáp án.

<!-- thinking:end -->

Mảng $\textit{nums}$ có tổng cộng $2n$ phần tử, gồm $n + 1$ giá trị khác nhau và một giá trị xuất hiện $n$ lần. Vì vậy, $n$ phần tử còn lại trong mảng đều khác nhau.

Do đó, ta chỉ cần duyệt mảng $\textit{nums}$ và dùng hash table $s$ để lưu các phần tử đã gặp. Khi gặp phần tử $x$ đã có trong $s$, ta biết $x$ là phần tử lặp lại và có thể trả về ngay.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def repeatedNTimes(self, nums: List[int]) -> int:
        s = set()
        for x in nums:
            if x in s:
                return x
            s.add(x)
```

#### Java

```java
class Solution {
    public int repeatedNTimes(int[] nums) {
        Set<Integer> s = new HashSet<>(nums.length / 2 + 1);
        for (int i = 0;; ++i) {
            if (!s.add(nums[i])) {
                return nums[i];
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int repeatedNTimes(vector<int>& nums) {
        unordered_set<int> s;
        for (int i = 0;; ++i) {
            if (s.count(nums[i])) {
                return nums[i];
            }
            s.insert(nums[i]);
        }
    }
};
```

#### Go

```go
func repeatedNTimes(nums []int) int {
	s := map[int]bool{}
	for i := 0; ; i++ {
		if s[nums[i]] {
			return nums[i]
		}
		s[nums[i]] = true
	}
}
```

#### TypeScript

```ts
function repeatedNTimes(nums: number[]): number {
    const s: Set<number> = new Set();
    for (const x of nums) {
        if (s.has(x)) {
            return x;
        }
        s.add(x);
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var repeatedNTimes = function (nums) {
    const s = new Set();
    for (const x of nums) {
        if (s.has(x)) {
            return x;
        }
        s.add(x);
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Set cần bộ nhớ tuyến tính. Khi đọc mảng theo vòng tròn, hai lần xuất hiện của giá trị lặp cách nhau nhiều nhất $2$ vị trí. Vì vậy, so sánh mỗi chỉ số $i\ge 2$ với hai phần tử đứng trước; nếu không có cặp nào khớp, đáp án là $nums[0]$.

<!-- thinking:end -->

Theo đề bài, một nửa số phần tử trong mảng $\textit{nums}$ giống nhau. Nếu xem mảng được sắp xếp theo vòng tròn, thì giữa hai phần tử giống nhau có nhiều nhất $1$ phần tử khác.

Do đó, ta duyệt mảng $\textit{nums}$ bắt đầu từ chỉ số $2$. Với mỗi chỉ số $i$, ta so sánh $\textit{nums}[i]$ với $\textit{nums}[i - 1]$ và $\textit{nums}[i - 2]$. Nếu có giá trị bằng nhau, ta trả về giá trị đó.

Nếu không tìm thấy phần tử lặp trong quá trình trên, thì phần tử đó phải là $\textit{nums}[0]$; ta có thể trả về ngay $\textit{nums}[0]$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def repeatedNTimes(self, nums: List[int]) -> int:
        for i in range(2, len(nums)):
            if nums[i] == nums[i - 1] or nums[i] == nums[i - 2]:
                return nums[i]
        return nums[0]
```

#### Java

```java
class Solution {
    public int repeatedNTimes(int[] nums) {
        for (int i = 2; i < nums.length; ++i) {
            if (nums[i] == nums[i - 1] || nums[i] == nums[i - 2]) {
                return nums[i];
            }
        }
        return nums[0];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int repeatedNTimes(vector<int>& nums) {
        for (int i = 2; i < nums.size(); ++i) {
            if (nums[i] == nums[i - 1] || nums[i] == nums[i - 2]) {
                return nums[i];
            }
        }
        return nums[0];
    }
};
```

#### Go

```go
func repeatedNTimes(nums []int) int {
	for i := 2; i < len(nums); i++ {
		if nums[i] == nums[i-1] || nums[i] == nums[i-2] {
			return nums[i]
		}
	}
	return nums[0]
}
```

#### TypeScript

```ts
function repeatedNTimes(nums: number[]): number {
    for (let i = 2; i < nums.length; ++i) {
        if (nums[i] === nums[i - 1] || nums[i] === nums[i - 2]) {
            return nums[i];
        }
    }
    return nums[0];
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var repeatedNTimes = function (nums) {
    for (let i = 2; i < nums.length; ++i) {
        if (nums[i] === nums[i - 1] || nums[i] === nums[i - 2]) {
            return nums[i];
        }
    }
    return nums[0];
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
