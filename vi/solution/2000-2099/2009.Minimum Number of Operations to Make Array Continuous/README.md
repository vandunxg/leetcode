---
comments: true
difficulty: Hard
rating: 2084
source: Biweekly Contest 61 Q4
tags:
    - Array
    - Hash Table
    - Binary Search
    - Sliding Window
---

<!-- problem:start -->

# [2009. Minimum Number of Operations to Make Array Continuous](https://leetcode.com/problems/minimum-number-of-operations-to-make-array-continuous)

[中文文档](/solution/2000-2099/2009.Minimum%20Number%20of%20Operations%20to%20Make%20Array%20Continuous/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>. Trong một phép toán, bạn có thể thay thế <strong>bất kỳ</strong> phần tử nào trong <code>nums</code> bằng <strong>bất kỳ</strong> số nguyên nào.</p>

<p><code>nums</code> được xem là <strong>liên tiếp</strong> nếu thỏa mãn cả hai điều kiện sau:</p>

<ul>
	<li>Tất cả phần tử trong <code>nums</code> đều <strong>đôi một khác nhau</strong>.</li>
	<li>Hiệu giữa phần tử <strong>lớn nhất</strong> và phần tử <strong>nhỏ nhất</strong> trong <code>nums</code> bằng <code>nums.length - 1</code>.</li>
</ul>

<p>Ví dụ, <code>nums = [4, 2, 5, 3]</code> là <strong>liên tiếp</strong>, nhưng <code>nums = [1, 2, 3, 5, 6]</code> thì <strong>không liên tiếp</strong>.</p>

<p>Hãy trả về <em>số phép toán <strong>ít nhất</strong> để biến </em><code>nums</code><em> thành một mảng </em><strong><em>liên tiếp</em></strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,2,5,3]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>&nbsp;nums đã liên tiếp.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,5,6]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>&nbsp;Một cách thực hiện là đổi phần tử cuối cùng thành 4.
Mảng thu được là [1,2,3,5,4], và mảng này liên tiếp.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,10,100,1000]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>&nbsp;Một cách thực hiện là:
- Đổi phần tử thứ hai thành 2.
- Đổi phần tử thứ ba thành 3.
- Đổi phần tử thứ tư thành 4.
Mảng thu được là [1,2,3,4], và mảng này liên tiếp.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Loại bỏ phần tử trùng lặp + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng liên tiếp chứa $n$ giá trị phân biệt, vì vậy các phần tử trùng lặp phải được thay đổi. Với $n \le 10^5$, ta không thể thử mọi đoạn đích. Sau khi sắp xếp các giá trị khác nhau, một cửa sổ hợp lệ là một đoạn có hiệu giữa giá trị lớn nhất và nhỏ nhất không vượt quá $n-1$.
>
> Với đầu trái $nums[i]$, đầu phải không thể vượt quá $nums[i]+n-1$. Tìm kiếm nhị phân giúp tìm phần tử tràn đầu tiên $j$; ta giữ lại $j-i$ phần tử và thực hiện $n-(j-i)$ phép toán.
>
> Sắp xếp là phần chi phối; mỗi truy vấn tốn $O(\log n)$.

<!-- thinking:end -->

Trước tiên, ta sắp xếp mảng và loại bỏ các phần tử trùng lặp.

Sau đó, ta duyệt mảng, lần lượt coi phần tử hiện tại $nums[i]$ là giá trị nhỏ nhất của mảng liên tiếp. Ta dùng tìm kiếm nhị phân để tìm vị trí đầu tiên $j$ lớn hơn $nums[i] + n - 1$. Khi đó, $j-i$ là độ dài của mảng liên tiếp khi phần tử hiện tại là giá trị nhỏ nhất. Ta cập nhật đáp án, tức là $ans = \min(ans, n - (j - i))$.

Cuối cùng, ta trả về $ans$.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(\log n)$. Ở đây, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: List[int]) -> int:
        ans = n = len(nums)
        nums = sorted(set(nums))
        for i, v in enumerate(nums):
            j = bisect_right(nums, v + n - 1)
            ans = min(ans, n - (j - i))
        return ans
```

#### Java

```java
class Solution {
    public int minOperations(int[] nums) {
        int n = nums.length;
        Arrays.sort(nums);
        int m = 1;
        for (int i = 1; i < n; ++i) {
            if (nums[i] != nums[i - 1]) {
                nums[m++] = nums[i];
            }
        }
        int ans = n;
        for (int i = 0; i < m; ++i) {
            int j = search(nums, nums[i] + n - 1, i, m);
            ans = Math.min(ans, n - (j - i));
        }
        return ans;
    }

