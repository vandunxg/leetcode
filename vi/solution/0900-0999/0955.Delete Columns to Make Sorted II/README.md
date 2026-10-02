---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - String
---

<!-- problem:start -->

# [955. Delete Columns to Make Sorted II](https://leetcode.com/problems/delete-columns-to-make-sorted-ii)

[中文文档](/solution/0900-0999/0955.Delete%20Columns%20to%20Make%20Sorted%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>strs</code> gồm <code>n</code> chuỗi có cùng độ dài.</p>

<p>Ta có thể chọn một số chỉ số cột cần xóa và xóa ký tự ở các chỉ số đó khỏi mọi chuỗi.</p>

<p>Ví dụ, nếu <code>strs = [&quot;abcdef&quot;,&quot;uvwxyz&quot;]</code> và các chỉ số cần xóa là <code>{0, 2, 3}</code>, thì mảng sau khi xóa là <code>[&quot;bef&quot;, &quot;vyz&quot;]</code>.</p>

<p>Giả sử ta chọn tập chỉ số cần xóa <code>answer</code> sao cho sau khi xóa, các phần tử trong mảng cuối cùng được sắp xếp theo <strong>thứ tự từ điển</strong> (tức là <code>strs[0] &lt;= strs[1] &lt;= strs[2] &lt;= ... &lt;= strs[n - 1]</code>). Hãy trả về <em>giá trị nhỏ nhất có thể của</em> <code>answer.length</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> strs = [&quot;ca&quot;,&quot;bb&quot;,&quot;ac&quot;]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> 
Sau khi xóa cột đầu tiên, strs = [&quot;a&quot;, &quot;b&quot;, &quot;c&quot;].
Lúc này, strs được sắp xếp theo thứ tự từ điển (tức là strs[0] &lt;= strs[1] &lt;= strs[2]).
Ta cần xóa ít nhất 1 cột vì ban đầu strs chưa được sắp xếp theo thứ tự từ điển, nên đáp án là 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> strs = [&quot;xc&quot;,&quot;yb&quot;,&quot;za&quot;]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> 
strs đã được sắp xếp theo thứ tự từ điển, nên không cần xóa gì cả.
Lưu ý, các chuỗi trong strs không nhất thiết phải được sắp xếp theo thứ tự từ điển:
tức là KHÔNG nhất thiết phải đúng rằng (strs[0][0] &lt;= strs[0][1] &lt;= ...)
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> strs = [&quot;zyx&quot;,&quot;wvu&quot;,&quot;tsr&quot;]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta phải xóa tất cả các cột.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == strs.length</code></li>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= strs[i].length &lt;= 100</code></li>
	<li><code>strs[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Xóa ít cột nhất để các hàng được sắp xếp không giảm theo thứ tự từ điển. Thứ tự từ điển được quyết định bởi cột đầu tiên có ký tự khác nhau, vì vậy thứ tự của một cặp đã được xác định sẽ không bị ảnh hưởng bởi các cột sau. Duyệt từ trái sang phải: nếu cột hiện tại làm một cặp chưa được phân định bị giảm thứ tự, phải xóa cột đó; nếu không, đánh dấu các cặp trở nên tăng nghiêm ngặt.

<!-- thinking:end -->

Khi so sánh các chuỗi theo thứ tự từ điển, ta so sánh từ trái sang phải; ký tự khác nhau đầu tiên sẽ quyết định thứ tự giữa hai chuỗi. Vì vậy, ta có thể duyệt từng cột từ trái sang phải để xác định có cần xóa cột hiện tại hay không.

Ta duy trì mảng boolean $\textit{st}$ có độ dài $n - 1$ để đánh dấu thứ tự giữa từng cặp chuỗi kề nhau đã được xác định hay chưa. Nếu thứ tự của một cặp đã được xác định, các phép so sánh ký tự tiếp theo giữa hai chuỗi đó sẽ không làm thay đổi thứ tự của chúng.

Với mỗi cột $j$, ta duyệt tất cả các cặp chuỗi kề nhau $(\textit{strs}[i], \textit{strs}[i + 1])$:

- Nếu $\textit{st}[i]$ là false và $\textit{strs}[i][j] > \textit{strs}[i + 1][j]$, cột hiện tại phải bị xóa. Ta tăng đáp án thêm một và bỏ qua việc xử lý cột này;
- Ngược lại, nếu $\textit{st}[i]$ là false và $\textit{strs}[i][j] < \textit{strs}[i + 1][j]$, thứ tự giữa hai chuỗi được xác định bởi cột hiện tại. Ta đặt $\textit{st}[i]$ thành true.

Sau khi duyệt tất cả các cột, đáp án là số cột cần xóa.

Chiến lược greedy này tối ưu vì thứ tự từ điển được quyết định bởi cột khác nhau đầu tiên tính từ trái sang phải. Nếu không xóa cột hiện tại và nó khiến một cặp chuỗi bị sai thứ tự, các cột sau không thể sửa được lỗi này, vì vậy bắt buộc phải xóa cột hiện tại. Nếu giữ cột hiện tại không gây sai thứ tự cho bất kỳ cặp chuỗi nào, thì việc giữ lại cột đó không ảnh hưởng đến thứ tự từ điển cuối cùng.

Độ phức tạp thời gian là $O(n \times m)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số chuỗi và $m$ là độ dài của mỗi chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minDeletionSize(self, strs: List[str]) -> int:
        n = len(strs)
        m = len(strs[0])
        st = [False] * (n - 1)
        ans = 0
        for j in range(m):
            must_del = False
            for i in range(n - 1):
                if not st[i] and strs[i][j] > strs[i + 1][j]:
                    must_del = True
                    break
            if must_del:
                ans += 1
            else:
                for i in range(n - 1):
                    if not st[i] and strs[i][j] < strs[i + 1][j]:
                        st[i] = True
        return ans
```

#### Java

```java
class Solution {
    public int minDeletionSize(String[] strs) {
        int n = strs.length;
        int m = strs[0].length();
        boolean[] st = new boolean[n - 1];
        int ans = 0;
        for (int j = 0; j < m; ++j) {
            boolean mustDel = false;
            for (int i = 0; i < n - 1; ++i) {
                if (!st[i] && strs[i].charAt(j) > strs[i + 1].charAt(j)) {
                    mustDel = true;
                    break;
                }
            }
            if (mustDel) {
                ++ans;
            } else {
                for (int i = 0; i < n - 1; ++i) {
                    if (!st[i] && strs[i].charAt(j) < strs[i + 1].charAt(j)) {
                        st[i] = true;
                    }
                }
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
    int minDeletionSize(vector<string>& strs) {
        int n = strs.size();
        int m = strs[0].size();
        vector<bool> st(n - 1, false);
        int ans = 0;
        for (int j = 0; j < m; ++j) {
            bool mustDel = false;
            for (int i = 0; i < n - 1; ++i) {
                if (!st[i] && strs[i][j] > strs[i + 1][j]) {
                    mustDel = true;
                    break;
                }
            }
            if (mustDel) {
                ++ans;
            } else {
                for (int i = 0; i < n - 1; ++i) {
                    if (!st[i] && strs[i][j] < strs[i + 1][j]) {
                        st[i] = true;
                    }
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minDeletionSize(strs []string) int {
	n := len(strs)
	m := len(strs[0])
	st := make([]bool, n-1)
	ans := 0
	for j := 0; j < m; j++ {
		mustDel := false
		for i := 0; i < n-1; i++ {
			if !st[i] && strs[i][j] > strs[i+1][j] {
				mustDel = true
				break
			}
		}
		if mustDel {
			ans++
		} else {
			for i := 0; i < n-1; i++ {
				if !st[i] && strs[i][j] < strs[i+1][j] {
					st[i] = true
				}
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function minDeletionSize(strs: string[]): number {
    const n = strs.length;
    const m = strs[0].length;
    const st: boolean[] = Array(n - 1).fill(false);
    let ans = 0;

    for (let j = 0; j < m; j++) {
        let mustDel = false;
        for (let i = 0; i < n - 1; i++) {
            if (!st[i] && strs[i][j] > strs[i + 1][j]) {
                mustDel = true;
                break;
            }
        }
        if (mustDel) {
            ans++;
        } else {
            for (let i = 0; i < n - 1; i++) {
                if (!st[i] && strs[i][j] < strs[i + 1][j]) {
                    st[i] = true;
                }
            }
        }
    }

    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_deletion_size(strs: Vec<String>) -> i32 {
        let n = strs.len();
        let m = strs[0].len();
        let mut st = vec![false; n - 1];
        let mut ans = 0;

        for j in 0..m {
            let mut must_del = false;
            for i in 0..n - 1 {
                if !st[i] && strs[i].as_bytes()[j] > strs[i + 1].as_bytes()[j] {
                    must_del = true;
                    break;
                }
            }
            if must_del {
                ans += 1;
            } else {
                for i in 0..n - 1 {
                    if !st[i] && strs[i].as_bytes()[j] < strs[i + 1].as_bytes()[j] {
                        st[i] = true;
                    }
                }
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
