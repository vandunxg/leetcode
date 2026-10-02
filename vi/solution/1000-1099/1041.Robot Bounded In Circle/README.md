---
comments: true
difficulty: Medium
rating: 1521
source: Weekly Contest 136 Q1
tags:
    - Math
    - String
    - Simulation
---

<!-- problem:start -->

# [1041. Robot Bounded In Circle](https://leetcode.com/problems/robot-bounded-in-circle)

[中文文档](/solution/1000-1099/1041.Robot%20Bounded%20In%20Circle/README.md)

## Mô tả

<!-- description:start -->

<p>Trên một mặt phẳng vô hạn, ban đầu robot đứng tại <code>(0, 0)</code> và hướng về phía bắc. Lưu ý:</p>

<ul>
	<li><strong>Hướng bắc</strong> là chiều dương của trục y.</li>
	<li><strong>Hướng nam</strong> là chiều âm của trục y.</li>
	<li><strong>Hướng đông</strong> là chiều dương của trục x.</li>
	<li><strong>Hướng tây</strong> là chiều âm của trục x.</li>
</ul>

<p>Robot có thể nhận một trong ba chỉ dẫn sau:</p>

<ul>
	<li><code>&quot;G&quot;</code>: đi thẳng 1 đơn vị.</li>
	<li><code>&quot;L&quot;</code>: rẽ trái 90 độ (ngược chiều kim đồng hồ).</li>
	<li><code>&quot;R&quot;</code>: rẽ phải 90 độ (theo chiều kim đồng hồ).</li>
</ul>

<p>Robot thực hiện lần lượt các chỉ dẫn trong <code>instructions</code>, rồi lặp lại chúng mãi mãi.</p>

<p>Trả về <code>true</code> khi và chỉ khi tồn tại một đường tròn trên mặt phẳng mà robot không bao giờ đi ra ngoài.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> instructions = &quot;GGLLGG&quot;
<strong>Output:</strong> true
<strong>Giải thích:</strong> Ban đầu robot ở (0, 0) và hướng về phía bắc.
&quot;G&quot;: đi một bước. Vị trí: (0, 1). Hướng: Bắc.
&quot;G&quot;: đi một bước. Vị trí: (0, 2). Hướng: Bắc.
&quot;L&quot;: rẽ trái 90 độ ngược chiều kim đồng hồ. Vị trí: (0, 2). Hướng: Tây.
&quot;L&quot;: rẽ trái 90 độ ngược chiều kim đồng hồ. Vị trí: (0, 2). Hướng: Nam.
&quot;G&quot;: đi một bước. Vị trí: (0, 1). Hướng: Nam.
&quot;G&quot;: đi một bước. Vị trí: (0, 0). Hướng: Nam.
Khi lặp lại các chỉ dẫn, robot đi vào chu kỳ: (0, 0) --&gt; (0, 1) --&gt; (0, 2) --&gt; (0, 1) --&gt; (0, 0).
Vì vậy, ta trả về true.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> instructions = &quot;GG&quot;
<strong>Output:</strong> false
<strong>Giải thích:</strong> Ban đầu robot ở (0, 0) và hướng về phía bắc.
&quot;G&quot;: đi một bước. Vị trí: (0, 1). Hướng: Bắc.
&quot;G&quot;: đi một bước. Vị trí: (0, 2). Hướng: Bắc.
Khi lặp lại các chỉ dẫn, robot tiếp tục đi về phía bắc và không rơi vào chu kỳ.
Vì vậy, ta trả về false.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> instructions = &quot;GL&quot;
<strong>Output:</strong> true
<strong>Giải thích:</strong> Ban đầu robot ở (0, 0) và hướng về phía bắc.
&quot;G&quot;: đi một bước. Vị trí: (0, 1). Hướng: Bắc.
&quot;L&quot;: rẽ trái 90 độ ngược chiều kim đồng hồ. Vị trí: (0, 1). Hướng: Tây.
&quot;G&quot;: đi một bước. Vị trí: (-1, 1). Hướng: Tây.
&quot;L&quot;: rẽ trái 90 độ ngược chiều kim đồng hồ. Vị trí: (-1, 1). Hướng: Nam.
&quot;G&quot;: đi một bước. Vị trí: (-1, 0). Hướng: Nam.
&quot;L&quot;: rẽ trái 90 độ ngược chiều kim đồng hồ. Vị trí: (-1, 0). Hướng: Đông.
&quot;G&quot;: đi một bước. Vị trí: (0, 0). Hướng: Đông.
&quot;L&quot;: rẽ trái 90 độ ngược chiều kim đồng hồ. Vị trí: (0, 0). Hướng: Bắc.
Khi lặp lại các chỉ dẫn, robot đi vào chu kỳ: (0, 0) --&gt; (0, 1) --&gt; (-1, 1) --&gt; (-1, 0) --&gt; (0, 0).
Vì vậy, ta trả về true.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= instructions.length &lt;= 100</code></li>
	<li><code>instructions[i]</code> là <code>&#39;G&#39;</code>, <code>&#39;L&#39;</code> hoặc <code>&#39;R&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Chương trình lặp vô hạn nên không thể mô phỏng mãi. Độ dời tổng và hướng sau một chu kỳ quyết định robot có bị giới hạn hay không: nếu quay về gốc hoặc không còn hướng bắc, các chu kỳ sau sẽ tạo thành một vòng lặp.
>
> $k\in[0,3]$ biểu diễn hướng di chuyển, còn $\textit{dist}$ đếm số bước theo bốn hướng. Rẽ trái cộng một, rẽ phải cộng ba, còn đi thẳng tăng bộ đếm của hướng hiện tại.
>
> Sau một chu kỳ, chấp nhận nếu số bước hướng bắc bằng hướng nam và hướng đông bằng hướng tây, hoặc nếu $k\neq 0$.

