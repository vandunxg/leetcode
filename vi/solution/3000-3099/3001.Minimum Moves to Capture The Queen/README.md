---
comments: true
difficulty: Medium
rating: 1796
source: Weekly Contest 379 Q2
tags:
    - Math
    - Enumeration
---

<!-- problem:start -->

# [3001. Minimum Moves to Capture The Queen](https://leetcode.com/problems/minimum-moves-to-capture-the-queen)

[中文文档](/solution/3000-3099/3001.Minimum%20Moves%20to%20Capture%20The%20Queen/README.md)

## Mô tả

<!-- description:start -->

<p>Có một bàn cờ vua <strong>được đánh số từ 1</strong>, kích thước <code>8 x 8</code>, chứa <code>3</code> quân cờ.</p>

<p>Bạn được cho <code>6</code> số nguyên <code>a</code>, <code>b</code>, <code>c</code>, <code>d</code>, <code>e</code> và <code>f</code>, trong đó:</p>

<ul>
	<li><code>(a, b)</code> là vị trí của xe trắng.</li>
	<li><code>(c, d)</code> là vị trí của tượng trắng.</li>
	<li><code>(e, f)</code> là vị trí của hậu đen.</li>
</ul>

<p>Vì bạn chỉ có thể di chuyển các quân trắng, hãy trả về <em>số nước đi <strong>nhỏ nhất</strong> cần thiết để bắt hậu đen</em>.</p>

<p><strong>Lưu ý</strong> rằng:</p>

<ul>
	<li>Xe có thể di chuyển đến bất kỳ số ô nào theo chiều dọc hoặc chiều ngang, nhưng không thể nhảy qua các quân cờ khác.</li>
	<li>Tượng có thể di chuyển đến bất kỳ số ô nào theo đường chéo, nhưng không thể nhảy qua các quân cờ khác.</li>
	<li>Xe hoặc tượng có thể bắt hậu nếu hậu nằm trên một ô mà quân đó có thể di chuyển tới.</li>
	<li>Hậu không di chuyển.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3001.Minimum%20Moves%20to%20Capture%20The%20Queen/images/ex1.png" style="width: 600px; height: 600px; padding: 10px; background: #fff; border-radius: .5rem;" />
<pre>
<strong>Đầu vào:</strong> a = 1, b = 1, c = 8, d = 8, e = 2, f = 3
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta có thể bắt hậu đen trong hai nước đi bằng cách di chuyển xe trắng đến (1, 3), sau đó đến (2, 3).
Không thể bắt hậu đen trong ít hơn hai nước đi vì ban đầu không có quân nào đang tấn công hậu.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3001.Minimum%20Moves%20to%20Capture%20The%20Queen/images/ex2.png" style="width: 600px; height: 600px;padding: 10px; background: #fff; border-radius: .5rem;" />
<pre>
<strong>Đầu vào:</strong> a = 5, b = 3, c = 3, d = 4, e = 5, f = 2
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Ta có thể bắt hậu đen trong một nước đi bằng một trong các cách sau:
- Di chuyển xe trắng đến (5, 2).
- Di chuyển tượng trắng đến (5, 2).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= a, b, c, d, e, f &lt;= 8</code></li>
	<li>Không có hai quân cờ nào nằm trên cùng một ô.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân tích trường hợp

<!-- thinking:start -->

> **Tư duy**
>
> Bàn cờ có kích thước $8 \times 8$, nên việc mô phỏng đường đi của xe và tượng chỉ mất thời gian hằng số. Mỗi quân cờ có thể đi đến mọi ô nằm trên cùng hàng, cột hoặc đường chéo trong nhiều nhất một nước đi, vì vậy đáp án không vượt quá $2$.
>
> Các trường hợp có thể bắt trong một nước đi duy nhất là khi đường đi theo hàng hoặc cột của xe, hoặc đường chéo của tượng đến hậu không bị cản.
>
> Các tích như $(d-b)(d-f)>0$ kiểm tra xem quân cờ chắn đường có nằm ngoài đoạn mở hay không. Nếu cả bốn đường thẳng đều bị cản, xe sẽ bắt hậu trong hai nước đi.

<!-- thinking:end -->

Theo mô tả bài toán, ta có thể phân loại các trường hợp bắt hậu đen như sau:

