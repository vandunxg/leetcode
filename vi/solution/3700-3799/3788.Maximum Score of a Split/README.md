---
comments: true
difficulty: Medium
rating: 1306
source: Weekly Contest 482 Q1
tags:
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [3788. Maximum Score of a Split](https://leetcode.com/problems/maximum-score-of-a-split)

[中文文档](/solution/3700-3799/3788.Maximum%20Score%20of%20a%20Split/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>.</p>

<p>Hãy chọn một chỉ số <code>i</code> sao cho <code>0 &lt;= i &lt; n - 1</code>.</p>

<p>Với chỉ số phân chia <code>i</code> đã chọn:</p>

<ul>
	<li>Gọi <code>prefixSum(i)</code> là tổng của <code>nums[0] + nums[1] + ... + nums[i]</code>.</li>
	<li>Gọi <code>suffixMin(i)</code> là giá trị nhỏ nhất trong các phần tử <code>nums[i + 1], nums[i + 2], ..., nums[n - 1]</code>.</li>
</ul>

<p><strong>Điểm số</strong> của phép phân chia tại chỉ số <code>i</code> được định nghĩa là:</p>

<p><code>score(i) = prefixSum(i) - suffixMin(i)</code></p>

<p>Hãy trả về một số nguyên biểu thị <strong>điểm số lớn nhất</strong> trong tất cả các chỉ số phân chia hợp lệ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [10,-1,3,-4,-5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">17</span></p>

<p><strong>Giải thích:</strong></p>

<p>Phép phân chia tối ưu tại <code>i = 2</code>, <code>score(2) = prefixSum(2) - suffixMin(2) = (10 + (-1) + 3) - (-5) = 17</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-7,-5,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Phép phân chia tối ưu tại <code>i = 0</code>, <code>score(0) = prefixSum(0) - suffixMin(0) = (-7) - (-5) = -2</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Phép phân chia hợp lệ duy nhất là tại <code>i = 0</code>, <code>score(0) = prefixSum(0) - suffixMin(0) = 1 - 1 = 0</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup>​​​​​​​ &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng tiền tố + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Điểm số là tổng tiền tố trừ đi giá trị nhỏ nhất của hậu tố. Ta tính trước các giá trị nhỏ nhất của hậu tố khi duyệt từ phải sang trái, sau đó duyệt các vị trí phân chia từ trái sang phải để duy trì tổng tiền tố và tìm điểm số lớn nhất.

<!-- thinking:end -->

Trước tiên, chúng ta định nghĩa một mảng $\textit{suf}$ có độ dài $n$, trong đó $\textit{suf}[i]$ biểu thị giá trị nhỏ nhất của mảng $\textit{nums}$ từ chỉ số $i$ đến chỉ số $n - 1$. Ta có thể duyệt mảng $\textit{nums}$ từ cuối về đầu để tính mảng $\textit{suf}$.

Tiếp theo, chúng ta định nghĩa biến $\textit{pre}$ để biểu thị tổng tiền tố của mảng $\textit{nums}$. Ta duyệt qua $n - 1$ phần tử đầu tiên của mảng $\textit{nums}$. Với mỗi chỉ số $i$, ta cộng $\textit{nums}[i]$ vào $\textit{pre}$ và tính điểm số phân chia $\textit{score}(i) = \textit{pre} - \textit{suf}[i + 1]$. Ta dùng biến $\textit{ans}$ để duy trì giá trị lớn nhất trong tất cả các điểm số phân chia.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumScore(self, nums: List[int]) -> int:
        n = len(nums)
        suf = [nums[-1]] * n
        for i in range(n - 2, -1, -1):
            suf[i] = min(nums[i], suf[i + 1])
        ans = -inf
        pre = 0
        for i in range(n - 1):
            pre += nums[i]
            ans = max(ans, pre - suf[i + 1])
        return ans
```

#### Java

```java
class Solution {
    public long maximumScore(int[] nums) {
        int n = nums.length;
        long[] suf = new long[n];
        suf[n - 1] = nums[n - 1];
        for (int i = n - 2; i >= 0; --i) {
            suf[i] = Math.min(nums[i], suf[i + 1]);
        }
        long ans = Long.MIN_VALUE;
        long pre = 0;
        for (int i = 0; i < n - 1; ++i) {
            pre += nums[i];
            ans = Math.max(ans, pre - suf[i + 1]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maximumScore(vector<int>& nums) {
        int n = nums.size();
        vector<long long> suf(n);
        suf[n - 1] = nums[n - 1];
        for (int i = n - 2; i >= 0; --i) {
            suf[i] = min((long long) nums[i], suf[i + 1]);
        }
        long long ans = LLONG_MIN;
        long long pre = 0;
        for (int i = 0; i < n - 1; ++i) {
            pre += nums[i];
            ans = max(ans, pre - suf[i + 1]);
        }
        return ans;
    }
};
```

#### Go

```go
func maximumScore(nums []int) int64 {
	n := len(nums)
	suf := make([]int64, n)
	suf[n-1] = int64(nums[n-1])
	for i := n - 2; i >= 0; i-- {
		suf[i] = min(int64(nums[i]), suf[i+1])
	}
	var pre int64 = 0
	var ans int64 = math.MinInt64
	for i := 0; i < n-1; i++ {
		pre += int64(nums[i])
		ans = max(ans, pre-suf[i+1])
	}
	return ans
}
```

#### TypeScript

```ts
function maximumScore(nums: number[]): number {
    const n = nums.length;
    const suf: number[] = new Array(n);
    suf[n - 1] = nums[n - 1];
    for (let i = n - 2; i >= 0; --i) {
        suf[i] = Math.min(nums[i], suf[i + 1]);
    }
    let ans = Number.NEGATIVE_INFINITY;
    let pre = 0;
    for (let i = 0; i < n - 1; ++i) {
        pre += nums[i];
        ans = Math.max(ans, pre - suf[i + 1]);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
