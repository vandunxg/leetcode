---
comments: true
difficulty: Easy
rating: 1165
source: Weekly Contest 259 Q1
tags:
    - Array
    - String
    - Simulation
---

<!-- problem:start -->

# [2011. Final Value of Variable After Performing Operations](https://leetcode.com/problems/final-value-of-variable-after-performing-operations)

[中文文档](/solution/2000-2099/2011.Final%20Value%20of%20Variable%20After%20Performing%20Operations/README.md)

## Mô tả

<!-- description:start -->

<p>Có một ngôn ngữ lập trình chỉ có <strong>bốn</strong> phép toán và <strong>một</strong> biến <code>X</code>:</p>

<ul>
	<li><code>++X</code> và <code>X++</code> <strong>tăng</strong> giá trị của biến <code>X</code> thêm <code>1</code>.</li>
	<li><code>--X</code> và <code>X--</code> <strong>giảm</strong> giá trị của biến <code>X</code> đi <code>1</code>.</li>
</ul>

<p>Ban đầu, giá trị của <code>X</code> là <code>0</code>.</p>

<p>Cho một mảng chuỗi <code>operations</code> chứa danh sách các phép toán, hãy trả về <em><strong>giá trị cuối cùng</strong> của </em><code>X</code> <em>sau khi thực hiện tất cả các phép toán</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> operations = [&quot;--X&quot;,&quot;X++&quot;,&quot;X++&quot;]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>&nbsp;Các phép toán được thực hiện như sau:
Ban đầu, X = 0.
--X: X giảm đi 1, X =  0 - 1 = -1.
X++: X tăng thêm 1, X = -1 + 1 =  0.
X++: X tăng thêm 1, X =  0 + 1 =  1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> operations = [&quot;++X&quot;,&quot;++X&quot;,&quot;X++&quot;]
<strong>Đầu ra:</strong> 3
<strong>Giải thích: </strong>Các phép toán được thực hiện như sau:
Ban đầu, X = 0.
++X: X tăng thêm 1, X = 0 + 1 = 1.
++X: X tăng thêm 1, X = 1 + 1 = 2.
X++: X tăng thêm 1, X = 2 + 1 = 3.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> operations = [&quot;X++&quot;,&quot;++X&quot;,&quot;--X&quot;,&quot;X--&quot;]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>&nbsp;Các phép toán được thực hiện như sau:
Ban đầu, X = 0.
X++: X tăng thêm 1, X = 0 + 1 = 1.
++X: X tăng thêm 1, X = 1 + 1 = 2.
--X: X giảm đi 1, X = 2 - 1 = 1.
X--: X giảm đi 1, X = 1 - 1 = 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= operations.length &lt;= 100</code></li>
	<li><code>operations[i]</code> sẽ là một trong các giá trị <code>&quot;++X&quot;</code>, <code>&quot;X++&quot;</code>, <code>&quot;--X&quot;</code> hoặc <code>&quot;X--&quot;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ có bốn phép toán và $n \le 100$, nên chỉ cần duyệt tuyến tính. Cả hai dạng tăng đều cộng một, còn cả hai dạng giảm đều trừ một; ký tự ở giữa quyết định dấu.
>
> Cộng $+1$ hoặc $-1$ tùy theo việc $s[1]$ có phải là `'+'` hay không.

<!-- thinking:end -->

Ta duyệt mảng $\textit{operations}$. Với mỗi phép toán $\textit{operations}[i]$, nếu nó chứa `'+'`, ta tăng đáp án thêm $1$; nếu không, ta giảm đáp án đi $1$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{operations}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def finalValueAfterOperations(self, operations: List[str]) -> int:
        return sum(1 if s[1] == '+' else -1 for s in operations)
```

#### Java

```java
class Solution {
    public int finalValueAfterOperations(String[] operations) {
        int ans = 0;
        for (var s : operations) {
            ans += (s.charAt(1) == '+' ? 1 : -1);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int finalValueAfterOperations(vector<string>& operations) {
        int ans = 0;
        for (auto& s : operations) {
            ans += s[1] == '+' ? 1 : -1;
        }
        return ans;
    }
};
```

#### Go

```go
func finalValueAfterOperations(operations []string) (ans int) {
	for _, s := range operations {
		if s[1] == '+' {
			ans += 1
		} else {
			ans -= 1
		}
	}
	return
}
```

#### TypeScript

```ts
function finalValueAfterOperations(operations: string[]): number {
    return operations.reduce((acc, op) => acc + (op[1] === '+' ? 1 : -1), 0);
}
```

#### Rust

```rust
impl Solution {
    pub fn final_value_after_operations(operations: Vec<String>) -> i32 {
        let mut ans = 0;
        for s in operations.iter() {
            ans += if s.as_bytes()[1] == b'+' { 1 } else { -1 };
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {string[]} operations
 * @return {number}
 */
var finalValueAfterOperations = function (operations) {
    return operations.reduce((acc, op) => acc + (op[1] === '+' ? 1 : -1), 0);
};
```

#### C

```c
int finalValueAfterOperations(char** operations, int operationsSize) {
    int ans = 0;
    for (int i = 0; i < operationsSize; i++) {
        ans += operations[i][1] == '+' ? 1 : -1;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
