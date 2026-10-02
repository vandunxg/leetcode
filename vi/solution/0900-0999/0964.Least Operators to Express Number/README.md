---
comments: true
difficulty: Hard
tags:
    - Memoization
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [964. Least Operators to Express Number](https://leetcode.com/problems/least-operators-to-express-number)

[中文文档](/solution/0900-0999/0964.Least%20Operators%20to%20Express%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên dương <code>x</code>. Ta sẽ viết biểu thức dạng <code>x (op1) x (op2) x (op3) x ...</code>, trong đó mỗi toán tử <code>op1</code>, <code>op2</code>, ... là phép cộng, trừ, nhân hoặc chia (<code>+</code>, <code>-</code>, <code>*</code> hoặc <code>/)</code>. Ví dụ, với <code>x = 3</code>, ta có thể viết <code>3 * 3 / 3 + 3 - 3</code>, biểu thức này có giá trị <font face="monospace">3</font>.</p>

<p>Khi viết biểu thức như vậy, ta tuân theo các quy ước sau:</p>

<ul>
	<li>Phép chia (<code>/</code>) cho kết quả là số hữu tỉ.</li>
	<li>Không được dùng dấu ngoặc đơn ở bất kỳ vị trí nào.</li>
	<li>Ta dùng thứ tự phép toán thông thường: phép nhân và chia được thực hiện trước phép cộng và trừ.</li>
	<li>Không được dùng toán tử phủ định một ngôi (<code>-</code>). Ví dụ, &quot;<code>x - x</code>&quot; là biểu thức hợp lệ vì chỉ dùng phép trừ, còn &quot;<code>-x + x</code>&quot; không hợp lệ vì có phép phủ định.</li>
</ul>

<p>Ta muốn viết một biểu thức có ít toán tử nhất sao cho giá trị bằng <code>target</code> đã cho. Hãy trả về số toán tử ít nhất cần dùng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> x = 3, target = 19
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> 3 * 3 + 3 * 3 + 3 / 3.
Biểu thức có 5 phép toán.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> x = 5, target = 501
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> 5 * 5 * 5 * 5 - 5 * 5 * 5 + 5 / 5.
Biểu thức có 8 phép toán.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> x = 100, target = 100000000
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> 100 * 100 * 100 * 100.
Biểu thức có 3 phép toán.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= x &lt;= 100</code></li>
	<li><code>1 &lt;= target &lt;= 2 * 10<sup>8</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có memoization

<!-- thinking:start -->

> **Tư duy**
>
> Tạo $\textit{target}$ từ $x$ và bốn phép toán với số toán tử ít nhất có thể. Các lũy thừa $x^k$ là những khối tự nhiên. Khi $v\le x$, có thể dùng nhiều bản sao của $x/x$. Nếu không, tìm $k$ nhỏ nhất sao cho $x^k\ge v$, rồi chọn giữa cách lấy $x^{k-1}$ cộng phần còn thiếu và (khi phần vượt nhỏ hơn $v$) cách lấy $x^k$ trừ phần dư. Dùng memoization cho lời gọi đệ quy.

<!-- thinking:end -->

Ta định nghĩa hàm $dfs(v)$ là số toán tử ít nhất cần để biểu diễn số $v$ bằng cách dùng $x$. Khi đó, đáp án là $dfs(target)$.

Hàm $dfs(v)$ hoạt động như sau:

Nếu $x \geq v$, ta có thể tạo $v$ bằng cách cộng $v$ biểu thức $x / x$, cần $v \times 2 - 1$ toán tử; hoặc lấy $x$ trừ đi $(x - v)$ biểu thức $x / x$, cần $(x - v) \times 2$ toán tử. Chọn giá trị nhỏ hơn trong hai cách.

Nếu không, lần lượt xét $x^k$ từ $k=2$ để tìm $k$ đầu tiên sao cho $x^k \geq v$:

- Nếu $x^k - v \geq v$, ta chỉ có thể tạo $x^{k-1}$ trước, rồi đệ quy tính $dfs(v - x^{k-1})$. Số toán tử trong trường hợp này là $k - 1 + dfs(v - x^{k-1})$;
- Nếu $x^k - v < v$, ta có thể tạo $v$ theo cách trên với $k - 1 + dfs(v - x^{k-1})$ toán tử; hoặc tạo $x^k$ trước, rồi đệ quy tính $dfs(x^k - v)$, cần $k + dfs(x^k - v)$ toán tử. Chọn giá trị nhỏ hơn trong hai cách.

Để tránh tính toán lặp lại, ta cài đặt hàm $dfs$ bằng tìm kiếm có memoization.

Độ phức tạp thời gian là $O(\log_{x}{target})$ và độ phức tạp không gian là $O(\log_{x}{target})$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def leastOpsExpressTarget(self, x: int, target: int) -> int:
        @cache
        def dfs(v: int) -> int:
            if x >= v:
                return min(v * 2 - 1, 2 * (x - v))
            k = 2
            while x**k < v:
                k += 1
            if x**k - v < v:
                return min(k + dfs(x**k - v), k - 1 + dfs(v - x ** (k - 1)))
            return k - 1 + dfs(v - x ** (k - 1))

        return dfs(target)
```

#### Java

```java
class Solution {
    private int x;
    private Map<Integer, Integer> f = new HashMap<>();

    public int leastOpsExpressTarget(int x, int target) {
        this.x = x;
        return dfs(target);
    }

    private int dfs(int v) {
        if (x >= v) {
            return Math.min(v * 2 - 1, 2 * (x - v));
        }
        if (f.containsKey(v)) {
            return f.get(v);
        }
        int k = 2;
        long y = (long) x * x;
        while (y < v) {
            y *= x;
            ++k;
        }
        int ans = k - 1 + dfs(v - (int) (y / x));
        if (y - v < v) {
            ans = Math.min(ans, k + dfs((int) y - v));
        }
        f.put(v, ans);
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int leastOpsExpressTarget(int x, int target) {
        unordered_map<int, int> f;
        function<int(int)> dfs = [&](int v) -> int {
            if (x >= v) {
                return min(v * 2 - 1, 2 * (x - v));
            }
            if (f.count(v)) {
                return f[v];
            }
            int k = 2;
            long long y = x * x;
            while (y < v) {
                y *= x;
                ++k;
            }
            int ans = k - 1 + dfs(v - y / x);
            if (y - v < v) {
                ans = min(ans, k + dfs(y - v));
            }
            f[v] = ans;
            return ans;
        };
        return dfs(target);
    }
};
```

#### Go

```go
func leastOpsExpressTarget(x int, target int) int {
	f := map[int]int{}
	var dfs func(int) int
	dfs = func(v int) int {
		if x > v {
			return min(v*2-1, 2*(x-v))
		}
		if val, ok := f[v]; ok {
			return val
		}
		k := 2
		y := x * x
		for y < v {
			y *= x
			k++
		}
		ans := k - 1 + dfs(v-y/x)
		if y-v < v {
			ans = min(ans, k+dfs(y-v))
		}
		f[v] = ans
		return ans
	}
	return dfs(target)
}
```

#### TypeScript

```ts
function leastOpsExpressTarget(x: number, target: number): number {
    const f: Map<number, number> = new Map();
    const dfs = (v: number): number => {
        if (x > v) {
            return Math.min(v * 2 - 1, 2 * (x - v));
        }
        if (f.has(v)) {
            return f.get(v)!;
        }
        let k = 2;
        let y = x * x;
        while (y < v) {
            y *= x;
            ++k;
        }
        let ans = k - 1 + dfs(v - Math.floor(y / x));
        if (y - v < v) {
            ans = Math.min(ans, k + dfs(y - v));
        }
        f.set(v, ans);
        return ans;
    };
    return dfs(target);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
