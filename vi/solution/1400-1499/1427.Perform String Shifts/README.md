---
comments: true
difficulty: Easy
tags:
    - Array
    - Math
    - String
---

<!-- problem:start -->

# [1427. Perform String Shifts 🔒](https://leetcode.com/problems/perform-string-shifts)

[Tài liệu tiếng Trung](/solution/1400-1499/1427.Perform%20String%20Shifts/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> chỉ chứa các chữ cái tiếng Anh viết thường và một ma trận <code>shift</code>, trong đó <code>shift[i] = [direction<sub>i</sub>, amount<sub>i</sub>]</code>:</p>

<ul>
	<li><code>direction<sub>i</sub></code> có thể là <code>0</code> (dịch sang trái) hoặc <code>1</code> (dịch sang phải).</li>
	<li><code>amount<sub>i</sub></code> là số vị trí mà chuỗi <code>s</code> cần được dịch.</li>
	<li>Dịch sang trái 1 vị trí nghĩa là xóa ký tự đầu tiên của <code>s</code> và thêm nó vào cuối chuỗi.</li>
	<li>Tương tự, dịch sang phải 1 vị trí nghĩa là xóa ký tự cuối cùng của <code>s</code> và thêm nó vào đầu chuỗi.</li>
</ul>

<p>Trả về chuỗi cuối cùng sau khi thực hiện tất cả các phép biến đổi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;abc&quot;, shift = [[0,1],[1,2]]
<strong>Output:</strong> &quot;cab&quot;
<strong>Explanation:</strong>&nbsp;
[0,1] nghĩa là dịch sang trái 1 vị trí. &quot;abc&quot; -&gt; &quot;bca&quot;
[1,2] nghĩa là dịch sang phải 2 vị trí. &quot;bca&quot; -&gt; &quot;cab&quot;</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;abcdefg&quot;, shift = [[1,1],[1,1],[0,2],[1,3]]
<strong>Output:</strong> &quot;efgabcd&quot;
<strong>Explanation:</strong>&nbsp;
[1,1] nghĩa là dịch sang phải 1 vị trí. &quot;abcdefg&quot; -&gt; &quot;gabcdef&quot;
[1,1] nghĩa là dịch sang phải 1 vị trí. &quot;gabcdef&quot; -&gt; &quot;fgabcde&quot;
[0,2] nghĩa là dịch sang trái 2 vị trí. &quot;fgabcde&quot; -&gt; &quot;abcdefg&quot;
[1,3] nghĩa là dịch sang phải 3 vị trí. &quot;abcdefg&quot; -&gt; &quot;efgabcd&quot;</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
	<li><code>1 &lt;= shift.length &lt;= 100</code></li>
	<li><code>shift[i].length == 2</code></li>
	<li><code>direction<sub>i</sub></code><sub> </sub> là <code>0</code> hoặc <code>1</code>.</li>
	<li><code>0 &lt;= amount<sub>i</sub> &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Các phép dịch sang trái và sang phải sẽ triệt tiêu lẫn nhau. Ta cộng chúng dưới dạng một độ lệch có dấu, lấy modulo $n$, rồi xoay chuỗi bằng một lần cắt. Vì $n,m\le 100$, ngay cả cách dịch từng bước cũng đủ nhanh; việc gộp các phép dịch có độ phức tạp $O(n+m)$.

<!-- thinking:end -->

Ta ký hiệu độ dài của chuỗi $s$ là $n$. Tiếp theo, ta duyệt mảng $shift$, cộng dồn để thu được độ lệch cuối cùng $x$, sau đó lấy $x$ modulo $n$; kết quả cuối cùng là đưa $n - x$ ký tự đầu tiên của $s$ ra cuối chuỗi.

Độ phức tạp thời gian là $O(n + m)$, trong đó $n$ và $m$ lần lượt là độ dài của chuỗi $s$ và mảng $shift$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def stringShift(self, s: str, shift: List[List[int]]) -> str:
        x = sum((b if a else -b) for a, b in shift)
        x %= len(s)
        return s[-x:] + s[:-x]
```

#### Java

```java
class Solution {
    public String stringShift(String s, int[][] shift) {
        int x = 0;
        for (var e : shift) {
            if (e[0] == 0) {
                e[1] *= -1;
            }
            x += e[1];
        }
        int n = s.length();
        x = (x % n + n) % n;
        return s.substring(n - x) + s.substring(0, n - x);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string stringShift(string s, vector<vector<int>>& shift) {
        int x = 0;
        for (auto& e : shift) {
            if (e[0] == 0) {
                e[1] = -e[1];
            }
            x += e[1];
        }
        int n = s.size();
        x = (x % n + n) % n;
        return s.substr(n - x, x) + s.substr(0, n - x);
    }
};
```

#### Go

```go
func stringShift(s string, shift [][]int) string {
	x := 0
	for _, e := range shift {
		if e[0] == 0 {
			e[1] = -e[1]
		}
		x += e[1]
	}
	n := len(s)
	x = (x%n + n) % n
	return s[n-x:] + s[:n-x]
}
```

#### TypeScript

```ts
function stringShift(s: string, shift: number[][]): string {
    let x = 0;
    for (const [a, b] of shift) {
        x += a === 0 ? -b : b;
    }
    x %= s.length;
    if (x < 0) {
        x += s.length;
    }
    return s.slice(-x) + s.slice(0, -x);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
