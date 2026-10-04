---
comments: true
difficulty: Easy
rating: 1255
source: Biweekly Contest 132 Q1
tags:
    - Stack
    - String
    - Simulation
---

<!-- problem:start -->

# [3174. Clear Digits](https://leetcode.com/problems/clear-digits)

[中文文档](/solution/3100-3199/3174.Clear%20Digits/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một chuỗi <code>s</code>.</p>

<p>Nhiệm vụ của bạn là xóa <strong>tất cả</strong> chữ số bằng cách lặp lại thao tác sau:</p>

<ul>
	<li>Xóa chữ số <em>đầu tiên</em> và ký tự <strong>không phải chữ số</strong> <b>gần nhất</b> ở bên <em>trái</em> của nó.</li>
</ul>

<p>Trả về chuỗi thu được sau khi xóa tất cả chữ số.</p>

<p><strong>Lưu ý</strong> rằng <em>không thể</em> thực hiện thao tác trên một chữ số nếu không có ký tự không phải chữ số nào ở bên trái nó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abc&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;abc&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi không có chữ số nào.<!-- notionvc: ff07e34f-b1d6-41fb-9f83-5d0ba3c1ecde --></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;cb34&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Đầu tiên, ta thực hiện thao tác trên <code>s[2]</code>, và <code>s</code> trở thành <code>&quot;c4&quot;</code>.</p>

<p>Sau đó, ta thực hiện thao tác trên <code>s[1]</code>, và <code>s</code> trở thành <code>&quot;&quot;</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ bao gồm các chữ cái tiếng Anh viết thường và chữ số.</li>
	<li>Dữ liệu đầu vào được tạo sao cho có thể xóa tất cả chữ số.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Stack + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Một chữ số tự xóa chính nó và chữ cái gần nhất bên trái. Việc liên tục chỉnh sửa chuỗi có độ phức tạp bậc hai.
>
> Khi duyệt từ trái sang phải, phần tử trên cùng của stack là ký tự còn lại gần nhất, nên chữ số chỉ cần pop.
>
> Đưa các chữ cái vào stack và pop khi gặp chữ số, sau đó nối các phần tử trong stack. Mỗi ký tự được thêm vào nhiều nhất một lần.

<!-- thinking:end -->

Ta sử dụng một stack `stk` để mô phỏng quá trình này. Ta duyệt chuỗi `s`. Nếu ký tự hiện tại là một chữ số, ta pop phần tử trên cùng của stack. Ngược lại, ta đưa ký tự hiện tại vào stack.

Cuối cùng, ta nối các phần tử trong stack thành một chuỗi và trả về chuỗi đó.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của chuỗi `s`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def clearDigits(self, s: str) -> str:
        stk = []
        for c in s:
            if c.isdigit():
                stk.pop()
            else:
                stk.append(c)
        return "".join(stk)
```

#### Java

```java
class Solution {
    public String clearDigits(String s) {
        StringBuilder stk = new StringBuilder();
        for (char c : s.toCharArray()) {
            if (Character.isDigit(c)) {
                stk.deleteCharAt(stk.length() - 1);
            } else {
                stk.append(c);
            }
        }
        return stk.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string clearDigits(string s) {
        string stk;
        for (char c : s) {
            if (isdigit(c)) {
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
func clearDigits(s string) string {
	stk := []byte{}
	for i := range s {
		if s[i] >= '0' && s[i] <= '9' {
			stk = stk[:len(stk)-1]
		} else {
			stk = append(stk, s[i])
		}
	}
	return string(stk)
}
```

#### TypeScript

```ts
function clearDigits(s: string): string {
    const stk: string[] = [];
    for (const c of s) {
        if (!isNaN(parseInt(c))) {
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
