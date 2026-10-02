---
comments: true
difficulty: Easy
tags:
    - Math
    - String
---

<!-- problem:start -->

# [504. Base 7](https://leetcode.com/problems/base-7)

[中文文档](/solution/0500-0599/0504.Base%207/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>num</code>, hãy trả về <em>chuỗi biểu diễn số đó ở <strong>hệ cơ số 7</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> num = 100
<strong>Đầu ra:</strong> "202"
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> num = -7
<strong>Đầu ra:</strong> "-10"
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>-10<sup>7</sup> &lt;= num &lt;= 10<sup>7</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đổi sang cơ số 7 bằng cách chia liên tiếp cho $7$. Xử lý riêng trường hợp $0$; với số âm, đổi trị tuyệt đối rồi khôi phục dấu ở cuối. Số dư thu được theo thứ tự từ hàng thấp nhất, nên cần đảo lại trước khi ghép thành chuỗi.
>
> Vòng lặp chạy một lần cho mỗi chữ số, nên độ phức tạp là $O(\log |num|)$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def convertToBase7(self, num: int) -> str:
        if num == 0:
            return '0'
        if num < 0:
            return '-' + self.convertToBase7(-num)
        ans = []
        while num:
            ans.append(str(num % 7))
            num //= 7
        return ''.join(ans[::-1])
```

#### Java

```java
class Solution {
    public String convertToBase7(int num) {
        if (num == 0) {
            return "0";
        }
        if (num < 0) {
            return "-" + convertToBase7(-num);
        }
        StringBuilder sb = new StringBuilder();
        while (num != 0) {
            sb.append(num % 7);
            num /= 7;
        }
        return sb.reverse().toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string convertToBase7(int num) {
        if (num == 0) return "0";
        if (num < 0) return "-" + convertToBase7(-num);
        string ans = "";
        while (num) {
            ans = to_string(num % 7) + ans;
            num /= 7;
        }
        return ans;
    }
};
```

#### Go

```go
func convertToBase7(num int) string {
	if num == 0 {
		return "0"
	}
	if num < 0 {
		return "-" + convertToBase7(-num)
	}
	ans := []byte{}
	for num != 0 {
		ans = append([]byte{'0' + byte(num%7)}, ans...)
		num /= 7
	}
	return string(ans)
}
```

#### TypeScript

```ts
function convertToBase7(num: number): string {
    if (num == 0) {
        return '0';
    }
    let res = '';
    const isMinus = num < 0;
    if (isMinus) {
        num = -num;
    }
    while (num != 0) {
        const r = num % 7;
        res = r + res;
        num = (num - r) / 7;
    }
    return isMinus ? '-' + res : res;
}
```

#### Rust

```rust
impl Solution {
    pub fn convert_to_base7(mut num: i32) -> String {
        if num == 0 {
            return String::from("0");
        }
        let mut res = String::new();
        let is_minus = num < 0;
        if is_minus {
            num = -num;
        }
        while num != 0 {
            res.push_str((num % 7).to_string().as_str());
            num /= 7;
        }
        if is_minus {
            res.push('-');
        }
        res.chars().rev().collect()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