<!-- thinking:end -->

Ta có thể mô phỏng chuyển động của robot. Dùng biến $k$ biểu diễn hướng của robot, khởi tạo bằng $0$ tương ứng với hướng bắc. $k$ nhận giá trị trong $[0, 3]$, lần lượt biểu diễn các hướng bắc, tây, nam và đông. Ngoài ra, dùng mảng $dist$ có độ dài $4$ để ghi lại quãng đường robot đi theo bốn hướng, khởi tạo là $[0, 0, 0, 0]$.

Duyệt chuỗi chỉ dẫn $\textit{instructions}$. Nếu chỉ dẫn hiện tại là `'L'`, robot rẽ sang hướng tây, tức là $k = (k + 1) \bmod 4$; nếu là `'R'`, robot rẽ sang hướng đông, tức là $k = (k + 3) \bmod 4$; nếu không, robot đi một bước theo hướng hiện tại, tức là $dist[k]++$.

Nếu thực hiện chuỗi chỉ dẫn $\textit{instructions}$ một lần mà robot quay về gốc, tức là $dist[0] = dist[2]$ và $dist[1] = dist[3]$, thì chắc chắn robot sẽ đi vào một chu kỳ. Dù lặp lại chỉ dẫn bao nhiêu lần, robot vẫn luôn quay về gốc nên sẽ tạo thành một vòng lặp.

Nếu thực hiện chuỗi chỉ dẫn $\textit{instructions}$ một lần mà robot không quay về gốc, giả sử lúc đó robot ở $(x, y)$ và có hướng $k$.

- Nếu $k=0$, tức robot vẫn hướng bắc, thì sau lần thực hiện chỉ dẫn thứ hai, độ dời là $(x, y)$; sau lần thứ ba, độ dời vẫn là $(x, y)$... Cộng dồn các độ dời này, robot sẽ đến $(n \times x, n \times y)$ với $n$ là số nguyên dương. Vì robot không quay về gốc, tức $x \neq 0$ hoặc $y \neq 0$, nên $n \times x \neq 0$ hoặc $n \times y \neq 0$; do đó robot không đi vào chu kỳ;
- Nếu $k=1$, tức robot hướng tây, thì sau lần thực hiện chỉ dẫn thứ hai, độ dời là $(-y, x)$; sau lần thứ ba là $(-x, -y)$; sau lần thứ tư là $(y, -x)$. Cộng dồn các độ dời này, robot cuối cùng sẽ quay về gốc $(0, 0)$;
- Nếu $k=2$, tức robot hướng nam, thì sau lần thực hiện chỉ dẫn thứ hai, độ dời là $(-x, -y)$. Cộng dồn hai độ dời này, robot cuối cùng sẽ quay về gốc $(0, 0)$;
- Nếu $k=3$, tức robot hướng đông, thì sau lần thực hiện chỉ dẫn thứ hai, độ dời là $(y, -x)$; sau lần thứ ba là $(-x, -y)$; sau lần thứ tư là $(-y, x)$. Cộng dồn các độ dời này, robot cuối cùng sẽ quay về gốc $(0, 0)$.

Tóm lại, nếu sau khi thực hiện chuỗi chỉ dẫn $\textit{instructions}$ một lần robot quay về gốc hoặc hướng của robot khác với hướng ban đầu, thì chắc chắn robot sẽ đi vào chu kỳ.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$, với $n$ là độ dài chuỗi chỉ dẫn $\textit{instructions}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isRobotBounded(self, instructions: str) -> bool:
        k = 0
        dist = [0] * 4
        for c in instructions:
            if c == 'L':
                k = (k + 1) % 4
            elif c == 'R':
                k = (k + 3) % 4
            else:
                dist[k] += 1
        return (dist[0] == dist[2] and dist[1] == dist[3]) or k != 0
```

#### Java

```java
class Solution {
    public boolean isRobotBounded(String instructions) {
        int k = 0;
        int[] dist = new int[4];
        for (int i = 0; i < instructions.length(); ++i) {
            char c = instructions.charAt(i);
            if (c == 'L') {
                k = (k + 1) % 4;
            } else if (c == 'R') {
                k = (k + 3) % 4;
            } else {
                ++dist[k];
            }
        }
        return (dist[0] == dist[2] && dist[1] == dist[3]) || (k != 0);
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isRobotBounded(string instructions) {
        int dist[4]{};
        int k = 0;
        for (char& c : instructions) {
            if (c == 'L') {
                k = (k + 1) % 4;
            } else if (c == 'R') {
                k = (k + 3) % 4;
            } else {
                ++dist[k];
            }
        }
        return (dist[0] == dist[2] && dist[1] == dist[3]) || k;
    }
};
```

#### Go

```go
func isRobotBounded(instructions string) bool {
	dist := [4]int{}
	k := 0
	for _, c := range instructions {
		if c == 'L' {
			k = (k + 1) % 4
		} else if c == 'R' {
			k = (k + 3) % 4
		} else {
			dist[k]++
		}
	}
	return (dist[0] == dist[2] && dist[1] == dist[3]) || k != 0
}
```

#### TypeScript

```ts
function isRobotBounded(instructions: string): boolean {
    const dist: number[] = new Array(4).fill(0);
    let k = 0;
    for (const c of instructions) {
        if (c === 'L') {
            k = (k + 1) % 4;
        } else if (c === 'R') {
            k = (k + 3) % 4;
        } else {
            ++dist[k];
        }
    }
    return (dist[0] === dist[2] && dist[1] === dist[3]) || k !== 0;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
