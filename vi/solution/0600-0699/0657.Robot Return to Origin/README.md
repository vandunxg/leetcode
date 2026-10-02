---
comments: true
difficulty: Easy
tags:
    - String
    - Simulation
---

<!-- problem:start -->

# [657. Robot Return to Origin](https://leetcode.com/problems/robot-return-to-origin)

[中文文档](/solution/0600-0699/0657.Robot%20Return%20to%20Origin/README.md)

## Mô tả

<!-- description:start -->

<p>Một robot bắt đầu tại vị trí <code>(0, 0)</code>, tức gốc tọa độ trên mặt phẳng 2D. Cho chuỗi các bước di chuyển của robot, hãy xác định robot có <strong>kết thúc tại </strong><code>(0, 0)</code> sau khi thực hiện xong các bước hay không.</p>

<p>Cho chuỗi <code>moves</code> biểu diễn các bước di chuyển của robot, trong đó <code>moves[i]</code> là bước di chuyển thứ <code>i<sup>th</sup></code>. Các bước hợp lệ là <code>&#39;R&#39;</code> (sang phải), <code>&#39;L&#39;</code> (sang trái), <code>&#39;U&#39;</code> (lên trên) và <code>&#39;D&#39;</code> (xuống dưới).</p>

<p>Trả về <code>true</code><em> nếu robot quay về gốc tọa độ sau khi hoàn tất mọi bước di chuyển, hoặc </em><code>false</code><em> nếu không</em>.</p>

<p><strong>Lưu ý</strong>: Hướng mà robot đang &quot;quay mặt về&quot; không ảnh hưởng đến bài toán. <code>&#39;R&#39;</code> luôn khiến robot di chuyển sang phải một đơn vị, <code>&#39;L&#39;</code> luôn khiến robot di chuyển sang trái, v.v. Giả sử độ dài mỗi bước di chuyển của robot đều như nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> moves = &quot;UD&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích</strong>: Robot di chuyển lên một lần rồi xuống một lần. Vì mỗi bước có cùng độ dài nên robot quay về gốc tọa độ ban đầu. Do đó, ta trả về true.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> moves = &quot;LL&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích</strong>: Robot di chuyển sang trái hai lần, nên kết thúc ở vị trí cách gốc tọa độ hai &quot;bước&quot; về bên trái. Ta trả về false vì sau khi đi xong, robot không ở gốc tọa độ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= moves.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>moves</code> chỉ chứa các ký tự <code>&#39;U&#39;</code>, <code>&#39;D&#39;</code>, <code>&#39;L&#39;</code> và <code>&#39;R&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duy trì tọa độ

<!-- thinking:start -->

> **Tư duy**
>
> Robot quay về điểm xuất phát khi và chỉ khi độ dời tổng cộng bằng 0.
>
> Cập nhật $(x,y)$ theo các bước `UDLR`, rồi kiểm tra cả hai tọa độ sau khi kết thúc.

<!-- thinking:end -->

Ta có thể duy trì tọa độ $(x, y)$ để biểu diễn chuyển động ngang và dọc của robot.

Duyệt chuỗi $\textit{moves}$ và cập nhật tọa độ $(x, y)$ dựa trên ký tự hiện tại:

- Nếu ký tự hiện tại là `'U'`, thì $y$ tăng thêm $1$;
- Nếu ký tự hiện tại là `'D'$, thì $y$ giảm đi $1$;
- Nếu ký tự hiện tại là `'L'$, thì $x$ giảm đi $1$;
- Nếu ký tự hiện tại là `'R'$, thì $x$ tăng thêm $1$.

Cuối cùng, kiểm tra xem cả $x$ và $y$ có bằng $0$ hay không.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi $\textit{moves}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def judgeCircle(self, moves: str) -> bool:
        x = y = 0
        for c in moves:
            match c:
                case "U":
                    y += 1
                case "D":
                    y -= 1
                case "L":
                    x -= 1
                case "R":
                    x += 1
        return x == 0 and y == 0
```

#### Java

```java
class Solution {
    public boolean judgeCircle(String moves) {
        int x = 0, y = 0;
        for (char c : moves.toCharArray()) {
            switch (c) {
                case 'U' -> y++;
                case 'D' -> y--;
                case 'L' -> x--;
                case 'R' -> x++;
            }
        }
        return x == 0 && y == 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool judgeCircle(string moves) {
        int x = 0, y = 0;
        for (char c : moves) {
            switch (c) {
            case 'U': y++; break;
            case 'D': y--; break;
            case 'L': x--; break;
            case 'R': x++; break;
            }
        }
        return x == 0 && y == 0;
    }
};
```

#### Go

```go
func judgeCircle(moves string) bool {
	x, y := 0, 0
	for _, c := range moves {
		switch c {
		case 'U':
			y++
		case 'D':
			y--
		case 'L':
			x--
		case 'R':
			x++
		}
	}
	return x == 0 && y == 0
}
```

#### TypeScript

```ts
function judgeCircle(moves: string): boolean {
    let [x, y] = [0, 0];
    for (const c of moves) {
        if (c === 'U') {
            y++;
        } else if (c === 'D') {
            y--;
        } else if (c === 'L') {
            x--;
        } else {
            x++;
        }
    }
    return x === 0 && y === 0;
}
```

#### Rust

```rust
impl Solution {
    pub fn judge_circle(moves: String) -> bool {
        let mut x: i32 = 0;
        let mut y: i32 = 0;

        for c in moves.chars() {
            match c {
                'U' => y += 1,
                'D' => y -= 1,
                'L' => x -= 1,
                'R' => x += 1,
                _ => {}
            }
        }

        x == 0 && y == 0
    }
}
```

#### JavaScript

```js
/**
 * @param {string} moves
 * @return {boolean}
 */
var judgeCircle = function (moves) {
    let [x, y] = [0, 0];
    for (const c of moves) {
        if (c === 'U') {
            y++;
        } else if (c === 'D') {
            y--;
        } else if (c === 'L') {
            x--;
        } else {
            x++;
        }
    }
    return x === 0 && y === 0;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
