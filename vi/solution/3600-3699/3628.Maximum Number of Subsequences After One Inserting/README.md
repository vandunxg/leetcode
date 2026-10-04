---
comments: true
difficulty: Medium
rating: 1753
source: Weekly Contest 460 Q2
tags:
    - Greedy
    - String
    - Dynamic Programming
    - Prefix Sum
---

<!-- problem:start -->

# [3628. Maximum Number of Subsequences After One Inserting](https://leetcode.com/problems/maximum-number-of-subsequences-after-one-inserting)

[Tài liệu tiếng Trung](/solution/3600-3699/3628.Maximum%20Number%20of%20Subsequences%20After%20One%20Inserting/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một chuỗi <code>s</code> chỉ gồm các chữ cái tiếng Anh viết hoa.</p>

<p>Bạn được phép chèn <strong>nhiều nhất một</strong> chữ cái tiếng Anh viết hoa vào <strong>bất kỳ</strong> vị trí nào (bao gồm cả đầu hoặc cuối) của chuỗi.</p>

<p>Hãy trả về số lượng <strong>lớn nhất</strong> các <span data-keyword="subsequence-string-nonempty">dãy con</span> <code>&quot;LCT&quot;</code> có thể tạo thành trong chuỗi kết quả sau khi <strong>chèn nhiều nhất một lần</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;LMCT&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể chèn một <code>&quot;L&quot;</code> vào đầu chuỗi s để tạo thành <code>&quot;LLMCT&quot;</code>, chuỗi này có 2 dãy con, tại các chỉ số [0, 3, 4] và [1, 3, 4].</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;LCCT&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể chèn một <code>&quot;L&quot;</code> vào đầu chuỗi s để tạo thành <code>&quot;LLCCT&quot;</code>, chuỗi này có 4 dãy con, tại các chỉ số [0, 2, 4], [0, 3, 4], [1, 2, 4] và [1, 3, 4].</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;L&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vì không thể tạo dãy con <code>&quot;LCT&quot;</code> bằng cách chèn một chữ cái duy nhất, kết quả là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
    <li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết hoa.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Các dãy con "LCT" được đếm bằng cách nhân số lượng $L$s ở bên trái với số lượng $T$s ở bên phải tại mỗi $C$. Chỉ được chèn tùy chọn một ký tự, còn $n\le 10^5$ khiến việc tính lại tại mọi vị trí không khả thi.
>
> Chèn $L$ sẽ thêm số cặp "CT", chèn $T$ sẽ thêm số cặp "LC", còn chèn $C$ sẽ thêm một giá trị $l\cdot r$ nào đó. Trường hợp cuối có thể được tính ngay trong cùng một lượt duyệt; hai trường hợp đầu là số lượng dãy con gồm hai ký tự.
>
> Trong khi duyệt, ta duy trì $l,r$ và giá trị lớn nhất của $l\cdot r$, sau đó lấy giá trị lớn nhất với $\textit{calc}(\text{LC})$ và $\textit{calc}(\text{CT})$, rồi cộng kết quả này vào số lượng dãy con "LCT" ban đầu.

<!-- thinking:end -->

Trước tiên, ta có thể tính số lượng dãy con "LCT" trong chuỗi ban đầu, sau đó xét trường hợp chèn một ký tự.

Có thể tính số lượng dãy con "LCT" bằng cách duyệt chuỗi. Ta liệt kê ký tự "C" ở giữa và dùng hai biến $l$ và $r$ để duy trì số lượng "L" ở bên trái và "T" ở bên phải tương ứng. Với mỗi "C", ta có thể tính số lượng "L" ở bên trái và số lượng "T" ở bên phải, từ đó số lượng dãy con "LCT" có ký tự "C" này làm ký tự giữa là $l \times r$, rồi cộng dồn vào tổng số lượng.

Tiếp theo, ta cần xét trường hợp chèn một ký tự. Hãy xét việc chèn "L", "C" hoặc "T":

- Chèn một "L": ta chỉ cần đếm số lượng dãy con "CT" trong chuỗi ban đầu.
- Chèn một "T": ta chỉ cần đếm số lượng dãy con "LC" trong chuỗi ban đầu.
- Chèn một "C": ta chỉ cần đếm số lượng dãy con "LT" trong chuỗi ban đầu. Trong quá trình liệt kê ở trên, ta có thể duy trì một biến $\textit{mx}$ biểu diễn giá trị lớn nhất hiện tại của $l \times r$.

Cuối cùng, ta cộng số lượng dãy con "LCT" trong chuỗi ban đầu với số lượng dãy con lớn nhất sau khi chèn một ký tự để thu được kết quả cuối cùng.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numOfSubsequences(self, s: str) -> int:
        def calc(t: str) -> int:
            cnt = a = 0
            for c in s:
                if c == t[1]:
                    cnt += a
                a += int(c == t[0])
            return cnt

        l, r = 0, s.count("T")
        ans = mx = 0
        for c in s:
            r -= int(c == "T")
            if c == "C":
                ans += l * r
            l += int(c == "L")
            mx = max(mx, l * r)
        mx = max(mx, calc("LC"), calc("CT"))
        ans += mx
        return ans
```

#### Java

```java
class Solution {
    private char[] s;

    public long numOfSubsequences(String S) {
        s = S.toCharArray();
        int l = 0, r = 0;
        for (char c : s) {
            if (c == 'T') {
                ++r;
            }
        }
        long ans = 0, mx = 0;
        for (char c : s) {
            r -= c == 'T' ? 1 : 0;
            if (c == 'C') {
                ans += 1L * l * r;
            }
            l += c == 'L' ? 1 : 0;
            mx = Math.max(mx, 1L * l * r);
        }
        mx = Math.max(mx, Math.max(calc("LC"), calc("CT")));
        ans += mx;
        return ans;
    }

    private long calc(String t) {
        long cnt = 0;
        int a = 0;
        for (char c : s) {
            if (c == t.charAt(1)) {
                cnt += a;
            }
            a += c == t.charAt(0) ? 1 : 0;
        }
        return cnt;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long numOfSubsequences(string s) {
        auto calc = [&](string t) {
            long long cnt = 0, a = 0;
            for (char c : s) {
                if (c == t[1]) {
                    cnt += a;
                }
                a += (c == t[0]);
            }
            return cnt;
        };

        long long l = 0, r = count(s.begin(), s.end(), 'T');
        long long ans = 0, mx = 0;
        for (char c : s) {
            r -= (c == 'T');
            if (c == 'C') {
                ans += l * r;
            }
            l += (c == 'L');
            mx = max(mx, l * r);
        }
        mx = max(mx, calc("LC"));
        mx = max(mx, calc("CT"));
        ans += mx;
        return ans;
    }
};
```

#### Go

```go
func numOfSubsequences(s string) int64 {
    calc := func(t string) int64 {
        cnt, a := int64(0), int64(0)
        for _, c := range s {
            if c == rune(t[1]) {
                cnt += a
            }
            if c == rune(t[0]) {
                a++
            }
        }
        return cnt
    }

    l, r := int64(0), int64(0)
    for _, c := range s {
        if c == 'T' {
            r++
        }
    }

    ans, mx := int64(0), int64(0)
    for _, c := range s {
        if c == 'T' {
            r--
        }
        if c == 'C' {
            ans += l * r
        }
        if c == 'L' {
            l++
        }
        mx = max(mx, l*r)
    }
    mx = max(mx, calc("LC"), calc("CT"))
    ans += mx
    return ans
}
```

#### TypeScript

```ts
function numOfSubsequences(s: string): number {
    const calc = (t: string): number => {
        let [cnt, a] = [0, 0];
        for (const c of s) {
            if (c === t[1]) cnt += a;
            if (c === t[0]) a++;
        }
        return cnt;
    };

    let [l, r] = [0, 0];
    for (const c of s) {
        if (c === 'T') r++;
    }

    let [ans, mx] = [0, 0];
    for (const c of s) {
        if (c === 'T') r--;
        if (c === 'C') ans += l * r;
        if (c === 'L') l++;
        mx = Math.max(mx, l * r);
    }

    mx = Math.max(mx, calc('LC'));
    mx = Math.max(mx, calc('CT'));
    ans += mx;
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
