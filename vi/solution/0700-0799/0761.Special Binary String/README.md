---
comments: true
difficulty: Hard
tags:
    - String
    - Divide and Conquer
    - Sorting
---

<!-- problem:start -->

# [761. Special Binary String](https://leetcode.com/problems/special-binary-string)

[中文文档](/solution/0700-0799/0761.Special%20Binary%20String/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Chuỗi nhị phân đặc biệt</strong> là chuỗi nhị phân có hai tính chất sau:</p>

<ul>
	<li>Số lượng <code>0</code> bằng số lượng <code>1</code>.</li>
	<li>Mọi tiền tố của chuỗi nhị phân đều có số lượng <code>1</code> không ít hơn số lượng <code>0</code>.</li>
</ul>

<p>Cho chuỗi <code>s</code> là một <strong>chuỗi nhị phân đặc biệt</strong>.</p>

<p>Mỗi thao tác chọn hai chuỗi con đặc biệt không rỗng, nằm liền kề nhau trong <code>s</code>, rồi hoán đổi vị trí của chúng. Hai chuỗi được xem là liền kề nếu ký tự cuối của chuỗi thứ nhất nằm ngay trước ký tự đầu của chuỗi thứ hai.</p>

<p>Hãy trả về <em>chuỗi lớn nhất theo thứ tự từ điển có thể thu được sau khi thực hiện các thao tác trên</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;11011000&quot;
<strong>Đầu ra:</strong> &quot;11100100&quot;
<strong>Giải thích:</strong> Hai chuỗi &quot;10&quot; [xuất hiện tại s[1]] và &quot;1100&quot; [tại s[3]] được hoán đổi.
Đây là chuỗi lớn nhất theo thứ tự từ điển có thể đạt được sau một số lần hoán đổi.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;10&quot;
<strong>Đầu ra:</strong> &quot;10&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 50</code></li>
	<li><code>s[i]</code> là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code>.</li>
	<li><code>s</code> là chuỗi nhị phân đặc biệt.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi nhị phân đặc biệt tương đương với chuỗi ngoặc hợp lệ, trong đó $1$/$0$ lần lượt đại diện cho ngoặc mở/đóng. Hoán đổi các phần đặc biệt liền kề giúp tăng thứ tự từ điển. $n\le 50$, nhưng cấu trúc đệ quy mới là điểm mấu chốt.
>
> Chuỗi là phép nối của các khối có dạng $1+\textit{special}+0$. Tối ưu phần bên trong từng khối, rồi sắp xếp các khối đó theo thứ tự giảm dần.
>
> Dùng biến đếm độ cân bằng để tách các khối; đệ quy xử lý phần bên trong, sắp xếp rồi nối lại.

<!-- thinking:end -->

Có thể xem chuỗi nhị phân đặc biệt như một chuỗi ngoặc hợp lệ, trong đó $1$ là ngoặc mở và $0$ là ngoặc đóng. Ví dụ, "11011000" tương ứng với "(()(()))".

Hoán đổi hai chuỗi con đặc biệt không rỗng liền kề tương đương với hoán đổi hai chuỗi ngoặc hợp lệ liền kề. Ta có thể dùng đệ quy để giải bài toán này.

Ta xem mỗi "chuỗi ngoặc hợp lệ" trong chuỗi $s$ là một phần, xử lý đệ quy từng phần rồi sắp xếp chúng để thu được đáp án cuối cùng.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def makeLargestSpecial(self, s: str) -> str:
        if s == '':
            return ''
        ans = []
        cnt = 0
        i = j = 0
        while i < len(s):
            cnt += 1 if s[i] == '1' else -1
            if cnt == 0:
                ans.append('1' + self.makeLargestSpecial(s[j + 1 : i]) + '0')
                j = i + 1
            i += 1
        ans.sort(reverse=True)
        return ''.join(ans)
```

#### Java

```java
class Solution {
    public String makeLargestSpecial(String s) {
        if ("".equals(s)) {
            return "";
        }
        List<String> ans = new ArrayList<>();
        int cnt = 0;
        for (int i = 0, j = 0; i < s.length(); ++i) {
            cnt += s.charAt(i) == '1' ? 1 : -1;
            if (cnt == 0) {
                String t = "1" + makeLargestSpecial(s.substring(j + 1, i)) + "0";
                ans.add(t);
                j = i + 1;
            }
        }
        ans.sort(Comparator.reverseOrder());
        return String.join("", ans);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string makeLargestSpecial(string s) {
        if (s == "") {
            return s;
        }
        vector<string> ans;
        int cnt = 0;
        for (int i = 0, j = 0; i < s.size(); ++i) {
            cnt += s[i] == '1' ? 1 : -1;
            if (cnt == 0) {
                ans.push_back("1" + makeLargestSpecial(s.substr(j + 1, i - j - 1)) + "0");
                j = i + 1;
            }
        }
        sort(ans.begin(), ans.end(), greater<string>{});
        return accumulate(ans.begin(), ans.end(), ""s);
    }
};
```

#### Go

```go
func makeLargestSpecial(s string) string {
	if s == "" {
		return ""
	}
	ans := sort.StringSlice{}
	cnt := 0
	for i, j := 0, 0; i < len(s); i++ {
		if s[i] == '1' {
			cnt++
		} else {
			cnt--
		}
		if cnt == 0 {
			ans = append(ans, "1"+makeLargestSpecial(s[j+1:i])+"0")
			j = i + 1
		}
	}
	sort.Sort(sort.Reverse(ans))
	return strings.Join(ans, "")
}
```

#### TypeScript

```ts
function makeLargestSpecial(s: string): string {
    if (s.length === 0) {
        return '';
    }

    const ans: string[] = [];
    let cnt = 0;

    for (let i = 0, j = 0; i < s.length; ++i) {
        cnt += s[i] === '1' ? 1 : -1;
        if (cnt === 0) {
            const t = '1' + makeLargestSpecial(s.substring(j + 1, i)) + '0';
            ans.push(t);
            j = i + 1;
        }
    }

    ans.sort((a, b) => b.localeCompare(a));
    return ans.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
