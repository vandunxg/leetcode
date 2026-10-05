---
comments: true
difficulty: Medium
rating: 1278
source: Weekly Contest 507 Q1
tags:
    - Math
    - String
    - Counting
---

<!-- problem:start -->

# [3968. Maximum Manhattan Distance After All Moves](https://leetcode.com/problems/maximum-manhattan-distance-after-all-moves)

[中文文档](/solution/3900-3999/3968.Maximum%20Manhattan%20Distance%20After%20All%20Moves/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>moves</code> gồm các ký tự <code>&#39;U&#39;</code>, <code>&#39;D&#39;</code>, <code>&#39;L&#39;</code>, <code>&#39;R&#39;</code> và <code>&#39;_&#39;</code>.</p>

<p>Bắt đầu từ gốc tọa độ <code>(0, 0)</code>, mỗi ký tự biểu thị một lần di chuyển trên mặt phẳng 2D:</p>

<ul>
	<li><code>&#39;U&#39;</code>: Di chuyển lên 1 đơn vị.</li>
	<li><code>&#39;D&#39;</code>: Di chuyển xuống 1 đơn vị.</li>
	<li><code>&#39;L&#39;</code>: Di chuyển sang trái 1 đơn vị.</li>
	<li><code>&#39;R&#39;</code>: Di chuyển sang phải 1 đơn vị.</li>
	<li><code>&#39;_&#39;</code>: Có thể được thay độc lập bằng một trong các ký tự <code>&#39;U&#39;</code>, <code>&#39;D&#39;</code>, <code>&#39;L&#39;</code> hoặc <code>&#39;R&#39;</code>.</li>
</ul>

<p>Trả về <span data-keyword="manhattan-distance"><strong>khoảng cách Manhattan</strong></span> lớn nhất tới gốc tọa độ có thể đạt được sau khi thực hiện tất cả các bước di chuyển.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">moves = &quot;L_D_&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một lựa chọn tối ưu là:</p>

<ul>
	<li><code>&#39;L&#39;</code>: <code>(0, 0) -&gt; (-1, 0)</code></li>
	<li><code>&#39;_&#39;</code> được xem là <code>&#39;D&#39;</code>: <code>(-1, 0) -&gt; (-1, -1)</code></li>
	<li><code>&#39;D&#39;</code>: <code>(-1, -1) -&gt; (-1, -2)</code></li>
	<li><code>&#39;_&#39;</code> được xem là <code>&#39;L&#39;</code>: <code>(-1, -2) -&gt; (-2, -2)</code></li>
</ul>

<p>Khoảng cách Manhattan cuối cùng tới gốc tọa độ là <code>|0 - (-2)| + |0 - (-2)| = 4</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">moves = &quot;U_R&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một lựa chọn tối ưu là:</p>

<ul>
	<li><code>&#39;U&#39;</code>: <code>(0, 0) -&gt; (0, 1)</code></li>
	<li><code>&#39;_&#39;</code> được xem là <code>&#39;U&#39;</code>: <code>(0, 1) -&gt; (0, 2)</code></li>
	<li><code>&#39;R&#39;</code>: <code>(0, 2) -&gt; (1, 2)</code></li>
</ul>

<p>Khoảng cách Manhattan cuối cùng tới gốc tọa độ là <code>|0 - 1| + |0 - 2| = 3</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= moves.length &lt;= 10<sup>5</sup></code></li>
	<li><code>moves</code> chỉ gồm các ký tự <code>&#39;U&#39;</code>, <code>&#39;D&#39;</code>, <code>&#39;L&#39;</code>, <code>&#39;R&#39;</code> và <code>&#39;_&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi `_` có thể trở thành bất kỳ hướng nào. Khoảng cách Manhattan cuối cùng là độ lớn của tổng di chuyển theo chiều dọc cộng với độ lớn của tổng di chuyển theo chiều ngang; mọi wildcard đều có thể được chọn cùng chiều với các tổng này và đóng góp trực tiếp vào kết quả.
>
> Duyệt một lần để cộng các bước $U/D$ vào $x$, $L/R$ vào $y$ và `_` vào $z$; đáp án là $|x|+|y|+z$.
>
> Không cần dựng đường đi cụ thể.

<!-- thinking:end -->

Ta có thể dùng biến $x$ để lưu tổng di chuyển theo chiều dọc, biến $y$ để lưu tổng di chuyển theo chiều ngang và biến $z$ để lưu số bước có thể thay thế.

Khi đó, khoảng cách Manhattan cuối cùng là $|x| + |y| + z$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi $\textit{moves}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxDistance(self, moves: str) -> int:
        x = y = z = 0
        for c in moves:
            if c == "U":
                x -= 1
            elif c == "D":
                x += 1
            elif c == "L":
                y -= 1
            elif c == "R":
                y += 1
            else:
                z += 1
        return abs(x) + abs(y) + z
```

#### Java

```java
class Solution {
    public int maxDistance(String moves) {
        int x = 0, y = 0, z = 0;
        for (char c : moves.toCharArray()) {
            if (c == 'U') {
                x -= 1;
            } else if (c == 'D') {
                x += 1;
            } else if (c == 'L') {
                y -= 1;
            } else if (c == 'R') {
                y += 1;
            } else {
                z += 1;
            }
        }
        return Math.abs(x) + Math.abs(y) + z;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxDistance(string moves) {
        int x = 0, y = 0, z = 0;
        for (char c : moves) {
            if (c == 'U') {
                x -= 1;
            } else if (c == 'D') {
                x += 1;
            } else if (c == 'L') {
                y -= 1;
            } else if (c == 'R') {
                y += 1;
            } else {
                z += 1;
            }
        }
        return abs(x) + abs(y) + z;
    }
};
```

#### Go

```go
func maxDistance(moves string) int {
    x, y, z := 0, 0, 0
    for _, c := range moves {
        if c == 'U' {
            x -= 1
        } else if c == 'D' {
            x += 1
        } else if c == 'L' {
            y -= 1
        } else if c == 'R' {
            y += 1
        } else {
            z += 1
        }
    }
    return abs(x) + abs(y) + z
}

func abs(x int) int {
    if x < 0 {
        return -x
    }
    return x
}
```

#### TypeScript

```ts
function maxDistance(moves: string): number {
    let [x, y, z] = [0, 0, 0];
    for (const c of moves) {
        if (c === 'U') {
            x -= 1;
        } else if (c === 'D') {
            x += 1;
        } else if (c === 'L') {
            y -= 1;
        } else if (c === 'R') {
            y += 1;
        } else {
            z += 1;
        }
    }
    return Math.abs(x) + Math.abs(y) + z;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
