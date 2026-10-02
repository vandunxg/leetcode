---
comments: true
difficulty: Hard
tags:
    - Depth-First Search
    - Graph
    - String
    - Eulerian Path
    - Eulerian Circuit
    - Eulerian Graph
---

<!-- problem:start -->

# [753. Cracking the Safe](https://leetcode.com/problems/cracking-the-safe)

[中文文档](/solution/0700-0799/0753.Cracking%20the%20Safe/README.md)

## Mô tả

<!-- description:start -->

<p>Có một chiếc két được bảo vệ bằng mật khẩu. Mật khẩu là chuỗi gồm <code>n</code> chữ số, mỗi chữ số nằm trong khoảng <code>[0, k - 1]</code>.</p>

<p>Két có cách kiểm tra mật khẩu đặc biệt. Mỗi khi bạn nhập thêm một chữ số, két sẽ kiểm tra <strong><code>n</code> chữ số mới nhất</strong> đã nhập.</p>

<ul>
	<li>Ví dụ, mật khẩu đúng là <code>&quot;345&quot;</code> và bạn nhập <code>&quot;012345&quot;</code>:

    <ul>
    	<li>Sau khi nhập <code>0</code>, <code>3</code> chữ số gần nhất là <code>&quot;0&quot;</code>, không đúng.</li>
    	<li>Sau khi nhập <code>1</code>, <code>3</code> chữ số gần nhất là <code>&quot;01&quot;</code>, không đúng.</li>
    	<li>Sau khi nhập <code>2</code>, <code>3</code> chữ số gần nhất là <code>&quot;012&quot;</code>, không đúng.</li>
    	<li>Sau khi nhập <code>3</code>, <code>3</code> chữ số gần nhất là <code>&quot;123&quot;</code>, không đúng.</li>
    	<li>Sau khi nhập <code>4</code>, <code>3</code> chữ số gần nhất là <code>&quot;234&quot;</code>, không đúng.</li>
    	<li>Sau khi nhập <code>5</code>, <code>3</code> chữ số gần nhất là <code>&quot;345&quot;</code>, đây là mật khẩu đúng và két sẽ mở.</li>
    </ul>
    </li>

</ul>

<p>Trả về <em>chuỗi có <strong>độ dài ngắn nhất</strong> có thể mở két <strong>tại một thời điểm nào đó</strong> trong quá trình nhập</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1, k = 2
<strong>Đầu ra:</strong> &quot;10&quot;
<strong>Giải thích:</strong> Mật khẩu chỉ gồm một chữ số nên hãy nhập lần lượt từng chữ số. &quot;01&quot; cũng có thể mở két.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2, k = 2
<strong>Đầu ra:</strong> &quot;01100&quot;
<strong>Giải thích:</strong> Với mỗi mật khẩu có thể có:
- Nhập &quot;00&quot; bắt đầu từ chữ số thứ 4.
- Nhập &quot;01&quot; bắt đầu từ chữ số thứ 1.
- Nhập &quot;10&quot; bắt đầu từ chữ số thứ 3.
- Nhập &quot;11&quot; bắt đầu từ chữ số thứ 2.
Vì vậy, &quot;01100&quot; sẽ mở két. &quot;10011&quot; và &quot;11001&quot; cũng có thể mở két.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 4</code></li>
	<li><code>1 &lt;= k &lt;= 10</code></li>
	<li><code>1 &lt;= k<sup>n</sup> &lt;= 4096</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Chu trình Euler

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi ngắn nhất chứa mọi mật khẩu độ dài $n$ được tạo từ $k$ chữ số là de Bruijn sequence, có độ dài $k^n+n-1$.
>
> Các node là các chuỗi độ dài $(n-1)$; mỗi chữ số bổ sung tạo thành một cạnh, tức một mật khẩu. Mỗi node có bậc vào và bậc ra đều bằng $k$, nên tồn tại chu trình Euler tạo ra đáp án.
>
> Bắt đầu Hierholzer từ $0$: đánh dấu cạnh $u\cdot 10+x$, đệ quy, rồi thêm chữ số vào sau khi quay lui (post-order); cuối cùng nối thêm $n-1$ chữ số 0 của điểm bắt đầu.

