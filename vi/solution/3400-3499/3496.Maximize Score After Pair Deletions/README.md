---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [3496. Maximize Score After Pair Deletions 🔒](https://leetcode.com/problems/maximize-score-after-pair-deletions)

[中文文档](/solution/3400-3499/3496.Maximize%20Score%20After%20Pair%20Deletions/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>. Bạn <strong>phải</strong> liên tục thực hiện một trong các thao tác sau khi mảng còn nhiều hơn hai phần tử:</p>

<ul>
	<li>Xóa hai phần tử đầu tiên.</li>
	<li>Xóa hai phần tử cuối cùng.</li>
	<li>Xóa phần tử đầu tiên và phần tử cuối cùng.</li>
</ul>

<p>Với mỗi thao tác, hãy cộng tổng các phần tử bị xóa vào tổng điểm của bạn.</p>

<p>Trả về <strong>điểm số lớn nhất</strong> có thể đạt được.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,4,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các thao tác có thể thực hiện là:</p>

<ul>
	<li>Xóa hai phần tử đầu tiên <code>(2 + 4) = 6</code>. Mảng còn lại là <code>[1]</code>.</li>
	<li>Xóa hai phần tử cuối cùng <code>(4 + 1) = 5</code>. Mảng còn lại là <code>[2]</code>.</li>
	<li>Xóa phần tử đầu tiên và phần tử cuối cùng <code>(2 + 1) = 3</code>. Mảng còn lại là <code>[4]</code>.</li>
</ul>

<p>Điểm số lớn nhất đạt được bằng cách xóa hai phần tử đầu tiên, khi đó điểm cuối cùng là 6.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,-1,4,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các thao tác có thể thực hiện là:</p>

<ul>
	<li>Xóa phần tử đầu tiên và phần tử cuối cùng <code>(5 + 2) = 7</code>. Mảng còn lại là <code>[-1, 4]</code>.</li>
	<li>Xóa hai phần tử đầu tiên <code>(5 + -1) = 4</code>. Mảng còn lại là <code>[4, 2]</code>.</li>
	<li>Xóa hai phần tử cuối cùng <code>(4 + 2) = 6</code>. Mảng còn lại là <code>[5, -1]</code>.</li>
</ul>

<p>Điểm số lớn nhất đạt được bằng cách xóa phần tử đầu tiên và phần tử cuối cùng, khi đó tổng điểm là 7.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>4</sup> &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tư duy ngược

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lần thực hiện thao tác, ta xóa một cặp phần tử ở hai đầu và ghi điểm bằng tổng của chúng, cho đến khi còn lại một hoặc hai phần tử. Việc thử tất cả thứ tự xóa có độ phức tạp lũy thừa.
>
> Xét theo chiều ngược lại: mảng có độ dài lẻ sẽ kết thúc với một phần tử, còn mảng có độ dài chẵn sẽ kết thúc với hai phần tử kề nhau. Điểm số bằng tổng toàn bộ mảng trừ đi phần còn lại.
>
> Tối đa hóa điểm số tương đương với tối thiểu hóa phần còn lại. Vì vậy, khi $n$ lẻ, ta trừ đi giá trị nhỏ nhất toàn cục; khi $n$ chẵn, ta trừ đi tổng nhỏ nhất của một cặp phần tử kề nhau.

<!-- thinking:end -->

Theo mô tả bài toán, mỗi thao tác sẽ xóa hai phần tử ở hai đầu mảng. Do đó, khi số lượng phần tử là lẻ, cuối cùng sẽ còn lại một phần tử; khi số lượng phần tử là chẵn, cuối cùng sẽ còn lại hai phần tử liên tiếp trong mảng.

Để tối đa hóa điểm số sau các lần xóa, ta cần tối thiểu hóa các phần tử còn lại.

Vì vậy, nếu mảng $\textit{nums}$ có số lượng phần tử lẻ, đáp án là tổng của tất cả phần tử $s$ trong mảng $\textit{nums}$ trừ đi giá trị nhỏ nhất $\textit{mi}$ trong $\textit{nums}$; nếu mảng $\textit{nums}$ có số lượng phần tử chẵn, đáp án là tổng của tất cả phần tử $s$ trong mảng $\textit{nums}$ trừ đi tổng nhỏ nhất của hai phần tử liên tiếp bất kỳ.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxScore(self, nums: List[int]) -> int:
        s = sum(nums)
        if len(nums) & 1:
            return s - min(nums)
        return s - min(a + b for a, b in pairwise(nums))
```

#### Java

```java
class Solution {
    public int maxScore(int[] nums) {
        final int inf = 1 << 30;
        int n = nums.length;
        int s = 0, mi = inf;
        int t = inf;
        for (int i = 0; i < n; ++i) {
            s += nums[i];
            mi = Math.min(mi, nums[i]);
            if (i + 1 < n) {
                t = Math.min(t, nums[i] + nums[i + 1]);
            }
        }
        if (n % 2 == 1) {
            return s - mi;
        }
        return s - t;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxScore(vector<int>& nums) {
        const int inf = 1 << 30;
        int n = nums.size();
        int s = 0, mi = inf;
        int t = inf;
        for (int i = 0; i < n; ++i) {
            s += nums[i];
            mi = min(mi, nums[i]);
            if (i + 1 < n) {
                t = min(t, nums[i] + nums[i + 1]);
            }
        }
        if (n % 2 == 1) {
            return s - mi;
        }
        return s - t;
    }
};
```

#### Go

```go
func maxScore(nums []int) int {
	const inf = 1 << 30
	n := len(nums)
	s, mi, t := 0, inf, inf
	for i, x := range nums {
		s += x
		mi = min(mi, x)
		if i+1 < n {
			t = min(t, x+nums[i+1])
		}
	}
	if n%2 == 1 {
		return s - mi
	}
	return s - t
}
```

#### TypeScript

```ts
function maxScore(nums: number[]): number {
    const inf = Infinity;
    const n = nums.length;
    let [s, mi, t] = [0, inf, inf];
    for (let i = 0; i < n; ++i) {
        s += nums[i];
        mi = Math.min(mi, nums[i]);
        if (i + 1 < n) {
            t = Math.min(t, nums[i] + nums[i + 1]);
        }
    }
    return n % 2 ? s - mi : s - t;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
