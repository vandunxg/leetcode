---
comments: true
difficulty: Easy
tags:
    - String
---

<!-- problem:start -->

# [434. Number of Segments in a String](https://leetcode.com/problems/number-of-segments-in-a-string)

[中文文档](/solution/0400-0499/0434.Number%20of%20Segments%20in%20a%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code>, hãy trả về số đoạn trong chuỗi.</p>

<p><strong>Đoạn</strong> là một dãy liên tiếp gồm các <strong>ký tự không phải dấu cách</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;Hello, my name is John&quot;
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Năm đoạn là [&quot;Hello,&quot;, &quot;my&quot;, &quot;name&quot;, &quot;is&quot;, &quot;John&quot;]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;Hello&quot;
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= s.length &lt;= 300</code></li>
	<li><code>s</code> gồm chữ cái tiếng Anh viết thường hoặc viết hoa, chữ số, hoặc một trong các ký tự sau: <code>&quot;!@#$%^&amp;*()_+-=&#39;,.:&quot;</code>.</li>
	<li>Ký tự khoảng trắng duy nhất trong <code>s</code> là <code>&#39; &#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tách chuỗi

<!-- thinking:start -->

> **Tư duy**
>
> Các đoạn được phân tách bằng dấu cách. Nếu tự duyệt, ta phải xử lý dấu cách ở đầu, ở cuối và các dấu cách liên tiếp. $\texttt{split}$ đã loại bỏ các phần tử rỗng, nên chỉ cần lấy độ dài kết quả.
>
> Dùng hàm có sẵn của thư viện là một lời giải đầu tiên phù hợp.

<!-- thinking:end -->

Ta tách chuỗi $\textit{s}$ theo dấu cách rồi đếm số đoạn không rỗng.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi $\textit{s}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSegments(self, s: str) -> int:
        return len(s.split())
```

#### Java

```java
class Solution {
    public int countSegments(String s) {
        int ans = 0;
        for (String t : s.split(" ")) {
            if (!"".equals(t)) {
                ++ans;
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
    int countSegments(string s) {
        int ans = 0;
        istringstream ss(s);
        while (ss >> s) ++ans;
        return ans;
    }
};
```

#### Go

```go
func countSegments(s string) int {
	ans := 0
	for _, t := range strings.Split(s, " ") {
		if len(t) > 0 {
			ans++
		}
	}
	return ans
}
```

#### TypeScript

```ts
function countSegments(s: string): number {
    return s.split(/\s+/).filter(Boolean).length;
}
```

#### PHP

```php
class Solution {
    /**
     * @param String $s
     * @return Integer
     */
    function countSegments($s) {
        $arr = explode(' ', $s);
        $cnt = 0;
        for ($i = 0; $i < count($arr); $i++) {
            if (strlen($arr[$i]) != 0) {
                $cnt++;
            }
        }
        return $cnt;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 tạo một danh sách các từ. Nếu chỉ đếm, một đoạn mới bắt đầu tại ký tự không phải dấu cách mà ký tự trước đó là dấu cách (hoặc tại chỉ số đầu tiên). Bộ nhớ phụ giảm còn $O(1)$.

<!-- thinking:end -->

Ta cũng có thể duyệt trực tiếp từng ký tự $\text{s[i]}$ trong chuỗi. Nếu $\text{s[i]}$ không phải dấu cách và $\text{s[i-1]}$ là dấu cách hoặc $i = 0$, thì $\text{s[i]}$ đánh dấu điểm bắt đầu của một đoạn mới, nên ta tăng đáp án thêm một.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi $\textit{s}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSegments(self, s: str) -> int:
        ans = 0
        for i, c in enumerate(s):
            if c != ' ' and (i == 0 or s[i - 1] == ' '):
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int countSegments(String s) {
        int ans = 0;
        for (int i = 0; i < s.length(); ++i) {
            if (s.charAt(i) != ' ' && (i == 0 || s.charAt(i - 1) == ' ')) {
                ++ans;
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
    int countSegments(string s) {
        int ans = 0;
        for (int i = 0; i < s.size(); ++i) {
            if (s[i] != ' ' && (i == 0 || s[i - 1] == ' ')) {
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countSegments(s string) int {
	ans := 0
	for i, c := range s {
		if c != ' ' && (i == 0 || s[i-1] == ' ') {
			ans++
		}
	}
	return ans
}
```

#### TypeScript

```ts
function countSegments(s: string): number {
    let ans = 0;
    for (let i = 0; i < s.length; i++) {
        let c = s[i];
        if (c !== ' ' && (i === 0 || s[i - 1] === ' ')) {
            ans++;
        }
    }
    return ans;
}
```

#### PHP

```php
class Solution {
    /**
     * @param String $s
     * @return Integer
     */
    function countSegments($s) {
        $ans = 0;
        $n = strlen($s);
        for ($i = 0; $i < $n; $i++) {
            $c = $s[$i];
            if ($c !== ' ' && ($i === 0 || $s[$i - 1] === ' ')) {
                $ans++;
            }
        }
        return $ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