<!-- thinking:end -->

Có thể xây dựng đồ thị có hướng theo đề bài: mỗi đỉnh là một chuỗi độ dài $n-1$ trên bảng chữ cái gồm $k$ chữ số, và mỗi cạnh mang một chữ số từ $0$ đến $k-1$. Nếu có cạnh có hướng $e$ từ đỉnh $u$ đến đỉnh $v$, trong đó cạnh $e$ mang chữ số $c$, thì $k-1$ ký tự cuối của $u+c$ tạo thành chuỗi $v$. Khi đó, cạnh $u+c$ biểu diễn một mật khẩu độ dài $n$.

Đồ thị có hướng này có $k^{n-1}$ đỉnh, mỗi đỉnh có $k$ cạnh đi ra và $k$ cạnh đi vào. Vì vậy, đồ thị có chu trình Euler, và đường đi theo chu trình Euler là đáp án.

Độ phức tạp thời gian là $O(k^n)$ và độ phức tạp không gian là $O(k^n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def crackSafe(self, n: int, k: int) -> str:
        def dfs(u):
            for x in range(k):
                e = u * 10 + x
                if e not in vis:
                    vis.add(e)
                    v = e % mod
                    dfs(v)
                    ans.append(str(x))

        mod = 10 ** (n - 1)
        vis = set()
        ans = []
        dfs(0)
        ans.append("0" * (n - 1))
        return "".join(ans)
```

#### Java

```java
class Solution {
    private Set<Integer> vis = new HashSet<>();
    private StringBuilder ans = new StringBuilder();
    private int mod;

    public String crackSafe(int n, int k) {
        mod = (int) Math.pow(10, n - 1);
        dfs(0, k);
        ans.append("0".repeat(n - 1));
        return ans.toString();
    }

    private void dfs(int u, int k) {
        for (int x = 0; x < k; ++x) {
            int e = u * 10 + x;
            if (vis.add(e)) {
                int v = e % mod;
                dfs(v, k);
                ans.append(x);
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    string crackSafe(int n, int k) {
        unordered_set<int> vis;
        int mod = pow(10, n - 1);
        string ans;
        function<void(int)> dfs = [&](int u) {
            for (int x = 0; x < k; ++x) {
                int e = u * 10 + x;
                if (!vis.count(e)) {
                    vis.insert(e);
                    dfs(e % mod);
                    ans += (x + '0');
                }
            }
        };
        dfs(0);
        ans += string(n - 1, '0');
        return ans;
    }
};
```

#### Go

```go
func crackSafe(n int, k int) string {
	mod := int(math.Pow(10, float64(n-1)))
	vis := map[int]bool{}
	ans := &strings.Builder{}
	var dfs func(int)
	dfs = func(u int) {
		for x := 0; x < k; x++ {
			e := u*10 + x
			if !vis[e] {
				vis[e] = true
				v := e % mod
				dfs(v)
				ans.WriteByte(byte('0' + x))
			}
		}
	}
	dfs(0)
	ans.WriteString(strings.Repeat("0", n-1))
	return ans.String()
}
```

#### TypeScript

```ts
function crackSafe(n: number, k: number): string {
    function dfs(u: number): void {
        for (let x = 0; x < k; x++) {
            const e = u * 10 + x;
            if (!vis.has(e)) {
                vis.add(e);
                const v = e % mod;
                dfs(v);
                ans.push(x.toString());
            }
        }
    }

    const mod = Math.pow(10, n - 1);
    const vis = new Set<number>();
    const ans: string[] = [];

    dfs(0);
    ans.push('0'.repeat(n - 1));
    return ans.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
