---
comments: true
difficulty: Medium
rating: 1495
source: Biweekly Contest 41 Q2
tags:
    - Array
    - Math
    - Prefix Sum
---

<!-- problem:start -->

# [1685. Sum of Absolute Differences in a Sorted Array](https://leetcode.com/problems/sum-of-absolute-differences-in-a-sorted-array)

[中文文档](/solution/1600-1699/1685.Sum%20of%20Absolute%20Differences%20in%20a%20Sorted%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> được sắp xếp theo thứ tự <strong>không giảm</strong>.</p>

<p>Hãy tạo và trả về <em>mảng số nguyên </em><code>result</code><em> có cùng độ dài với </em><code>nums</code><em>, sao cho </em><code>result[i]</code><em> bằng <strong>tổng các hiệu tuyệt đối</strong> giữa </em><code>nums[i]</code><em> và mọi phần tử khác trong mảng.</em></p>

<p>Nói cách khác, <code>result[i]</code> bằng <code>sum(|nums[i]-nums[j]|)</code> với <code>0 &lt;= j &lt; nums.length</code> và <code>j != i</code> (<strong>đánh chỉ số từ 0</strong>).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [2,3,5]
<strong>Output:</strong> [4,3,5]
<strong>Giải thích:</strong> Giả sử các mảng được đánh chỉ số từ 0, khi đó
result[0] = |2-2| + |2-3| + |2-5| = 0 + 1 + 3 = 4,
result[1] = |3-2| + |3-3| + |3-5| = 1 + 0 + 2 = 3,
result[2] = |5-2| + |5-3| + |5-5| = 3 + 2 + 0 = 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,4,6,8,10]
<strong>Output:</strong> [24,15,13,15,21]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= nums[i + 1] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tính tổng + Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Mảng đã được sắp xếp, nên $\lvert x-y \rvert$ là $x-y$ ở bên trái và $y-x$ ở bên phải. Tổng hiệu tuyệt đối tại mỗi $i$ có thể tính trong $O(1)$ từ tổng bên trái và bên phải.
>
> Với tổng toàn bộ $s$ và tổng đã duyệt $t$, ta có $\textit{ans}[i]=x\cdot i-t+(s-t)-x\cdot(n-i)$, sau đó cộng $x$ vào $t$.

<!-- thinking:end -->

Đầu tiên, ta tính tổng mọi phần tử trong mảng $nums$, ký hiệu là $s$. Dùng biến $t$ để lưu tổng các phần tử đã duyệt.

Tiếp theo, ta duyệt $nums[i]$. Khi đó $ans[i] = nums[i] \times i - t + s - t - nums[i] \times (n - i)$. Sau đó cập nhật $t$, tức $t = t + nums[i]$. Tiếp tục với phần tử kế tiếp cho đến khi duyệt hết.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getSumAbsoluteDifferences(self, nums: List[int]) -> List[int]:
        ans = []
        s, t = sum(nums), 0
        for i, x in enumerate(nums):
            v = x * i - t + s - t - x * (len(nums) - i)
            ans.append(v)
            t += x
        return ans
```

#### Java

```java
class Solution {
    public int[] getSumAbsoluteDifferences(int[] nums) {
        // int s = Arrays.stream(nums).sum();
        int s = 0, t = 0;
        for (int x : nums) {
            s += x;
        }
        int n = nums.length;
        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            int v = nums[i] * i - t + s - t - nums[i] * (n - i);
            ans[i] = v;
            t += nums[i];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> getSumAbsoluteDifferences(vector<int>& nums) {
        int s = accumulate(nums.begin(), nums.end(), 0), t = 0;
        int n = nums.size();
        vector<int> ans(n);
        for (int i = 0; i < n; ++i) {
            int v = nums[i] * i - t + s - t - nums[i] * (n - i);
            ans[i] = v;
            t += nums[i];
        }
        return ans;
    }
};
```

#### Go

```go
func getSumAbsoluteDifferences(nums []int) (ans []int) {
	var s, t int
	for _, x := range nums {
		s += x
	}
	for i, x := range nums {
		v := x*i - t + s - t - x*(len(nums)-i)
		ans = append(ans, v)
		t += x
	}
	return
}
```

#### TypeScript

```ts
function getSumAbsoluteDifferences(nums: number[]): number[] {
    const s = nums.reduce((a, b) => a + b);
    let t = 0;
    const n = nums.length;
    const ans = new Array(n);
    for (let i = 0; i < n; ++i) {
        const v = nums[i] * i - t + s - t - nums[i] * (n - i);
        ans[i] = v;
        t += nums[i];
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var getSumAbsoluteDifferences = function (nums) {
    const s = nums.reduce((a, b) => a + b);
    let t = 0;
    const n = nums.length;
    const ans = new Array(n);
    for (let i = 0; i < n; ++i) {
        const v = nums[i] * i - t + s - t - nums[i] * (n - i);
        ans[i] = v;
        t += nums[i];
    }
    return ans;
};
```

#### C#

```cs
public class Solution {
    public int[] GetSumAbsoluteDifferences(int[] nums) {
        int s = 0, t = 0;
        foreach (int x in nums) {
            s += x;
        }
        int n = nums.Length;
        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            int v = nums[i] * i - t + s - t - nums[i] * (n - i);
            ans[i] = v;
            t += nums[i];
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
