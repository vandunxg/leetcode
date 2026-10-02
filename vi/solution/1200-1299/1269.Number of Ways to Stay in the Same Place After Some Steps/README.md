---
comments: true
difficulty: Hard
rating: 1854
source: Weekly Contest 164 Q4
tags:
    - Dynamic Programming
---

<!-- problem:start -->

# [1269. Number of Ways to Stay in the Same Place After Some Steps](https://leetcode.com/problems/number-of-ways-to-stay-in-the-same-place-after-some-steps)

[中文文档](/solution/1200-1299/1269.Number%20of%20Ways%20to%20Stay%20in%20the%20Same%20Place%20After%20Some%20Steps/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có một con trỏ ở chỉ số <code>0</code> trong mảng có kích thước <code>arrLen</code>. Ở mỗi bước, bạn có thể di chuyển sang trái 1 vị trí, sang phải 1 vị trí hoặc giữ nguyên vị trí (con trỏ không được nằm ngoài mảng ở bất kỳ thời điểm nào).</p>

<p>Cho hai số nguyên <code>steps</code> và <code>arrLen</code>, hãy trả về số cách để con trỏ vẫn ở chỉ số <code>0</code> sau <strong>đúng</strong> <code>steps</code> bước. Vì kết quả có thể rất lớn, hãy trả về kết quả <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> steps = 3, arrLen = 2
<strong>Output:</strong> 4
<strong>Giải thích: </strong>Có 4 cách để quay về chỉ số 0 sau 3 bước.
Right, Left, Stay
Stay, Right, Left
Right, Stay, Left
Stay, Stay, Stay
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> steps = 2, arrLen = 4
<strong>Output:</strong> 2
<strong>Giải thích:</strong> Có 2 cách để quay về chỉ số 0 sau 2 bước.
Right, Left
Stay, Stay
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> steps = 4, arrLen = 2
<strong>Output:</strong> 8
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= steps &lt;= 500</code></li>
	<li><code>1 &lt;= arrLen &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Ta đi $steps$ bước trên mảng dài $arrLen$ rồi quay về vị trí ban đầu. $arrLen$ có thể tới $10^6$, nhưng $steps \le 500$, nên số ô có thể đến được nhiều nhất là $steps$. Số cách phụ thuộc vào vị trí hiện tại và số bước còn lại.
>
> $dfs(i,j)$ là số cách để từ chỉ số $i$, còn $j$ bước, kết thúc tại $0$; mỗi bước ta có thể đi trái, đi phải hoặc đứng yên. Nếu $i>j$ thì không thể quay về. Số state được ghi nhớ là $O(steps^2)$.

<!-- thinking:end -->

Từ giới hạn dữ liệu, ta thấy $steps$ không vượt quá $500$, nên vị trí xa nhất ta có thể đi sang phải cũng chỉ là $500$ bước.

Ta định nghĩa hàm $dfs(i, j)$ là số cách khi đang ở vị trí $i$ và còn lại $j$ bước. Vậy đáp án là $dfs(0, steps)$.

Hàm $dfs(i, j)$ hoạt động như sau:

1. Nếu $i \gt j$ hoặc $i \geq arrLen$ hoặc $i \lt 0$ hoặc $j \lt 0$, trả về $0$.
1. Nếu $i = 0$ và $j = 0$, con trỏ đã dừng ở vị trí ban đầu và không còn bước nào, nên trả về $1$.
1. Trong các trường hợp còn lại, ta có thể đi sang trái một bước, sang phải một bước hoặc đứng yên, nên trả về $dfs(i - 1, j - 1) + dfs(i + 1, j - 1) + dfs(i, j - 1)$. Nhớ lấy modulo cho kết quả.

Trong quá trình này, ta dùng tìm kiếm có ghi nhớ để tránh tính toán lặp lại.

Độ phức tạp thời gian và không gian đều là $O(steps \times steps)$, trong đó $steps$ là số bước được cho trong đề bài.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numWays(self, steps: int, arrLen: int) -> int:
        @cache
        def dfs(i, j):
            if i > j or i >= arrLen or i < 0 or j < 0:
                return 0
            if i == 0 and j == 0:
                return 1
            ans = 0
            for k in range(-1, 2):
                ans += dfs(i + k, j - 1)
                ans %= mod
            return ans

        mod = 10**9 + 7
        return dfs(0, steps)
```

#### Java

```java
class Solution {
    private Integer[][] f;
    private int n;

    public int numWays(int steps, int arrLen) {
        f = new Integer[steps][steps + 1];
        n = arrLen;
        return dfs(0, steps);
    }

    private int dfs(int i, int j) {
        if (i > j || i >= n || i < 0 || j < 0) {
            return 0;
        }
        if (i == 0 && j == 0) {
            return 1;
        }
        if (f[i][j] != null) {
            return f[i][j];
        }
        int ans = 0;
        final int mod = (int) 1e9 + 7;
        for (int k = -1; k <= 1; ++k) {
            ans = (ans + dfs(i + k, j - 1)) % mod;
        }
        return f[i][j] = ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numWays(int steps, int arrLen) {
        int f[steps][steps + 1];
        memset(f, -1, sizeof f);
        const int mod = 1e9 + 7;
        function<int(int, int)> dfs = [&](int i, int j) -> int {
            if (i > j || i >= arrLen || i < 0 || j < 0) {
                return 0;
            }
            if (i == 0 && j == 0) {
                return 1;
            }
            if (f[i][j] != -1) {
                return f[i][j];
            }
            int ans = 0;
            for (int k = -1; k <= 1; ++k) {
                ans = (ans + dfs(i + k, j - 1)) % mod;
            }
            return f[i][j] = ans;
        };
        return dfs(0, steps);
    }
};
```

#### Go

```go
func numWays(steps int, arrLen int) int {
	const mod int = 1e9 + 7
	f := make([][]int, steps)
	for i := range f {
		f[i] = make([]int, steps+1)
		for j := range f[i] {
			f[i][j] = -1
		}
	}
	var dfs func(i, j int) int
	dfs = func(i, j int) (ans int) {
		if i > j || i >= arrLen || i < 0 || j < 0 {
			return 0
		}
		if i == 0 && j == 0 {
			return 1
		}
		if f[i][j] != -1 {
			return f[i][j]
		}
		for k := -1; k <= 1; k++ {
			ans += dfs(i+k, j-1)
			ans %= mod
		}
		f[i][j] = ans
		return
	}
	return dfs(0, steps)
}
```

#### TypeScript

```ts
function numWays(steps: number, arrLen: number): number {
    const f = Array.from({ length: steps }, () => Array(steps + 1).fill(-1));
    const mod = 10 ** 9 + 7;
    const dfs = (i: number, j: number) => {
        if (i > j || i >= arrLen || i < 0 || j < 0) {
            return 0;
        }
        if (i == 0 && j == 0) {
            return 1;
        }
        if (f[i][j] != -1) {
            return f[i][j];
        }
        let ans = 0;
        for (let k = -1; k <= 1; ++k) {
            ans = (ans + dfs(i + k, j - 1)) % mod;
        }
        return (f[i][j] = ans);
    };
    return dfs(0, steps);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
