---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - String
    - Sorting
---

<!-- problem:start -->

# [833. Find And Replace in String](https://leetcode.com/problems/find-and-replace-in-string)

[中文文档](/solution/0800-0899/0833.Find%20And%20Replace%20in%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <strong>đánh chỉ số từ 0</strong> <code>s</code> cần thực hiện <code>k</code> thao tác thay thế. Các thao tác được mô tả bởi ba mảng song song <strong>đánh chỉ số từ 0</strong> là <code>indices</code>, <code>sources</code> và <code>targets</code>, mỗi mảng có độ dài <code>k</code>.</p>

<p>Để thực hiện thao tác thay thế thứ <code>i<sup>th</sup></code>:</p>

<ol>
	<li>Kiểm tra xem <strong>chuỗi con</strong> <code>sources[i]</code> có xuất hiện tại chỉ số <code>indices[i]</code> trong <strong>chuỗi ban đầu</strong> <code>s</code> hay không.</li>
	<li>Nếu không xuất hiện, <strong>không làm gì</strong>.</li>
	<li>Nếu có, hãy <strong>thay thế</strong> chuỗi con đó bằng <code>targets[i]</code>.</li>
</ol>

<p>Ví dụ, nếu <code>s = &quot;<u>ab</u>cd&quot;</code>, <code>indices[i] = 0</code>, <code>sources[i] = &quot;ab&quot;</code> và <code>targets[i] = &quot;eee&quot;</code>, kết quả thay thế sẽ là <code>&quot;<u>eee</u>cd&quot;</code>.</p>

<p>Tất cả thao tác thay thế phải được thực hiện <strong>đồng thời</strong>, nghĩa là chúng không ảnh hưởng đến chỉ số của nhau. Các test được tạo sao cho những vị trí thay thế <strong>không chồng lấn</strong>.</p>

<ul>
	<li>Ví dụ, test có <code>s = &quot;abc&quot;</code>, <code>indices = [0, 1]</code> và <code>sources = [&quot;ab&quot;,&quot;bc&quot;]</code> sẽ không được tạo vì hai thao tác thay thế <code>&quot;ab&quot;</code> và <code>&quot;bc&quot;</code> chồng lấn nhau.</li>
</ul>

<p>Trả về <em><strong>chuỗi kết quả</strong> sau khi thực hiện tất cả thao tác thay thế trên </em><code>s</code>.</p>

<p><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp trong chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0833.Find%20And%20Replace%20in%20String/images/833-ex1.png" style="width: 411px; height: 251px;" />
<pre>
<strong>Đầu vào:</strong> s = &quot;abcd&quot;, indices = [0, 2], sources = [&quot;a&quot;, &quot;cd&quot;], targets = [&quot;eee&quot;, &quot;ffff&quot;]
<strong>Đầu ra:</strong> &quot;eeebffff&quot;
<strong>Giải thích:</strong>
&quot;a&quot; xuất hiện tại chỉ số 0 trong s, nên ta thay thế bằng &quot;eee&quot;.
&quot;cd&quot; xuất hiện tại chỉ số 2 trong s, nên ta thay thế bằng &quot;ffff&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0833.Find%20And%20Replace%20in%20String/images/833-ex2-1.png" style="width: 411px; height: 251px;" />
<pre>
<strong>Đầu vào:</strong> s = &quot;abcd&quot;, indices = [0, 2], sources = [&quot;ab&quot;,&quot;ec&quot;], targets = [&quot;eee&quot;,&quot;ffff&quot;]
<strong>Đầu ra:</strong> &quot;eeecd&quot;
<strong>Giải thích:</strong>
&quot;ab&quot; xuất hiện tại chỉ số 0 trong s, nên ta thay thế bằng &quot;eee&quot;.
&quot;ec&quot; không xuất hiện tại chỉ số 2 trong s, nên ta không làm gì.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>k == indices.length == sources.length == targets.length</code></li>
	<li><code>1 &lt;= k &lt;= 100</code></li>
	<li><code>0 &lt;= indexes[i] &lt; s.length</code></li>
	<li><code>1 &lt;= sources[i].length, targets[i].length &lt;= 50</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>sources[i]</code> và <code>targets[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Các thao tác thay thế được áp dụng đồng thời và chỉ thực hiện khi $\textit{source}$ khớp với $s$ tại chỉ số đã cho. Nếu chỉnh sửa ngay trong lúc duyệt, các chỉ số phía sau sẽ bị thay đổi.
>
> Đánh dấu các vị trí khớp trong chuỗi ban đầu, sau đó tạo kết quả từ trái sang phải: khi gặp vị trí khớp, thêm $\textit{target}$ rồi bỏ qua đoạn $\textit{source}$; nếu không thì sao chép ký tự hiện tại.

<!-- thinking:end -->

Ta duyệt từng thao tác thay thế. Với thao tác thứ $k$, ký hiệu $(i, \text{src})$, nếu $s[i..i+|\text{src}|-1]$ bằng $\text{src}$ thì ghi nhận rằng đoạn bắt đầu tại chỉ số $i$ cần được thay bằng chuỗi thứ $k$ trong $\text{targets}$; nếu không thì không cần thay thế.

Tiếp theo, chỉ cần duyệt chuỗi ban đầu $s$ và thực hiện thay thế dựa trên thông tin đã ghi nhận.

Độ phức tạp thời gian là $O(L)$ và độ phức tạp không gian là $O(n)$, trong đó $L$ là tổng độ dài của tất cả chuỗi, còn $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findReplaceString(
        self, s: str, indices: List[int], sources: List[str], targets: List[str]
    ) -> str:
        n = len(s)
        d = [-1] * n
        for k, (i, src) in enumerate(zip(indices, sources)):
            if s.startswith(src, i):
                d[i] = k
        ans = []
        i = 0
        while i < n:
            if ~d[i]:
                ans.append(targets[d[i]])
                i += len(sources[d[i]])
            else:
                ans.append(s[i])
                i += 1
        return "".join(ans)
```

#### Java

```java
class Solution {
    public String findReplaceString(String s, int[] indices, String[] sources, String[] targets) {
        int n = s.length();
        var d = new int[n];
        Arrays.fill(d, -1);
        for (int k = 0; k < indices.length; ++k) {
            int i = indices[k];
            if (s.startsWith(sources[k], i)) {
                d[i] = k;
            }
        }
        var ans = new StringBuilder();
        for (int i = 0; i < n;) {
            if (d[i] >= 0) {
                ans.append(targets[d[i]]);
                i += sources[d[i]].length();
            } else {
                ans.append(s.charAt(i++));
            }
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string findReplaceString(string s, vector<int>& indices, vector<string>& sources, vector<string>& targets) {
        int n = s.size();
        vector<int> d(n, -1);
        for (int k = 0; k < indices.size(); ++k) {
            int i = indices[k];
            if (s.compare(i, sources[k].size(), sources[k]) == 0) {
                d[i] = k;
            }
        }
        string ans;
        for (int i = 0; i < n;) {
            if (~d[i]) {
                ans += targets[d[i]];
                i += sources[d[i]].size();
            } else {
                ans += s[i++];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findReplaceString(s string, indices []int, sources []string, targets []string) string {
	n := len(s)
	d := make([]int, n)
	for k, i := range indices {
		if strings.HasPrefix(s[i:], sources[k]) {
			d[i] = k + 1
		}
	}
	ans := &strings.Builder{}
	for i := 0; i < n; {
		if d[i] > 0 {
			ans.WriteString(targets[d[i]-1])
			i += len(sources[d[i]-1])
		} else {
			ans.WriteByte(s[i])
			i++
		}
	}
	return ans.String()
}
```

#### TypeScript

```ts
function findReplaceString(
    s: string,
    indices: number[],
    sources: string[],
    targets: string[],
): string {
    const n = s.length;
    const d: number[] = Array(n).fill(-1);
    for (let k = 0; k < indices.length; ++k) {
        const [i, src] = [indices[k], sources[k]];
        if (s.startsWith(src, i)) {
            d[i] = k;
        }
    }
    const ans: string[] = [];
    for (let i = 0; i < n;) {
        if (d[i] >= 0) {
            ans.push(targets[d[i]]);
            i += sources[d[i]].length;
        } else {
            ans.push(s[i++]);
        }
    }
    return ans.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
