---
comments: true
difficulty: Medium
rating: 1794
source: Biweekly Contest 121 Q3
tags:
    - Breadth-First Search
    - Memoization
    - Dynamic Programming
---

<!-- problem:start -->

# [2998. Minimum Number of Operations to Make X and Y Equal](https://leetcode.com/problems/minimum-number-of-operations-to-make-x-and-y-equal)

[中文文档](/solution/2900-2999/2998.Minimum%20Number%20of%20Operations%20to%20Make%20X%20and%20Y%20Equal/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp hai số nguyên dương <code>x</code> và <code>y</code>.</p>

<p>Trong một thao tác, bạn có thể thực hiện một trong bốn thao tác sau:</p>

<ol>
	<li>Chia <code>x</code> cho <code>11</code> nếu <code>x</code> là bội số của <code>11</code>.</li>
	<li>Chia <code>x</code> cho <code>5</code> nếu <code>x</code> là bội số của <code>5</code>.</li>
	<li>Giảm <code>x</code> đi <code>1</code>.</li>
	<li>Tăng <code>x</code> thêm <code>1</code>.</li>
</ol>

<p>Trả về <em><strong>số thao tác nhỏ nhất</strong> cần thực hiện để làm cho </em> <code>x</code> <i>và</i> <code>y</code> bằng nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> x = 26, y = 1
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta có thể biến 26 thành 1 bằng cách thực hiện các thao tác sau:
1. Giảm x đi 1
2. Chia x cho 5
3. Chia x cho 5
Có thể chứng minh rằng 3 là số thao tác nhỏ nhất cần thực hiện để biến 26 thành 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> x = 54, y = 2
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Ta có thể biến 54 thành 2 bằng cách thực hiện các thao tác sau:
1. Tăng x thêm 1
2. Chia x cho 11
3. Chia x cho 5
4. Tăng x thêm 1
Có thể chứng minh rằng 4 là số thao tác nhỏ nhất cần thực hiện để biến 54 thành 2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> x = 25, y = 30
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Ta có thể biến 25 thành 30 bằng cách thực hiện các thao tác sau:
1. Tăng x thêm 1
2. Tăng x thêm 1
3. Tăng x thêm 1
4. Tăng x thêm 1
5. Tăng x thêm 1
Có thể chứng minh rằng 5 là số thao tác nhỏ nhất cần thực hiện để biến 25 thành 30.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= x, y &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các thao tác là $\pm 1$ và, khi chia hết, phép chia cho $5$ hoặc $11$. $x,y \le 10^4$. Nếu $y \ge x$ thì chỉ cần giảm, với chi phí $y-x$. Nếu không, phép chia có thể giúp giảm nhanh, nhưng có thể cần dùng $\pm$ để đưa $x$ đến bội số tiếp theo trước.
>
> $dfs(x)$ so sánh việc giảm trực tiếp đến $y$ với bốn nhánh "căn chỉnh rồi chia cho $5$ hoặc $11$". Memoization giúp tái sử dụng các trạng thái; quá trình tìm kiếm chỉ làm $x$ nhỏ đi nên sẽ kết thúc.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumOperationsToMakeEqual(self, x: int, y: int) -> int:
        @cache
        def dfs(x: int) -> int:
            if y >= x:
                return y - x
            ans = x - y
            ans = min(ans, x % 5 + 1 + dfs(x // 5))
            ans = min(ans, 5 - x % 5 + 1 + dfs(x // 5 + 1))
            ans = min(ans, x % 11 + 1 + dfs(x // 11))
            ans = min(ans, 11 - x % 11 + 1 + dfs(x // 11 + 1))
            return ans

        return dfs(x)
```

#### Java

```java
class Solution {
    private Map<Integer, Integer> f = new HashMap<>();
    private int y;

    public int minimumOperationsToMakeEqual(int x, int y) {
        this.y = y;
        return dfs(x);
    }

    private int dfs(int x) {
        if (y >= x) {
            return y - x;
        }
        if (f.containsKey(x)) {
            return f.get(x);
        }
        int ans = x - y;
        int a = x % 5 + 1 + dfs(x / 5);
        int b = 5 - x % 5 + 1 + dfs(x / 5 + 1);
        int c = x % 11 + 1 + dfs(x / 11);
        int d = 11 - x % 11 + 1 + dfs(x / 11 + 1);
        ans = Math.min(ans, Math.min(a, Math.min(b, Math.min(c, d))));
        f.put(x, ans);
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumOperationsToMakeEqual(int x, int y) {
        unordered_map<int, int> f;
        function<int(int)> dfs = [&](int x) {
            if (y >= x) {
                return y - x;
            }
            if (f.count(x)) {
                return f[x];
            }
            int a = x % 5 + 1 + dfs(x / 5);
            int b = 5 - x % 5 + 1 + dfs(x / 5 + 1);
            int c = x % 11 + 1 + dfs(x / 11);
            int d = 11 - x % 11 + 1 + dfs(x / 11 + 1);
            return f[x] = min({x - y, a, b, c, d});
        };
        return dfs(x);
    }
};
```

#### Go

```go
func minimumOperationsToMakeEqual(x int, y int) int {
	f := map[int]int{}
	var dfs func(int) int
	dfs = func(x int) int {
		if y >= x {
			return y - x
		}
		if v, ok := f[x]; ok {
			return v
		}
		a := x%5 + 1 + dfs(x/5)
		b := 5 - x%5 + 1 + dfs(x/5+1)
		c := x%11 + 1 + dfs(x/11)
		d := 11 - x%11 + 1 + dfs(x/11+1)
		f[x] = min(x-y, a, b, c, d)
		return f[x]
	}
	return dfs(x)
}
```

#### TypeScript

```ts
function minimumOperationsToMakeEqual(x: number, y: number): number {
    const f: Map<number, number> = new Map();
    const dfs = (x: number): number => {
        if (y >= x) {
            return y - x;
        }
        if (f.has(x)) {
            return f.get(x)!;
        }
        const a = (x % 5) + 1 + dfs((x / 5) | 0);
        const b = 5 - (x % 5) + 1 + dfs(((x / 5) | 0) + 1);
        const c = (x % 11) + 1 + dfs((x / 11) | 0);
        const d = 11 - (x % 11) + 1 + dfs(((x / 11) | 0) + 1);
        const ans = Math.min(x - y, a, b, c, d);
        f.set(x, ans);
        return ans;
    };
    return dfs(x);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
