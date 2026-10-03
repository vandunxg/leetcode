---
comments: true
difficulty: Easy
rating: 1187
source: Weekly Contest 273 Q1
tags:
    - Math
---

<!-- problem:start -->

# [2119. A Number After a Double Reversal](https://leetcode.com/problems/a-number-after-a-double-reversal)

[中文文档](/solution/2100-2199/2119.A%20Number%20After%20a%20Double%20Reversal/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Đảo ngược</strong> một số nguyên nghĩa là đảo ngược tất cả các chữ số của số đó.</p>

<ul>
	<li>Ví dụ, đảo ngược <code>2021</code> cho ta <code>1202</code>. Đảo ngược <code>12300</code> cho ta <code>321</code> vì <strong>các số 0 ở đầu không được giữ lại</strong>.</li>
</ul>

<p>Cho một số nguyên <code>num</code>, hãy <strong>đảo ngược</strong> <code>num</code> để nhận được <code>reversed1</code>, <strong>sau đó đảo ngược</strong> <code>reversed1</code> để nhận được <code>reversed2</code>. Trả về <code>true</code> <em>nếu</em> <code>reversed2</code> <em>bằng</em> <code>num</code>. Ngược lại, trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 526
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Đảo ngược num để nhận được 625, sau đó đảo ngược 625 để nhận được 526, bằng với num.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 1800
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Đảo ngược num để nhận được 81, sau đó đảo ngược 81 để nhận được 18, không bằng num.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 0
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Đảo ngược num để nhận được 0, sau đó đảo ngược 0 để nhận được 0, bằng với num.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= num &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Khi đảo ngược một số nguyên, các số 0 ở đầu sẽ bị loại bỏ. Vì vậy, đảo ngược lần hai sẽ trả về số ban đầu khi và chỉ khi lần đảo ngược đầu tiên không loại bỏ các số 0 ở cuối. Ta có thể thực hiện cả hai lần đảo ngược, nhưng quy tắc về chữ số cho ta câu trả lời ngay lập tức.
>
> Số 0 vẫn giữ nguyên. Với các số khác 0, lần đảo ngược đầu tiên làm mất các số 0 ở cuối khi và chỉ khi $num$ chia hết cho $10$, tức là chữ số cuối cùng của nó bằng $0$.
>
> Do đó, kết quả là true khi và chỉ khi $num=0$ hoặc $num\bmod 10\neq 0$.

<!-- thinking:end -->

Nếu số đó bằng $0$, hoặc chữ số cuối cùng của số đó khác $0$, thì số nhận được sau khi đảo ngược hai lần sẽ giống với số ban đầu.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isSameAfterReversals(self, num: int) -> bool:
        return num == 0 or num % 10 != 0
```

#### Java

```java
class Solution {
    public boolean isSameAfterReversals(int num) {
        return num == 0 || num % 10 != 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isSameAfterReversals(int num) {
        return num == 0 || num % 10 != 0;
    }
};
```

#### Go

```go
func isSameAfterReversals(num int) bool {
	return num == 0 || num%10 != 0
}
```

#### TypeScript

```ts
function isSameAfterReversals(num: number): boolean {
    return num === 0 || num % 10 !== 0;
}
```

#### Rust

```rust
impl Solution {
    pub fn is_same_after_reversals(num: i32) -> bool {
        num == 0 || num % 10 != 0
    }
}
```

#### JavaScript

```js
/**
 * @param {number} num
 * @return {boolean}
 */
var isSameAfterReversals = function (num) {
    return num === 0 || num % 10 !== 0;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
