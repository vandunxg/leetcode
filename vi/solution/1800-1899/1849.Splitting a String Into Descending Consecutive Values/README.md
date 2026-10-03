---
comments: true
difficulty: Medium
rating: 1746
source: Weekly Contest 239 Q2
tags:
    - String
    - Backtracking
    - Enumeration
---

<!-- problem:start -->

# [1849. Splitting a String Into Descending Consecutive Values](https://leetcode.com/problems/splitting-a-string-into-descending-consecutive-values)

[中文文档](/solution/1800-1899/1849.Splitting%20a%20String%20Into%20Descending%20Consecutive%20Values/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> chỉ gồm các chữ số.</p>

<p>Kiểm tra xem ta có thể tách <code>s</code> thành <strong>từ hai chuỗi con không rỗng trở lên</strong> sao cho <strong>giá trị số</strong> của các chuỗi con theo <strong>thứ tự giảm dần</strong>, và <strong>hiệu</strong> giữa giá trị số của mỗi cặp <strong>chuỗi con</strong> <strong>liền kề</strong> bằng <code>1</code> hay không.</p>

<ul>
	<li>Ví dụ, chuỗi <code>s = &quot;0090089&quot;</code> có thể được tách thành <code>[&quot;0090&quot;, &quot;089&quot;]</code> với các giá trị số <code>[90,89]</code>. Các giá trị theo thứ tự giảm dần và hai giá trị liền kề chênh nhau <code>1</code>, nên cách tách này hợp lệ.</li>
	<li>Một ví dụ khác, chuỗi <code>s = &quot;001&quot;</code> có thể được tách thành <code>[&quot;0&quot;, &quot;01&quot;]</code>, <code>[&quot;00&quot;, &quot;1&quot;]</code> hoặc <code>[&quot;0&quot;, &quot;0&quot;, &quot;1&quot;]</code>. Tuy nhiên, tất cả các cách đều không hợp lệ vì chúng có các giá trị số lần lượt là <code>[0,1]</code>, <code>[0,1]</code> và <code>[0,0,1]</code>, không cách nào theo thứ tự giảm dần.</li>
</ul>

<p>Trả về <code>true</code> <em>nếu có thể tách</em> <code>s</code>​​​​​​ <em>theo mô tả trên</em><em>, nếu không thì trả về </em><code>false</code><em>.</em></p>

<p><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;1234&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không có cách tách hợp lệ cho s.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;050043&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> s có thể được tách thành [&quot;05&quot;, &quot;004&quot;, &quot;3&quot;] với các giá trị số [5,4,3].
Các giá trị theo thứ tự giảm dần và hai giá trị liền kề chênh nhau 1.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;9080701&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không có cách tách hợp lệ cho s.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 20</code></li>
	<li><code>s</code> chỉ gồm các chữ số.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi phải được tách thành ít nhất hai phần có giá trị nguyên giảm đúng $1$ sau mỗi phần. Các số 0 ở đầu được phép xuất hiện nhưng không làm thay đổi giá trị. Số vị trí cắt là cấp số mũ, nhưng vì $n\le 20$ nên vẫn có thể tìm kiếm.
>
> Mở rộng phần hiện tại từ trái sang phải, đồng thời tích lũy $y$. Phần đầu tiên không bị ràng buộc; các phần sau phải nhỏ hơn giá trị trước đó đúng 1. Phần đầu tiên không được chiếm toàn bộ chuỗi. DFS thành công nếu ta đi tới cuối chuỗi.

<!-- thinking:end -->

Ta có thể bắt đầu từ ký tự đầu tiên của chuỗi và thử tách nó thành một hoặc nhiều chuỗi con, sau đó đệ quy xử lý phần còn lại.

Cụ thể, ta thiết kế hàm $\textit{dfs}(i, x)$, trong đó $i$ là vị trí hiện tại đang được xử lý và $x$ là giá trị của phần đã tách trước đó. Ban đầu, $x = -1$, cho biết ta chưa tách được giá trị nào.

Trong $\textit{dfs}(i, x)$, trước tiên ta tính giá trị tách hiện tại $y$. Nếu $x = -1$ hoặc $x - y = 1$, ta có thể dùng $y$ làm giá trị tiếp theo và tiếp tục đệ quy xử lý phần còn lại. Nếu kết quả đệ quy là $\textit{true}$, ta đã tìm được một cách tách hợp lệ và trả về $\textit{true}$.

Sau khi thử tất cả cách tách có thể, nếu không tìm được cách tách hợp lệ, ta trả về $\textit{false}$.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def splitString(self, s: str) -> bool:
        def dfs(i: int, x: int) -> bool:
            if i >= len(s):
                return True
            y = 0
            r = len(s) - 1 if x < 0 else len(s)
            for j in range(i, r):
                y = y * 10 + int(s[j])
                if (x < 0 or x - y == 1) and dfs(j + 1, y):
                    return True
            return False

        return dfs(0, -1)
```

#### Java

```java
class Solution {
    private char[] s;

    public boolean splitString(String s) {
        this.s = s.toCharArray();
        return dfs(0, -1);
    }

    private boolean dfs(int i, long x) {
        if (i >= s.length) {
            return true;
        }
        long y = 0;
        int r = x < 0 ? s.length - 1 : s.length;
        for (int j = i; j < r; ++j) {
            y = y * 10 + s[j] - '0';
            if ((x < 0 || x - y == 1) && dfs(j + 1, y)) {
                return true;
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool splitString(string s) {
        auto dfs = [&](this auto&& dfs, int i, long long x) -> bool {
            if (i >= s.size()) {
                return true;
            }
            long long y = 0;
            int r = x < 0 ? s.size() - 1 : s.size();
            for (int j = i; j < r; ++j) {
                y = y * 10 + s[j] - '0';
                if (y > 1e10) {
                    break;
                }
                if ((x < 0 || x - y == 1) && dfs(j + 1, y)) {
                    return true;
                }
            }
            return false;
        };
        return dfs(0, -1);
    }
};
```

#### Go

```go
func splitString(s string) bool {
	var dfs func(i, x int) bool
	dfs = func(i, x int) bool {
		if i >= len(s) {
			return true
		}
		y := 0
		r := len(s)
		if x < 0 {
			r--
		}
		for j := i; j < r; j++ {
			y = y*10 + int(s[j]-'0')
			if (x < 0 || x-y == 1) && dfs(j+1, y) {
				return true
			}
		}
		return false
	}
	return dfs(0, -1)
}
```

#### TypeScript

```ts
function splitString(s: string): boolean {
    const dfs = (i: number, x: number): boolean => {
        if (i >= s.length) {
            return true;
        }
        let y = 0;
        const r = x < 0 ? s.length - 1 : s.length;
        for (let j = i; j < r; ++j) {
            y = y * 10 + +s[j];
            if ((x < 0 || x - y === 1) && dfs(j + 1, y)) {
                return true;
            }
        }
        return false;
    };
    return dfs(0, -1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
