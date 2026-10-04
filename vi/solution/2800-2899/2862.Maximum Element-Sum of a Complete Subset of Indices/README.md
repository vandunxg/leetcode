---
comments: true
difficulty: Hard
rating: 2291
source: Weekly Contest 363 Q4
tags:
    - Array
    - Math
    - Number Theory
---

<!-- problem:start -->

# [2862. Maximum Element-Sum of a Complete Subset of Indices](https://leetcode.com/problems/maximum-element-sum-of-a-complete-subset-of-indices)

[中文文档](/solution/2800-2899/2862.Maximum%20Element-Sum%20of%20a%20Complete%20Subset%20of%20Indices/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một mảng <code>nums</code> <strong>được đánh chỉ số từ</strong> <strong>1</strong>. Nhiệm vụ của bạn là chọn một <strong>tập con hoàn chỉnh</strong> từ <code>nums</code> sao cho tích của mọi cặp chỉ số được chọn là <span data-keyword="perfect-square">số chính phương</span>, tức là nếu bạn chọn <code>a<sub>i</sub></code> và <code>a<sub>j</sub></code> thì <code>i * j</code> phải là một số chính phương.</p>

<p>Trả về <em>tổng</em> của tập con hoàn chỉnh có <em>tổng lớn nhất</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [8,7,3,5,7,2,4,9]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">16</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta chọn các phần tử tại chỉ số 2 và 8, và <code>2 * 8</code> là một số chính phương.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [8,10,3,8,1,13,7,9,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">20</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta chọn các phần tử tại chỉ số 1, 4 và 9. <code>1 * 4</code>, <code>1 * 9</code>, <code>4 * 9</code> đều là các số chính phương.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Một tập chỉ số là hoàn chỉnh khi và chỉ khi tích của mọi cặp chỉ số đều là số chính phương, tức là chúng có cùng phần lõi không chứa bình phương. Ta liệt kê kernel $k$ và tính tổng $nums[k\cdot j^2-1]$ với mọi $j$ hợp lệ.

<!-- thinking:end -->

Ta nhận thấy nếu một số có thể được biểu diễn dưới dạng $k \times j^2$, thì mọi số có dạng này đều có cùng $k$.

Do đó, ta có thể liệt kê $k$ trong khoảng $[1,..n]$, sau đó bắt đầu liệt kê $j$ từ $1$, mỗi lần cộng giá trị của $nums[k \times j^2 - 1]$ vào $t$, cho đến khi $k \times j^2 > n$. Khi đó, cập nhật đáp án thành $ans = \max(ans, t)$.

Cuối cùng, trả về đáp án $ans$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumSum(self, nums: List[int]) -> int:
        n = len(nums)
        ans = 0
        for k in range(1, n + 1):
            t = 0
            j = 1
            while k * j * j <= n:
                t += nums[k * j * j - 1]
                j += 1
            ans = max(ans, t)
        return ans
```

#### Java

```java
class Solution {
    public long maximumSum(List<Integer> nums) {
        long ans = 0;
        int n = nums.size();
        for (int k = 1; k <= n; ++k) {
            long t = 0;
            for (int j = 1; k * j * j <= n; ++j) {
                t += nums.get(k * j * j - 1);
            }
            ans = Math.max(ans, t);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maximumSum(vector<int>& nums) {
        long long ans = 0;
        int n = nums.size();
        for (int k = 1; k <= n; ++k) {
            long long t = 0;
            for (int j = 1; k * j * j <= n; ++j) {
                t += nums[k * j * j - 1];
            }
            ans = max(ans, t);
        }
        return ans;
    }
};
```

#### Go

```go
func maximumSum(nums []int) (ans int64) {
	n := len(nums)
	for k := 1; k <= n; k++ {
		var t int64
		for j := 1; k*j*j <= n; j++ {
			t += int64(nums[k*j*j-1])
		}
		ans = max(ans, t)
	}
	return
}
```

#### TypeScript

```ts
function maximumSum(nums: number[]): number {
    let ans = 0;
    const n = nums.length;
    for (let k = 1; k <= n; ++k) {
        let t = 0;
        for (let j = 1; k * j * j <= n; ++j) {
            t += nums[k * j * j - 1];
        }
        ans = Math.max(ans, t);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
