---
comments: true
difficulty: Medium
rating: 1732
source: Biweekly Contest 109 Q3
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [2786. Visit Array Positions to Maximize Score](https://leetcode.com/problems/visit-array-positions-to-maximize-score)

[中文文档](/solution/2700-2799/2786.Visit%20Array%20Positions%20to%20Maximize%20Score/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>0-indexed</strong> <code>nums</code> và một số nguyên dương <code>x</code>.</p>

<p><strong>Ban đầu</strong>, bạn ở vị trí <code>0</code> trong mảng và có thể đi qua các vị trí khác theo những quy tắc sau:</p>

<ul>
	<li>Nếu hiện tại bạn đang ở vị trí <code>i</code>, bạn có thể di chuyển đến <strong>bất kỳ</strong> vị trí <code>j</code> nào thỏa mãn <code>i &lt; j</code>.</li>
	<li>Với mỗi vị trí <code>i</code> bạn đi qua, bạn nhận được số điểm bằng <code>nums[i]</code>.</li>
	<li>Nếu bạn di chuyển từ vị trí <code>i</code> đến vị trí <code>j</code> và <strong>tính chẵn lẻ</strong> của <code>nums[i]</code> và <code>nums[j]</code> khác nhau, bạn bị trừ <code>x</code> điểm.</li>
</ul>

<p>Hãy trả về <em><strong>tổng điểm lớn nhất</strong> bạn có thể nhận được</em>.</p>

<p><strong>Lưu ý</strong> rằng ban đầu bạn có <code>nums[0]</code> điểm.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,3,6,1,9,2], x = 5
<strong>Đầu ra:</strong> 13
<strong>Giải thích:</strong> Ta có thể đi qua các vị trí sau trong mảng: 0 -&gt; 2 -&gt; 3 -&gt; 4.
Các giá trị tương ứng là 2, 6, 1 và 9. Vì các số nguyên 6 và 1 có tính chẵn lẻ khác nhau, bước di chuyển 2 -&gt; 3 sẽ khiến ta bị trừ x = 5 điểm.
Tổng điểm là: 2 + 6 + 1 + 9 - 5 = 13.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,4,6,8], x = 3
<strong>Đầu ra:</strong> 20
<strong>Giải thích:</strong> Tất cả các số nguyên trong mảng đều có cùng tính chẵn lẻ, nên ta có thể đi qua tất cả chúng mà không bị trừ điểm.
Tổng điểm là: 2 + 4 + 6 + 8 = 20.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i], x &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Ta đi sang phải từ chỉ số $0$, cộng giá trị và trừ $x$ khi tính chẵn lẻ thay đổi; các vị trí có thể được bỏ qua. Số chuỗi di chuyển là rất lớn.
>
> Điểm số chỉ phụ thuộc vào tính chẵn lẻ trước đó. $f[0]$ và $f[1]$ lần lượt là điểm số tốt nhất khi kết thúc ở giá trị chẵn hoặc lẻ: giữ nguyên tính chẵn lẻ thì không mất thêm điểm, còn đổi tính chẵn lẻ thì mất $x$ điểm, sau đó cộng giá trị hiện tại. Đáp án là giá trị lớn hơn trong hai trạng thái.

<!-- thinking:end -->

Dựa trên mô tả bài toán, ta có thể rút ra những kết luận sau:

1. Khi di chuyển từ vị trí $i$ đến vị trí $j$, nếu $nums[i]$ và $nums[j]$ có tính chẵn lẻ khác nhau, ta bị trừ $x$ điểm;
2. Khi di chuyển từ vị trí $i$ đến vị trí $j$, nếu $nums[i]$ và $nums[j]$ có cùng tính chẵn lẻ, ta không bị trừ điểm.

Do đó, ta có thể dùng một mảng $f$ có độ dài $2$ để biểu diễn điểm số lớn nhất khi tính chẵn lẻ của vị trí hiện tại lần lượt là $0$ và $1$. Ban đầu, các giá trị của $f$ là $-\infty$, sau đó ta khởi tạo $f[nums[0] \& 1] = nums[0]$, biểu diễn điểm số tại vị trí ban đầu.

Tiếp theo, ta bắt đầu duyệt mảng $nums$ từ vị trí $1$. Với mỗi vị trí $i$ tương ứng với giá trị $v$, ta cập nhật $f[v \& 1]$ thành giá trị lớn hơn giữa $f[v \& 1]$ và $f[v \& 1 \oplus 1] - x$ rồi cộng thêm $v$, tức là $f[v \& 1] = \max(f[v \& 1], f[v \& 1 \oplus 1] - x) + v$.

Đáp án là giá trị lớn hơn giữa $f[0]$ và $f[1]$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxScore(self, nums: List[int], x: int) -> int:
        f = [-inf] * 2
        f[nums[0] & 1] = nums[0]
        for v in nums[1:]:
            f[v & 1] = max(f[v & 1], f[v & 1 ^ 1] - x) + v
        return max(f)
```

#### Java

```java
class Solution {
    public long maxScore(int[] nums, int x) {
        long[] f = new long[2];
        Arrays.fill(f, -(1L << 60));
        f[nums[0] & 1] = nums[0];
        for (int i = 1; i < nums.length; ++i) {
            int v = nums[i];
            f[v & 1] = Math.max(f[v & 1], f[v & 1 ^ 1] - x) + v;
        }
        return Math.max(f[0], f[1]);
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxScore(vector<int>& nums, int x) {
        const long long inf = 1LL << 60;
        vector<long long> f(2, -inf);
        f[nums[0] & 1] = nums[0];
        int n = nums.size();
        for (int i = 1; i < n; ++i) {
            int v = nums[i];
            f[v & 1] = max(f[v & 1], f[v & 1 ^ 1] - x) + v;
        }
        return max(f[0], f[1]);
    }
};
```

#### Go

```go
func maxScore(nums []int, x int) int64 {
	const inf int = 1 << 40
	f := [2]int{-inf, -inf}
	f[nums[0]&1] = nums[0]
	for _, v := range nums[1:] {
		f[v&1] = max(f[v&1], f[v&1^1]-x) + v
	}
	return int64(max(f[0], f[1]))
}
```

#### TypeScript

```ts
function maxScore(nums: number[], x: number): number {
    const f: number[] = Array(2).fill(-Infinity);
    f[nums[0] & 1] = nums[0];
    for (let i = 1; i < nums.length; ++i) {
        const v = nums[i];
        f[v & 1] = Math.max(f[v & 1], f[(v & 1) ^ 1] - x) + v;
    }
    return Math.max(...f);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
