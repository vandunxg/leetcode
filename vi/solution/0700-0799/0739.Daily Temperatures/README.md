---
comments: true
difficulty: Medium
tags:
    - Stack
    - Array
    - Monotonic Stack
---

<!-- problem:start -->

# [739. Daily Temperatures](https://leetcode.com/problems/daily-temperatures)

[中文文档](/solution/0700-0799/0739.Daily%20Temperatures/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>temperatures</code> biểu thị nhiệt độ mỗi ngày. Hãy trả về mảng <code>answer</code> sao cho <code>answer[i]</code> là <em>số ngày cần chờ sau ngày thứ</em> <code>i<sup>th</sup></code> <em>để gặp ngày có nhiệt độ cao hơn</em>. Nếu không có ngày nào trong tương lai thỏa mãn, giữ <code>answer[i] == 0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> temperatures = [73,74,75,71,69,72,76,73]
<strong>Đầu ra:</strong> [1,1,4,2,1,1,0,0]
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> temperatures = [30,40,50,60]
<strong>Đầu ra:</strong> [1,1,1,0]
</pre><p><strong class="example">Ví dụ 3:</strong></p>
<pre><strong>Đầu vào:</strong> temperatures = [30,60,90]
<strong>Đầu ra:</strong> [1,1,0]
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;=&nbsp;temperatures.length &lt;= 10<sup>5</sup></code></li>
	<li><code>30 &lt;=&nbsp;temperatures[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Monotonic Stack

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi ngày, cần tìm thời gian chờ đến ngày có nhiệt độ cao hơn. Vì $n\le 10^5$, không thể quét sang phải từ từng chỉ số.
>
> Đây là bài toán next-greater-element. Duyệt từ phải sang trái với monotonic stack lưu các chỉ số đang chờ ngày nóng hơn; pop mọi nhiệt độ không cao hơn nhiệt độ hiện tại, khi đó top mới là đáp án.
>
> Nhiệt độ tăng dần từ top xuống đáy stack. Mỗi chỉ số được push và pop một lần, nên độ phức tạp là $O(n)$.

<!-- thinking:end -->

Bài toán yêu cầu tìm vị trí phần tử đầu tiên lớn hơn ở bên phải mỗi phần tử, đây là ứng dụng điển hình của monotonic stack.

Ta duyệt mảng $\textit{temperatures}$ từ phải sang trái, duy trì stack $\textit{stk}$ có nhiệt độ tăng dần từ top xuống đáy. Stack lưu các chỉ số của phần tử trong mảng. Với mỗi phần tử $\textit{temperatures}[i]$, ta liên tục so sánh nó với phần tử ở top. Nếu nhiệt độ tại top nhỏ hơn hoặc bằng $\textit{temperatures}[i]$, ta pop top trong vòng lặp cho đến khi stack rỗng hoặc nhiệt độ tại top lớn hơn $\textit{temperatures}[i]$. Lúc này, top là phần tử đầu tiên ở bên phải có giá trị lớn hơn $\textit{temperatures}[i]$, và khoảng cách là $\textit{stk.top()} - i$. Ta cập nhật mảng kết quả tương ứng. Sau đó, push $\textit{temperatures}[i]$ vào stack rồi tiếp tục duyệt.

Sau khi duyệt xong, ta trả về mảng kết quả.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, với $n$ là độ dài của mảng $\textit{temperatures}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        stk = []
        n = len(temperatures)
        ans = [0] * n
        for i in range(n - 1, -1, -1):
            while stk and temperatures[stk[-1]] <= temperatures[i]:
                stk.pop()
            if stk:
                ans[i] = stk[-1] - i
            stk.append(i)
        return ans
```

#### Java

```java
class Solution {
    public int[] dailyTemperatures(int[] temperatures) {
        int n = temperatures.length;
        Deque<Integer> stk = new ArrayDeque<>();
        int[] ans = new int[n];
        for (int i = n - 1; i >= 0; --i) {
            while (!stk.isEmpty() && temperatures[stk.peek()] <= temperatures[i]) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                ans[i] = stk.peek() - i;
            }
            stk.push(i);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        int n = temperatures.size();
        stack<int> stk;
        vector<int> ans(n);
        for (int i = n - 1; ~i; --i) {
            while (!stk.empty() && temperatures[stk.top()] <= temperatures[i]) {
                stk.pop();
            }
            if (!stk.empty()) {
                ans[i] = stk.top() - i;
            }
            stk.push(i);
        }
        return ans;
    }
};
```

#### Go

```go
func dailyTemperatures(temperatures []int) []int {
	n := len(temperatures)
	ans := make([]int, n)
	stk := []int{}
	for i := n - 1; i >= 0; i-- {
		for len(stk) > 0 && temperatures[stk[len(stk)-1]] <= temperatures[i] {
			stk = stk[:len(stk)-1]
		}
		if len(stk) > 0 {
			ans[i] = stk[len(stk)-1] - i
		}
		stk = append(stk, i)
	}
	return ans
}
```

#### TypeScript

```ts
function dailyTemperatures(temperatures: number[]): number[] {
    const n = temperatures.length;
    const ans: number[] = Array(n).fill(0);
    const stk: number[] = [];
    for (let i = n - 1; ~i; --i) {
        while (stk.length && temperatures[stk.at(-1)!] <= temperatures[i]) {
            stk.pop();
        }
        if (stk.length) {
            ans[i] = stk.at(-1)! - i;
        }
        stk.push(i);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn daily_temperatures(temperatures: Vec<i32>) -> Vec<i32> {
        let n = temperatures.len();
        let mut stk: Vec<usize> = Vec::new();
        let mut ans = vec![0; n];

        for i in (0..n).rev() {
            while let Some(&top) = stk.last() {
                if temperatures[top] <= temperatures[i] {
                    stk.pop();
                } else {
                    break;
                }
            }
            if let Some(&top) = stk.last() {
                ans[i] = (top - i) as i32;
            }
            stk.push(i);
        }

        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} temperatures
 * @return {number[]}
 */
var dailyTemperatures = function (temperatures) {
    const n = temperatures.length;
    const ans = Array(n).fill(0);
    const stk = [];
    for (let i = n - 1; ~i; --i) {
        while (stk.length && temperatures[stk.at(-1)] <= temperatures[i]) {
            stk.pop();
        }
        if (stk.length) {
            ans[i] = stk.at(-1) - i;
        }
        stk.push(i);
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
