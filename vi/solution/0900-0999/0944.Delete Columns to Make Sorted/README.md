---
comments: true
difficulty: Easy
tags:
    - Array
    - String
    - Longest Increasing Subsequence
---

<!-- problem:start -->

# [944. Delete Columns to Make Sorted](https://leetcode.com/problems/delete-columns-to-make-sorted)

[中文文档](/solution/0900-0999/0944.Delete%20Columns%20to%20Make%20Sorted/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng gồm <code>n</code> chuỗi <code>strs</code>, tất cả có cùng độ dài.</p>

<p>Có thể xếp các chuỗi thành một bảng, mỗi chuỗi nằm trên một hàng.</p>

<ul>
	<li>Ví dụ, có thể xếp <code>strs = [&quot;abc&quot;, &quot;bce&quot;, &quot;cae&quot;]</code> như sau:</li>
</ul>

<pre>
abc
bce
cae
</pre>

<p>Bạn cần <strong>xóa</strong> các cột <strong>không được sắp xếp theo thứ tự từ điển</strong>. Trong ví dụ trên (đánh chỉ số từ <strong>0</strong>), cột 0 (<code>&#39;a&#39;</code>, <code>&#39;b&#39;</code>, <code>&#39;c&#39;</code>) và cột 2 (<code>&#39;c&#39;</code>, <code>&#39;e&#39;</code>, <code>&#39;e&#39;</code>) đã được sắp xếp, còn cột 1 (<code>&#39;b&#39;</code>, <code>&#39;c&#39;</code>, <code>&#39;a&#39;</code>) thì chưa, nên bạn sẽ xóa cột 1.</p>

<p>Trả về <em>số cột cần xóa</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> strs = [&quot;cba&quot;,&quot;daf&quot;,&quot;ghi&quot;]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Bảng có dạng như sau:
  cba
  daf
  ghi
Cột 0 và 2 đã được sắp xếp, còn cột 1 thì chưa, nên chỉ cần xóa 1 cột.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> strs = [&quot;a&quot;,&quot;b&quot;]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Bảng có dạng như sau:
  a
  b
Cột 0 là cột duy nhất và đã được sắp xếp, nên không cần xóa cột nào.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> strs = [&quot;zyx&quot;,&quot;wvu&quot;,&quot;tsr&quot;]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Bảng có dạng như sau:
  zyx
  wvu
  tsr
Cả 3 cột đều chưa được sắp xếp, nên bạn sẽ xóa cả 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == strs.length</code></li>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= strs[i].length &lt;= 1000</code></li>
	<li><code>strs[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: So sánh từng cột

<!-- thinking:start -->

> **Tư duy**
>
> Xóa ít cột nhất sao cho các cột còn lại không giảm từ trên xuống dưới. Các cột độc lập với nhau: chỉ cần có một cặp kề nhau giảm là phải xóa cột đó. Duyệt từng cột và đếm ngay khi gặp chỗ giảm.

<!-- thinking:end -->

Gọi số hàng của mảng chuỗi $\textit{strs}$ là $n$, số cột là $m$.

Ta duyệt từng cột, bắt đầu từ hàng thứ hai, rồi so sánh ký tự ở hàng hiện tại với ký tự ở hàng trước. Nếu ký tự ở hàng hiện tại nhỏ hơn ký tự ở hàng trước, cột đó không được sắp xếp theo thứ tự từ điển không giảm; ta xóa cột, tăng kết quả lên một rồi thoát khỏi vòng lặp bên trong.

Cuối cùng, trả về kết quả.

Độ phức tạp thời gian là $O(L)$, với $L$ là tổng độ dài các chuỗi trong mảng $\textit{strs}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minDeletionSize(self, strs: List[str]) -> int:
        m, n = len(strs[0]), len(strs)
        ans = 0
        for j in range(m):
            for i in range(1, n):
                if strs[i][j] < strs[i - 1][j]:
                    ans += 1
                    break
        return ans
```

#### Java

```java
class Solution {
    public int minDeletionSize(String[] strs) {
        int m = strs[0].length(), n = strs.length;
        int ans = 0;
        for (int j = 0; j < m; ++j) {
            for (int i = 1; i < n; ++i) {
                if (strs[i].charAt(j) < strs[i - 1].charAt(j)) {
                    ++ans;
                    break;
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
        int m = strs[0].size(), n = strs.size();
        int ans = 0;
        for (int j = 0; j < m; ++j) {
            for (int i = 1; i < n; ++i) {
                if (strs[i][j] < strs[i - 1][j]) {
                    ++ans;
                    break;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minDeletionSize(strs []string) (ans int) {
	m, n := len(strs[0]), len(strs)
	for j := 0; j < m; j++ {
		for i := 1; i < n; i++ {
			if strs[i][j] < strs[i-1][j] {
				ans++
				break
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function minDeletionSize(strs: string[]): number {
    const [m, n] = [strs[0].length, strs.length];
    let ans = 0;
    for (let j = 0; j < m; ++j) {
        for (let i = 1; i < n; ++i) {
            if (strs[i][j] < strs[i - 1][j]) {
                ++ans;
                break;
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
        let mut ans = 0;
        for j in 0..m {
            for i in 1..n {
                if strs[i].as_bytes()[j] < strs[i - 1].as_bytes()[j] {
                    ans += 1;
                    break;
                }
            }
        }
        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int MinDeletionSize(string[] strs) {
        int m = strs[0].Length;
        int n = strs.Length;
        int ans = 0;

        for (int j = 0; j < m; ++j) {
            for (int i = 1; i < n; ++i) {
                if (strs[i][j] < strs[i - 1][j]) {
                    ++ans;
                    break;
                }
            }
        }

        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
