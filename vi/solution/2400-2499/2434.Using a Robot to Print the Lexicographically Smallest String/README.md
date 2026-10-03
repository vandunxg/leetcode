---
comments: true
difficulty: Medium
rating: 1953
source: Weekly Contest 314 Q3
tags:
    - Stack
    - Greedy
    - Hash Table
    - String
---

<!-- problem:start -->

# [2434. Using a Robot to Print the Lexicographically Smallest String](https://leetcode.com/problems/using-a-robot-to-print-the-lexicographically-smallest-string)

[中文文档](/solution/2400-2499/2434.Using%20a%20Robot%20to%20Print%20the%20Lexicographically%20Smallest%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> và một robot hiện đang giữ chuỗi rỗng <code>t</code>. Thực hiện một trong các thao tác sau cho đến khi cả <code>s</code> và <code>t</code> <strong>đều rỗng</strong>:</p>

<ul>
	<li>Xóa <strong>ký tự đầu tiên</strong> của chuỗi <code>s</code> và đưa cho robot. Robot sẽ nối ký tự này vào chuỗi <code>t</code>.</li>
	<li>Xóa <strong>ký tự cuối cùng</strong> của chuỗi <code>t</code> và đưa cho robot. Robot sẽ ghi ký tự này lên giấy.</li>
</ul>

<p>Trả về <em>chuỗi nhỏ nhất theo thứ tự từ điển có thể được ghi lên giấy.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;zza&quot;
<strong>Đầu ra:</strong> &quot;azz&quot;
<strong>Giải thích:</strong> Gọi p là chuỗi được ghi.
Ban đầu p=&quot;&quot;, s=&quot;zza&quot;, t=&quot;&quot;.
Thực hiện thao tác thứ nhất ba lần: p=&quot;&quot;, s=&quot;&quot;, t=&quot;zza&quot;.
Thực hiện thao tác thứ hai ba lần: p=&quot;azz&quot;, s=&quot;&quot;, t=&quot;&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;bac&quot;
<strong>Đầu ra:</strong> &quot;abc&quot;
<strong>Giải thích:</strong> Gọi p là chuỗi được ghi.
Thực hiện thao tác thứ nhất hai lần: p=&quot;&quot;, s=&quot;c&quot;, t=&quot;ba&quot;.
Thực hiện thao tác thứ hai hai lần: p=&quot;ab&quot;, s=&quot;c&quot;, t=&quot;&quot;.
Thực hiện thao tác thứ nhất một lần: p=&quot;ab&quot;, s=&quot;&quot;, t=&quot;c&quot;.
Thực hiện thao tác thứ hai một lần: p=&quot;abc&quot;, s=&quot;&quot;, t=&quot;&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;bdda&quot;
<strong>Đầu ra:</strong> &quot;addb&quot;
<strong>Giải thích:</strong> Gọi p là chuỗi được ghi.
Ban đầu p=&quot;&quot;, s=&quot;bdda&quot;, t=&quot;&quot;.
Thực hiện thao tác thứ nhất bốn lần: p=&quot;&quot;, s=&quot;&quot;, t=&quot;bdda&quot;.
Thực hiện thao tác thứ hai bốn lần: p=&quot;addb&quot;, s=&quot;&quot;, t=&quot;&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Stack

<!-- thinking:start -->

> **Tư duy**
>
> Với $n\le 10^5$, dải giấy hoạt động như một stack: một ký tự phải được đưa vào trước khi có thể in ra. Để tạo chuỗi nhỏ nhất theo thứ tự từ điển, ta lấy ra ngay khi phần tử trên cùng không lớn hơn ký tự nhỏ nhất còn chưa đọc.
>
> Đếm các ký tự còn lại và duy trì ký tự nhỏ nhất còn lại $\textit{mi}$. Đưa từng ký tự vào stack, sau đó lấy ra khi phần tử trên cùng $\le \textit{mi}$.

<!-- thinking:end -->

Bài toán có thể được chuyển thành: cho một chuỗi, sử dụng một stack phụ để biến đổi nó thành chuỗi nhỏ nhất theo thứ tự từ điển.

Ta có thể sử dụng một mảng $\textit{cnt}$ để duy trì số lần xuất hiện của mỗi ký tự trong chuỗi $s$, sử dụng một stack $\textit{stk}$ làm stack phụ được đề cập trong đề bài, và sử dụng một biến $\textit{mi}$ để theo dõi ký tự nhỏ nhất chưa được duyệt trong chuỗi.

Ta duyệt chuỗi $s$. Với mỗi ký tự $c$, trước tiên giảm số đếm của ký tự đó trong mảng $\textit{cnt}$ và cập nhật $\textit{mi}$. Sau đó đưa $c$ vào stack. Lúc này, nếu phần tử trên cùng của stack nhỏ hơn hoặc bằng $\textit{mi}$, liên tục lấy phần tử trên cùng ra khỏi stack và thêm nó vào đáp án.

Sau khi duyệt xong, trả về đáp án.

Độ phức tạp thời gian là $O(n + |\Sigma|)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$ và $|\Sigma|$ là kích thước của tập ký tự, bằng $26$ trong bài toán này.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def robotWithString(self, s: str) -> str:
        cnt = Counter(s)
        ans = []
        stk = []
        mi = 'a'
        for c in s:
            cnt[c] -= 1
            while mi < 'z' and cnt[mi] == 0:
                mi = chr(ord(mi) + 1)
            stk.append(c)
            while stk and stk[-1] <= mi:
                ans.append(stk.pop())
        return ''.join(ans)
```

#### Java

```java
class Solution {
    public String robotWithString(String s) {
        int[] cnt = new int[26];
        for (char c : s.toCharArray()) {
            ++cnt[c - 'a'];
        }
        StringBuilder ans = new StringBuilder();
        Deque<Character> stk = new ArrayDeque<>();
        char mi = 'a';
        for (char c : s.toCharArray()) {
            --cnt[c - 'a'];
            while (mi < 'z' && cnt[mi - 'a'] == 0) {
                ++mi;
            }
            stk.push(c);
            while (!stk.isEmpty() && stk.peek() <= mi) {
                ans.append(stk.pop());
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
    string robotWithString(string s) {
        int cnt[26] = {0};
        for (char& c : s) ++cnt[c - 'a'];
        char mi = 'a';
        string stk;
        string ans;
        for (char& c : s) {
            --cnt[c - 'a'];
            while (mi < 'z' && cnt[mi - 'a'] == 0) ++mi;
            stk += c;
            while (!stk.empty() && stk.back() <= mi) {
                ans += stk.back();
                stk.pop_back();
            }
        }
        return ans;
    }
};
```

#### Go

```go
func robotWithString(s string) string {
	cnt := make([]int, 26)
	for _, c := range s {
		cnt[c-'a']++
	}
	mi := byte('a')
	stk := []byte{}
	ans := []byte{}
	for i := range s {
		cnt[s[i]-'a']--
		for mi < 'z' && cnt[mi-'a'] == 0 {
			mi++
		}
		stk = append(stk, s[i])
		for len(stk) > 0 && stk[len(stk)-1] <= mi {
			ans = append(ans, stk[len(stk)-1])
			stk = stk[:len(stk)-1]
		}
	}
	return string(ans)
}
```

#### TypeScript

```ts
function robotWithString(s: string): string {
    const cnt = new Map<string, number>();
    for (const c of s) {
        cnt.set(c, (cnt.get(c) || 0) + 1);
    }
    const ans: string[] = [];
    const stk: string[] = [];
    let mi = 'a';
    for (const c of s) {
        cnt.set(c, (cnt.get(c) || 0) - 1);
        while (mi < 'z' && (cnt.get(mi) || 0) === 0) {
            mi = String.fromCharCode(mi.charCodeAt(0) + 1);
        }
        stk.push(c);
        while (stk.length > 0 && stk[stk.length - 1] <= mi) {
            ans.push(stk.pop()!);
        }
    }
    return ans.join('');
}
```

#### Rust

```rust
impl Solution {
    pub fn robot_with_string(s: String) -> String {
        let mut cnt = [0; 26];
        for &c in s.as_bytes() {
            cnt[(c - b'a') as usize] += 1;
        }

        let mut ans = Vec::with_capacity(s.len());
        let mut stk = Vec::new();
        let mut mi = 0;

        for &c in s.as_bytes() {
            cnt[(c - b'a') as usize] -= 1;
            while mi < 26 && cnt[mi] == 0 {
                mi += 1;
            }
            stk.push(c);
            while let Some(&top) = stk.last() {
                if (top - b'a') as usize <= mi {
                    ans.push(stk.pop().unwrap());
                } else {
                    break;
                }
            }
        }

        String::from_utf8(ans).unwrap()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
