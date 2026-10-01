---
comments: true
difficulty: Easy
tags:
    - Brainteaser
    - Minimax
    - Math
    - Game Theory
    - Nim Game
    - Impartial Game
---

<!-- problem:start -->

# [292. Nim Game](https://leetcode.com/problems/nim-game)

[中文文档](/solution/0200-0299/0292.Nim%20Game/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn đang chơi trò Nim sau với một người bạn:</p>

<ul>
	<li>Ban đầu, trên bàn có một đống đá.</li>
	<li>Bạn và người bạn lần lượt chơi, và <strong>bạn đi trước</strong>.</li>
	<li>Trong mỗi lượt, người chơi sẽ lấy từ 1 đến 3 viên đá khỏi đống.</li>
	<li>Ai lấy viên đá cuối cùng sẽ thắng.</li>
</ul>

<p>Cho <code>n</code> là số viên đá trong đống. Hãy trả về <code>true</code><em> nếu bạn có thể thắng khi giả sử cả hai đều chơi tối ưu; nếu không thì trả về </em><code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Các kết quả có thể xảy ra:
1. Bạn lấy 1 viên đá. Người bạn lấy 3 viên, bao gồm cả viên cuối cùng, và thắng.
2. Bạn lấy 2 viên đá. Người bạn lấy 2 viên, bao gồm cả viên cuối cùng, và thắng.
3. Bạn lấy 3 viên đá. Người bạn lấy viên cuối cùng và thắng.
Trong mọi trường hợp, người bạn đều thắng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1
<strong>Đầu ra:</strong> true
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> true
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm quy luật

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lượt lấy từ $1$ đến $3$ viên đá. Không thể lấy hết một số đá là bội của $4$ chỉ trong một lượt, và người chơi sau luôn có thể đưa số đá còn lại về một bội của $4$. Người đi trước thắng khi và chỉ khi $n$ không chia hết cho $4$.

<!-- thinking:end -->

Người chơi đến lượt khi số đá còn lại là bội của $4$ (tức $n$ chia hết cho $4$) sẽ thua.

Chứng minh:

1. Khi $n \lt 4$, người đi trước có thể lấy hết đá ngay nên sẽ thắng.
1. Khi $n = 4$, dù người đi trước lấy $1, 2$ hay $3$ viên, người đi sau luôn có thể lấy số viên còn lại và người đi trước sẽ thua.
1. Khi $4 \lt n \lt 8$, tức $n = 5, 6, 7$, người đi trước có thể lấy số viên phù hợp để đưa số đá còn lại về $4$. Khi đó, người đi sau phải đối mặt với mốc thua $4$ và sẽ thua.
1. Khi $n = 8$, dù người đi trước lấy $1, 2$ hay $3$ viên, số đá để lại cho người đi sau sẽ nằm trong khoảng $4 \lt n \lt 8$, nên người đi trước sẽ thua.
1. ...
1. Theo quy nạp, khi đến lượt một người chơi mà số đá còn lại là $n$ và $n$ chia hết cho $4$, người đó sẽ thua; nếu không, người đó sẽ thắng.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canWinNim(self, n: int) -> bool:
        return n % 4 != 0
```

#### Java

```java
class Solution {
    public boolean canWinNim(int n) {
        return n % 4 != 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canWinNim(int n) {
        return n % 4 != 0;
    }
};
```

#### Go

```go
func canWinNim(n int) bool {
	return n%4 != 0
}
```

#### TypeScript

```ts
function canWinNim(n: number): boolean {
    return n % 4 != 0;
}
```

#### Rust

```rust
impl Solution {
    pub fn can_win_nim(n: i32) -> bool {
        n % 4 != 0
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
