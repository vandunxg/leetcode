---
comments: true
difficulty: Easy
rating: 1300
source: Weekly Contest 425 Q1
tags:
    - Array
    - Prefix Sum
    - Sliding Window
---

<!-- problem:start -->

# [3364. Minimum Positive Sum Subarray](https://leetcode.com/problems/minimum-positive-sum-subarray)

[中文文档](/solution/3300-3399/3364.Minimum%20Positive%20Sum%20Subarray/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và <strong>hai</strong> số nguyên <code>l</code> và <code>r</code>. Nhiệm vụ của bạn là tìm tổng <strong>nhỏ nhất</strong> của một <strong>mảng con</strong> có kích thước nằm trong khoảng từ <code>l</code> đến <code>r</code> (bao gồm cả hai đầu mút) và có tổng lớn hơn 0.</p>

<p>Trả về tổng <strong>nhỏ nhất</strong> của mảng con như vậy. Nếu không tồn tại mảng con nào, trả về -1.</p>

<p><strong>Mảng con</strong> là một dãy phần tử liên tiếp <b>không rỗng</b> trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3, -2, 1, 4], l = 2, r = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con có độ dài từ <code>l = 2</code> đến <code>r = 3</code> và có tổng lớn hơn 0 là:</p>

<ul>
	<li><code>[3, -2]</code> có tổng bằng 1</li>
	<li><code>[1, 4]</code> có tổng bằng 5</li>
	<li><code>[3, -2, 1]</code> có tổng bằng 2</li>
	<li><code>[-2, 1, 4]</code> có tổng bằng 3</li>
</ul>

<p>Trong số này, mảng con <code>[3, -2]</code> có tổng bằng 1, là tổng dương nhỏ nhất. Do đó, đáp án là 1.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-2, 2, -3, 1], l = 2, r = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có mảng con nào có độ dài từ <code>l</code> đến <code>r</code> và có tổng lớn hơn 0. Vì vậy, đáp án là -1.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1, 2, 3, 4], l = 2, r = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng con <code>[1, 2]</code> có độ dài bằng 2 và có tổng dương nhỏ nhất. Vì vậy, đáp án là 3.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= l &lt;= r &lt;= nums.length</code></li>
	<li><code>-1000 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm tổng dương nhỏ nhất trong các mảng con có độ dài nằm trong $[l,r]$. Vì $n \le 100$, ta có thể liệt kê cả hai đầu mút.
>
> Cố định đầu trái và cộng dồn $s$ về phía bên phải; cập nhật đáp án khi độ dài hợp lệ và $s>0$.
>
> Nếu không xuất hiện tổng dương nào, trả về $-1$.

<!-- thinking:end -->

Ta có thể liệt kê điểm bắt đầu $i$ của mảng con, sau đó liệt kê điểm kết thúc $j$ từ $i$ đến $n$ trong khoảng $[i, n)$. Ta tính tổng $s$ của khoảng $[i, j]$. Nếu $s$ lớn hơn $0$ và độ dài khoảng nằm trong $[l, r]$, ta cập nhật đáp án.

Cuối cùng, nếu đáp án vẫn có giá trị khởi tạo, nghĩa là không có mảng con nào thỏa mãn điều kiện, nên ta trả về $-1$. Nếu không, ta trả về đáp án.

Độ phức tạp thời gian là $O(n^2)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumSumSubarray(self, nums: List[int], l: int, r: int) -> int:
        n = len(nums)
        ans = inf
        for i in range(n):
            s = 0
            for j in range(i, n):
                s += nums[j]
                if l <= j - i + 1 <= r and s > 0:
                    ans = min(ans, s)
        return -1 if ans == inf else ans
```

#### Java

```java
class Solution {
    public int minimumSumSubarray(List<Integer> nums, int l, int r) {
        int n = nums.size();
        final int inf = Integer.MAX_VALUE;
        int ans = inf;
        for (int i = 0; i < n; ++i) {
            int s = 0;
            for (int j = i; j < n; ++j) {
                s += nums.get(j);
                int k = j - i + 1;
                if (k >= l && k <= r && s > 0) {
                    ans = Math.min(ans, s);
                }
            }
        }
        return ans == inf ? -1 : ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumSumSubarray(vector<int>& nums, int l, int r) {
        int n = nums.size();
        const int inf = INT_MAX;
        int ans = inf;
        for (int i = 0; i < n; ++i) {
            int s = 0;
            for (int j = i; j < n; ++j) {
                s += nums[j];
                int k = j - i + 1;
                if (k >= l && k <= r && s > 0) {
                    ans = min(ans, s);
                }
            }
        }
        return ans == inf ? -1 : ans;
    }
};
```

#### Go

```go
func minimumSumSubarray(nums []int, l int, r int) int {
	const inf int = 1 << 30
	ans := inf
	for i := range nums {
		s := 0
		for j := i; j < len(nums); j++ {
			s += nums[j]
			k := j - i + 1
			if k >= l && k <= r && s > 0 {
				ans = min(ans, s)
			}
		}
	}
	if ans == inf {
		return -1
	}
	return ans
}
```

#### TypeScript

```ts
function minimumSumSubarray(nums: number[], l: number, r: number): number {
    const n = nums.length;
    let ans = Infinity;
    for (let i = 0; i < n; ++i) {
        let s = 0;
        for (let j = i; j < n; ++j) {
            s += nums[j];
            const k = j - i + 1;
            if (k >= l && k <= r && s > 0) {
                ans = Math.min(ans, s);
            }
        }
    }
    return ans == Infinity ? -1 : ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