    private int search(int[] nums, int x, int left, int right) {
        while (left < right) {
            int mid = (left + right) >> 1;
            if (nums[mid] > x) {
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
    int minOperations(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int m = unique(nums.begin(), nums.end()) - nums.begin();
        int n = nums.size();
        int ans = n;
        for (int i = 0; i < m; ++i) {
            int j = upper_bound(nums.begin() + i, nums.begin() + m, nums[i] + n - 1) - nums.begin();
            ans = min(ans, n - (j - i));
        }
        return ans;
    }
};
```

#### Go

```go
func minOperations(nums []int) int {
	sort.Ints(nums)
	n := len(nums)
	m := 1
	for i := 1; i < n; i++ {
		if nums[i] != nums[i-1] {
			nums[m] = nums[i]
			m++
		}
	}
	ans := n
	for i := 0; i < m; i++ {
		j := sort.Search(m, func(k int) bool { return nums[k] > nums[i]+n-1 })
		ans = min(ans, n-(j-i))
	}
	return ans
}
```

#### TypeScript

```ts
function minOperations(nums: number[]): number {
    const n = nums.length;
    nums.sort((a, b) => a - b);
    let m = 1;
    for (let i = 1; i < n; ++i) {
        if (nums[i] !== nums[i - 1]) {
            nums[m++] = nums[i];
        }
    }
    let ans = n;
    for (let i = 0; i < m; ++i) {
        const j = search(nums, nums[i] + n - 1, i, m);
        ans = Math.min(ans, n - (j - i));
    }
    return ans;
}

function search(nums: number[], x: number, left: number, right: number): number {
    while (left < right) {
        const mid = (left + right) >> 1;
        if (nums[mid] > x) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    return left;
}
```

#### Rust

```rust
use std::collections::BTreeSet;

impl Solution {
    #[allow(dead_code)]
    pub fn min_operations(nums: Vec<i32>) -> i32 {
        let n = nums.len();
        let nums = nums.into_iter().collect::<BTreeSet<i32>>();

        let m = nums.len();
        let nums = nums.into_iter().collect::<Vec<i32>>();

        let mut ans = n;

        for i in 0..m {
            let j = match nums.binary_search(&(nums[i] + (n as i32))) {
                Ok(idx) => idx,
                Err(idx) => idx,
            };
            ans = std::cmp::min(ans, n - (j - i));
        }

        ans as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Sắp xếp + Loại bỏ phần tử trùng lặp + Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 tìm đầu phải bằng tìm kiếm nhị phân ở mỗi bước. Khi $i$ tăng dần, $j$ cũng đơn điệu tăng, nên không cần tìm kiếm nhị phân.
>
> Duyệt bằng hai con trỏ giúp $j$ chỉ tăng một lần, loại bỏ thừa số $\log n$.

<!-- thinking:end -->

Tương tự Lời giải 1, trước tiên ta sắp xếp mảng và loại bỏ các phần tử trùng lặp.

Sau đó, ta duyệt mảng, lần lượt coi phần tử hiện tại $nums[i]$ là giá trị nhỏ nhất của mảng liên tiếp. Ta dùng hai con trỏ để tìm vị trí đầu tiên $j$ lớn hơn $nums[i] + n - 1$. Khi đó, $j-i$ là độ dài của mảng liên tiếp khi phần tử hiện tại là giá trị nhỏ nhất. Ta cập nhật đáp án, tức là $ans = \min(ans, n - (j - i))$.

Cuối cùng, ta trả về $ans$.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(\log n)$. Ở đây, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: List[int]) -> int:
        n = len(nums)
        nums = sorted(set(nums))
        ans, j = n, 0
        for i, v in enumerate(nums):
            while j < len(nums) and nums[j] - v <= n - 1:
                j += 1
            ans = min(ans, n - (j - i))
        return ans
```

#### Java

```java
class Solution {
    public int minOperations(int[] nums) {
        int n = nums.length;
        Arrays.sort(nums);
        int m = 1;
        for (int i = 1; i < n; ++i) {
            if (nums[i] != nums[i - 1]) {
                nums[m++] = nums[i];
            }
        }
        int ans = n;
        for (int i = 0, j = 0; i < m; ++i) {
            while (j < m && nums[j] - nums[i] <= n - 1) {
                ++j;
            }
            ans = Math.min(ans, n - (j - i));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minOperations(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int m = unique(nums.begin(), nums.end()) - nums.begin();
        int n = nums.size();
        int ans = n;
        for (int i = 0, j = 0; i < m; ++i) {
            while (j < m && nums[j] - nums[i] <= n - 1) {
                ++j;
            }
            ans = min(ans, n - (j - i));
        }
        return ans;
    }
};
```

#### Go

```go
func minOperations(nums []int) int {
	sort.Ints(nums)
	n := len(nums)
	m := 1
	for i := 1; i < n; i++ {
		if nums[i] != nums[i-1] {
			nums[m] = nums[i]
			m++
		}
	}
	ans := n
	for i, j := 0, 0; i < m; i++ {
		for j < m && nums[j]-nums[i] <= n-1 {
			j++
		}
		ans = min(ans, n-(j-i))
	}
	return ans
}
```

#### TypeScript

```ts
function minOperations(nums: number[]): number {
    nums.sort((a, b) => a - b);
    const n = nums.length;
    let m = 1;
    for (let i = 1; i < n; i++) {
        if (nums[i] !== nums[i - 1]) {
            nums[m] = nums[i];
            m++;
        }
    }
    let ans = n;
    for (let i = 0, j = 0; i < m; i++) {
        while (j < m && nums[j] - nums[i] <= n - 1) {
            j++;
        }
        ans = Math.min(ans, n - (j - i));
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_operations(mut nums: Vec<i32>) -> i32 {
        nums.sort();
        let n = nums.len();
        if n == 0 {
            return 0;
        }
        let mut m = 1usize;
        for i in 1..n {
            if nums[i] != nums[i - 1] {
                nums[m] = nums[i];
                m += 1;
            }
        }
        let mut ans = n as i32;
        let mut j = 0usize;
        for i in 0..m {
            while j < m && nums[j] - nums[i] <= n as i32 - 1 {
                j += 1;
            }
            ans = ans.min(n as i32 - (j as i32 - i as i32));
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
