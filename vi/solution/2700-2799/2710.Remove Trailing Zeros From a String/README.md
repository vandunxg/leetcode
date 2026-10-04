---
comments: true
difficulty: Easy
rating: 1164
source: Weekly Contest 347 Q1
tags:
    - String
---

<!-- problem:start -->

# [2710. Remove Trailing Zeros From a String](https://leetcode.com/problems/remove-trailing-zeros-from-a-string)

[Tài liệu tiếng Trung](/solution/2700-2799/2710.Remove%20Trailing%20Zeros%20From%20a%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <strong>dương</strong> <code>num</code> được biểu diễn dưới dạng chuỗi, hãy trả về <em>số nguyên </em><code>num</code><em> dưới dạng chuỗi sau khi bỏ các số 0 ở cuối</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;51230100&quot;
<strong>Đầu ra:</strong> &quot;512301&quot;
<strong>Giải thích:</strong> Số nguyên &quot;51230100&quot; có 2 số 0 ở cuối, ta xóa chúng và trả về số nguyên &quot;512301&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;123&quot;
<strong>Đầu ra:</strong> &quot;123&quot;
<strong>Giải thích:</strong> Số nguyên &quot;123&quot; không có số 0 ở cuối, ta trả về số nguyên &quot;123&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num.length &lt;= 1000</code></li>
	<li><code>num</code> chỉ gồm các chữ số.</li>
	<li><code>num</code> không có số 0 ở đầu.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ các số 0 ở cuối biểu diễn thập phân mới cần được xóa; các số 0 ở giữa hoặc ở đầu vẫn được giữ lại. Khi duyệt từ trái sang phải, ta chưa thể biết số 0 nào là số 0 ở cuối cho đến khi biết điểm kết thúc.
>
> Việc xóa các số 0 liên tiếp từ bên phải chính là thao tác $rstrip$.

<!-- thinking:end -->

Ta có thể duyệt chuỗi từ cuối về đầu, dừng lại khi gặp ký tự đầu tiên khác `0`. Sau đó, ta trả về chuỗi con từ đầu chuỗi đến ký tự này.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi. Không tính phần không gian được dùng cho chuỗi kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def removeTrailingZeros(self, num: str) -> str:
        return num.rstrip("0")
```

#### Java

```java
class Solution {
    public String removeTrailingZeros(String num) {
        int i = num.length() - 1;
        while (num.charAt(i) == '0') {
            --i;
        }
        return num.substring(0, i + 1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string removeTrailingZeros(string num) {
        while (num.back() == '0') {
            num.pop_back();
        }
        return num;
    }
};
```

#### Go

```go
func removeTrailingZeros(num string) string {
	i := len(num) - 1
	for num[i] == '0' {
		i--
	}
	return num[:i+1]
}
```

#### TypeScript

```ts
function removeTrailingZeros(num: string): string {
    let i = num.length - 1;
    while (num[i] === '0') {
        --i;
    }
    return num.substring(0, i + 1);
}
```

#### Rust

```rust
impl Solution {
    pub fn remove_trailing_zeros(num: String) -> String {
        let mut i = num.len() - 1;

        while num.chars().nth(i) == Some('0') {
            i -= 1;
        }

        num[..i + 1].to_string()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
