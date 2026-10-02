---
comments: true
difficulty: Easy
rating: 1435
source: Weekly Contest 132 Q1
tags:
    - Brainteaser
    - Math
    - Dynamic Programming
    - Game Theory
    - Impartial Game
---

<!-- problem:start -->

# [1025. Divisor Game](https://leetcode.com/problems/divisor-game)

[中文文档](/solution/1000-1099/1025.Divisor%20Game/README.md)

## Mô tả

<!-- description:start -->

<p>Alice và Bob lần lượt chơi một trò chơi, Alice đi trước.</p>

<p>Ban đầu, trên bảng có số <code>n</code>. Trong lượt của mình, người chơi thực hiện một nước đi gồm:</p>

<ul>
	<li>Chọn một số nguyên <code>x</code> thỏa mãn <code>0 &lt; x &lt; n</code> và <code>n % x == 0</code>.</li>
	<li>Thay số <code>n</code> trên bảng bằng <code>n - x</code>.</li>
</ul>

<p>Người chơi không thể thực hiện nước đi sẽ thua.</p>

<p>Trả về <code>true</code> <em>khi và chỉ khi Alice thắng, giả sử cả hai người chơi đều chơi tối ưu</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Alice chọn 1, Bob không còn nước đi nào.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Alice chọn 1, Bob chọn 1, Alice không còn nước đi nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy nạp toán học

<!-- thinking:start -->

> **Tư duy**
>
> Với $n\le 1000$, ta có thể dùng quy hoạch động cho từng giá trị còn lại: người đi trước thắng nếu chọn được $x$ khiến đối thủ rơi vào thế thua. Các trường hợp nhỏ cho thấy $n$ lẻ thì người đi trước thua, còn $n$ chẵn thì thắng; quy nạp sẽ chứng minh điều này.
>
> Ước thực sự của một số lẻ luôn là số lẻ, nên đối thủ nhận được một số chẵn. Với số chẵn, ta có thể trừ đi $1$ để đối thủ nhận số lẻ. Kết quả chỉ phụ thuộc vào tính chẵn lẻ của $n$.
>
> Vì vậy, chỉ cần kiểm tra $n$ có chẵn hay không.

<!-- thinking:end -->

- Khi $n=1$, người chơi đầu tiên thua.
- Khi $n=2$, người chơi đầu tiên lấy $1$, để lại $1$; người chơi thứ hai thua, nên người đầu tiên thắng.
- Khi $n=3$, người chơi đầu tiên lấy $1$, để lại $2$; người chơi thứ hai thắng, nên người đầu tiên thua.
- Khi $n=4$, người chơi đầu tiên lấy $1$, để lại $3$; người chơi thứ hai thua, nên người đầu tiên thắng.
- ...

Ta dự đoán rằng nếu $n$ lẻ thì người chơi đầu tiên thua, còn nếu $n$ chẵn thì người chơi đầu tiên thắng.

Chứng minh:

1. Nếu $n=1$ hoặc $n=2$, kết luận đúng.
1. Nếu $n \gt 2$, giả sử kết luận đúng với $n \le k$. Xét trường hợp $n=k+1$:
    - Nếu $k+1$ là số lẻ, vì $x$ là ước của $k+1$ nên $x$ chỉ có thể là số lẻ. Do đó $k+1-x$ là số chẵn, người chơi thứ hai thắng và người chơi đầu tiên thua.
    - Nếu $k+1$ là số chẵn, $x$ có thể là số lẻ (bằng 1) hoặc số chẵn. Nếu $x$ là số lẻ, $k+1-x$ là số lẻ; người chơi thứ hai thua, nên người chơi đầu tiên thắng.

Tóm lại, nếu $n$ lẻ thì người chơi đầu tiên thua; nếu $n$ chẵn thì người chơi đầu tiên thắng. Kết luận được chứng minh.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def divisorGame(self, n: int) -> bool:
        return n % 2 == 0
```

#### Java

```java
class Solution {
    public boolean divisorGame(int n) {
        return n % 2 == 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool divisorGame(int n) {
        return n % 2 == 0;
    }
};
```

#### Go

```go
func divisorGame(n int) bool {
	return n%2 == 0
}
```

#### JavaScript

```js
var divisorGame = function (n) {
    return n % 2 === 0;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
