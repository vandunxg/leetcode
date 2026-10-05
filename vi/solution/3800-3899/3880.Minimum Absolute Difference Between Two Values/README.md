---
comments: true
difficulty: Easy
rating: 1257
source: Biweekly Contest 179 Q1
tags:
    - Array
    - Enumeration
---

<!-- problem:start -->

# [3880. Minimum Absolute Difference Between Two Values](https://leetcode.com/problems/minimum-absolute-difference-between-two-values)

[中文文档](/solution/3800-3899/3880.Minimum%20Absolute%20Difference%20Between%20Two%20Values/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> chỉ gồm các giá trị 0, 1 và 2.</p>

<p>Một cặp chỉ số <code>(i, j)</code> được gọi là <strong>hợp lệ</strong> nếu <code>nums[i] == 1</code> và <code>nums[j] == 2</code>.</p>

<p>Hãy trả về <strong>giá trị nhỏ nhất</strong> của hiệu tuyệt đối giữa <code>i</code> và <code>j</code> trong tất cả các cặp hợp lệ. Nếu không tồn tại cặp hợp lệ nào, trả về -1.</p>

<p>Hiệu tuyệt đối giữa các chỉ số <code>i</code> và <code>j</code> được định nghĩa là <code>abs(i - j)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,0,0,2,0,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các cặp hợp lệ là:</p>

<ul>
	<li>(0, 3) có hiệu tuyệt đối bằng <code>abs(0 - 3) = 3</code>.</li>
	<li>(5, 3) có hiệu tuyệt đối bằng <code>abs(5 - 3) = 2</code>.</li>
</ul>

<p>Do đó, đáp án là 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,0,1,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Trong mảng không có cặp hợp lệ nào, do đó đáp án là -1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 2</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lần

<!-- thinking:start -->

> **Tư duy**
>
> Mảng chỉ chứa $0,1,2$; ta cần tìm khoảng cách chỉ số nhỏ nhất giữa một giá trị $1$ và một giá trị $2$. Với độ dài $\le 100$, ta chỉ cần duyệt mảng một lần.
>
> Giá trị đối lập gần nhất chính là lần xuất hiện cuối cùng của $3-x$.
>
> Lưu chỉ số gần nhất của $1$ và $2$; khi gặp một giá trị khác 0, lấy chỉ số hiện tại trừ đi chỉ số cuối cùng của giá trị đối lập.
>
> Nếu chưa từng tạo được cặp nào, trả về $-1$.

<!-- thinking:end -->

Ta sử dụng một mảng $\textit{last}$ có độ dài $3$ để ghi lại chỉ số xuất hiện gần nhất của các chữ số $0$, $1$ và $2$. Ban đầu, $\textit{last} = [-(n+1), -(n+1), -(n+1)]$. Ta duyệt qua mảng $\textit{nums}$. Với số hiện tại $x$, nếu $x$ khác $0$, ta cập nhật đáp án $\textit{ans} = \min(\textit{ans}, i - \textit{last}[3 - x])$, trong đó $i$ là chỉ số của số hiện tại $x$. Sau đó, ta cập nhật $\textit{last}[x] = i$.

Sau khi duyệt xong, nếu $\textit{ans}$ lớn hơn độ dài của mảng $\textit{nums}$, điều đó có nghĩa là không tồn tại cặp chỉ số hợp lệ nào, nên ta trả về -1; ngược lại, ta trả về $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minAbsoluteDifference(self, nums: list[int]) -> int:
        n = len(nums)
        ans = n + 1
        last = [-inf] * 3
        for i, x in enumerate(nums):
            if x:
                ans = min(ans, i - last[3 - x])
                last[x] = i
        return -1 if ans > n else ans
```

#### Java

```java
class Solution {
    public int minAbsoluteDifference(int[] nums) {
        int n = nums.length;
        int ans = n + 1;
        int[] last = new int[3];
        Arrays.fill(last, -(n + 1));

        for (int i = 0; i < n; ++i) {
            int x = nums[i];
            if (x != 0) {
                ans = Math.min(ans, i - last[3 - x]);
                last[x] = i;
            }
        }
        return ans > n ? -1 : ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minAbsoluteDifference(vector<int>& nums) {
        int n = nums.size();
        int ans = n + 1;
        vector<int> last(3, -(n + 1));

        for (int i = 0; i < n; ++i) {
            int x = nums[i];
            if (x != 0) {
                ans = min(ans, i - last[3 - x]);
                last[x] = i;
            }
        }
        return ans > n ? -1 : ans;
    }
};
```

#### Go

```go
func minAbsoluteDifference(nums []int) int {
	n := len(nums)
	ans := n + 1

	last := []int{-ans, -ans, -ans}

	for i, x := range nums {
		if x != 0 {
			ans = min(ans, i-last[3-x])
			last[x] = i
		}
	}

	if ans > n {
		return -1
	}
	return ans
}
```

#### TypeScript

```ts
function minAbsoluteDifference(nums: number[]): number {
    const n = nums.length;
    let ans = n + 1;
    const last = Array(3).fill(-ans);

    for (let i = 0; i < n; ++i) {
        const x = nums[i];
        if (x) {
            ans = Math.min(ans, i - last[3 - x]);
            last[x] = i;
        }
    }

    return ans > n ? -1 : ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
