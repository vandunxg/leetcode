---
comments: true
difficulty: Easy
rating: 1164
source: Weekly Contest 445 Q1
tags:
    - Math
---

<!-- problem:start -->

# [3516. Find Closest Person](https://leetcode.com/problems/find-closest-person)

[中文文档](/solution/3500-3599/3516.Find%20Closest%20Person/README.md)

## Mô tả

<!-- description:start -->

<p data-end="116" data-start="0">Bạn được cho ba số nguyên <code data-end="33" data-start="30">x</code>, <code data-end="38" data-start="35">y</code> và <code data-end="47" data-start="44">z</code>, đại diện cho vị trí của ba người trên một trục số:</p>

<ul data-end="252" data-start="118">
	<li data-end="154" data-start="118"><code data-end="123" data-start="120">x</code> là vị trí của Người 1.</li>
	<li data-end="191" data-start="155"><code data-end="160" data-start="157">y</code> là vị trí của Người 2.</li>
	<li data-end="252" data-start="192"><code data-end="197" data-start="194">z</code> là vị trí của Người 3, người <strong>không</strong> di chuyển.</li>
</ul>

<p data-end="322" data-start="254">Người 1 và Người 2 cùng di chuyển về phía Người 3 với <strong>cùng</strong> tốc độ.</p>

<p data-end="372" data-start="324">Hãy xác định người đến Người 3 <strong>trước</strong>:</p>

<ul data-end="505" data-start="374">
	<li data-end="415" data-start="374">Trả về 1 nếu Người 1 đến trước.</li>
	<li data-end="457" data-start="416">Trả về 2 nếu Người 2 đến trước.</li>
	<li data-end="505" data-start="458">Trả về 0 nếu cả hai đến cùng <strong>một</strong> lúc.</li>
</ul>

<p data-end="537" data-is-last-node="" data-is-only-node="" data-start="507">Trả về kết quả tương ứng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">x = 2, y = 7, z = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul data-end="258" data-start="113">
	<li data-end="193" data-start="113">Người 1 ở vị trí 2 và có thể đến Người 3 (ở vị trí 4) sau 2 bước.</li>
	<li data-end="258" data-start="194">Người 2 ở vị trí 7 và có thể đến Người 3 sau 3 bước.</li>
</ul>

<p data-end="317" data-is-last-node="" data-is-only-node="" data-start="260">Vì Người 1 đến Người 3 trước, kết quả là 1.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">x = 2, y = 5, z = 6</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul data-end="245" data-start="92">
	<li data-end="174" data-start="92">Người 1 ở vị trí 2 và có thể đến Người 3 (ở vị trí 6) sau 4 bước.</li>
	<li data-end="245" data-start="175">Người 2 ở vị trí 5 và có thể đến Người 3 sau 1 bước.</li>
</ul>

<p data-end="304" data-is-last-node="" data-is-only-node="" data-start="247">Vì Người 2 đến Người 3 trước, kết quả là 2.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">x = 1, y = 5, z = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<ul data-end="245" data-start="92">
	<li data-end="174" data-start="92">Người 1 ở vị trí 1 và có thể đến Người 3 (ở vị trí 3) sau 2 bước.</li>
	<li data-end="245" data-start="175">Người 2 ở vị trí 5 và có thể đến Người 3 sau 2 bước.</li>
</ul>

<p data-end="304" data-is-last-node="" data-is-only-node="" data-start="247">Vì Người 1 và Người 2 đến Người 3 cùng lúc, kết quả là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= x, y, z &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Vì ba người di chuyển với cùng tốc độ trên trục số, người đến $z$ trước được quyết định bởi khoảng cách. Ta so sánh $|x-z|$ và $|y-z|$.
>
> Không cần mô phỏng theo thời gian; đáp án là $0$, $1$ hoặc $2$ và có thể tính trong thời gian hằng số.

<!-- thinking:end -->

Ta tính khoảng cách $a$ giữa Người 1 và Người 3, cùng khoảng cách $b$ giữa Người 2 và Người 3.

- Nếu $a = b$, nghĩa là cả hai người đến cùng lúc, trả về $0$;
- Nếu $a \lt b$, nghĩa là Người 1 sẽ đến trước, trả về $1$;
- Ngược lại, nghĩa là Người 2 sẽ đến trước, trả về $2$.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findClosest(self, x: int, y: int, z: int) -> int:
        a = abs(x - z)
        b = abs(y - z)
        return 0 if a == b else (1 if a < b else 2)
```

#### Java

```java
class Solution {
    public int findClosest(int x, int y, int z) {
        int a = Math.abs(x - z);
        int b = Math.abs(y - z);
        return a == b ? 0 : (a < b ? 1 : 2);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findClosest(int x, int y, int z) {
        int a = abs(x - z);
        int b = abs(y - z);
        return a == b ? 0 : (a < b ? 1 : 2);
    }
};
```

#### Go

```go
func findClosest(x int, y int, z int) int {
	a, b := abs(x-z), abs(y-z)
	if a == b {
		return 0
	}
	if a < b {
		return 1
	}
	return 2
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
function findClosest(x: number, y: number, z: number): number {
    const a = Math.abs(x - z);
    const b = Math.abs(y - z);
    return a === b ? 0 : a < b ? 1 : 2;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_closest(x: i32, y: i32, z: i32) -> i32 {
        let a = (x - z).abs();
        let b = (y - z).abs();
        if a == b {
            0
        } else if a < b {
            1
        } else {
            2
        }
    }
}
```

#### JavaScript

```js
/**
 * @param {number} x
 * @param {number} y
 * @param {number} z
 * @return {number}
 */
var findClosest = function (x, y, z) {
    const a = Math.abs(x - z);
    const b = Math.abs(y - z);
    return a === b ? 0 : a < b ? 1 : 2;
};
```

#### C#

```cs
public class Solution {
    public int FindClosest(int x, int y, int z) {
        int a = Math.Abs(x - z);
        int b = Math.Abs(y - z);
        return a == b ? 0 : (a < b ? 1 : 2);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
