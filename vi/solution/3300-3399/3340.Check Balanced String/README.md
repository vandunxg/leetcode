---
comments: true
difficulty: Easy
rating: 1190
source: Weekly Contest 422 Q1
tags:
    - String
---

<!-- problem:start -->

# [3340. Check Balanced String](https://leetcode.com/problems/check-balanced-string)

[中文文档](/solution/3300-3399/3340.Check%20Balanced%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>num</code> chỉ gồm các chữ số. Một chuỗi chữ số được gọi là <b>cân bằng</b> nếu tổng các chữ số ở chỉ số chẵn bằng tổng các chữ số ở chỉ số lẻ.</p>

<p>Trả về <code>true</code> nếu <code>num</code> là chuỗi <strong>cân bằng</strong>, ngược lại trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> num<span class="example-io"> = &quot;1234&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Tổng các chữ số ở chỉ số chẵn là <code>1 + 3 == 4</code>, còn tổng các chữ số ở chỉ số lẻ là <code>2 + 4 == 6</code>.</li>
    <li>Vì 4 không bằng 6 nên <code>num</code> không cân bằng.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> num<span class="example-io"> = &quot;24123&quot;</span></p>

<p><strong>Đầu ra:</strong> true</p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Tổng các chữ số ở chỉ số chẵn là <code>2 + 1 + 3 == 6</code>, còn tổng các chữ số ở chỉ số lẻ là <code>4 + 2 == 6</code>.</li>
    <li>Vì hai tổng bằng nhau nên <code>num</code> là chuỗi cân bằng.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= num.length &lt;= 100</code></li>
    <li><code><font face="monospace">num</font></code> chỉ gồm các chữ số</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Một chuỗi cân bằng có tổng các chữ số ở chỉ số chẵn và lẻ bằng nhau. Với $n \le 100$, chỉ cần duyệt qua chuỗi một lần.
>
> Một mảng gồm hai phần tử sẽ cộng dồn theo $i \bmod 2$; chuỗi cân bằng khi và chỉ khi hai phần tử bằng nhau.
>
> Chúng ta không cần tạo ra hai chuỗi con; chỉ số chẵn lẻ đã đủ.

<!-- thinking:end -->

Chúng ta có thể sử dụng một mảng $f$ có độ dài $2$ để lưu tổng các chữ số ở chỉ số chẵn và chỉ số lẻ. Sau đó, duyệt qua chuỗi $\textit{nums}$ và cộng các chữ số vào vị trí tương ứng dựa trên tính chẵn lẻ của chỉ số. Cuối cùng, kiểm tra xem $f[0]$ có bằng $f[1]$ hay không.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isBalanced(self, num: str) -> bool:
        f = [0, 0]
        for i, x in enumerate(map(int, num)):
            f[i & 1] += x
        return f[0] == f[1]
```

#### Java

```java
class Solution {
    public boolean isBalanced(String num) {
        int[] f = new int[2];
        for (int i = 0; i < num.length(); ++i) {
            f[i & 1] += num.charAt(i) - '0';
        }
        return f[0] == f[1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isBalanced(string num) {
        int f[2]{};
        for (int i = 0; i < num.size(); ++i) {
            f[i & 1] += num[i] - '0';
        }
        return f[0] == f[1];
    }
};
```

#### Go

```go
func isBalanced(num string) bool {
    f := [2]int{}
    for i, c := range num {
        f[i&1] += int(c - '0')
    }
    return f[0] == f[1]
}
```

#### TypeScript

```ts
function isBalanced(num: string): boolean {
    const f = [0, 0];
    for (let i = 0; i < num.length; ++i) {
        f[i & 1] += +num[i];
    }
    return f[0] === f[1];
}
```

#### Rust

```rust
impl Solution {
    pub fn is_balanced(num: String) -> bool {
        let mut f = [0; 2];
        for (i, x) in num.as_bytes().iter().enumerate() {
            f[i & 1] += (x - b'0') as i32;
        }
        f[0] == f[1]
    }
}
```

#### JavaScript

```js
/**
 * @param {string} num
 * @return {boolean}
 */
var isBalanced = function (num) {
    const f = [0, 0];
    for (let i = 0; i < num.length; ++i) {
        f[i & 1] += +num[i];
    }
    return f[0] === f[1];
};
```

#### C#

```cs
public class Solution {
    public bool IsBalanced(string num) {
        int[] f = new int[2];
        for (int i = 0; i < num.Length; ++i) {
            f[i & 1] += num[i] - '0';
        }
        return f[0] == f[1];
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param String $num
     * @return Boolean
     */
    function isBalanced($num) {
        $f = [0, 0];
        foreach (str_split($num) as $i => $ch) {
            $f[$i & 1] += ord($ch) - 48;
        }
        return $f[0] == $f[1];
    }
}
```

#### Scala

```scala
object Solution {
    def isBalanced(num: String): Boolean = {
        val f = Array(0, 0)
        for (i <- num.indices) {
            f(i & 1) += num(i) - '0'
        }
        f(0) == f(1)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
