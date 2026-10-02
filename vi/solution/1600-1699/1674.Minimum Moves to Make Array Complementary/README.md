---
comments: true
difficulty: Medium
rating: 2333
source: Weekly Contest 217 Q3
tags:
    - Array
    - Hash Table
    - Prefix Sum
---

<!-- problem:start -->

# [1674. Minimum Moves to Make Array Complementary](https://leetcode.com/problems/minimum-moves-to-make-array-complementary)

[中文文档](/solution/1600-1699/1674.Minimum%20Moves%20to%20Make%20Array%20Complementary/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> có độ dài <strong>chẵn</strong> <code>n</code> và số nguyên <code>limit</code>. Trong một thao tác, bạn có thể thay bất kỳ số nguyên nào trong <code>nums</code> bằng một số nguyên khác trong đoạn từ <code>1</code> đến <code>limit</code>, bao gồm cả hai đầu mút.</p>

<p>Mảng <code>nums</code> là <strong>complementary</strong> nếu với mọi chỉ số <code>i</code> (<strong>đánh chỉ số từ 0</strong>), <code>nums[i] + nums[n - 1 - i]</code> đều bằng cùng một số. Ví dụ, mảng <code>[1,2,3,4]</code> là complementary vì với mọi chỉ số <code>i</code>, <code>nums[i] + nums[n - 1 - i] = 5</code>.</p>

<p>Hãy trả về <em><strong>số thao tác ít nhất</strong> cần thực hiện để </em><code>nums</code><em> trở thành <strong>complementary</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,2,4,3], limit = 4
<strong>Output:</strong> 1
<strong>Giải thích:</strong> Với 1 thao tác, có thể đổi nums thành [1,2,<u>2</u>,3] (các phần tử gạch chân là phần tử bị đổi).
nums[0] + nums[3] = 1 + 3 = 4.
nums[1] + nums[2] = 2 + 2 = 4.
nums[2] + nums[1] = 2 + 2 = 4.
nums[3] + nums[0] = 3 + 1 = 4.
Do đó nums[i] + nums[n-1-i] = 4 với mọi i, nên nums là complementary.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,2,2,1], limit = 2
<strong>Output:</strong> 2
<strong>Giải thích:</strong> Với 2 thao tác, có thể đổi nums thành [<u>2</u>,2,2,<u>2</u>]. Không thể đổi số nào thành 3 vì 3 &gt; limit.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,2,1,2], limit = 2
<strong>Output:</strong> 0
<strong>Giải thích:</strong> nums đã là complementary.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>2 &lt;= n&nbsp;&lt;=&nbsp;10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i]&nbsp;&lt;= limit &lt;=&nbsp;10<sup>5</sup></code></li>
	<li><code>n</code> is even.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng hiệu

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi cặp $\textit{nums}[i]+\textit{nums}[n-1-i]$ phải trở thành cùng một tổng $s$, và mỗi thao tác đổi một giá trị thành một số trong $[1,\textit{limit}]$. Cả $n$ và $\textit{limit}$ đều có thể bằng $10^5$, nên không thể duyệt lại mọi cặp với từng $s$.
>
> Một cặp $(x,y)$ ($x\le y$) cần lần lượt $2,1,0,1,2$ thao tác trên các đoạn liên tiếp của $s$. Mảng hiệu cho phép cộng các cập nhật đoạn đó trên $[2,2\cdot\textit{limit}]$.
>
> Tổng tiền tố nhỏ nhất chính là số thao tác ít nhất.

<!-- thinking:end -->

Giả sử trong mảng cuối cùng, tổng của cặp $\textit{nums}[i]$ và $\textit{nums}[n-i-1]$ là $s$.

Gọi $x$ là giá trị nhỏ hơn giữa $\textit{nums}[i]$ và $\textit{nums}[n-i-1]$, còn $y$ là giá trị lớn hơn.

Với mỗi cặp số, ta có các trường hợp sau:

- Nếu không cần thay thế thì $x + y = s$.
- Nếu thay thế một lần thì $x + 1 \le s \le y + \textit{limit}$.
- Nếu thay thế hai lần thì $2 \le s \le x$ hoặc $y + \textit{limit} + 1 \le s \le 2 \times \textit{limit}$.

Cụ thể:

