---
comments: true
difficulty: Medium
tags:
    - String
    - String Matching
    - KMP
    - Boyer–Moore
    - Extended KMP
---

<!-- problem:start -->

# [686. Repeated String Match](https://leetcode.com/problems/repeated-string-match)

[中文文档](/solution/0600-0699/0686.Repeated%20String%20Match/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>a</code> và <code>b</code>, hãy trả về <em>số lần lặp chuỗi </em><code>a</code><em> ít nhất để chuỗi </em><code>b</code> <em>trở thành chuỗi con của nó</em>. Nếu không thể khiến <code>b</code> là chuỗi con của <code>a</code> sau khi lặp, trả về <code>-1</code>.</p>

<p><strong>Lưu ý:</strong> Chuỗi <code>&quot;abc&quot;</code> lặp 0 lần là <code>&quot;&quot;</code>, lặp 1 lần là <code>&quot;abc&quot;</code> và lặp 2 lần là <code>&quot;abcabc&quot;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> a = &quot;abcd&quot;, b = &quot;cdabcdab&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta trả về 3 vì khi lặp a ba lần, &quot;ab<strong>cdabcdab</strong>cd&quot; chứa b như một chuỗi con.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> a = &quot;a&quot;, b = &quot;aa&quot;
<strong>Đầu ra:</strong> 2
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= a.length, b.length &lt;= 10<sup>4</sup></code></li>
	<li><code>a</code> và <code>b</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> $b$ phải xuất hiện trong một chuỗi được tạo bằng cách lặp $a$. Cần ít nhất $\lceil |b|/|a|\rceil$ bản sao, rồi có thể cần thêm vài bản sao để khớp trường hợp vắt qua ranh giới giữa các lần lặp.
>
> Bắt đầu từ cận dưới đó và nối thêm $a$ tối đa ba lần. Nếu `b` là chuỗi con, trả về số lần lặp; nếu không thì trả về $-1$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def repeatedStringMatch(self, a: str, b: str) -> int:
        m, n = len(a), len(b)
        ans = ceil(n / m)
        t = [a] * ans
        for _ in range(3):
            if b in ''.join(t):
                return ans
            ans += 1
            t.append(a)
        return -1
```

#### Java

```java
class Solution {
    public int repeatedStringMatch(String a, String b) {
        int m = a.length(), n = b.length();
        int ans = (n + m - 1) / m;
        StringBuilder t = new StringBuilder(a.repeat(ans));
        for (int i = 0; i < 3; ++i) {
            if (t.toString().contains(b)) {
                return ans;
            }
            ++ans;
            t.append(a);
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int repeatedStringMatch(string a, string b) {
        int m = a.size(), n = b.size();
        int ans = (n + m - 1) / m;
        string t = "";
        for (int i = 0; i < ans; ++i) t += a;
        for (int i = 0; i < 3; ++i) {
            if (t.find(b) != -1) return ans;
            ++ans;
            t += a;
        }
        return -1;
    }
};
```

#### Go

```go
func repeatedStringMatch(a string, b string) int {
	m, n := len(a), len(b)
	ans := (n + m - 1) / m
	t := strings.Repeat(a, ans)
	for i := 0; i < 3; i++ {
		if strings.Contains(t, b) {
			return ans
		}
		ans++
		t += a
	}
	return -1
}
```

#### TypeScript

```ts
function repeatedStringMatch(a: string, b: string): number {
    const m: number = a.length,
        n: number = b.length;
    let ans: number = Math.ceil(n / m);
    let t: string = a.repeat(ans);

    for (let i = 0; i < 3; i++) {
        if (t.includes(b)) {
            return ans;
        }
        ans++;
        t += a;
    }

    return -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
