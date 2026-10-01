---
comments: true
difficulty: Medium
tags:
    - Stack
    - Recursion
    - String
---

<!-- problem:start -->

# [394. Decode String](https://leetcode.com/problems/decode-string)

[中文文档](/solution/0300-0399/0394.Decode%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi đã mã hóa, hãy trả về chuỗi sau khi giải mã.</p>

<p>Quy tắc mã hóa là: <code>k[encoded_string]</code>, trong đó <code>encoded_string</code> nằm trong ngoặc vuông được lặp lại đúng <code>k</code> lần. Đảm bảo <code>k</code> là số nguyên dương.</p>

<p>Có thể giả định chuỗi đầu vào luôn hợp lệ: không có khoảng trắng thừa, các cặp ngoặc vuông luôn đúng định dạng, v.v. Ngoài ra, dữ liệu gốc không chứa chữ số; chữ số chỉ dùng để biểu thị số lần lặp <code>k</code>. Chẳng hạn, đầu vào sẽ không có dạng <code>3a</code> hoặc <code>2[4]</code>.</p>

<p>Các test case được tạo sao cho độ dài chuỗi đầu ra không vượt quá <code>10<sup>5</sup></code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;3[a]2[bc]&quot;
<strong>Đầu ra:</strong> &quot;aaabcbc&quot;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;3[a2[c]]&quot;
<strong>Đầu ra:</strong> &quot;accaccacc&quot;
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;2[abc]3[cd]ef&quot;
<strong>Đầu ra:</strong> &quot;abcabccdcdcdef&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 30</code></li>
	<li><code>s</code> chỉ gồm chữ cái tiếng Anh viết thường, chữ số và dấu ngoặc vuông <code>&#39;[]&#39;</code>.</li>
	<li>Đảm bảo <code>s</code> là đầu vào <strong>hợp lệ</strong>.</li>
	<li>Tất cả số nguyên trong <code>s</code> đều nằm trong khoảng <code>[1, 300]</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Giải mã các chuỗi lặp $k[encoded]$ có thể lồng nhau. Có thể dùng đệ quy; hoặc dùng stack để lưu số lần lặp cùng chuỗi bên ngoài tương ứng.
>
> Các chữ số tạo thành `num`; khi gặp `[`, đẩy số lần lặp và kết quả hiện tại vào stack; khi gặp `]`, lấy chúng ra và nối đoạn lặp vào chuỗi; các chữ cái được nối trực tiếp. Stack giúp giải mã từ trong ra ngoài.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def decodeString(self, s: str) -> str:
        s1, s2 = [], []
        num, res = 0, ''
        for c in s:
            if c.isdigit():
                num = num * 10 + int(c)
            elif c == '[':
                s1.append(num)
                s2.append(res)
                num, res = 0, ''
            elif c == ']':
                res = s2.pop() + res * s1.pop()
            else:
                res += c
        return res
```

#### Java

```java
class Solution {
    public String decodeString(String s) {
        Deque<Integer> s1 = new ArrayDeque<>();
        Deque<String> s2 = new ArrayDeque<>();
        int num = 0;
        String res = "";
        for (char c : s.toCharArray()) {
            if ('0' <= c && c <= '9') {
                num = num * 10 + c - '0';
            } else if (c == '[') {
                s1.push(num);
                s2.push(res);
                num = 0;
                res = "";
            } else if (c == ']') {
                StringBuilder t = new StringBuilder();
                for (int i = 0, n = s1.pop(); i < n; ++i) {
                    t.append(res);
                }
                res = s2.pop() + t.toString();
            } else {
                res += String.valueOf(c);
            }
        }
        return res;
    }
}
```

#### TypeScript

```ts
function decodeString(s: string): string {
    let ans = '';
    let stack = [];
    let count = 0; // repeatCount
    for (let cur of s) {
        if (/[0-9]/.test(cur)) {
            count = count * 10 + Number(cur);
        } else if (/[a-z]/.test(cur)) {
            ans += cur;
        } else if ('[' == cur) {
            stack.push([ans, count]);
            // reset
            ans = '';
            count = 0;
        } else {
            // match ']'
            let [pre, count] = stack.pop();
            ans = pre + ans.repeat(count);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
