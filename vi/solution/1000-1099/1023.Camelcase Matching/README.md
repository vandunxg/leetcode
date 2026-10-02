---
comments: true
difficulty: Medium
rating: 1537
source: Weekly Contest 131 Q3
tags:
    - Trie
    - Array
    - Two Pointers
    - String
    - String Matching
---

<!-- problem:start -->

# [1023. Camelcase Matching](https://leetcode.com/problems/camelcase-matching)

[中文文档](/solution/1000-1099/1023.Camelcase%20Matching/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng chuỗi <code>queries</code> và chuỗi <code>pattern</code>. Trả về mảng boolean <code>answer</code>, trong đó <code>answer[i]</code> là <code>true</code> nếu <code>queries[i]</code> khớp với <code>pattern</code>, và là <code>false</code> nếu không.</p>

<p>Từ truy vấn <code>queries[i]</code> khớp với <code>pattern</code> nếu có thể chèn các chữ cái tiếng Anh viết thường vào <code>pattern</code> để tạo thành từ đó. Có thể chèn ký tự ở bất kỳ vị trí nào trong <code>pattern</code>, hoặc <strong>không chèn</strong> ký tự nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> queries = [&quot;FooBar&quot;,&quot;FooBarTest&quot;,&quot;FootBall&quot;,&quot;FrameBuffer&quot;,&quot;ForceFeedBack&quot;], pattern = &quot;FB&quot;
<strong>Đầu ra:</strong> [true,false,true,true,false]
<strong>Giải thích:</strong> Có thể tạo &quot;FooBar&quot; như sau: &quot;F&quot; + &quot;oo&quot; + &quot;B&quot; + &quot;ar&quot;.
Có thể tạo &quot;FootBall&quot; như sau: &quot;F&quot; + &quot;oot&quot; + &quot;B&quot; + &quot;all&quot;.
Có thể tạo &quot;FrameBuffer&quot; như sau: &quot;F&quot; + &quot;rame&quot; + &quot;B&quot; + &quot;uffer&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> queries = [&quot;FooBar&quot;,&quot;FooBarTest&quot;,&quot;FootBall&quot;,&quot;FrameBuffer&quot;,&quot;ForceFeedBack&quot;], pattern = &quot;FoBa&quot;
<strong>Đầu ra:</strong> [true,false,true,false,false]
<strong>Giải thích:</strong> Có thể tạo &quot;FooBar&quot; như sau: &quot;Fo&quot; + &quot;o&quot; + &quot;Ba&quot; + &quot;r&quot;.
Có thể tạo &quot;FootBall&quot; như sau: &quot;Fo&quot; + &quot;ot&quot; + &quot;Ba&quot; + &quot;ll&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> queries = [&quot;FooBar&quot;,&quot;FooBarTest&quot;,&quot;FootBall&quot;,&quot;FrameBuffer&quot;,&quot;ForceFeedBack&quot;], pattern = &quot;FoBaT&quot;
<strong>Đầu ra:</strong> [false,true,false,false,false]
<strong>Giải thích:</strong> Có thể tạo &quot;FooBarTest&quot; như sau: &quot;Fo&quot; + &quot;o&quot; + &quot;Ba&quot; + &quot;r&quot; + &quot;T&quot; + &quot;est&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= pattern.length, queries.length &lt;= 100</code></li>
	<li><code>1 &lt;= queries[i].length &lt;= 100</code></li>
	<li><code>queries[i]</code> và <code>pattern</code> chỉ gồm các chữ cái tiếng Anh.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Có thể kiểm tra mỗi query với pattern trong thời gian tuyến tính; số query và độ dài mỗi chuỗi đều không quá $100$. Các ký tự dư trong query chỉ có thể là chữ thường được chèn thêm, còn mọi ký tự của pattern phải xuất hiện đúng thứ tự.
>
> Dùng hai con trỏ duyệt query và pattern. Con trỏ trong query bỏ qua các ký tự viết thường không khớp; nếu đi quá cuối chuỗi hoặc gặp ký tự viết hoa không khớp thì thất bại. Sau khi duyệt hết pattern, phần hậu tố còn lại của query phải toàn chữ thường.
>
> Thực hiện phép kiểm tra này với từng query.

<!-- thinking:end -->

Duyệt từng chuỗi trong `queries` và kiểm tra xem có khớp với `pattern` hay không. Nếu khớp, thêm `true` vào mảng kết quả; nếu không, thêm `false`.

Tiếp theo, cài đặt hàm $check(s, t)$ để kiểm tra chuỗi $s$ có khớp với chuỗi $t$ hay không.

Dùng hai con trỏ $i$ và $j$ để duyệt hai chuỗi. Nếu ký tự tại hai vị trí không giống nhau và $s[i]$ là chữ thường, tăng con trỏ $i$ lên vị trí tiếp theo.

Nếu con trỏ $i$ đã đến cuối chuỗi $s$ hoặc hai ký tự đang xét không giống nhau, trả về `false`. Nếu không, tăng cả $i$ và $j$. Khi con trỏ $j$ đến cuối chuỗi $t$, cần kiểm tra các ký tự còn lại trong $s$ có đều là chữ thường không. Nếu đúng, trả về `true`; nếu không, trả về `false`.

Độ phức tạp thời gian là $O(n \times m)$, trong đó $n$ là số chuỗi trong mảng `queries`, còn $m$ là độ dài chuỗi `pattern`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def camelMatch(self, queries: List[str], pattern: str) -> List[bool]:
        def check(s, t):
            m, n = len(s), len(t)
            i = j = 0
            while j < n:
                while i < m and s[i] != t[j] and s[i].islower():
                    i += 1
                if i == m or s[i] != t[j]:
                    return False
                i, j = i + 1, j + 1
            while i < m and s[i].islower():
                i += 1
            return i == m

        return [check(q, pattern) for q in queries]
```

#### Java

```java
class Solution {
    public List<Boolean> camelMatch(String[] queries, String pattern) {
        List<Boolean> ans = new ArrayList<>();
        for (var q : queries) {
            ans.add(check(q, pattern));
        }
        return ans;
    }

    private boolean check(String s, String t) {
        int m = s.length(), n = t.length();
        int i = 0, j = 0;
        for (; j < n; ++i, ++j) {
            while (i < m && s.charAt(i) != t.charAt(j) && Character.isLowerCase(s.charAt(i))) {
                ++i;
            }
            if (i == m || s.charAt(i) != t.charAt(j)) {
                return false;
            }
        }
        while (i < m && Character.isLowerCase(s.charAt(i))) {
            ++i;
        }
        return i == m;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<bool> camelMatch(vector<string>& queries, string pattern) {
        vector<bool> ans;
        auto check = [](string& s, string& t) {
            int m = s.size(), n = t.size();
            int i = 0, j = 0;
            for (; j < n; ++i, ++j) {
                while (i < m && s[i] != t[j] && islower(s[i])) {
                    ++i;
                }
                if (i == m || s[i] != t[j]) {
                    return false;
                }
            }
            while (i < m && islower(s[i])) {
                ++i;
            }
            return i == m;
        };
        for (auto& q : queries) {
            ans.push_back(check(q, pattern));
        }
        return ans;
    }
};
```

#### Go

```go
func camelMatch(queries []string, pattern string) (ans []bool) {
	check := func(s, t string) bool {
		m, n := len(s), len(t)
		i, j := 0, 0
		for ; j < n; i, j = i+1, j+1 {
			for i < m && s[i] != t[j] && (s[i] >= 'a' && s[i] <= 'z') {
				i++
			}
			if i == m || s[i] != t[j] {
				return false
			}
		}
		for i < m && s[i] >= 'a' && s[i] <= 'z' {
			i++
		}
		return i == m
	}
	for _, q := range queries {
		ans = append(ans, check(q, pattern))
	}
	return
}
```

#### TypeScript

```ts
function camelMatch(queries: string[], pattern: string): boolean[] {
    const check = (s: string, t: string) => {
        const m = s.length;
        const n = t.length;
        let i = 0;
        let j = 0;
        for (; j < n; ++i, ++j) {
            while (i < m && s[i] !== t[j] && s[i].codePointAt(0) >= 97) {
                ++i;
            }
            if (i === m || s[i] !== t[j]) {
                return false;
            }
        }
        while (i < m && s[i].codePointAt(0) >= 97) {
            ++i;
        }
        return i == m;
    };
    const ans: boolean[] = [];
    for (const q of queries) {
        ans.push(check(q, pattern));
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
