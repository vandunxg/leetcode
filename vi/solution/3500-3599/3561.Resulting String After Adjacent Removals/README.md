---
comments: true
difficulty: Medium
rating: 1397
source: Weekly Contest 451 Q2
tags:
    - Stack
    - String
    - Simulation
---

<!-- problem:start -->

# [3561. Resulting String After Adjacent Removals](https://leetcode.com/problems/resulting-string-after-adjacent-removals)

[中文文档](/solution/3500-3599/3561.Resulting%20String%20After%20Adjacent%20Removals/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</p>

<p>Bạn <strong>phải</strong> liên tục thực hiện thao tác sau khi chuỗi <code>s</code> còn <strong>ít nhất</strong> hai ký tự <strong>liền kề</strong>:</p>

<ul>
    <li>Xóa cặp ký tự <strong>liền kề</strong> <strong>ngoài cùng bên trái</strong> trong chuỗi, là hai ký tự <strong>liên tiếp</strong> trong bảng chữ cái theo một trong hai thứ tự (ví dụ, <code>&#39;a&#39;</code> và <code>&#39;b&#39;</code>, hoặc <code>&#39;b&#39;</code> và <code>&#39;a&#39;</code>).</li>
    <li>Dịch các ký tự còn lại sang trái để lấp chỗ trống.</li>
</ul>

<p>Trả về chuỗi thu được sau khi không thể thực hiện thêm thao tác nào.</p>

<p><strong>Lưu ý:</strong> Hãy xem bảng chữ cái là vòng tròn, do đó <code>&#39;a&#39;</code> và <code>&#39;z&#39;</code> là hai ký tự liên tiếp.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abc&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;c&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Xóa <code>&quot;ab&quot;</code> khỏi chuỗi, còn lại <code>&quot;c&quot;</code>.</li>
    <li>Không thể thực hiện thêm thao tác nào. Vì vậy, chuỗi thu được sau tất cả các lần xóa có thể thực hiện là <code>&quot;c&quot;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;adcb&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Xóa <code>&quot;dc&quot;</code> khỏi chuỗi, còn lại <code>&quot;ab&quot;</code>.</li>
    <li>Xóa <code>&quot;ab&quot;</code> khỏi chuỗi, còn lại <code>&quot;&quot;</code>.</li>
    <li>Không thể thực hiện thêm thao tác nào. Vì vậy, chuỗi thu được sau tất cả các lần xóa có thể thực hiện là <code>&quot;&quot;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;zadb&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;db&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Xóa <code>&quot;za&quot;</code> khỏi chuỗi, còn lại <code>&quot;db&quot;</code>.</li>
    <li>Không thể thực hiện thêm thao tác nào. Vì vậy, chuỗi thu được sau tất cả các lần xóa có thể thực hiện là <code>&quot;db&quot;</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
    <li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Stack

<!-- thinking:start -->

> **Tư duy**
>
> Các chữ cái liền kề và liên tiếp trong bảng chữ cái (bao gồm `a`/`z`) biến mất theo từng cặp và có thể tạo ra các lần xóa liên tiếp, tương tự như việc khớp ngoặc. Một stack lưu hậu tố chưa bị xóa.
>
> Với mỗi ký tự, lấy phần tử trên cùng ra nếu nó cách ký tự hiện tại $1$ hoặc $25$ đơn vị; nếu không thì thêm ký tự hiện tại vào stack. Stack chính là chuỗi còn lại.

<!-- thinking:end -->

Ta có thể dùng một stack để mô phỏng quá trình xóa các ký tự liền kề. Duyệt qua từng ký tự trong chuỗi. Nếu ký tự trên cùng của stack và ký tự hiện tại là liên tiếp (tức là hiệu giữa các giá trị ASCII của chúng bằng 1 hoặc 25), lấy phần tử trên cùng ra khỏi stack; nếu không, thêm ký tự hiện tại vào stack. Cuối cùng, các ký tự còn lại trong stack là những ký tự không thể xóa thêm. Nối các ký tự trong stack thành một chuỗi và trả về chuỗi đó.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def resultingString(self, s: str) -> str:
        stk = []
        for c in s:
            if stk and abs(ord(c) - ord(stk[-1])) in (1, 25):
                stk.pop()
            else:
                stk.append(c)
        return "".join(stk)
```

#### Java

```java
class Solution {
    public String resultingString(String s) {
        StringBuilder stk = new StringBuilder();
        for (char c : s.toCharArray()) {
            if (stk.length() > 0 && isContiguous(stk.charAt(stk.length() - 1), c)) {
                stk.deleteCharAt(stk.length() - 1);
            } else {
                stk.append(c);
            }
        }
        return stk.toString();
    }

    private boolean isContiguous(char a, char b) {
        int t = Math.abs(a - b);
        return t == 1 || t == 25;
    }
}
```

#### C++

```cpp
class Solution {
public:
    string resultingString(string s) {
        string stk;
        for (char c : s) {
            if (stk.size() && (abs(stk.back() - c) == 1 || abs(stk.back() - c) == 25)) {
                stk.pop_back();
            } else {
                stk.push_back(c);
            }
        }
        return stk;
    }
};
```

#### Go

```go
func resultingString(s string) string {
    isContiguous := func(a, b rune) bool {
        x := abs(int(a - b))
        return x == 1 || x == 25
    }
    stk := []rune{}
    for _, c := range s {
        if len(stk) > 0 && isContiguous(stk[len(stk)-1], c) {
            stk = stk[:len(stk)-1]
        } else {
            stk = append(stk, c)
        }
    }
    return string(stk)
}

func abs(x int) int {
    if x < 0 {
        return -x
    }
    return x
}
```

#### TypeScript

```ts
function resultingString(s: string): string {
    const stk: string[] = [];
    const isContiguous = (a: string, b: string): boolean => {
        const x = Math.abs(a.charCodeAt(0) - b.charCodeAt(0));
        return x === 1 || x === 25;
    };
    for (const c of s) {
        if (stk.length && isContiguous(stk.at(-1)!, c)) {
            stk.pop();
        } else {
            stk.push(c);
        }
    }
    return stk.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
