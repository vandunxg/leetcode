---
comments: true
difficulty: Medium
rating: 1619
source: Biweekly Contest 96 Q2
tags:
    - Greedy
    - Array
    - Math
---

<!-- problem:start -->

# [2541. Minimum Operations to Make Array Equal II](https://leetcode.com/problems/minimum-operations-to-make-array-equal-ii)

[中文文档](/solution/2500-2599/2541.Minimum%20Operations%20to%20Make%20Array%20Equal%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>nums1</code> và <code>nums2</code> có cùng độ dài <code>n</code>, cùng một số nguyên <code>k</code>. Bạn có thể thực hiện thao tác sau trên <code>nums1</code>:</p>

<ul>
	<li>Chọn hai chỉ số <code>i</code> và <code>j</code>, tăng <code>nums1[i]</code> thêm <code>k</code> và giảm <code>nums1[j]</code> đi <code>k</code>. Nói cách khác, <code>nums1[i] = nums1[i] + k</code> và <code>nums1[j] = nums1[j] - k</code>.</li>
</ul>

<p><code>nums1</code> được xem là <strong>bằng</strong> <code>nums2</code> nếu với mọi chỉ số <code>i</code> thỏa mãn <code>0 &lt;= i &lt; n</code>, ta có <code>nums1[i] == nums2[i]</code>.</p>

<p>Trả về <em><strong>số thao tác ít nhất</strong> cần thực hiện để biến </em><code>nums1</code><em> thành </em><code>nums2</code>. Nếu không thể biến đổi để hai mảng bằng nhau, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> nums1 = [4,3,1,4], nums2 = [1,3,7,1], k = 3
<strong>Output:</strong> 2
<strong>Explanation:</strong> In 2 operations, we can transform nums1 to nums2.
1<sup>st</sup> operation: i = 2, j = 0. After applying the operation, nums1 = [1,3,4,4].
2<sup>nd</sup> operation: i = 2, j = 3. After applying the operation, nums1 = [1,3,7,1].
One can prove that it is impossible to make arrays equal in fewer operations.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> nums1 = [3,8,5,2], nums2 = [2,4,1,6], k = 1
<strong>Output:</strong> -1
<strong>Explanation:</strong> It can be proved that it is impossible to make the two arrays equal.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums1.length == nums2.length</code></li>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums1[i], nums2[j] &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= k &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lần

<!-- thinking:start -->

> **Tư duy**
>
> Một thao tác cộng $k$ vào một chỉ số và trừ $k$ khỏi một chỉ số khác, nên tổng được bảo toàn. Nếu phần dư không chia hết cho $k$, hoặc $k=0$ nhưng có phần tử khác nhau, thì không thể thực hiện biến đổi.
>
> Hãy đếm số lần cần cộng $k$ ($a$) và số lần cần trừ đi giá trị đó ($b$). Các thao tác luôn ghép thành từng cặp, vì vậy hai mảng có thể trở nên giống nhau khi và chỉ khi $a=b$, đáp án là $a$.

<!-- thinking:end -->

Ta dùng hai biến $a$ và $b$ để ghi nhận số lần các phần tử trong $\textit{nums1}$ được tăng thêm $k$ và giảm đi $k$ tương ứng.

Ta duyệt đồng thời hai mảng. Nếu hai phần tử tại vị trí hiện tại bằng nhau, ta bỏ qua. Ngược lại, nếu $k$ bằng $0$ hoặc hiệu của hai phần tử không chia hết cho $k$, ta trả về $-1$. Nếu không, ta tính $t = (x - y) / k$. Nếu $t < 0$, ta cộng $-t$ vào $a$; ngược lại, ta cộng $t$ vào $b$.

Cuối cùng, nếu $a$ bằng $b$, ta trả về $a$; nếu không, ta trả về $-1$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của hai mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums1: List[int], nums2: List[int], k: int) -> int:
        a = b = 0
        for x, y in zip(nums1, nums2):
            if x == y:
                continue
            if k == 0 or (x - y) % k:
                return -1
            t = (x - y) // k
            if t < 0:
                a += -t
            else:
                b += t
        return a if a == b else -1
```

#### Java

```java
class Solution {
    public long minOperations(int[] nums1, int[] nums2, int k) {
        long a = 0, b = 0;
        for (int i = 0; i < nums1.length; ++i) {
            int x = nums1[i], y = nums2[i];
            if (x == y) {
                continue;
            }
            if (k == 0 || (x - y) % k != 0) {
                return -1;
            }
            int t = (x - y) / k;
            if (t < 0) {
                a += -t;
            } else {
                b += t;
            }
        }
        return a == b ? a : -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minOperations(vector<int>& nums1, vector<int>& nums2, int k) {
        long long a = 0, b = 0;
        for (int i = 0; i < nums1.size(); ++i) {
            int x = nums1[i], y = nums2[i];
            if (x == y) {
                continue;
            }
            if (k == 0 || (x - y) % k != 0) {
                return -1;
            }
            int t = (x - y) / k;
            if (t < 0) {
                a += -t;
            } else {
                b += t;
            }
        }
        return a == b ? a : -1;
    }
};
```

#### Go

```go
func minOperations(nums1 []int, nums2 []int, k int) int64 {
	var a, b int64
	for i, x := range nums1 {
		y := nums2[i]
		if x == y {
			continue
		}
		if k == 0 || (x-y)%k != 0 {
			return -1
		}
		t := (x - y) / k
		if t < 0 {
			a += int64(-t)
		} else {
			b += int64(t)
		}
	}
	if a == b {
		return a
	}
	return -1
}
```

#### TypeScript

```ts
function minOperations(nums1: number[], nums2: number[], k: number): number {
    let [a, b] = [0, 0];
    for (let i = 0; i < nums1.length; ++i) {
        const [x, y] = [nums1[i], nums2[i]];
        if (x === y) {
            continue;
        }
        if (k === 0 || (x - y) % k !== 0) {
            return -1;
        }
        const t = (x - y) / k;
        if (t < 0) {
            a += -t;
        } else {
            b += t;
        }
    }
    return a === b ? a : -1;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_operations(nums1: Vec<i32>, nums2: Vec<i32>, k: i32) -> i64 {
        let mut a: i64 = 0;
        let mut b: i64 = 0;
        for (&x, &y) in nums1.iter().zip(nums2.iter()) {
            if x == y {
                continue;
            }
            if k == 0 || (x - y) % k != 0 {
                return -1;
            }
            let t = (x - y) / k;
            if t < 0 {
                a += (-t) as i64;
            } else {
                b += t as i64;
            }
        }
        if a == b {
            a
        } else {
            -1
        }
    }
}
```

#### C

```c
long long minOperations(int* nums1, int nums1Size, int* nums2, int nums2Size, int k) {
    long long a = 0, b = 0;
    for (int i = 0; i < nums1Size; ++i) {
        int x = nums1[i], y = nums2[i];
        if (x == y) {
            continue;
        }
        if (k == 0 || (x - y) % k != 0) {
            return -1;
        }
        int t = (x - y) / k;
        if (t < 0) {
            a += -t;
        } else {
            b += t;
        }
    }
    return a == b ? a : -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
