---
comments: true
difficulty: Medium
rating: 2034
source: Weekly Contest 147 Q4
tags:
    - Minimax
    - Array
    - Math
    - Dynamic Programming
    - Game Theory
    - Prefix Sum
    - Zero-Sum Game
---

<!-- problem:start -->

# [1140. Stone Game II](https://leetcode.com/problems/stone-game-ii)

[中文文档](/solution/1100-1199/1140.Stone%20Game%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Alice và Bob tiếp tục chơi trò chơi với các đống đá. Có một số đống đá <strong>xếp thành một hàng</strong>, mỗi đống có số viên đá nguyên dương <code>piles[i]</code>. Mục tiêu của trò chơi là kết thúc với nhiều viên đá nhất.</p>

<p>Alice và Bob lần lượt đi, Alice đi trước.</p>

<p>Trong lượt của mình, mỗi người có thể lấy <strong>toàn bộ số đá</strong> trong <strong>X</strong> đống đầu tiên còn lại, với <code>1 &lt;= X &lt;= 2M</code>. Sau đó, cập nhật <code>M = max(M, X)</code>. Ban đầu, M = 1.</p>

<p>Trò chơi tiếp tục cho đến khi tất cả đá được lấy hết.</p>

<p>Giả sử Alice và Bob đều chơi tối ưu, hãy trả về số viên đá lớn nhất Alice có thể lấy được.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">piles = [2,7,9,4,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">10</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Nếu Alice lấy một đống lúc đầu, Bob lấy hai đống, rồi Alice lại lấy 2 đống. Tổng cộng Alice lấy được <code>2 + 4 + 4 = 10</code> viên đá.</li>
	<li>Nếu Alice lấy hai đống lúc đầu, Bob có thể lấy cả ba đống còn lại. Khi đó, Alice lấy được tổng cộng <code>2 + 7 = 9</code> viên đá.</li>
</ul>

<p>Vì 10 lớn hơn nên ta trả về 10.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">piles = [1,2,3,4,5,100]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">104</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= piles.length &lt;= 100</code></li>
	<li><code>1 &lt;= piles[i]&nbsp;&lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum + tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Người chơi lấy từ $1..2M$ đống trong phần còn lại; trạng thái gồm chỉ số bắt đầu và $M$. Tìm kiếm ngây thơ sẽ tính lặp lại các bài toán con. Prefix sum cho biết tổng hậu tố từ $i$; số đá người chơi hiện tại lấy được bằng tổng đó trừ đi số đá tối ưu mà đối thủ lấy được.
>
> Nếu $2M$ có thể lấy hết phần còn lại thì lấy tất cả; nếu không, thử từng giá trị $X$ bằng memoization. Alice bắt đầu ở trạng thái $(0,1)$.

<!-- thinking:end -->

Vì mỗi lượt người chơi có thể lấy toàn bộ đá từ $X$ đống đầu tiên, tức là lấy các đống trong một đoạn, trước tiên ta tính trước mảng prefix sum $s$ có độ dài $n+1$, trong đó $s[i]$ là tổng của $i$ phần tử đầu tiên trong mảng `piles`.

Sau đó, ta thiết kế hàm $dfs(i, m)$, biểu diễn số viên đá lớn nhất người chơi hiện tại có thể lấy khi họ bắt đầu từ chỉ số $i$ trong mảng `piles` và giá trị $M$ hiện tại là $m$. Ban đầu, Alice bắt đầu ở chỉ số $0$ và $M=1$, nên đáp án cần tìm là $dfs(0, 1)$.

Quá trình tính toán của hàm $dfs(i, m)$ như sau:

- Nếu người chơi hiện tại có thể lấy hết số đá còn lại, số đá lớn nhất họ có thể lấy là $s[n] - s[i]$;
- Nếu không, người chơi hiện tại có thể lấy toàn bộ đá trong $x$ đống đầu tiên của phần còn lại, với $1 \leq x \leq 2m$. Số đá lớn nhất họ có thể lấy là $s[n] - s[i] - dfs(i + x, max(m, x))$. Nói cách khác, số đá người chơi hiện tại lấy được bằng tổng số đá còn lại trừ đi số đá đối thủ có thể lấy trong lượt tiếp theo. Ta cần duyệt mọi giá trị $x$ và lấy giá trị lớn nhất làm kết quả trả về của hàm $dfs(i, m)$.

Để tránh tính toán lặp, ta dùng tìm kiếm có ghi nhớ.

Cuối cùng, trả về $dfs(0, 1)$ làm đáp án.

Độ phức tạp thời gian là $O(n^3)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là độ dài của mảng `piles`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def stoneGameII(self, piles: List[int]) -> int:
        @cache
        def dfs(i, m):
            if m * 2 >= n - i:
                return s[n] - s[i]
            return max(
                s[n] - s[i] - dfs(i + x, max(m, x)) for x in range(1, m << 1 | 1)
            )

        n = len(piles)
        s = list(accumulate(piles, initial=0))
        return dfs(0, 1)
```

#### Java

```java
class Solution {
    private int[] s;
    private Integer[][] f;
    private int n;

    public int stoneGameII(int[] piles) {
        n = piles.length;
        s = new int[n + 1];
        f = new Integer[n][n + 1];
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + piles[i];
        }
        return dfs(0, 1);
    }

    private int dfs(int i, int m) {
        if (m * 2 >= n - i) {
            return s[n] - s[i];
        }
        if (f[i][m] != null) {
            return f[i][m];
        }
        int res = 0;
        for (int x = 1; x <= m * 2; ++x) {
            res = Math.max(res, s[n] - s[i] - dfs(i + x, Math.max(m, x)));
        }
        return f[i][m] = res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int stoneGameII(vector<int>& piles) {
        int n = piles.size();
        int s[n + 1];
        s[0] = 0;
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + piles[i];
        }
        int f[n][n + 1];
        memset(f, 0, sizeof f);
        function<int(int, int)> dfs = [&](int i, int m) -> int {
            if (m * 2 >= n - i) {
                return s[n] - s[i];
            }
            if (f[i][m]) {
                return f[i][m];
            }
            int res = 0;
            for (int x = 1; x <= m << 1; ++x) {
                res = max(res, s[n] - s[i] - dfs(i + x, max(x, m)));
            }
            return f[i][m] = res;
        };
        return dfs(0, 1);
    }
};
```

#### Go

```go
func stoneGameII(piles []int) int {
	n := len(piles)
	s := make([]int, n+1)
	f := make([][]int, n+1)
	for i, x := range piles {
		s[i+1] = s[i] + x
		f[i] = make([]int, n+1)
	}
	var dfs func(i, m int) int
	dfs = func(i, m int) int {
		if m*2 >= n-i {
			return s[n] - s[i]
		}
		if f[i][m] > 0 {
			return f[i][m]
		}
		f[i][m] = 0
		for x := 1; x <= m<<1; x++ {
			f[i][m] = max(f[i][m], s[n]-s[i]-dfs(i+x, max(m, x)))
		}
		return f[i][m]
	}
	return dfs(0, 1)
}
```

#### TypeScript

```ts
function stoneGameII(piles: number[]): number {
    const n = piles.length;
    const f = Array.from({ length: n }, _ => new Array(n + 1).fill(0));
    const s = new Array(n + 1).fill(0);
    for (let i = 0; i < n; ++i) {
        s[i + 1] = s[i] + piles[i];
    }
    const dfs = (i: number, m: number) => {
        if (m * 2 >= n - i) {
            return s[n] - s[i];
        }
        if (f[i][m]) {
            return f[i][m];
        }
        let res = 0;
        for (let x = 1; x <= m * 2; ++x) {
            res = Math.max(res, s[n] - s[i] - dfs(i + x, Math.max(m, x)));
        }
        return (f[i][m] = res);
    };
    return dfs(0, 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 trả về tổng hậu tố khi có thể lấy hết. Lời giải 2 luôn duyệt mọi giá trị $X$ và biểu diễn cùng giá trị tối ưu dưới dạng tổng hậu tố trừ đi số đá tối thiểu đối thủ có thể lấy, với trường hợp cơ sở ngoài phạm vi là $0$. Trạng thái không đổi; cách viết này đồng nhất hơn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def stoneGameII(self, piles: List[int]) -> int:
        @cache
        def dfs(i: int, m: int = 1) -> int:
            if i >= len(piles):
                return 0
            t = inf
            for x in range(1, m << 1 | 1):
                t = min(t, dfs(i + x, max(m, x)))
            return s[-1] - s[i] - t

        s = list(accumulate(piles, initial=0))
        return dfs(0)
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
