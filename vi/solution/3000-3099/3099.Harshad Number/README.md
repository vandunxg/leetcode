---
comments: true
difficulty: Easy
rating: 1100
source: Weekly Contest 391 Q1
tags:
    - Math
---

<!-- problem:start -->

# [3099. Harshad Number](https://leetcode.com/problems/harshad-number)

[中文文档](/solution/3000-3099/3099.Harshad%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Một số nguyên chia hết cho <strong>tổng</strong> các chữ số của nó được gọi là số <strong>Harshad</strong>. Cho một số nguyên <code>x</code>. Trả về<em> tổng các chữ số </em>của<em> </em><code>x</code><em> </em>nếu<em> </em><code>x</code><em> </em>là số <strong>Harshad</strong>, nếu không, trả về<em> </em><code>-1</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">x = 18</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">9</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tổng các chữ số của <code>x</code> là <code>9</code>. <code>18</code> chia hết cho <code>9</code>. Vì vậy, <code>18</code> là một số Harshad và đáp án là <code>9</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">x = 23</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tổng các chữ số của <code>x</code> là <code>5</code>. <code>23</code> không chia hết cho <code>5</code>. Vì vậy, <code>23</code> không phải là số Harshad và đáp án là <code>-1</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= x &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Một số Harshad chia hết cho tổng các chữ số của nó; ta trả về tổng đó hoặc $-1$. Vì $x \le 100$, chỉ cần tách các chữ số.
>
> Một biến tạm tích lũy các chữ số để vẫn có thể kiểm tra giá trị $x$ ban đầu với tổng này.

<!-- thinking:end -->

Ta có thể tính tổng các chữ số của $x$, ký hiệu là $s$, bằng cách mô phỏng. Nếu $x$ chia hết cho $s$, ta trả về $s$; nếu không, ta trả về $-1$.

Độ phức tạp thời gian là $O(\log x)$, trong đó $x$ là số nguyên đầu vào. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumOfTheDigitsOfHarshadNumber(self, x: int) -> int:
        s, y = 0, x
        while y:
            s += y % 10
            y //= 10
        return s if x % s == 0 else -1
```

#### Java

```java
class Solution {
    public int sumOfTheDigitsOfHarshadNumber(int x) {
        int s = 0;
        for (int y = x; y > 0; y /= 10) {
            s += y % 10;
        }
        return x % s == 0 ? s : -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int sumOfTheDigitsOfHarshadNumber(int x) {
        int s = 0;
        for (int y = x; y > 0; y /= 10) {
            s += y % 10;
        }
        return x % s == 0 ? s : -1;
    }
};
```

#### Go

```go
func sumOfTheDigitsOfHarshadNumber(x int) int {
	s := 0
	for y := x; y > 0; y /= 10 {
		s += y % 10
	}
	if x%s == 0 {
		return s
	}
	return -1
}
```

#### TypeScript

```ts
function sumOfTheDigitsOfHarshadNumber(x: number): number {
    let s = 0;
    for (let y = x; y; y = Math.floor(y / 10)) {
        s += y % 10;
    }
    return x % s === 0 ? s : -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