- Trong đoạn $[2,..x]$, cần $2$ lần thay thế.
- Trong đoạn $[x+1,..x+y-1]$, cần $1$ lần thay thế.
- Tại $[x+y]$, không cần thay thế.
- Trong đoạn $[x+y+1,..y + \textit{limit}]$, cần $1$ lần thay thế.
- Trong đoạn $[y + \textit{limit} + 1,..2 \times \textit{limit}]$, cần $2$ lần thay thế.

Ta duyệt từng cặp số và dùng mảng hiệu để cập nhật số lần thay thế cần thiết trên các đoạn khác nhau.

Cuối cùng, ta tìm giá trị nhỏ nhất trong các tổng tiền tố từ chỉ số $2$ đến $2 \times \textit{limit}$; đó là số lần thay thế ít nhất.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài mảng $\textit{nums}$.

Các bài tương tự:

- [3224. Minimum Array Changes to Make Differences Equal](https://github.com/doocs/leetcode/blob/main/solution/3200-3299/3224.Minimum%20Array%20Changes%20to%20Make%20Differences%20Equal/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minMoves(self, nums: List[int], limit: int) -> int:
        d = [0] * (2 * limit + 2)
        n = len(nums)
        for i in range(n // 2):
            x, y = nums[i], nums[-i - 1]
            if x > y:
                x, y = y, x
            d[2] += 2
            d[x + 1] -= 2
            d[x + 1] += 1
            d[x + y] -= 1
            d[x + y + 1] += 1
            d[y + limit + 1] -= 1
            d[y + limit + 1] += 2
        return min(accumulate(d[2:]))
```

#### Java

```java
class Solution {
    public int minMoves(int[] nums, int limit) {
        int[] d = new int[2 * limit + 2];
        int n = nums.length;
        for (int i = 0; i < n / 2; ++i) {
            int x = Math.min(nums[i], nums[n - i - 1]);
            int y = Math.max(nums[i], nums[n - i - 1]);
            d[2] += 2;
            d[x + 1] -= 2;
            d[x + 1] += 1;
            d[x + y] -= 1;
            d[x + y + 1] += 1;
            d[y + limit + 1] -= 1;
            d[y + limit + 1] += 2;
        }
        int ans = n;
        for (int i = 2, s = 0; i < d.length; ++i) {
            s += d[i];
            ans = Math.min(ans, s);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minMoves(vector<int>& nums, int limit) {
        int n = nums.size();
        int d[limit * 2 + 2];
        memset(d, 0, sizeof(d));
        for (int i = 0; i < n / 2; ++i) {
            int x = nums[i], y = nums[n - i - 1];
            if (x > y) {
                swap(x, y);
            }
            d[2] += 2;
            d[x + 1] -= 2;
            d[x + 1] += 1;
            d[x + y] -= 1;
            d[x + y + 1] += 1;
            d[y + limit + 1] -= 1;
            d[y + limit + 1] += 2;
        }
        int ans = n;
        for (int i = 2, s = 0; i <= limit * 2; ++i) {
            s += d[i];
            ans = min(ans, s);
        }
        return ans;
    }
};
```

#### Go

```go
func minMoves(nums []int, limit int) int {
	n := len(nums)
	d := make([]int, 2*limit+2)
	for i := 0; i < n/2; i++ {
		x, y := nums[i], nums[n-1-i]
		if x > y {
			x, y = y, x
		}
		d[2] += 2
		d[x+1] -= 2
		d[x+1] += 1
		d[x+y] -= 1
		d[x+y+1] += 1
		d[y+limit+1] -= 1
		d[y+limit+1] += 2
	}
	ans, s := n, 0
	for _, x := range d[2:] {
		s += x
		ans = min(ans, s)
	}
	return ans
}
```

#### TypeScript

```ts
function minMoves(nums: number[], limit: number): number {
    const n = nums.length;
    const d: number[] = Array(limit * 2 + 2).fill(0);
    for (let i = 0; i < n >> 1; ++i) {
        const x = Math.min(nums[i], nums[n - 1 - i]);
        const y = Math.max(nums[i], nums[n - 1 - i]);
        d[2] += 2;
        d[x + 1] -= 2;
        d[x + 1] += 1;
        d[x + y] -= 1;
        d[x + y + 1] += 1;
        d[y + limit + 1] -= 1;
        d[y + limit + 1] += 2;
    }
    let ans = n;
    let s = 0;
    for (let i = 2; i < d.length; ++i) {
        s += d[i];
        ans = Math.min(ans, s);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
