---
comments: true
difficulty: Medium
rating: 1802
source: Weekly Contest 371 Q3
tags:
    - Array
    - Enumeration
---

<!-- problem:start -->

# [2934. Minimum Operations to Maximize Last Elements in Arrays](https://leetcode.com/problems/minimum-operations-to-maximize-last-elements-in-arrays)

[中文文档](/solution/2900-2999/2934.Minimum%20Operations%20to%20Maximize%20Last%20Elements%20in%20Arrays/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <strong>0-indexed</strong> là <code>nums1</code> và <code>nums2</code>, cả hai đều có độ dài <code>n</code>.</p>

<p>Bạn được phép thực hiện một chuỗi <strong>thao tác</strong> (<strong>có thể không thực hiện thao tác nào</strong>).</p>

<p>Trong mỗi thao tác, bạn chọn một chỉ số <code>i</code> trong phạm vi <code>[0, n - 1]</code> và <strong>hoán đổi</strong> các giá trị của <code>nums1[i]</code> và <code>nums2[i]</code>.</p>

<p>Nhiệm vụ của bạn là tìm số thao tác <strong>nhỏ nhất</strong> cần thực hiện để thỏa mãn các điều kiện sau:</p>

<ul>
	<li><code>nums1[n - 1]</code> bằng <strong>giá trị lớn nhất</strong> trong tất cả phần tử của <code>nums1</code>, tức là <code>nums1[n - 1] = max(nums1[0], nums1[1], ..., nums1[n - 1])</code>.</li>
	<li><code>nums2[n - 1]</code> bằng <strong>giá trị</strong> <strong>lớn nhất</strong> trong tất cả phần tử của <code>nums2</code>, tức là <code>nums2[n - 1] = max(nums2[0], nums2[1], ..., nums2[n - 1])</code>.</li>
</ul>

<p>Trả về <em>một số nguyên biểu thị số thao tác <strong>nhỏ nhất</strong> cần thực hiện để thỏa mãn <strong>cả hai</strong> điều kiện</em>, <em>hoặc </em><code>-1</code><em> nếu <strong>không thể</strong> thỏa mãn cả hai điều kiện.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [1,2,7], nums2 = [4,5,3]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Trong ví dụ này, ta có thể thực hiện một thao tác tại chỉ số i = 2.
Khi hoán đổi nums1[2] và nums2[2], nums1 trở thành [1,2,3] và nums2 trở thành [4,5,7].
Cả hai điều kiện đều được thỏa mãn.
Có thể chứng minh rằng số thao tác nhỏ nhất cần thực hiện là 1.
Vì vậy, đáp án là 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [2,3,4,5,9], nums2 = [8,8,4,4,4]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Trong ví dụ này, ta có thể thực hiện các thao tác sau:
Thao tác đầu tiên tại chỉ số i = 4.
Khi hoán đổi nums1[4] và nums2[4], nums1 trở thành [2,3,4,5,4] và nums2 trở thành [8,8,4,4,9].
Thao tác tiếp theo tại chỉ số i = 3.
Khi hoán đổi nums1[3] và nums2[3], nums1 trở thành [2,3,4,4,4] và nums2 trở thành [8,8,4,5,9].
Cả hai điều kiện đều được thỏa mãn.
Có thể chứng minh rằng số thao tác nhỏ nhất cần thực hiện là 2.
Vì vậy, đáp án là 2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [1,5,4], nums2 = [2,5,3]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Trong ví dụ này, không thể thỏa mãn cả hai điều kiện.
Vì vậy, đáp án là -1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums1.length == nums2.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums1[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= nums2[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân tích trường hợp + Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Giá trị lớn nhất của $nums1$ và $nums2$ phải nằm ở cuối, và chỉ được phép hoán đổi các phần tử cùng chỉ số. Cặp cuối cùng hoặc được giữ nguyên hoặc được hoán đổi một lần; lựa chọn đó quyết định cách định hướng của mọi cặp trước đó.
>
> $f(x,y)$ giả sử hai giá trị cuối là $(x,y)$. Mỗi cặp trước đó được giữ nguyên, hoán đổi hoặc không thể xử lý. Đáp án là giá trị tốt hơn giữa “không hoán đổi cặp cuối” và “hoán đổi cặp cuối rồi cộng thêm một thao tác”.

<!-- thinking:end -->

Ta xét hai trường hợp:

1. Không hoán đổi các giá trị của $nums1[n - 1]$ và $nums2[n - 1]$
2. Hoán đổi các giá trị của $nums1[n - 1]$ và $nums2[n - 1]$

Với mỗi trường hợp, ta ký hiệu các giá trị cuối của hai mảng $nums1$ và $nums2$ lần lượt là $x$ và $y$. Sau đó, ta duyệt qua $n - 1$ giá trị đầu tiên của hai mảng $nums1$ và $nums2$, đồng thời dùng biến $cnt$ để ghi nhận số lần hoán đổi. Nếu $nums1[i] \leq x$ và $nums2[i] \leq y$ thì không cần hoán đổi. Ngược lại, nếu $nums1[i] \leq y$ và $nums2[i] \leq x$ thì cần hoán đổi. Nếu cả hai điều kiện đều không thỏa mãn, trả về $-1$. Cuối cùng, trả về $cnt$.

Ta ký hiệu số lần hoán đổi trong hai trường hợp lần lượt là $a$ và $b$. Nếu $a + b = -2$, không thể thỏa mãn cả hai điều kiện, nên trả về $-1$. Ngược lại, trả về $\min(a, b + 1)$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums1: List[int], nums2: List[int]) -> int:
        def f(x: int, y: int) -> int:
            cnt = 0
            for a, b in zip(nums1[:-1], nums2[:-1]):
                if a <= x and b <= y:
                    continue
                if not (a <= y and b <= x):
                    return -1
                cnt += 1
            return cnt

        a, b = f(nums1[-1], nums2[-1]), f(nums2[-1], nums1[-1])
        return -1 if a + b == -2 else min(a, b + 1)
```

#### Java

```java
class Solution {
    private int n;

    public int minOperations(int[] nums1, int[] nums2) {
        n = nums1.length;
        int a = f(nums1, nums2, nums1[n - 1], nums2[n - 1]);
        int b = f(nums1, nums2, nums2[n - 1], nums1[n - 1]);
        return a + b == -2 ? -1 : Math.min(a, b + 1);
    }

    private int f(int[] nums1, int[] nums2, int x, int y) {
        int cnt = 0;
        for (int i = 0; i < n - 1; ++i) {
            if (nums1[i] <= x && nums2[i] <= y) {
                continue;
            }
            if (!(nums1[i] <= y && nums2[i] <= x)) {
                return -1;
            }
            ++cnt;
        }
        return cnt;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minOperations(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size();
        auto f = [&](int x, int y) {
            int cnt = 0;
            for (int i = 0; i < n - 1; ++i) {
                if (nums1[i] <= x && nums2[i] <= y) {
                    continue;
                }
                if (!(nums1[i] <= y && nums2[i] <= x)) {
                    return -1;
                }
                ++cnt;
            }
            return cnt;
        };
        int a = f(nums1.back(), nums2.back());
        int b = f(nums2.back(), nums1.back());
        return a + b == -2 ? -1 : min(a, b + 1);
    }
};
```

#### Go

```go
func minOperations(nums1 []int, nums2 []int) int {
	n := len(nums1)
	f := func(x, y int) (cnt int) {
		for i, a := range nums1[:n-1] {
			b := nums2[i]
			if a <= x && b <= y {
				continue
			}
			if !(a <= y && b <= x) {
				return -1
			}
			cnt++
		}
		return
	}
	a, b := f(nums1[n-1], nums2[n-1]), f(nums2[n-1], nums1[n-1])
	if a+b == -2 {
		return -1
	}
	return min(a, b+1)
}
```

#### TypeScript

```ts
function minOperations(nums1: number[], nums2: number[]): number {
    const n = nums1.length;
    const f = (x: number, y: number): number => {
        let cnt = 0;
        for (let i = 0; i < n - 1; ++i) {
            if (nums1[i] <= x && nums2[i] <= y) {
                continue;
            }
            if (!(nums1[i] <= y && nums2[i] <= x)) {
                return -1;
            }
            ++cnt;
        }
        return cnt;
    };
    const a = f(nums1.at(-1), nums2.at(-1));
    const b = f(nums2.at(-1), nums1.at(-1));
    return a + b === -2 ? -1 : Math.min(a, b + 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