1. Xe trắng và hậu đen nằm trên cùng một hàng, không có quân cờ nào ở giữa. Khi đó, xe chỉ cần di chuyển một lần.
2. Xe trắng và hậu đen nằm trên cùng một cột, không có quân cờ nào ở giữa. Khi đó, xe chỉ cần di chuyển một lần.
3. Tượng trắng và hậu đen nằm trên cùng đường chéo `\`, không có quân cờ nào ở giữa. Khi đó, tượng chỉ cần di chuyển một lần.
4. Tượng trắng và hậu đen nằm trên cùng đường chéo `/`, không có quân cờ nào ở giữa. Khi đó, tượng chỉ cần di chuyển một lần.
5. Trong các trường hợp khác, chỉ cần hai nước đi.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minMovesToCaptureTheQueen(
        self, a: int, b: int, c: int, d: int, e: int, f: int
    ) -> int:
        if a == e and (c != a or (d - b) * (d - f) > 0):
            return 1
        if b == f and (d != b or (c - a) * (c - e) > 0):
            return 1
        if c - e == d - f and (a - e != b - f or (a - c) * (a - e) > 0):
            return 1
        if c - e == f - d and (a - e != f - b or (a - c) * (a - e) > 0):
            return 1
        return 2
```

#### Java

```java
class Solution {
    public int minMovesToCaptureTheQueen(int a, int b, int c, int d, int e, int f) {
        if (a == e && (c != a || (d - b) * (d - f) > 0)) {
            return 1;
        }
        if (b == f && (d != b || (c - a) * (c - e) > 0)) {
            return 1;
        }
        if (c - e == d - f && (a - e != b - f || (a - c) * (a - e) > 0)) {
            return 1;
        }
        if (c - e == f - d && (a - e != f - b || (a - c) * (a - e) > 0)) {
            return 1;
        }
        return 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minMovesToCaptureTheQueen(int a, int b, int c, int d, int e, int f) {
        if (a == e && (c != a || (d - b) * (d - f) > 0)) {
            return 1;
        }
        if (b == f && (d != b || (c - a) * (c - e) > 0)) {
            return 1;
        }
        if (c - e == d - f && (a - e != b - f || (a - c) * (a - e) > 0)) {
            return 1;
        }
        if (c - e == f - d && (a - e != f - b || (a - c) * (a - e) > 0)) {
            return 1;
        }
        return 2;
    }
};
```

#### Go

```go
func minMovesToCaptureTheQueen(a int, b int, c int, d int, e int, f int) int {
	if a == e && (c != a || (d-b)*(d-f) > 0) {
		return 1
	}
	if b == f && (d != b || (c-a)*(c-e) > 0) {
		return 1
	}
	if c-e == d-f && (a-e != b-f || (a-c)*(a-e) > 0) {
		return 1
	}
	if c-e == f-d && (a-e != f-b || (a-c)*(a-e) > 0) {
		return 1
	}
	return 2
}
```

#### TypeScript

```ts
function minMovesToCaptureTheQueen(
    a: number,
    b: number,
    c: number,
    d: number,
    e: number,
    f: number,
): number {
    if (a === e && (c !== a || (d - b) * (d - f) > 0)) {
        return 1;
    }
    if (b === f && (d !== b || (c - a) * (c - e) > 0)) {
        return 1;
    }
    if (c - e === d - f && (a - e !== b - f || (a - c) * (a - e) > 0)) {
        return 1;
    }
    if (c - e === f - d && (a - e !== f - b || (a - c) * (a - e) > 0)) {
        return 1;
    }
    return 2;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_moves_to_capture_the_queen(a: i32, b: i32, c: i32, d: i32, e: i32, f: i32) -> i32 {
        if a == e && (c != a || (d - b) * (d - f) > 0) {
            return 1;
        }
        if b == f && (d != b || (c - a) * (c - e) > 0) {
            return 1;
        }
        if c - e == d - f && (a - e != b - f || (a - c) * (a - e) > 0) {
            return 1;
        }
        if c - e == f - d && (a - e != f - b || (a - c) * (a - e) > 0) {
            return 1;
        }
        return 2;
    }
}
```

#### Cangjie

```cj
class Solution {
    func minMovesToCaptureTheQueen(a: Int64, b: Int64, c: Int64, d: Int64, e: Int64, f: Int64): Int64 {
        if (a == e && (c != a || (d - b) * (d - f) > 0)) {
            return 1
        }
        if (b == f && (d != b || (c - a) * (c - e) > 0)) {
            return 1
        }
        if (c - e == d - f && (a - e != b - f || (a - c) * (a - e) > 0)) {
            return 1
        }
        if (c - e == f - d && (a - e != f - b || (a - c) * (a - e) > 0)) {
            return 1
        }
        2
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
