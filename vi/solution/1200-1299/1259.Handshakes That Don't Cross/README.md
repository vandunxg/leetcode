---
comments: true
difficulty: Hard
rating: 1951
source: Biweekly Contest 13 Q4
tags:
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [1259. Handshakes That Don't Cross 🔒](https://leetcode.com/problems/handshakes-that-dont-cross)

[中文文档](/solution/1200-1299/1259.Handshakes%20That%20Don%27t%20Cross/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>numPeople</code> người, là một số <strong>chẵn</strong>, đứng thành vòng tròn. Mỗi người bắt tay với một người khác, tạo thành tổng cộng <code>numPeople / 2</code> cái bắt tay.</p>

<p>Trả về <em>số cách bắt tay sao cho không có hai cái bắt tay nào giao nhau</em>.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về kết quả theo <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1259.Handshakes%20That%20Don%27t%20Cross/images/5125_example_2.png" style="width: 450px; height: 215px;" />
<pre>
<strong>Đầu vào:</strong> numPeople = 4
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có hai cách thực hiện: cách thứ nhất là [(1,2),(3,4)] và cách thứ hai là [(2,3),(4,1)].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1259.Handshakes%20That%20Don%27t%20Cross/images/5125_example_3.png" style="width: 335px; height: 500px;" />
<pre>
<strong>Đầu vào:</strong> numPeople = 6
<strong>Đầu ra:</strong> 5
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= numPeople &lt;= 1000</code></li>
	<li><code>numPeople</code> là số chẵn.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Khi một số chẵn người bắt tay quanh vòng tròn mà không có hai cái bắt tay nào giao nhau, số cách có dạng số Catalan. Vì $n \le 1000$, không thể liệt kê mọi cách ghép cặp. Cố định một người: người họ bắt tay chia vòng tròn thành hai phần nhỏ hơn có số người chẵn; với mỗi lựa chọn hợp lệ, ta nhân số cách của hai phần.
>
> $dfs(i)$ thử từng kích thước chẵn $l$ cho phần bên trái; phần bên phải có kích thước $i-l-2$. Memoization giúp dùng lại kết quả của các vòng tròn con. Ta lấy modulo $10^9+7$.

<!-- thinking:end -->

Ta định nghĩa hàm $dfs(i)$ là số cách bắt tay của $i$ người. Đáp án là $dfs(n)$.

Hàm $dfs(i)$ hoạt động như sau:

- Nếu $i \lt 2$, chỉ có một cách bắt tay là không bắt tay với ai, nên trả về $1$.
- Nếu không, ta lần lượt xét người đầu tiên bắt tay với từng người có thể. Gọi số người còn lại ở bên trái là $l$, và số người ở bên phải là $r=i-l-2$. Khi đó, $dfs(i)= \sum_{l=0}^{i-1} dfs(l) \times dfs(r)$.

Để tránh tính toán lặp lại, ta dùng tìm kiếm có ghi nhớ.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là giá trị của $numPeople$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfWays(self, numPeople: int) -> int:
        @cache
        def dfs(i: int) -> int:
            if i < 2:
                return 1
            ans = 0
            for l in range(0, i, 2):
                r = i - l - 2
                ans += dfs(l) * dfs(r)
                ans %= mod
            return ans

        mod = 10**9 + 7
        return dfs(numPeople)
```

#### Java

```java
class Solution {
    private int[] f;
    private final int mod = (int) 1e9 + 7;

    public int numberOfWays(int numPeople) {
        f = new int[numPeople + 1];
        return dfs(numPeople);
    }

    private int dfs(int i) {
        if (i < 2) {
            return 1;
        }
        if (f[i] != 0) {
            return f[i];
        }
        for (int l = 0; l < i; l += 2) {
            int r = i - l - 2;
            f[i] = (int) ((f[i] + (1L * dfs(l) * dfs(r) % mod)) % mod);
        }
        return f[i];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfWays(int numPeople) {
        const int mod = 1e9 + 7;
        int f[numPeople + 1];
        memset(f, 0, sizeof(f));
        function<int(int)> dfs = [&](int i) {
            if (i < 2) {
                return 1;
            }
            if (f[i]) {
                return f[i];
            }
            for (int l = 0; l < i; l += 2) {
                int r = i - l - 2;
                f[i] = (f[i] + 1LL * dfs(l) * dfs(r) % mod) % mod;
            }
            return f[i];
        };
        return dfs(numPeople);
    }
};
```

#### Go

```go
func numberOfWays(numPeople int) int {
	const mod int = 1e9 + 7
	f := make([]int, numPeople+1)
	var dfs func(int) int
	dfs = func(i int) int {
		if i < 2 {
			return 1
		}
		if f[i] != 0 {
			return f[i]
		}
		for l := 0; l < i; l += 2 {
			r := i - l - 2
			f[i] = (f[i] + dfs(l)*dfs(r)) % mod
		}
		return f[i]
	}
	return dfs(numPeople)
}
```

#### TypeScript

```ts
function numberOfWays(numPeople: number): number {
    const mod = 10 ** 9 + 7;
    const f: number[] = Array(numPeople + 1).fill(0);
    const dfs = (i: number): number => {
        if (i < 2) {
            return 1;
        }
        if (f[i] !== 0) {
            return f[i];
        }
        for (let l = 0; l < i; l += 2) {
            const r = i - l - 2;
            f[i] += Number((BigInt(dfs(l)) * BigInt(dfs(r))) % BigInt(mod));
            f[i] %= mod;
        }
        return f[i];
    };
    return dfs(numPeople);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
