---
comments: true
difficulty: Medium
rating: 1328
source: Weekly Contest 172 Q2
tags:
    - Array
    - String
    - Simulation
---

<!-- problem:start -->

# [1324. Print Words Vertically](https://leetcode.com/problems/print-words-vertically)

[中文文档](/solution/1300-1399/1324.Print%20Words%20Vertically/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code>.&nbsp;Trả về tất cả các từ theo chiều dọc, giữ nguyên thứ tự xuất hiện trong <code>s</code>.<br />
Kết quả là một danh sách chuỗi, có thêm&nbsp;khoảng trắng khi cần. (Không được có khoảng trắng ở cuối chuỗi).<br />
Mỗi từ chỉ nằm trên một cột và mỗi cột chỉ chứa một từ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;HOW ARE YOU&quot;
<strong>Đầu ra:</strong> [&quot;HAY&quot;,&quot;ORO&quot;,&quot;WEU&quot;]
<strong>Giải thích: </strong>Mỗi từ được in theo chiều dọc. 
 &quot;HAY&quot;
&nbsp;&quot;ORO&quot;
&nbsp;&quot;WEU&quot;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;TO BE OR NOT TO BE&quot;
<strong>Đầu ra:</strong> [&quot;TBONTB&quot;,&quot;OEROOE&quot;,&quot;   T&quot;]
<strong>Giải thích: </strong>Không được có khoảng trắng ở cuối. 
&quot;TBONTB&quot;
&quot;OEROOE&quot;
&quot;   T&quot;
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;CONTEST IS COMING&quot;
<strong>Đầu ra:</strong> [&quot;CIC&quot;,&quot;OSO&quot;,&quot;N M&quot;,&quot;T I&quot;,&quot;E N&quot;,&quot;S G&quot;,&quot;T&quot;]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 200</code></li>
	<li><code>s</code>&nbsp;chỉ chứa các chữ cái tiếng Anh viết hoa.</li>
	<li>Đảm bảo chỉ có một&nbsp;khoảng trắng giữa hai từ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Sau khi tách chuỗi, ta đọc các từ theo từng cột; chiều cao là độ dài của từ dài nhất, các từ ngắn hơn được đệm thêm khoảng trắng, rồi xóa khoảng trắng ở cuối mỗi cột. Sau khi tính độ dài đó là $n$, cột $j$ lấy ký tự thứ $j$ của mỗi từ (hoặc một khoảng trắng), sau đó xóa các khoảng trắng cuối trước khi ghép thành chuỗi.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def printVertically(self, s: str) -> List[str]:
        words = s.split()
        n = max(len(w) for w in words)
        ans = []
        for j in range(n):
            t = [w[j] if j < len(w) else ' ' for w in words]
            while t[-1] == ' ':
                t.pop()
            ans.append(''.join(t))
        return ans
```

#### Java

```java
class Solution {
    public List<String> printVertically(String s) {
        String[] words = s.split(" ");
        int n = 0;
        for (var w : words) {
            n = Math.max(n, w.length());
        }
        List<String> ans = new ArrayList<>();
        for (int j = 0; j < n; ++j) {
            StringBuilder t = new StringBuilder();
            for (var w : words) {
                t.append(j < w.length() ? w.charAt(j) : ' ');
            }
            while (t.length() > 0 && t.charAt(t.length() - 1) == ' ') {
                t.deleteCharAt(t.length() - 1);
            }
            ans.add(t.toString());
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> printVertically(string s) {
        stringstream ss(s);
        vector<string> words;
        string word;
        int n = 0;
        while (ss >> word) {
            words.emplace_back(word);
            n = max(n, (int) word.size());
        }
        vector<string> ans;
        for (int j = 0; j < n; ++j) {
            string t;
            for (auto& w : words) {
                t += j < w.size() ? w[j] : ' ';
            }
            while (t.size() && t.back() == ' ') {
                t.pop_back();
            }
            ans.emplace_back(t);
        }
        return ans;
    }
};
```

#### Go

```go
func printVertically(s string) (ans []string) {
	words := strings.Split(s, " ")
	n := 0
	for _, w := range words {
		n = max(n, len(w))
	}
	for j := 0; j < n; j++ {
		t := []byte{}
		for _, w := range words {
			if j < len(w) {
				t = append(t, w[j])
			} else {
				t = append(t, ' ')
			}
		}
		for len(t) > 0 && t[len(t)-1] == ' ' {
			t = t[:len(t)-1]
		}
		ans = append(ans, string(t))
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
