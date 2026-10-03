---
comments: true
difficulty: Easy
rating: 1328
source: Biweekly Contest 49 Q1
tags:
    - Math
    - String
---

<!-- problem:start -->

# [1812. Determine Color of a Chessboard Square](https://leetcode.com/problems/determine-color-of-a-chessboard-square)

[中文文档](/solution/1800-1899/1812.Determine%20Color%20of%20a%20Chessboard%20Square/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>coordinates</code>, một chuỗi biểu diễn tọa độ của một ô trên bàn cờ. Bàn cờ được cung cấp bên dưới để tham khảo.</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1800-1899/1812.Determine%20Color%20of%20a%20Chessboard%20Square/images/screenshot-2021-02-20-at-22159-pm.png" style="width: 400px; height: 396px;" /></p>

<p>Trả về <code>true</code><em> nếu ô đó màu trắng và </em><code>false</code><em> nếu ô đó màu đen</em>.</p>

<p>Tọa độ luôn biểu diễn một ô hợp lệ trên bàn cờ. Tọa độ luôn có chữ cái đứng trước và chữ số đứng sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> coordinates = &quot;a1&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Theo bàn cờ ở trên, ô có tọa độ &quot;a1&quot; màu đen, nên trả về false.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> coordinates = &quot;h3&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Theo bàn cờ ở trên, ô có tọa độ &quot;h3&quot; màu trắng, nên trả về true.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> coordinates = &quot;c7&quot;
<strong>Đầu ra:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>coordinates.length == 2</code></li>
	<li><code>&#39;a&#39; &lt;= coordinates[0] &lt;= &#39;h&#39;</code></li>
	<li><code>&#39;1&#39; &lt;= coordinates[1] &lt;= &#39;8&#39;</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nhận dạng quy luật

<!-- thinking:start -->

> **Tư duy**
>
> Các ô trên bàn cờ luân phiên màu. Với một truy vấn, không cần dựng toàn bộ bàn cờ.
>
> Hai ô kề nhau có màu đối nhau, đúng bằng tính chẵn lẻ của tổng chỉ số cột và hàng. Chuyển chữ cái và chữ số thành số nguyên, rồi kiểm tra tổng của chúng là lẻ (màu trắng) hay chẵn (màu đen).

<!-- thinking:end -->

Quan sát bàn cờ, ta thấy hai ô $(x_1, y_1)$ và $(x_2, y_2)$ cùng màu khi cả $x_1 + y_1$ và $x_2 + y_2$ đều lẻ hoặc đều chẵn.

Vì vậy, ta lấy tọa độ tương ứng $(x, y)$ từ $\textit{coordinates}$. Nếu $x + y$ là số lẻ, ô đó màu trắng và ta trả về $\textit{true}$; ngược lại, ta trả về $\textit{false}$.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def squareIsWhite(self, coordinates: str) -> bool:
        return (ord(coordinates[0]) + ord(coordinates[1])) % 2 == 1
```

#### Java

```java
class Solution {
    public boolean squareIsWhite(String coordinates) {
        return (coordinates.charAt(0) + coordinates.charAt(1)) % 2 == 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool squareIsWhite(string coordinates) {
        return (coordinates[0] + coordinates[1]) % 2;
    }
};
```

#### Go

```go
func squareIsWhite(coordinates string) bool {
	return (coordinates[0]+coordinates[1])%2 == 1
}
```

#### TypeScript

```ts
function squareIsWhite(coordinates: string): boolean {
    return ((coordinates.charCodeAt(0) + coordinates.charCodeAt(1)) & 1) === 1;
}
```

#### Rust

```rust
impl Solution {
    pub fn square_is_white(coordinates: String) -> bool {
        let s = coordinates.as_bytes();
        ((s[0] + s[1]) & 1) == 1
    }
}
```

#### JavaScript

```js
/**
 * @param {string} coordinates
 * @return {boolean}
 */
var squareIsWhite = function (coordinates) {
    return (coordinates[0].charCodeAt() + coordinates[1].charCodeAt()) % 2 == 1;
};
```

#### C

```c
bool squareIsWhite(char* coordinates) {
    return (coordinates[0] + coordinates[1]) & 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
