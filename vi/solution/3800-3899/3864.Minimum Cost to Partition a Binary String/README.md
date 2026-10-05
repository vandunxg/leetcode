---
comments: true
difficulty: Hard
rating: 2032
source: Weekly Contest 492 Q4
tags:
    - String
    - Divide and Conquer
    - Prefix Sum
---

<!-- problem:start -->

# [3864. Minimum Cost to Partition a Binary String](https://leetcode.com/problems/minimum-cost-to-partition-a-binary-string)

[中文文档](/solution/3800-3899/3864.Minimum%20Cost%20to%20Partition%20a%20Binary%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi nhị phân <code>s</code> và hai số nguyên <code>encCost</code> và <code>flatCost</code>.</p>

<p>Với mỗi chỉ số <code>i</code>, <code>s[i] = &#39;1&#39;</code> cho biết phần tử thứ <code>i<sup>th</sup></code> nhạy cảm, còn <code>s[i] = &#39;0&#39;</code> cho biết phần tử đó không nhạy cảm.</p>

<p>Chuỗi phải được phân hoạch thành các <strong>đoạn</strong>. Ban đầu, toàn bộ chuỗi tạo thành một đoạn duy nhất.</p>

<p>Với một đoạn có độ dài <code>L</code> và chứa <code>X</code> phần tử nhạy cảm:</p>

<ul>
	<li>Nếu <code>X = 0</code>, chi phí là <code>flatCost</code>.</li>
	<li>Nếu <code>X &gt; 0</code>, chi phí là <code>L * X * encCost</code>.</li>
</ul>

<p>Nếu một đoạn có <strong>độ dài chẵn</strong>, bạn có thể tách đoạn đó thành <strong>hai đoạn liên tiếp</strong> có <strong>độ dài bằng nhau</strong>, với chi phí tách bằng <strong>tổng</strong> <strong>chi phí</strong> của hai đoạn kết quả.</p>

<p>Trả về một số nguyên biểu thị <strong>tổng chi phí nhỏ nhất</strong> có thể đạt được qua tất cả các phân hoạch hợp lệ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;1010&quot;, encCost = 2, flatCost = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Toàn bộ chuỗi <code>s = &quot;1010&quot;</code> có độ dài 4 và chứa 2 phần tử nhạy cảm, nên chi phí là <code>4 * 2 * 2 = 16</code>.</li>
	<li>Vì độ dài là số chẵn, chuỗi có thể được tách thành <code>&quot;10&quot;</code> và <code>&quot;10&quot;</code>. Mỗi đoạn có độ dài 2 và chứa 1 phần tử nhạy cảm, nên chi phí của mỗi đoạn là <code>2 * 1 * 2 = 4</code>, tổng cộng là 8.</li>
	<li>Tách cả hai đoạn thành bốn đoạn gồm một ký tự sẽ tạo ra các đoạn <code>&quot;1&quot;</code>, <code>&quot;0&quot;</code>, <code>&quot;1&quot;</code> và <code>&quot;0&quot;</code>. Đoạn chứa <code>&quot;1&quot;</code> có độ dài 1 và chứa đúng một phần tử nhạy cảm, nên chi phí là <code>1 * 1 * 2 = 2</code>, còn đoạn chứa <code>&quot;0&quot;</code> không có phần tử nhạy cảm nên chi phí là <code>flatCost = 1</code>.</li>
	<li>​​​​​​​Vậy tổng chi phí là <code>2 + 1 + 2 + 1 = 6</code>, đây là tổng chi phí nhỏ nhất có thể.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;1010&quot;, encCost = 3, flatCost = 10</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Toàn bộ chuỗi <code>s = &quot;1010&quot;</code> có độ dài 4 và chứa 2 phần tử nhạy cảm, nên chi phí là <code>4 * 2 * 3 = 24</code>.</li>
	<li>Vì độ dài là số chẵn, chuỗi có thể được tách thành hai đoạn <code>&quot;10&quot;</code> và <code>&quot;10&quot;</code>.</li>
	<li>Mỗi đoạn có độ dài 2 và chứa một phần tử nhạy cảm, nên chi phí của mỗi đoạn là <code>2 * 1 * 3 = 6</code>, tổng cộng là 12, đây là tổng chi phí nhỏ nhất có thể.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;00&quot;, encCost = 1, flatCost = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi <code>s = &quot;00&quot;</code> có độ dài 2 và không chứa phần tử nhạy cảm nào, nên lưu dưới dạng một đoạn duy nhất có chi phí <code>flatCost = 2</code>, đây là tổng chi phí nhỏ nhất có thể.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm <code>&#39;0&#39;</code> và <code>&#39;1&#39;</code>.</li>
	<li><code>1 &lt;= encCost, flatCost &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Một đoạn có độ dài chẵn có thể được tách đôi với chi phí bằng tổng chi phí của hai nửa; nếu không tách, ta tính chi phí theo số phần tử nhạy cảm. $n \le 10^5$, nhưng cây tách luôn chia đôi nên chỉ có số lượng đoạn tuyến tính.
>
> Chi phí khi không tách một đoạn chỉ phụ thuộc vào độ dài và số phần tử nhạy cảm, có thể lấy từ prefix sum trong $O(1)$.
>
> Thực hiện đệ quy bằng cách so sánh chi phí không tách với tổng chi phí của hai nửa. Việc chia đôi xác định duy nhất mỗi đoạn và các đoạn không chồng lấn, nên có $O(n)$ trạng thái.
>
> Đáp án là giá trị của $[0,n)$.

<!-- thinking:end -->

Ta định nghĩa hàm $\text{dfs}(l, r)$ biểu thị chi phí nhỏ nhất của khoảng $[l, r)$ trong chuỗi $s$. Ta có thể dùng mảng prefix sum $\text{pre}$ để tính số phần tử nhạy cảm $x$ trong khoảng $[l, r)$, từ đó tính được chi phí khi không tách.

Quy trình tính hàm $\text{dfs}(l, r)$ như sau:

1. Tính số phần tử nhạy cảm $x$ trong khoảng $[l, r)$.
2. Tính chi phí khi không tách: nếu $x > 0$, chi phí là $(r - l) \cdot x \cdot \text{encCost}$; nếu $x = 0$, chi phí là $\text{flatCost}$.
3. Nếu độ dài khoảng là số chẵn, ta có thể thử tách khoảng đó thành hai đoạn liên tiếp có độ dài bằng nhau, rồi tính chi phí sau khi tách là $\text{dfs}(l, m) + \text{dfs}(m, r)$, trong đó $m = \frac{l + r}{2}$. Cuối cùng, trả về giá trị nhỏ hơn trong hai chi phí.

Đáp án là $\text{dfs}(0, n)$, trong đó $n$ là độ dài của chuỗi $s$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCost(self, s: str, encCost: int, flatCost: int) -> int:
        def dfs(l: int, r: int) -> int:
            x = pre[r] - pre[l]
            res = (r - l) * x * encCost if x else flatCost
            if (r - l) % 2 == 0:
                m = (l + r) // 2
                res = min(res, dfs(l, m) + dfs(m, r))
            return res

        n = len(s)
        pre = [0] * (n + 1)
        for i, c in enumerate(s, 1):
            pre[i] = pre[i - 1] + int(c)
        return dfs(0, n)
```

#### Java

```java
class Solution {
    private int[] pre;
    private int encCost;
    private int flatCost;

    public long minCost(String s, int encCost, int flatCost) {
        int n = s.length();
        this.encCost = encCost;
        this.flatCost = flatCost;

        pre = new int[n + 1];
        for (int i = 1; i <= n; ++i) {
            pre[i] = pre[i - 1] + (s.charAt(i - 1) - '0');
        }

        return dfs(0, n);
    }

    private long dfs(int l, int r) {
        int x = pre[r] - pre[l];
        long res = x != 0 ? (long) (r - l) * x * encCost : flatCost;

        if ((r - l) % 2 == 0) {
            int m = (l + r) >> 1;
            res = Math.min(res, dfs(l, m) + dfs(m, r));
        }

        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minCost(string s, int encCost, int flatCost) {
        int n = s.size();
        vector<int> pre(n + 1);

        for (int i = 1; i <= n; ++i) {
            pre[i] = pre[i - 1] + (s[i - 1] - '0');
        }

        auto dfs = [&](this auto&& dfs, int l, int r) -> long long {
            int x = pre[r] - pre[l];
            long long res = x ? 1LL * (r - l) * x * encCost : flatCost;

            if ((r - l) % 2 == 0) {
                int m = (l + r) >> 1;
                res = min(res, dfs(l, m) + dfs(m, r));
            }

            return res;
        };

        return dfs(0, n);
    }
};
```

#### Go

```go
func minCost(s string, encCost int, flatCost int) int64 {
	n := len(s)
	pre := make([]int, n+1)

	for i := 1; i <= n; i++ {
		pre[i] = pre[i-1] + int(s[i-1]-'0')
	}

	var dfs func(int, int) int64
	dfs = func(l, r int) int64 {
		x := pre[r] - pre[l]

		var res int64
		if x != 0 {
			res = int64(r-l) * int64(x) * int64(encCost)
		} else {
			res = int64(flatCost)
		}

		if (r-l)%2 == 0 {
			m := (l + r) / 2
			res = min(res, dfs(l, m)+dfs(m, r))
		}

		return res
	}

	return dfs(0, n)
}
```

#### TypeScript

```ts
function minCost(s: string, encCost: number, flatCost: number): number {
    const n = s.length;
    const pre: number[] = new Array(n + 1).fill(0);

    for (let i = 1; i <= n; i++) {
        pre[i] = pre[i - 1] + Number(s[i - 1]);
    }

    const dfs = (l: number, r: number): number => {
        const x = pre[r] - pre[l];
        let res = x ? (r - l) * x * encCost : flatCost;

        if ((r - l) % 2 === 0) {
            const m = (l + r) >> 1;
            res = Math.min(res, dfs(l, m) + dfs(m, r));
        }

        return res;
    };

    return dfs(0, n);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
