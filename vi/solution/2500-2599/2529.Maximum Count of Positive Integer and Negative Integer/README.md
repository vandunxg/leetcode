---
comments: true
difficulty: Easy
rating: 1195
source: Weekly Contest 327 Q1
tags:
    - Array
    - Binary Search
    - Counting
---

<!-- problem:start -->

# [2529. Maximum Count of Positive Integer and Negative Integer](https://leetcode.com/problems/maximum-count-of-positive-integer-and-negative-integer)

[中文文档](/solution/2500-2599/2529.Maximum%20Count%20of%20Positive%20Integer%20and%20Negative%20Integer/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <code>nums</code> được sắp xếp theo thứ tự <strong>không giảm</strong>, hãy trả về <em>số lớn hơn giữa số lượng số nguyên dương và số lượng số nguyên âm.</em></p>

<ul>
	<li>Nói cách khác, nếu số lượng số nguyên dương trong <code>nums</code> là <code>pos</code> và số lượng số nguyên âm là <code>neg</code>, thì trả về giá trị lớn hơn giữa <code>pos</code> và <code>neg</code>.</li>
</ul>

<p><strong>Lưu ý</strong> rằng <code>0</code> không phải số dương cũng không phải số âm.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-2,-1,-1,1,2,3]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Có 3 số nguyên dương và 3 số nguyên âm. Số lượng lớn hơn giữa chúng là 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-3,-2,-1,0,0,1,2]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Có 2 số nguyên dương và 3 số nguyên âm. Số lượng lớn hơn giữa chúng là 3.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,20,66,1314]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Có 4 số nguyên dương và 0 số nguyên âm. Số lượng lớn hơn giữa chúng là 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 2000</code></li>
	<li><code>-2000 &lt;= nums[i] &lt;= 2000</code></li>
	<li><code>nums</code> được sắp xếp theo thứ tự <strong>không giảm</strong>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể giải bài toán với độ phức tạp thời gian <code>O(log(n))</code> không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Trả về số lượng lớn hơn giữa số dương và số âm; bỏ qua các số 0. Một lần duyệt là đủ với $n\le 10^5$.

<!-- thinking:end -->

Ta có thể duyệt trực tiếp qua mảng, đếm số lượng số nguyên dương và số nguyên âm lần lượt vào $a$ và $b$, rồi trả về giá trị lớn hơn giữa $a$ và $b$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumCount(self, nums: List[int]) -> int:
        a = sum(x > 0 for x in nums)
        b = sum(x < 0 for x in nums)
        return max(a, b)
```

#### Java

```java
class Solution {
    public int maximumCount(int[] nums) {
        int a = 0, b = 0;
        for (int x : nums) {
            if (x > 0) {
                ++a;
            } else if (x < 0) {
                ++b;
            }
        }
        return Math.max(a, b);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumCount(vector<int>& nums) {
        int a = 0, b = 0;
        for (int x : nums) {
            if (x > 0) {
                ++a;
            } else if (x < 0) {
                ++b;
            }
        }
        return max(a, b);
    }
};
```

#### Go

```go
func maximumCount(nums []int) int {
	var a, b int
	for _, x := range nums {
		if x > 0 {
			a++
		} else if x < 0 {
			b++
		}
	}
	return max(a, b)
}
```

#### TypeScript

```ts
function maximumCount(nums: number[]): number {
    let [a, b] = [0, 0];
    for (const x of nums) {
        if (x > 0) {
            ++a;
        } else if (x < 0) {
            ++b;
        }
    }
    return Math.max(a, b);
}
```

#### Rust

```rust
impl Solution {
    pub fn maximum_count(nums: Vec<i32>) -> i32 {
        let mut a = 0;
        let mut b = 0;

        for x in nums {
            if x > 0 {
                a += 1;
            } else if x < 0 {
                b += 1;
            }
        }

        std::cmp::max(a, b)
    }
}
```

#### C

```c
#define max(a, b) (a > b ? a : b)

int maximumCount(int* nums, int numsSize) {
    int a = 0, b = 0;
    for (int i = 0; i < numsSize; ++i) {
        if (nums[i] > 0) {
            ++a;
        } else if (nums[i] < 0) {
            ++b;
        }
    }
    return max(a, b);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 chưa tận dụng việc mảng được sắp xếp theo thứ tự không giảm. Các số dương là hậu tố gồm các giá trị $\ge 1$, còn các số âm là tiền tố gồm các giá trị $<0$, tức là các cận dưới của $1$ và $0$. Hai lần gọi $\textit{bisect\_left}$ sẽ lấy được cả hai số lượng trong thời gian logarithmic.

<!-- thinking:end -->

Vì mảng được sắp xếp theo thứ tự không giảm, ta có thể dùng tìm kiếm nhị phân để tìm chỉ số $i$ của phần tử đầu tiên lớn hơn hoặc bằng $1$, và chỉ số $j$ của phần tử đầu tiên lớn hơn hoặc bằng $0$. Số lượng số nguyên dương là $a = n - i$, còn số lượng số nguyên âm là $b = j$. Ta trả về giá trị lớn hơn giữa $a$ và $b$.

Độ phức tạp thời gian là $O(\log n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumCount(self, nums: List[int]) -> int:
        a = len(nums) - bisect_left(nums, 1)
        b = bisect_left(nums, 0)
        return max(a, b)
```

#### Java

```java
class Solution {
    public int maximumCount(int[] nums) {
        int a = nums.length - search(nums, 1);
        int b = search(nums, 0);
        return Math.max(a, b);
    }

    private int search(int[] nums, int x) {
        int left = 0, right = nums.length;
        while (left < right) {
            int mid = (left + right) >> 1;
            if (nums[mid] >= x) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumCount(vector<int>& nums) {
        int a = nums.end() - lower_bound(nums.begin(), nums.end(), 1);
        int b = lower_bound(nums.begin(), nums.end(), 0) - nums.begin();
        return max(a, b);
    }
};
```

#### Go

```go
func maximumCount(nums []int) int {
	a := len(nums) - sort.SearchInts(nums, 1)
	b := sort.SearchInts(nums, 0)
	return max(a, b)
}
```

#### TypeScript

```ts
function maximumCount(nums: number[]): number {
    const i = _.sortedLastIndex(nums, 0);
    const j = _.sortedIndex(nums, 0);
    const [a, b] = [nums.length - i, j];
    return Math.max(a, b);
}
```

#### Rust

```rust
impl Solution {
    fn search(nums: &Vec<i32>, x: i32) -> usize {
        let mut left = 0;
        let mut right = nums.len();
        while left < right {
            let mid = (left + right) >> 1;
            if nums[mid] >= x {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        left
    }

    pub fn maximum_count(nums: Vec<i32>) -> i32 {
        let n = nums.len();
        let i = Self::search(&nums, 1);
        let j = Self::search(&nums, 0);
        (n - i).max(j) as i32
    }
}
```

#### C

```c
#define max(a, b) (a > b ? a : b)

int search(int* nums, int numsSize, int x) {
    int left = 0;
    int right = numsSize;
    while (left < right) {
        int mid = (left + right) >> 1;
        if (nums[mid] >= x) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    return left;
}

int maximumCount(int* nums, int numsSize) {
    int i = search(nums, numsSize, 1);
    int j = search(nums, numsSize, 0);
    return max(numsSize - i, j);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
