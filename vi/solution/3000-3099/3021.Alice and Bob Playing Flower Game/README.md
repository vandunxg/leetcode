---
comments: true
difficulty: Medium
rating: 1581
source: Weekly Contest 382 Q3
tags:
    - Math
---

<!-- problem:start -->

# [3021. Alice and Bob Playing Flower Game](https://leetcode.com/problems/alice-and-bob-playing-flower-game)

[中文文档](/solution/3000-3099/3021.Alice%20and%20Bob%20Playing%20Flower%20Game/README.md)

## Mô tả

<!-- description:start -->

<p>Alice và Bob đang chơi một trò chơi theo lượt trên một cánh đồng, với hai hàng hoa nằm giữa họ. Có <code>x</code> bông hoa ở hàng thứ nhất giữa Alice và Bob, và <code>y</code> bông hoa ở hàng thứ hai giữa họ.</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3021.Alice%20and%20Bob%20Playing%20Flower%20Game/images/3021.png" style="width: 300px; height: 150px;" /></p>

<p>Trò chơi diễn ra như sau:</p>

<ol>
	<li>Alice đi trước.</li>
	<li>Trong mỗi lượt, người chơi phải chọn một trong hai hàng và lấy một bông hoa từ phía đó.</li>
	<li>Kết thúc lượt, nếu cả hai hàng đều không còn bông hoa nào, người chơi <strong>hiện tại</strong> sẽ bắt đối thủ và thắng trò chơi.</li>
</ol>

<p>Cho hai số nguyên <code>n</code> và <code>m</code>, hãy tính số cặp <code>(x, y)</code> có thể thỏa mãn các điều kiện sau:</p>

<ul>
	<li>Alice phải thắng trò chơi theo các luật đã mô tả.</li>
	<li>Số bông hoa <code>x</code> ở hàng thứ nhất phải nằm trong khoảng <code>[1,n]</code>.</li>
	<li>Số bông hoa <code>y</code> ở hàng thứ hai phải nằm trong khoảng <code>[1,m]</code>.</li>
</ul>

<p>Trả về <em>số cặp có thể có</em> <code>(x, y)</code> <em>thỏa mãn các điều kiện được nêu trong đề bài</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, m = 2
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Các cặp thỏa mãn điều kiện được nêu trong đề bài là: (1,2), (3,2), (2,1).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1, m = 1
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có cặp nào thỏa mãn các điều kiện được nêu trong đề bài.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n, m &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi nước đi lấy một bông hoa khỏi một hàng, và người chơi đầu tiên thắng khi và chỉ khi $x+y$ là số lẻ. Vì $n,m \le 10^5$, ta không thể liệt kê mọi ô.
>
> Các cặp lẻ–chẵn và chẵn–lẻ chính là các vị trí chiến thắng. Số lượng số lẻ và số chẵn ở mỗi phía được nhân với nhau rồi cộng lại.
>
> Phép chia nguyên cho ta bốn số lượng đó, sau đó ta nhân chéo chúng.

<!-- thinking:end -->

Theo mô tả đề bài, trong mỗi nước đi, người chơi sẽ chọn di chuyển theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ, sau đó lấy một bông hoa. Vì Alice đi trước, khi $x + y$ là số lẻ, Alice chắc chắn sẽ thắng trò chơi.

Do đó, số bông hoa $x$ và $y$ phải thỏa mãn các điều kiện sau:

1. $x + y$ là số lẻ;
2. $1 \le x \le n$;
3. $1 \le y \le m$.

Nếu $x$ là số lẻ thì $y$ phải là số chẵn. Khi đó, số giá trị của $x$ là $\lceil \frac{n}{2} \rceil$, số giá trị của $y$ là $\lfloor \frac{m}{2} \rfloor$, nên số cặp thỏa mãn điều kiện là $\lceil \frac{n}{2} \rceil \times \lfloor \frac{m}{2} \rfloor$.

Nếu $x$ là số chẵn thì $y$ phải là số lẻ. Khi đó, số giá trị của $x$ là $\lfloor \frac{n}{2} \rfloor$, số giá trị của $y$ là $\lceil \frac{m}{2} \rceil$, nên số cặp thỏa mãn điều kiện là $\lfloor \frac{n}{2} \rfloor \times \lceil \frac{m}{2} \rceil$.

Vì vậy, số cặp thỏa mãn điều kiện là $\lceil \frac{n}{2} \rceil \times \lfloor \frac{m}{2} \rfloor + \lfloor \frac{n}{2} \rfloor \times \lceil \frac{m}{2} \rceil$, tương đương với $\lfloor \frac{n + 1}{2} \rfloor \times \lfloor \frac{m}{2} \rfloor + \lfloor \frac{n}{2} \rfloor \times \lfloor \frac{m + 1}{2} \rfloor$.

