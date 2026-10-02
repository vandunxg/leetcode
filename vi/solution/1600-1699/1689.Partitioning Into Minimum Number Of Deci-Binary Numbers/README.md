---
comments: true
difficulty: Medium
rating: 1355
source: Weekly Contest 219 Q2
tags:
    - Greedy
    - String
---

<!-- problem:start -->

# [1689. Partitioning Into Minimum Number Of Deci-Binary Numbers](https://leetcode.com/problems/partitioning-into-minimum-number-of-deci-binary-numbers)

[中文文档](/solution/1600-1699/1689.Partitioning%20Into%20Minimum%20Number%20Of%20Deci-Binary%20Numbers/README.md)

## Mô tả

<!-- description:start -->

<p>Một số thập phân được gọi là <strong>deci-binary</strong> nếu mỗi chữ số của nó là <code>0</code> hoặc <code>1</code> và không có số 0 ở đầu. Ví dụ, <code>101</code> và <code>1100</code> là số <strong>deci-binary</strong>, còn <code>112</code> và <code>3001</code> thì không.</p>

<p>Cho chuỗi <code>n</code> biểu diễn một số nguyên thập phân dương, hãy trả về <em><strong>số lượng nhỏ nhất</strong> các số <strong>deci-binary</strong> dương cần dùng để tổng của chúng bằng </em><code>n</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> n = &quot;32&quot;
<strong>Output:</strong> 3
<strong>Giải thích:</strong> 10 + 11 + 11 = 32
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> n = &quot;82734&quot;
<strong>Output:</strong> 8
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> n = &quot;27346209830709182346&quot;
<strong>Output:</strong> 9
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n.length &lt;= 10<sup>5</sup></code></li>
	<li><code>n</code> chỉ gồm các chữ số.</li>
	<li><code>n</code> không chứa số 0 ở đầu và biểu diễn một số nguyên dương.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nhận xét nhanh

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi chữ số deci-binary chỉ có thể là $0$ hoặc $1$. Để tổng của chúng bằng $n$, tại một vị trí có chữ số $d$ cần ít nhất $d$ chữ số 1 ở vị trí đó. Vì vậy đáp án là chữ số lớn nhất của $n$.

<!-- thinking:end -->

Bài toán tương đương với việc tìm chữ số lớn nhất trong chuỗi.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minPartitions(self, n: str) -> int:
        return int(max(n))
```

#### Java

```java
class Solution {
    public int minPartitions(String n) {
        int ans = 0;
        for (int i = 0; i < n.length(); ++i) {
            ans = Math.max(ans, n.charAt(i) - '0');
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minPartitions(string n) {
        int ans = 0;
        for (char& c : n) {
            ans = max(ans, c - '0');
        }
        return ans;
    }
};
```

#### Go

```go
func minPartitions(n string) (ans int) {
	for _, c := range n {
		ans = max(ans, int(c-'0'))
	}
	return
}
```

#### TypeScript

```ts
function minPartitions(n: string): number {
    return Math.max(...n.split('').map(Number));
}
```

#### Rust

```rust
impl Solution {
    pub fn min_partitions(n: String) -> i32 {
        n.as_bytes().iter().fold(0, |ans, &c| ans.max((c - b'0') as i32))
    }
}
```

#### C

```c
int minPartitions(char* n) {
    int ans = 0;
    for (int i = 0; n[i]; i++) {
        int v = n[i] - '0';
        if (v > ans) {
            ans = v;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
