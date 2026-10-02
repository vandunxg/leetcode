---
comments: true
difficulty: Easy
rating: 1368
source: Weekly Contest 205 Q1
tags:
    - String
---

<!-- problem:start -->

# [1576. Replace All 's to Avoid Consecutive Repeating Characters](https://leetcode.com/problems/replace-all-s-to-avoid-consecutive-repeating-characters)

[中文文档](/solution/1500-1599/1576.Replace%20All%20%27s%20to%20Avoid%20Consecutive%20Repeating%20Characters/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> chỉ chứa các chữ cái tiếng Anh viết thường và ký tự <code>&#39;?&#39;</code>, hãy chuyển <strong>tất cả</strong> ký tự <code>&#39;?&#39;</code> thành chữ cái viết thường sao cho chuỗi cuối cùng không có các ký tự <strong>lặp liên tiếp</strong>. Bạn <strong>không thể</strong> sửa các ký tự khác <code>&#39;?&#39;</code>.</p>

<p>Đề bài <strong>đảm bảo</strong> chuỗi đã cho không có ký tự lặp liên tiếp <strong>ngoại trừ</strong> <code>&#39;?&#39;</code>.</p>

<p>Trả về <em>chuỗi cuối cùng sau khi thực hiện tất cả các phép chuyển đổi (có thể bằng không)</em>. Nếu có nhiều đáp án, hãy trả về <strong>bất kỳ đáp án nào</strong>. Có thể chứng minh rằng luôn tồn tại đáp án với các ràng buộc đã cho.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;?zs&quot;
<strong>Đầu ra:</strong> &quot;azs&quot;
<strong>Giải thích:</strong> Có 25 đáp án cho bài toán này. Từ &quot;azs&quot; đến &quot;yzs&quot; đều hợp lệ. Chỉ thay bằng &quot;z&quot; là không hợp lệ vì chuỗi sẽ có các ký tự lặp liên tiếp trong &quot;zzs&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;ubv?w&quot;
<strong>Đầu ra:</strong> &quot;ubvaw&quot;
<strong>Giải thích:</strong> Có 24 đáp án cho bài toán này. Chỉ thay bằng &quot;v&quot; và &quot;w&quot; là không hợp lệ vì chuỗi sẽ có các ký tự lặp liên tiếp trong &quot;ubvvw&quot; và &quot;ubvww&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> gồm các chữ cái tiếng Anh viết thường và <code>&#39;?&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Thay mỗi dấu hỏi bằng một chữ cái viết thường sao cho không có hai ký tự kề nhau bằng nhau. Một vị trí trống có nhiều nhất hai hàng xóm khác nhau, nên một trong các chữ $\texttt{a},\texttt{b},\texttt{c}$ luôn dùng được.
>
> Điền từ trái sang phải: thử ba ứng viên và bỏ qua ứng viên bằng $s[i-1]$ hoặc $s[i+1]$ chưa được thay thế. Điền phía bên trái trước giúp vị trí trống tiếp theo luôn thấy được hàng xóm đã xác định.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def modifyString(self, s: str) -> str:
        s = list(s)
        n = len(s)
        for i in range(n):
            if s[i] == "?":
                for c in "abc":
                    if (i and s[i - 1] == c) or (i + 1 < n and s[i + 1] == c):
                        continue
                    s[i] = c
                    break
        return "".join(s)
```

#### Java

```java
class Solution {
    public String modifyString(String s) {
        char[] cs = s.toCharArray();
        int n = cs.length;
        for (int i = 0; i < n; ++i) {
            if (cs[i] == '?') {
                for (char c = 'a'; c <= 'c'; ++c) {
                    if ((i > 0 && cs[i - 1] == c) || (i + 1 < n && cs[i + 1] == c)) {
                        continue;
                    }
                    cs[i] = c;
                    break;
                }
            }
        }
        return String.valueOf(cs);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string modifyString(string s) {
        int n = s.size();
        for (int i = 0; i < n; ++i) {
            if (s[i] == '?') {
                for (char c : "abc") {
                    if ((i && s[i - 1] == c) || (i + 1 < n && s[i + 1] == c)) {
                        continue;
                    }
                    s[i] = c;
                    break;
                }
            }
        }
        return s;
    }
};
```

#### Go

```go
func modifyString(s string) string {
	n := len(s)
	cs := []byte(s)
	for i := range s {
		if cs[i] == '?' {
			for c := byte('a'); c <= byte('c'); c++ {
				if (i > 0 && cs[i-1] == c) || (i+1 < n && cs[i+1] == c) {
					continue
				}
				cs[i] = c
				break
			}
		}
	}
	return string(cs)
}
```

#### TypeScript

```ts
function modifyString(s: string): string {
    const cs = s.split('');
    const n = s.length;
    for (let i = 0; i < n; ++i) {
        if (cs[i] === '?') {
            for (const c of 'abc') {
                if ((i > 0 && cs[i - 1] === c) || (i + 1 < n && cs[i + 1] === c)) {
                    continue;
                }
                cs[i] = c;
                break;
            }
        }
    }
    return cs.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
