---
comments: true
difficulty: Medium
rating: 1238
source: Weekly Contest 446 Q1
tags:
    - Array
    - Hash Table
    - String
    - Simulation
---

<!-- problem:start -->

# [3522. Calculate Score After Performing Instructions](https://leetcode.com/problems/calculate-score-after-performing-instructions)

[中文文档](/solution/3500-3599/3522.Calculate%20Score%20After%20Performing%20Instructions/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng <code>instructions</code> và <code>values</code>, cả hai đều có kích thước <code>n</code>.</p>

<p>Hãy mô phỏng một quy trình theo các quy tắc sau:</p>

<ul>
    <li>Bắt đầu tại instruction đầu tiên ở chỉ số <code>i = 0</code> với score ban đầu bằng 0.</li>
    <li>Nếu <code>instructions[i]</code> là <code>&quot;add&quot;</code>:
    <ul>
        <li>Cộng <code>values[i]</code> vào score.</li>
        <li>Chuyển đến instruction tiếp theo <code>(i + 1)</code>.</li>
    </ul>
    </li>
    <li>Nếu <code>instructions[i]</code> là <code>&quot;jump&quot;</code>:
    <ul>
        <li>Chuyển đến instruction ở chỉ số <code>(i + values[i])</code> mà không thay đổi score.</li>
    </ul>
    </li>
</ul>

<p>Quy trình kết thúc khi xảy ra một trong hai trường hợp:</p>

<ul>
    <li>Đi ra ngoài phạm vi (tức là <code>i &lt; 0 or i &gt;= n</code>), hoặc</li>
    <li>Cố gắng truy cập lại một instruction đã được thực thi trước đó. Instruction được truy cập lại sẽ không được thực thi.</li>
</ul>

<p>Trả về score sau khi quy trình kết thúc.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">instructions = [&quot;jump&quot;,&quot;add&quot;,&quot;add&quot;,&quot;jump&quot;,&quot;add&quot;,&quot;jump&quot;], values = [2,1,3,1,-2,-3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mô phỏng quy trình bắt đầu từ instruction 0:</p>

<ul>
    <li>Tại chỉ số 0: Instruction là <code>&quot;jump&quot;</code>, chuyển đến chỉ số <code>0 + 2 = 2</code>.</li>
    <li>Tại chỉ số 2: Instruction là <code>&quot;add&quot;</code>, cộng <code>values[2] = 3</code> vào score và chuyển đến chỉ số 3. Score lúc này là 3.</li>
    <li>Tại chỉ số 3: Instruction là <code>&quot;jump&quot;</code>, chuyển đến chỉ số <code>3 + 1 = 4</code>.</li>
    <li>Tại chỉ số 4: Instruction là <code>&quot;add&quot;</code>, cộng <code>values[4] = -2</code> vào score và chuyển đến chỉ số 5. Score lúc này là 1.</li>
    <li>Tại chỉ số 5: Instruction là <code>&quot;jump&quot;</code>, chuyển đến chỉ số <code>5 + (-3) = 2</code>.</li>
    <li>Tại chỉ số 2: Đã được truy cập. Quy trình kết thúc.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">instructions = [&quot;jump&quot;,&quot;add&quot;,&quot;add&quot;], values = [3,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mô phỏng quy trình bắt đầu từ instruction 0:</p>

<ul>
    <li>Tại chỉ số 0: Instruction là <code>&quot;jump&quot;</code>, chuyển đến chỉ số <code>0 + 3 = 3</code>.</li>
    <li>Tại chỉ số 3: Vượt ra ngoài phạm vi. Quy trình kết thúc.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">instructions = [&quot;jump&quot;], values = [0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mô phỏng quy trình bắt đầu từ instruction 0:</p>

<ul>
    <li>Tại chỉ số 0: Instruction là <code>&quot;jump&quot;</code>, chuyển đến chỉ số <code>0 + 0 = 0</code>.</li>
    <li>Tại chỉ số 0: Đã được truy cập. Quy trình kết thúc.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>n == instructions.length == values.length</code></li>
    <li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
    <li><code>instructions[i]</code> là <code>&quot;add&quot;</code> hoặc <code>&quot;jump&quot;</code>.</li>
    <li><code>-10<sup>5</sup> &lt;= values[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Các instruction tạo thành một đường đi có các bước nhảy; việc truy cập lại một chỉ số đồng nghĩa với một vòng lặp. Đánh dấu các chỉ số đã truy cập, bắt đầu từ $0$, rồi di chuyển theo instruction, cộng điểm hoặc nhảy theo quy định.
>
> Dừng khi chỉ số nằm ngoài phạm vi hoặc bị lặp lại. Mỗi instruction được thực thi nhiều nhất một lần, vì vậy quá trình duyệt có độ phức tạp tuyến tính.

<!-- thinking:end -->

Ta có thể mô phỏng quy trình theo mô tả bài toán.

Định nghĩa một mảng boolean $\textit{vis}$ có độ dài $n$ để ghi lại instruction nào đã được thực thi. Ban đầu, tất cả phần tử được gán là $\text{false}$.

Sau đó, bắt đầu từ chỉ số $i = 0$, lặp lại các bước sau:

1. Gán $\textit{vis}[i]$ bằng $\text{true}$.
2. Nếu ký tự đầu tiên của $\textit{instructions}[i]$ là 'a', cộng $\textit{value}[i]$ vào đáp án và tăng $i$ lên $1$. Ngược lại, tăng $i$ lên $\textit{value}[i]$.

Vòng lặp tiếp tục cho đến khi $i \lt 0$, $i \ge n$, hoặc $\textit{vis}[i]$ là $\text{true}$.

Cuối cùng, trả về đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{value}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def calculateScore(self, instructions: List[str], values: List[int]) -> int:
        n = len(values)
        vis = [False] * n
        ans = i = 0
        while 0 <= i < n and not vis[i]:
            vis[i] = True
            if instructions[i][0] == "a":
                ans += values[i]
                i += 1
            else:
                i = i + values[i]
        return ans
```

#### Java

```java
class Solution {
    public long calculateScore(String[] instructions, int[] values) {
        int n = values.length;
        boolean[] vis = new boolean[n];
        long ans = 0;
        int i = 0;

        while (i >= 0 && i < n && !vis[i]) {
            vis[i] = true;
            if (instructions[i].charAt(0) == 'a') {
                ans += values[i];
                i += 1;
            } else {
                i = i + values[i];
            }
        }

        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long calculateScore(vector<string>& instructions, vector<int>& values) {
        int n = values.size();
        vector<bool> vis(n, false);
        long long ans = 0;
        int i = 0;

        while (i >= 0 && i < n && !vis[i]) {
            vis[i] = true;
            if (instructions[i][0] == 'a') {
                ans += values[i];
                i += 1;
            } else {
                i += values[i];
            }
        }

        return ans;
    }
};
```

#### Go

```go
func calculateScore(instructions []string, values []int) (ans int64) {
    n := len(values)
    vis := make([]bool, n)
    i := 0
    for i >= 0 && i < n && !vis[i] {
        vis[i] = true
        if instructions[i][0] == 'a' {
            ans += int64(values[i])
            i += 1
        } else {
            i += values[i]
        }
    }
    return
}
```

#### TypeScript

```ts
function calculateScore(instructions: string[], values: number[]): number {
    const n = values.length;
    const vis: boolean[] = Array(n).fill(false);
    let ans = 0;
    let i = 0;

    while (i >= 0 && i < n && !vis[i]) {
        vis[i] = true;
        if (instructions[i][0] === 'a') {
            ans += values[i];
            i += 1;
        } else {
            i += values[i];
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