Độ phức tạp thời gian là $O(1)$, và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def flowerGame(self, n: int, m: int) -> int:
        a1 = (n + 1) // 2
        b1 = (m + 1) // 2
        a2 = n // 2
        b2 = m // 2
        return a1 * b2 + a2 * b1
```

#### Java

```java
class Solution {
    public long flowerGame(int n, int m) {
        long a1 = (n + 1) / 2;
        long b1 = (m + 1) / 2;
        long a2 = n / 2;
        long b2 = m / 2;
        return a1 * b2 + a2 * b1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long flowerGame(int n, int m) {
        long long a1 = (n + 1) / 2;
        long long b1 = (m + 1) / 2;
        long long a2 = n / 2;
        long long b2 = m / 2;
        return a1 * b2 + a2 * b1;
    }
};
```

#### Go

```go
func flowerGame(n int, m int) int64 {
	a1, b1 := (n+1)/2, (m+1)/2
	a2, b2 := n/2, m/2
	return int64(a1*b2 + a2*b1)
}
```

#### TypeScript

```ts
function flowerGame(n: number, m: number): number {
    const [a1, b1] = [(n + 1) >> 1, (m + 1) >> 1];
    const [a2, b2] = [n >> 1, m >> 1];
    return a1 * b2 + a2 * b1;
}
```

#### Rust

```rust
impl Solution {
    pub fn flower_game(n: i32, m: i32) -> i64 {
        let a1 = ((n + 1) / 2) as i64;
        let b1 = ((m + 1) / 2) as i64;
        let a2 = (n / 2) as i64;
        let b2 = (m / 2) as i64;
        a1 * b2 + a2 * b1
    }
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @param {number} m
 * @return {number}
 */
var flowerGame = function (n, m) {
    const [a1, b1] = [(n + 1) >> 1, (m + 1) >> 1];
    const [a2, b2] = [n >> 1, m >> 1];
    return a1 * b2 + a2 * b1;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Toán học (Tối ưu hóa)

<!-- thinking:start -->

> **Tư duy**
>
> Khai triển tích chéo theo tính chẵn lẻ của $n$ và $m$ cho phép gộp cả bốn trường hợp thành $\lfloor nm/2 \rfloor$.
>
> Vì vậy, chỉ một phép nhân và chia nguyên đã thay thế được công thức gồm bốn hạng tử.

<!-- thinking:end -->

Kết quả thu được từ Lời giải 1 là $\lfloor \frac{n + 1}{2} \rfloor \times \lfloor \frac{m}{2} \rfloor + \lfloor \frac{n}{2} \rfloor \times \lfloor \frac{m + 1}{2} \rfloor$.

Nếu cả $n$ và $m$ đều là số lẻ, kết quả là $\frac{n + 1}{2} \times \frac{m - 1}{2} + \frac{n - 1}{2} \times \frac{m + 1}{2}$, tương đương với $\frac{n \times m - 1}{2}$.

Nếu cả $n$ và $m$ đều là số chẵn, kết quả là $\frac{n}{2} \times \frac{m}{2} + \frac{n}{2} \times \frac{m}{2}$, tương đương với $\frac{n \times m}{2}$.

Nếu $n$ là số lẻ và $m$ là số chẵn, kết quả là $\frac{n + 1}{2} \times \frac{m}{2} + \frac{n - 1}{2} \times \frac{m}{2}$, tương đương với $\frac{n \times m}{2}$.

Nếu $n$ là số chẵn và $m$ là số lẻ, kết quả là $\frac{n}{2} \times \frac{m - 1}{2} + \frac{n}{2} \times \frac{m + 1}{2}$, tương đương với $\frac{n \times m}{2}$.

Bốn trường hợp trên có thể gộp lại thành $\lfloor \frac{n \times m}{2} \rfloor$.

Độ phức tạp thời gian là $O(1)$, và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def flowerGame(self, n: int, m: int) -> int:
        return (n * m) // 2
```

#### Java

```java
class Solution {
    public long flowerGame(int n, int m) {
        return ((long) n * m) / 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long flowerGame(int n, int m) {
        return ((long long) n * m) / 2;
    }
};
```

#### Go

```go
func flowerGame(n int, m int) int64 {
	return int64((n * m) / 2)
}
```

#### TypeScript

```ts
function flowerGame(n: number, m: number): number {
    return Number(((BigInt(n) * BigInt(m)) / 2n) | 0n);
}
```

#### Rust

```rust
impl Solution {
    pub fn flower_game(n: i32, m: i32) -> i64 {
        (n as i64 * m as i64) / 2
    }
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @param {number} m
 * @return {number}
 */
var flowerGame = function (n, m) {
    return Number(((BigInt(n) * BigInt(m)) / 2n) | 0n);
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
