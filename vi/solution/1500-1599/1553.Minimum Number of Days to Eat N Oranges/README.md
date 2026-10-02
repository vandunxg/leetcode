---
comments: true
difficulty: Hard
rating: 2048
source: Weekly Contest 202 Q4
tags:
    - Memoization
    - Dynamic Programming
---

<!-- problem:start -->

# [1553. Minimum Number of Days to Eat N Oranges](https://leetcode.com/problems/minimum-number-of-days-to-eat-n-oranges)

[中文文档](/solution/1500-1599/1553.Minimum%20Number%20of%20Days%20to%20Eat%20N%20Oranges/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> quả cam trong bếp và bạn quyết định ăn một số quả mỗi ngày như sau:</p>

<ul>
	<li>Ăn một quả cam.</li>
	<li>Nếu số cam còn lại <code>n</code> chia hết cho <code>2</code>, bạn có thể ăn <code>n / 2</code> quả cam.</li>
	<li>Nếu số cam còn lại <code>n</code> chia hết cho <code>3</code>, bạn có thể ăn <code>2 * (n / 3)</code> quả cam.</li>
</ul>

<p>Mỗi ngày bạn chỉ được chọn một trong các hành động trên.</p>

<p>Cho số nguyên <code>n</code>, hãy trả về <em>số ngày nhỏ nhất để ăn hết</em> <code>n</code> <em>quả cam</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 10
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Bạn có 10 quả cam.
Ngày 1: Ăn 1 quả cam,  10 - 1 = 9.  
Ngày 2: Ăn 6 quả cam, 9 - 2*(9/3) = 9 - 6 = 3. (Vì 9 chia hết cho 3)
Ngày 3: Ăn 2 quả cam, 3 - 2*(3/3) = 3 - 2 = 1. 
Ngày 4: Ăn quả cam cuối cùng  1 - 1  = 0.
Bạn cần ít nhất 4 ngày để ăn hết 10 quả cam.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 6
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Bạn có 6 quả cam.
Ngày 1: Ăn 3 quả cam, 6 - 6/2 = 6 - 3 = 3. (Vì 6 chia hết cho 2).
Ngày 2: Ăn 2 quả cam, 3 - 2*(3/3) = 3 - 2 = 1. (Vì 3 chia hết cho 3)
Ngày 3: Ăn quả cam cuối cùng  1 - 1  = 0.
Bạn cần ít nhất 3 ngày để ăn hết 6 quả cam.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 2 * 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi ngày ta có thể ăn một quả, hoặc một nửa / hai phần ba khi số lượng chia hết. $n$ có thể đạt $2\times 10^9$, nên không thể dùng mảng DP theo số dư, còn giảm từng quả một là bất khả thi.
>
> Trong lời giải tối ưu, trước tiên ta ăn $n\bmod 2$ hoặc $n\bmod 3$ quả để phép chia cho hai hoặc ba trở nên hợp lệ. Vì vậy $dfs(n)=1+\min(n\bmod 2+dfs(\lfloor n/2\rfloor), n\bmod 3+dfs(\lfloor n/3\rfloor))$. Phép chia làm $n$ giảm nhanh; memoization giữ khoảng $O(\log^2 n)$ trạng thái.

<!-- thinking:end -->

Theo mô tả đề bài, với mỗi $n$, ta có thể chọn một trong ba cách:

1. Giảm $n$ đi $1$;
2. Nếu $n$ chia hết cho $2$, chia $n$ cho $2$;
3. Nếu $n$ chia hết cho $3$, chia $n$ cho $3$.

Do đó, bài toán tương đương với việc tìm số ngày nhỏ nhất để giảm $n$ về $0$ bằng ba cách trên.

Ta xây dựng hàm $dfs(n)$ biểu diễn số ngày nhỏ nhất để giảm $n$ về $0$. Hàm $dfs(n)$ được thực hiện như sau:

1. Nếu $n < 2$, trả về $n$;
2. Nếu không, ta có thể trước tiên giảm $n$ về bội của $2$ bằng $n \bmod 2$ phép toán loại $1$, rồi thực hiện phép toán loại $2$ để giảm $n$ thành $n/2$; hoặc giảm $n$ về bội của $3$ bằng $n \bmod 3$ phép toán loại $1$, rồi thực hiện phép toán loại $3$ để giảm $n$ thành $n/3$. Ta chọn giá trị nhỏ hơn trong hai cách, tức là $1 + \min(n \bmod 2 + dfs(n/2), n \bmod 3 + dfs(n/3))$.

Để tránh tính toán lặp lại, ta dùng memoization và lưu các giá trị đã tính của $dfs(n)$ trong một hash table.

Độ phức tạp thời gian là $O(\log^2 n)$, còn độ phức tạp không gian là $O(\log^2 n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minDays(self, n: int) -> int:
        @cache
        def dfs(n: int) -> int:
            if n < 2:
                return n
            return 1 + min(n % 2 + dfs(n // 2), n % 3 + dfs(n // 3))

        return dfs(n)
```

#### Java

```java
class Solution {
    private Map<Integer, Integer> f = new HashMap<>();

    public int minDays(int n) {
        return dfs(n);
    }

    private int dfs(int n) {
        if (n < 2) {
            return n;
        }
        if (f.containsKey(n)) {
            return f.get(n);
        }
        int res = 1 + Math.min(n % 2 + dfs(n / 2), n % 3 + dfs(n / 3));
        f.put(n, res);
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    unordered_map<int, int> f;

    int minDays(int n) {
        return dfs(n);
    }

    int dfs(int n) {
        if (n < 2) {
            return n;
        }
        if (f.count(n)) {
            return f[n];
        }
        int res = 1 + min(n % 2 + dfs(n / 2), n % 3 + dfs(n / 3));
        f[n] = res;
        return res;
    }
};
```

#### Go

```go
func minDays(n int) int {
	f := map[int]int{0: 0, 1: 1}
	var dfs func(int) int
	dfs = func(n int) int {
		if v, ok := f[n]; ok {
			return v
		}
		res := 1 + min(n%2+dfs(n/2), n%3+dfs(n/3))
		f[n] = res
		return res
	}
	return dfs(n)
}
```

#### TypeScript

```ts
function minDays(n: number): number {
    const f: Record<number, number> = {};
    const dfs = (n: number): number => {
        if (n < 2) {
            return n;
        }
        if (f[n] !== undefined) {
            return f[n];
        }
        f[n] = 1 + Math.min((n % 2) + dfs((n / 2) | 0), (n % 3) + dfs((n / 3) | 0));
        return f[n];
    };
    return dfs(n);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
