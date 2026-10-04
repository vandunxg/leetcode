---
comments: true
difficulty: Medium
rating: 1517
source: Weekly Contest 345 Q2
tags:
    - Bit Manipulation
    - Array
---

<!-- problem:start -->

# [2683. Neighboring Bitwise XOR](https://leetcode.com/problems/neighboring-bitwise-xor)

[中文文档](/solution/2600-2699/2683.Neighboring%20Bitwise%20XOR/README.md)

## Mô tả

<!-- description:start -->

<p>Một mảng <code>derived</code> <strong>được đánh chỉ số từ 0</strong> có độ dài <code>n</code> được tạo ra bằng cách tính phép <strong>XOR bit</strong>&nbsp;(&oplus;) của các giá trị kề nhau trong một <strong>mảng nhị phân</strong> <code>original</code> có độ dài <code>n</code>.</p>

<p>Cụ thể, với mỗi chỉ số <code>i</code> trong phạm vi <code>[0, n - 1]</code>:</p>

<ul>
	<li>Nếu <code>i = n - 1</code>, thì <code>derived[i] = original[i] &oplus; original[0]</code>.</li>
	<li>Nếu không, <code>derived[i] = original[i] &oplus; original[i + 1]</code>.</li>
</ul>

<p>Cho một mảng <code>derived</code>, nhiệm vụ của bạn là xác định xem có tồn tại một <strong>mảng nhị phân hợp lệ</strong> <code>original</code> có thể tạo ra <code>derived</code> hay không.</p>

<p>Trả về <em><strong>true</strong> nếu tồn tại một mảng như vậy, hoặc <strong>false</strong> nếu không.</em></p>

<ul>
	<li>Mảng nhị phân là một mảng chỉ chứa các giá trị <strong>0&#39;s</strong> và <strong>1&#39;s</strong></li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> derived = [1,1,0]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Một mảng original hợp lệ tạo ra derived là [0,1,0].
derived[0] = original[0] &oplus; original[1] = 0 &oplus; 1 = 1
derived[1] = original[1] &oplus; original[2] = 1 &oplus; 0 = 1
derived[2] = original[2] &oplus; original[0] = 0 &oplus; 0 = 0
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> derived = [1,1]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Một mảng original hợp lệ tạo ra derived là [0,1].
derived[0] = original[0] &oplus; original[1] = 1
derived[1] = original[1] &oplus; original[0] = 1
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> derived = [1,0]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không tồn tại mảng original hợp lệ nào tạo ra derived.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == derived.length</code></li>
	<li><code>1 &lt;= n&nbsp;&lt;= 10<sup>5</sup></code></li>
	<li>Các giá trị trong <code>derived</code>&nbsp;là <strong>0&#39;s</strong> hoặc <strong>1&#39;s</strong></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phép toán bit

<!-- thinking:start -->

> **Tư duy**
>
> $derived[i]=a[i]\oplus a[i+1]$ có thể được tạo ra từ một mảng $a$ hoặc không. Việc khôi phục từ $a[0]$ vẫn đủ nhanh với $n \le 10^5$, nhưng mỗi $a_i$ xuất hiện hai lần trong phép XOR của $derived$, nên điều kiện cần và đủ là $\bigoplus derived=0$.
>
> Chỉ cần thực hiện một lần gộp XOR là quyết định được.

<!-- thinking:end -->

Giả sử mảng nhị phân ban đầu là $a$, còn mảng được tạo ra là $b$. Khi đó:

$$
b_0 = a_0 \oplus a_1 \\
b_1 = a_1 \oplus a_2 \\
\cdots \\
b_{n-1} = a_{n-1} \oplus a_0
$$

Vì phép XOR có tính giao hoán và kết hợp, ta có:

$$
b_0 \oplus b_1 \oplus \cdots \oplus b_{n-1} = (a_0 \oplus a_1) \oplus (a_1 \oplus a_2) \oplus \cdots \oplus (a_{n-1} \oplus a_0) = 0
$$

Do đó, chỉ cần tổng XOR của tất cả phần tử trong mảng derived bằng $0$ thì chắc chắn tồn tại một mảng nhị phân ban đầu thỏa mãn yêu cầu.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def doesValidArrayExist(self, derived: List[int]) -> bool:
        return reduce(xor, derived) == 0
```

#### Java

```java
class Solution {
    public boolean doesValidArrayExist(int[] derived) {
        int s = 0;
        for (int x : derived) {
            s ^= x;
        }
        return s == 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool doesValidArrayExist(vector<int>& derived) {
        int s = 0;
        for (int x : derived) {
            s ^= x;
        }
        return s == 0;
    }
};
```

#### Go

```go
func doesValidArrayExist(derived []int) bool {
	s := 0
	for _, x := range derived {
		s ^= x
	}
	return s == 0
}
```

#### TypeScript

```ts
function doesValidArrayExist(derived: number[]): boolean {
    return derived.reduce((acc, x) => acc ^ x) === 0;
}
```

#### Rust

```rust
impl Solution {
    pub fn does_valid_array_exist(derived: Vec<i32>) -> bool {
        derived.iter().fold(0, |acc, &x| acc ^ x) == 0
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} derived
 * @return {boolean}
 */
var doesValidArrayExist = function (derived) {
    return derived.reduce((acc, x) => acc ^ x) === 0;
};
```

#### C#

```cs
public class Solution {
    public bool DoesValidArrayExist(int[] derived) {
        return derived.Aggregate(0, (acc, x) => acc ^ x) == 0;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
