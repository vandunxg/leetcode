---
comments: true
difficulty: Medium
tags:
    - Stack
    - Array
    - Simulation
---

<!-- problem:start -->

# [946. Validate Stack Sequences](https://leetcode.com/problems/validate-stack-sequences)

[中文文档](/solution/0900-0999/0946.Validate%20Stack%20Sequences/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>pushed</code> và <code>popped</code>, mỗi mảng chứa các giá trị phân biệt. Trả về <code>true</code><em> nếu đây có thể là kết quả của một chuỗi thao tác push và pop trên stack ban đầu rỗng, nếu không thì trả về </em><code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Ta có thể thực hiện chuỗi thao tác sau:
push(1), push(2), push(3), push(4),
pop() -&gt; 4,
push(5),
pop() -&gt; 5, pop() -&gt; 3, pop() -&gt; 2, pop() -&gt; 1
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> pushed = [1,2,3,4,5], popped = [4,3,5,1,2]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không thể pop 1 trước 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= pushed.length &lt;= 1000</code></li>
	<li><code>0 &lt;= pushed[i] &lt;= 1000</code></li>
	<li>Tất cả phần tử trong <code>pushed</code> đều <strong>duy nhất</strong>.</li>
	<li><code>popped.length == pushed.length</code></li>
	<li><code>popped</code> là một hoán vị của <code>pushed</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng stack

<!-- thinking:start -->

> **Tư duy**
>
> Kiểm tra xem $\textit{popped}$ có phải là một chuỗi pop hợp lệ tương ứng với $\textit{pushed}$ hay không. Vì các giá trị đều phân biệt, ta push theo thứ tự đã cho và pop mỗi khi phần tử trên cùng trùng với phần tử cần pop tiếp theo. Nếu thực hiện được tất cả thao tác pop thì chuỗi này hợp lệ.

<!-- thinking:end -->

Ta duyệt mảng $\textit{pushed}$. Với mỗi phần tử hiện tại $x$, ta push nó vào stack $\textit{stk}$. Sau đó, kiểm tra xem phần tử trên cùng của stack có bằng phần tử tiếp theo cần pop trong mảng $\textit{popped}$ hay không. Nếu bằng nhau, ta pop phần tử trên cùng khỏi stack và tăng chỉ số $i$ của phần tử tiếp theo cần pop trong mảng $\textit{popped}$. Cuối cùng, nếu có thể pop tất cả phần tử theo thứ tự trong $\textit{popped}$ thì trả về $\textit{true}$; nếu không thì trả về $\textit{false}$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài mảng $\textit{pushed}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def validateStackSequences(self, pushed: List[int], popped: List[int]) -> bool:
        stk = []
        i = 0
        for x in pushed:
            stk.append(x)
            while stk and stk[-1] == popped[i]:
                stk.pop()
                i += 1
        return i == len(popped)
```

#### Java

```java
class Solution {
    public boolean validateStackSequences(int[] pushed, int[] popped) {
        Deque<Integer> stk = new ArrayDeque<>();
        int i = 0;
        for (int x : pushed) {
            stk.push(x);
            while (!stk.isEmpty() && stk.peek() == popped[i]) {
                stk.pop();
                ++i;
            }
        }
        return i == popped.length;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
        stack<int> stk;
        int i = 0;
        for (int x : pushed) {
            stk.push(x);
            while (stk.size() && stk.top() == popped[i]) {
                stk.pop();
                ++i;
            }
        }
        return i == popped.size();
    }
};
```

#### Go

```go
func validateStackSequences(pushed []int, popped []int) bool {
	stk := []int{}
	i := 0
	for _, x := range pushed {
		stk = append(stk, x)
		for len(stk) > 0 && stk[len(stk)-1] == popped[i] {
			stk = stk[:len(stk)-1]
			i++
		}
	}
	return i == len(popped)
}
```

#### TypeScript

```ts
function validateStackSequences(pushed: number[], popped: number[]): boolean {
    const stk: number[] = [];
    let i = 0;
    for (const x of pushed) {
        stk.push(x);
        while (stk.length && stk.at(-1)! === popped[i]) {
            stk.pop();
            i++;
        }
    }
    return i === popped.length;
}
```

#### Rust

```rust
impl Solution {
    pub fn validate_stack_sequences(pushed: Vec<i32>, popped: Vec<i32>) -> bool {
        let mut stk: Vec<i32> = Vec::new();
        let mut i = 0;
        for &x in &pushed {
            stk.push(x);
            while !stk.is_empty() && *stk.last().unwrap() == popped[i] {
                stk.pop();
                i += 1;
            }
        }
        i == popped.len()
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} pushed
 * @param {number[]} popped
 * @return {boolean}
 */
var validateStackSequences = function (pushed, popped) {
    const stk = [];
    let i = 0;
    for (const x of pushed) {
        stk.push(x);
        while (stk.length && stk.at(-1) === popped[i]) {
            stk.pop();
            i++;
        }
    }
    return i === popped.length;
};
```

#### C#

```cs
public class Solution {
    public bool ValidateStackSequences(int[] pushed, int[] popped) {
        Stack<int> stk = new Stack<int>();
        int i = 0;

        foreach (int x in pushed) {
            stk.Push(x);
            while (stk.Count > 0 && stk.Peek() == popped[i]) {
                stk.Pop();
                i++;
            }
        }

        return i == popped.Length;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
