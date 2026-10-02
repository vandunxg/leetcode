---
comments: true
difficulty: Hard
tags:
    - Greedy
    - String
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [420. Strong Password Checker](https://leetcode.com/problems/strong-password-checker)

[中文文档](/solution/0400-0499/0420.Strong%20Password%20Checker/README.md)

## Mô tả

<!-- description:start -->

<p>Một mật khẩu được xem là mạnh khi thỏa mãn tất cả các điều kiện sau:</p>

<ul>
	<li>Mật khẩu có ít nhất <code>6</code> ký tự và nhiều nhất <code>20</code> ký tự.</li>
	<li>Mật khẩu chứa ít nhất <strong>một chữ thường</strong>, ít nhất <strong>một chữ hoa</strong> và ít nhất <strong>một chữ số</strong>.</li>
	<li>Mật khẩu không chứa ba ký tự giống nhau liên tiếp (ví dụ, <code>&quot;B<u><strong>aaa</strong></u>bb0&quot;</code> là mật khẩu yếu, còn <code>&quot;B<strong><u>aa</u></strong>b<u><strong>a</strong></u>0&quot;</code> là mật khẩu mạnh).</li>
</ul>

<p>Cho chuỗi <code>password</code>, hãy trả về số bước ít nhất cần thực hiện để biến <code>password</code> thành mật khẩu mạnh. Nếu <code>password</code> đã mạnh, trả về <code>0</code>.</p>

<p>Mỗi bước, bạn có thể:</p>

<ul>
	<li>Chèn một ký tự vào <code>password</code>,</li>
	<li>Xóa một ký tự khỏi <code>password</code>, hoặc</li>
	<li>Thay một ký tự trong <code>password</code> bằng ký tự khác.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> password = "a"
<strong>Đầu ra:</strong> 5
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> password = "aA1"
<strong>Đầu ra:</strong> 3
</pre><p><strong class="example">Ví dụ 3:</strong></p>
<pre><strong>Đầu vào:</strong> password = "1337C0d3"
<strong>Đầu ra:</strong> 0
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= password.length &lt;= 50</code></li>
	<li><code>password</code> chỉ gồm chữ cái, chữ số, dấu chấm&nbsp;<code>&#39;.&#39;</code> hoặc dấu chấm than <code>&#39;!&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Solution 1

<!-- thinking:start -->

> **Tư duy**
>
> Mật khẩu mạnh bị ràng buộc về độ dài, nhóm ký tự và các đoạn lặp liên tiếp từ ba ký tự trở lên. Chèn, xóa và thay thế xử lý các điều kiện thiếu theo những cách khác nhau, nên không thể chỉ dùng một loại thao tác.
>
> Chia trường hợp theo độ dài. Nếu $n<6$, thao tác chèn vừa tăng độ dài vừa bổ sung các nhóm ký tự còn thiếu. Nếu $6\le n\le 20$, dùng thay thế để phá các đoạn lặp liên tiếp có độ dài ít nhất $3$, rồi lấy giá trị lớn hơn giữa số lần thay thế cần thiết và số nhóm ký tự còn thiếu. Nếu $n>20$, bắt buộc phải xóa ký tự; ưu tiên xóa trước ở các chuỗi có độ dài $0\bmod 3$, vì một lần xóa sẽ giảm số lần thay thế cần thiết đi một.
>
> Chia trường hợp theo độ dài giúp xác định thao tác thực sự cần thiết trong từng trường hợp.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def strongPasswordChecker(self, password: str) -> int:
        def countTypes(s):
            a = b = c = 0
            for ch in s:
                if ch.islower():
                    a = 1
                elif ch.isupper():
                    b = 1
                elif ch.isdigit():
                    c = 1
            return a + b + c

        types = countTypes(password)
        n = len(password)
        if n < 6:
            return max(6 - n, 3 - types)
        if n <= 20:
            replace = cnt = 0
            prev = '~'
            for curr in password:
                if curr == prev:
                    cnt += 1
                else:
                    replace += cnt // 3
                    cnt = 1
                    prev = curr
            replace += cnt // 3
            return max(replace, 3 - types)
        replace = cnt = 0
        remove, remove2 = n - 20, 0
        prev = '~'
        for curr in password:
            if curr == prev:
                cnt += 1
            else:
                if remove > 0 and cnt >= 3:
                    if cnt % 3 == 0:
                        remove -= 1
                        replace -= 1
                    elif cnt % 3 == 1:
                        remove2 += 1
                replace += cnt // 3
                cnt = 1
                prev = curr
        if remove > 0 and cnt >= 3:
            if cnt % 3 == 0:
                remove -= 1
                replace -= 1
            elif cnt % 3 == 1:
                remove2 += 1
        replace += cnt // 3
        use2 = min(replace, remove2, remove // 2)
        replace -= use2
        remove -= use2 * 2

        use3 = min(replace, remove // 3)
        replace -= use3
        remove -= use3 * 3
        return n - 20 + max(replace, 3 - types)
```

#### Java

```java
class Solution {
    public int strongPasswordChecker(String password) {
        int types = countTypes(password);
        int n = password.length();
        if (n < 6) {
            return Math.max(6 - n, 3 - types);
        }
        char[] chars = password.toCharArray();
        if (n <= 20) {
            int replace = 0;
            int cnt = 0;
            char prev = '~';
            for (char curr : chars) {
                if (curr == prev) {
                    ++cnt;
                } else {
                    replace += cnt / 3;
                    cnt = 1;
                    prev = curr;
                }
            }
            replace += cnt / 3;
            return Math.max(replace, 3 - types);
        }
        int replace = 0, remove = n - 20;
        int remove2 = 0;
        int cnt = 0;
        char prev = '~';
        for (char curr : chars) {
            if (curr == prev) {
                ++cnt;
            } else {
                if (remove > 0 && cnt >= 3) {
                    if (cnt % 3 == 0) {
                        --remove;
                        --replace;
                    } else if (cnt % 3 == 1) {
                        ++remove2;
                    }
                }
                replace += cnt / 3;
                cnt = 1;
                prev = curr;
            }
        }
        if (remove > 0 && cnt >= 3) {
            if (cnt % 3 == 0) {
                --remove;
                --replace;
            } else if (cnt % 3 == 1) {
                ++remove2;
            }
        }
        replace += cnt / 3;

        int use2 = Math.min(Math.min(replace, remove2), remove / 2);
        replace -= use2;
        remove -= use2 * 2;

        int use3 = Math.min(replace, remove / 3);
        replace -= use3;
        remove -= use3 * 3;
        return (n - 20) + Math.max(replace, 3 - types);
    }

    private int countTypes(String s) {
        int a = 0, b = 0, c = 0;
        for (char ch : s.toCharArray()) {
            if (Character.isLowerCase(ch)) {
                a = 1;
            } else if (Character.isUpperCase(ch)) {
                b = 1;
            } else if (Character.isDigit(ch)) {
                c = 1;
            }
        }
        return a + b + c;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int strongPasswordChecker(string password) {
        int types = countTypes(password);
        int n = password.size();
        if (n < 6) return max(6 - n, 3 - types);
        if (n <= 20) {
            int replace = 0, cnt = 0;
            char prev = '~';
            for (char& curr : password) {
                if (curr == prev)
                    ++cnt;
                else {
                    replace += cnt / 3;
                    cnt = 1;
                    prev = curr;
                }
            }
            replace += cnt / 3;
            return max(replace, 3 - types);
        }
        int replace = 0, remove = n - 20;
        int remove2 = 0;
        int cnt = 0;
        char prev = '~';
        for (char& curr : password) {
            if (curr == prev)
                ++cnt;
            else {
                if (remove > 0 && cnt >= 3) {
                    if (cnt % 3 == 0) {
                        --remove;
                        --replace;
                    } else if (cnt % 3 == 1)
                        ++remove2;
                }
                replace += cnt / 3;
                cnt = 1;
                prev = curr;
            }
        }
        if (remove > 0 && cnt >= 3) {
            if (cnt % 3 == 0) {
                --remove;
                --replace;
            } else if (cnt % 3 == 1)
                ++remove2;
        }
        replace += cnt / 3;

        int use2 = min(min(replace, remove2), remove / 2);
        replace -= use2;
        remove -= use2 * 2;

        int use3 = min(replace, remove / 3);
        replace -= use3;
        remove -= use3 * 3;
        return (n - 20) + max(replace, 3 - types);
    }

    int countTypes(string& s) {
        int a = 0, b = 0, c = 0;
        for (char& ch : s) {
            if (islower(ch))
                a = 1;
            else if (isupper(ch))
                b = 1;
            else if (isdigit(ch))
                c = 1;
        }
        return a + b + c;
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
