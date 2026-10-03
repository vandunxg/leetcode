---
comments: true
difficulty: Medium
rating: 1514
source: Weekly Contest 240 Q2
tags:
    - Array
    - Two Pointers
    - Binary Search
---

<!-- problem:start -->

# [1855. Maximum Distance Between a Pair of Values](https://leetcode.com/problems/maximum-distance-between-a-pair-of-values)

[中文文档](/solution/1800-1899/1855.Maximum%20Distance%20Between%20a%20Pair%20of%20Values/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <strong>không tăng, đánh chỉ số từ 0</strong> là <code>nums1</code>​​​​​​ và <code>nums2</code>​​​​​​.</p>

<p>Một cặp chỉ số <code>(i, j)</code>, trong đó <code>0 &lt;= i &lt; nums1.length</code> và <code>0 &lt;= j &lt; nums2.length</code>, là <strong>hợp lệ</strong> nếu đồng thời <code>i &lt;= j</code> và <code>nums1[i] &lt;= nums2[j]</code>. <strong>Khoảng cách</strong> của cặp là <code>j - i</code>​​​​.</p>

<p>Trả về <em><strong>khoảng cách lớn nhất</strong> của một cặp <strong>hợp lệ</strong> bất kỳ </em><code>(i, j)</code><em>. Nếu không có cặp hợp lệ, trả về </em><code>0</code>.</p>

<p>Một mảng <code>arr</code> là <strong>không tăng</strong> nếu <code>arr[i-1] &gt;= arr[i]</code> với mọi <code>1 &lt;= i &lt; arr.length</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [55,30,5,4,2], nums2 = [100,20,10,10,5]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Các cặp hợp lệ là (0,0), (2,2), (2,3), (2,4), (3,3), (3,4) và (4,4).
Khoảng cách lớn nhất là 2 với cặp (2,4).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [2,2,2], nums2 = [10,10,1]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Các cặp hợp lệ là (0,0), (0,1) và (1,1).
Khoảng cách lớn nhất là 1 với cặp (0,1).
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [30,29,19,5], nums2 = [25,25,25,25,25]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Các cặp hợp lệ là (2,2), (2,3), (2,4), (3,3) và (3,4).
Khoảng cách lớn nhất là 2 với cặp (2,4).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums1.length, nums2.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums1[i], nums2[j] &lt;= 10<sup>5</sup></code></li>
	<li>Cả <code>nums1</code> và <code>nums2</code> đều <strong>không tăng</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Cả hai mảng đều không tăng; ta cần giá trị lớn nhất của $j-i$ với $i\le j$ và $nums1[i]\le nums2[j]$. Thử mọi cặp có độ phức tạp $O(mn)$, quá chậm khi $n\le 10^5$.
>
> Với một $i$ cố định, $j$ xa nhất hợp lệ là chỉ số cuối cùng trong $nums2[i:]$ có giá trị ít nhất bằng $nums1[i]$. Đảo ngược $nums2$ cho phép ta tìm chỉ số đó bằng tìm kiếm nhị phân.

<!-- thinking:end -->

Giả sử độ dài của $nums1$ và $nums2$ lần lượt là $m$ và $n$.

Duyệt mảng $nums1$, với mỗi số $nums1[i]$, thực hiện tìm kiếm nhị phân trên các số trong $nums2$ thuộc phạm vi $[i,n)$, tìm vị trí **cuối cùng** $j$ có giá trị lớn hơn hoặc bằng $nums1[i]$, tính khoảng cách giữa vị trí này và $i$, rồi cập nhật giá trị khoảng cách lớn nhất $ans$.

Độ phức tạp thời gian là $O(m \times \log n)$, trong đó $m$ và $n$ lần lượt là độ dài của $nums1$ và $nums2$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxDistance(self, nums1: List[int], nums2: List[int]) -> int:
        ans = 0
        nums2 = nums2[::-1]
        for i, v in enumerate(nums1):
            j = len(nums2) - bisect_left(nums2, v) - 1
            ans = max(ans, j - i)
        return ans
```

#### Java

```java
class Solution {
    public int maxDistance(int[] nums1, int[] nums2) {
        int ans = 0;
        int m = nums1.length, n = nums2.length;
        for (int i = 0; i < m; ++i) {
            int left = i, right = n - 1;
            while (left < right) {
                int mid = (left + right + 1) >> 1;
                if (nums2[mid] >= nums1[i]) {
                    left = mid;
                } else {
                    right = mid - 1;
                }
            }
            ans = Math.max(ans, left - i);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxDistance(vector<int>& nums1, vector<int>& nums2) {
        int ans = 0;
        reverse(nums2.begin(), nums2.end());
        for (int i = 0; i < nums1.size(); ++i) {
            int j = nums2.size() - (lower_bound(nums2.begin(), nums2.end(), nums1[i]) - nums2.begin()) - 1;
            ans = max(ans, j - i);
        }
        return ans;
    }
};
```

#### Go

```go
func maxDistance(nums1 []int, nums2 []int) int {
	ans, n := 0, len(nums2)
	for i, num := range nums1 {
		left, right := i, n-1
		for left < right {
			mid := (left + right + 1) >> 1
			if nums2[mid] >= num {
				left = mid
			} else {
				right = mid - 1
			}
		}
		if ans < left-i {
			ans = left - i
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maxDistance(nums1: number[], nums2: number[]): number {
    let ans = 0;
    let m = nums1.length;
    let n = nums2.length;
    for (let i = 0; i < m; ++i) {
        let left = i;
        let right = n - 1;
        while (left < right) {
            const mid = (left + right + 1) >> 1;
            if (nums2[mid] >= nums1[i]) {
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        ans = Math.max(ans, left - i);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_distance(nums1: Vec<i32>, nums2: Vec<i32>) -> i32 {
        let m = nums1.len();
        let n = nums2.len();
        let mut res = 0;
        for i in 0..m {
            let mut left = i;
            let mut right = n;
            while left < right {
                let mid = left + (right - left) / 2;
                if nums2[mid] >= nums1[i] {
                    left = mid + 1;
                } else {
                    right = mid;
                }
            }
            res = res.max((left - i - 1) as i32);
        }
        res
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number}
 */
var maxDistance = function (nums1, nums2) {
    let ans = 0;
    let m = nums1.length;
    let n = nums2.length;
    for (let i = 0; i < m; ++i) {
        let left = i;
        let right = n - 1;
        while (left < right) {
            const mid = (left + right + 1) >> 1;
            if (nums2[mid] >= nums1[i]) {
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        ans = Math.max(ans, left - i);
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 chỉ sử dụng tính đơn điệu của $nums2$. $nums1$ cũng không tăng, nên $j$ xa nhất không bao giờ dịch sang trái khi $i$ tăng. Hai con trỏ có thể cùng tiến về phía trước trong thời gian tuyến tính.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxDistance(self, nums1: List[int], nums2: List[int]) -> int:
        m, n = len(nums1), len(nums2)
        ans = i = j = 0
        while i < m:
            while j < n and nums1[i] <= nums2[j]:
                j += 1
            ans = max(ans, j - i - 1)
            i += 1
        return ans
```

#### Java

```java
class Solution {
    public int maxDistance(int[] nums1, int[] nums2) {
        int m = nums1.length, n = nums2.length;
        int ans = 0;
        for (int i = 0, j = 0; i < m; ++i) {
            while (j < n && nums1[i] <= nums2[j]) {
                ++j;
            }
            ans = Math.max(ans, j - i - 1);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxDistance(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size(), n = nums2.size();
        int ans = 0;
        for (int i = 0, j = 0; i < m; ++i) {
            while (j < n && nums1[i] <= nums2[j]) {
                ++j;
            }
            ans = max(ans, j - i - 1);
        }
        return ans;
    }
};
```

#### Go

```go
func maxDistance(nums1 []int, nums2 []int) int {
	m, n := len(nums1), len(nums2)
	ans := 0
	for i, j := 0, 0; i < m; i++ {
		for j < n && nums1[i] <= nums2[j] {
			j++
		}
		if ans < j-i-1 {
			ans = j - i - 1
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maxDistance(nums1: number[], nums2: number[]): number {
    let ans = 0;
    const m = nums1.length;
    const n = nums2.length;
    for (let i = 0, j = 0; i < m; ++i) {
        while (j < n && nums1[i] <= nums2[j]) {
            j++;
        }
        ans = Math.max(ans, j - i - 1);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_distance(nums1: Vec<i32>, nums2: Vec<i32>) -> i32 {
        let m = nums1.len();
        let n = nums2.len();
        let mut res = 0;
        let mut j = 0;
        for i in 0..m {
            while j < n && nums1[i] <= nums2[j] {
                j += 1;
            }
            res = res.max((j - i - 1) as i32);
        }
        res
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number}
 */
var maxDistance = function (nums1, nums2) {
    let ans = 0;
    const m = nums1.length;
    const n = nums2.length;
    for (let i = 0, j = 0; i < m; ++i) {
        while (j < n && nums1[i] <= nums2[j]) {
            j++;
        }
        ans = Math.max(ans, j - i - 1);
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
