---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Hash Table
    - Two Pointers
    - String
---

<!-- problem:start -->

# [763. Partition Labels](https://leetcode.com/problems/partition-labels)

[中文文档](/solution/0700-0799/0763.Partition%20Labels/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code>. Hãy chia chuỗi thành nhiều phần nhất có thể sao cho mỗi chữ cái chỉ xuất hiện trong tối đa một phần. Ví dụ, chuỗi <code>&quot;ababcc&quot;</code> có thể được chia thành <code>[&quot;abab&quot;, &quot;cc&quot;]</code>, nhưng các cách chia như <code>[&quot;aba&quot;, &quot;bcc&quot;]</code> hoặc <code>[&quot;ab&quot;, &quot;ab&quot;, &quot;cc&quot;]</code> không hợp lệ.</p>

<p>Lưu ý, khi nối các phần theo đúng thứ tự, chuỗi thu được phải là <code>s</code>.</p>

<p>Trả về <em>danh sách số nguyên biểu thị kích thước của các phần</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;ababcbacadefegdehijhklij&quot;
<strong>Đầu ra:</strong> [9,7,8]
<strong>Giải thích:</strong>
Các phần được chia là &quot;ababcbaca&quot;, &quot;defegde&quot;, &quot;hijhklij&quot;.
Cách chia này đảm bảo mỗi chữ cái chỉ xuất hiện trong tối đa một phần.
Cách chia thành &quot;ababcbacadefegde&quot; và &quot;hijhklij&quot; không hợp lệ vì tạo ra ít phần hơn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;eccbbbbdec&quot;
<strong>Đầu ra:</strong> [10]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 500</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Chia $s$ thành nhiều phần nhất có thể sao cho không chữ cái nào xuất hiện ở hai phần. Chỉ số xuất hiện cuối cùng của một chữ cái xác định phần chứa nó phải kéo dài đến đâu.
>
> Duyệt từ trái sang phải và mở rộng điểm cuối hiện tại theo $last[c]$. Khi chỉ số đang duyệt chạm điểm cuối đó, ta kết thúc phần hiện tại.
>
> Tạo mảng $\textit{last}$ rồi chia chuỗi trong một lượt duyệt. Độ phức tạp $O(n)$.

<!-- thinking:end -->

Trước tiên, dùng mảng hoặc hash table $\textit{last}$ để lưu vị trí xuất hiện cuối cùng của mỗi chữ cái trong chuỗi $s$.

Tiếp theo, dùng greedy để chia chuỗi thành nhiều đoạn nhất có thể.

Duyệt chuỗi $s$ từ trái sang phải, đồng thời duy trì chỉ số bắt đầu $j$ và chỉ số kết thúc $i$ của đoạn hiện tại; ban đầu cả hai đều bằng $0$.

Với mỗi chữ cái $c$ được duyệt, lấy vị trí xuất hiện cuối cùng $\textit{last}[c]$. Vì chỉ số kết thúc của đoạn hiện tại phải lớn hơn hoặc bằng $\textit{last}[c]$, cập nhật $\textit{mx} = \max(\textit{mx}, \textit{last}[c])$.

Khi duyệt đến chỉ số $\textit{mx}$, đoạn hiện tại kết thúc. Đoạn có phạm vi chỉ số $[j,.. i]$ và độ dài $i - j + 1$. Thêm độ dài này vào mảng kết quả, rồi đặt $j = i + 1$ để tiếp tục tìm đoạn kế tiếp.

Lặp lại quá trình trên cho đến khi duyệt hết chuỗi để thu được độ dài của mọi đoạn.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(|\Sigma|)$, trong đó $n$ là độ dài chuỗi $s$, còn $|\Sigma|$ là kích thước tập ký tự. Trong bài này, $|\Sigma| = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def partitionLabels(self, s: str) -> List[int]:
        last = {c: i for i, c in enumerate(s)}
        mx = j = 0
        ans = []
        for i, c in enumerate(s):
            mx = max(mx, last[c])
            if mx == i:
                ans.append(i - j + 1)
                j = i + 1
        return ans
```

#### Java

```java
class Solution {
    public List<Integer> partitionLabels(String s) {
        int[] last = new int[26];
        int n = s.length();
        for (int i = 0; i < n; ++i) {
            last[s.charAt(i) - 'a'] = i;
        }
        List<Integer> ans = new ArrayList<>();
        int mx = 0, j = 0;
        for (int i = 0; i < n; ++i) {
            mx = Math.max(mx, last[s.charAt(i) - 'a']);
            if (mx == i) {
                ans.add(i - j + 1);
                j = i + 1;
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
    vector<int> partitionLabels(string s) {
        int last[26] = {0};
        int n = s.size();
        for (int i = 0; i < n; ++i) {
            last[s[i] - 'a'] = i;
        }
        vector<int> ans;
        int mx = 0, j = 0;
        for (int i = 0; i < n; ++i) {
            mx = max(mx, last[s[i] - 'a']);
            if (mx == i) {
                ans.push_back(i - j + 1);
                j = i + 1;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func partitionLabels(s string) (ans []int) {
	last := [26]int{}
	for i, c := range s {
		last[c-'a'] = i
	}
	var mx, j int
	for i, c := range s {
		mx = max(mx, last[c-'a'])
		if mx == i {
			ans = append(ans, i-j+1)
			j = i + 1
		}
	}
	return
}
```

#### TypeScript

```ts
function partitionLabels(s: string): number[] {
    const last: number[] = Array(26).fill(0);
    const idx = (c: string) => c.charCodeAt(0) - 'a'.charCodeAt(0);
    const n = s.length;
    for (let i = 0; i < n; ++i) {
        last[idx(s[i])] = i;
    }
    const ans: number[] = [];
    for (let i = 0, j = 0, mx = 0; i < n; ++i) {
        mx = Math.max(mx, last[idx(s[i])]);
        if (mx === i) {
            ans.push(i - j + 1);
            j = i + 1;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn partition_labels(s: String) -> Vec<i32> {
        let n = s.len();
        let bytes = s.as_bytes();
        let mut last = [0; 26];
        for i in 0..n {
            last[(bytes[i] - b'a') as usize] = i;
        }
        let mut ans = vec![];
        let mut j = 0;
        let mut mx = 0;
        for i in 0..n {
            mx = mx.max(last[(bytes[i] - b'a') as usize]);
            if mx == i {
                ans.push((i - j + 1) as i32);
                j = i + 1;
            }
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @return {number[]}
 */
var partitionLabels = function (s) {
    const last = new Array(26).fill(0);
    const idx = c => c.charCodeAt() - 'a'.charCodeAt();
    const n = s.length;
    for (let i = 0; i < n; ++i) {
        last[idx(s[i])] = i;
    }
    const ans = [];
    for (let i = 0, j = 0, mx = 0; i < n; ++i) {
        mx = Math.max(mx, last[idx(s[i])]);
        if (mx === i) {
            ans.push(i - j + 1);
            j = i + 1;
        }
    }
    return ans;
};
```

#### C#

```cs
public class Solution {
    public IList<int> PartitionLabels(string s) {
        int[] last = new int[26];
        int n = s.Length;
        for (int i = 0; i < n; i++) {
            last[s[i] - 'a'] = i;
        }
        IList<int> ans = new List<int>();
        for (int i = 0, j = 0, mx = 0; i < n; ++i) {
            mx = Math.Max(mx, last[s[i] - 'a']);
            if (mx == i) {
                ans.Add(i - j + 1);
                j = i + 1;
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
