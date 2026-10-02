---
comments: true
difficulty: Medium
rating: 1561
source: Biweekly Contest 13 Q1
tags:
    - Bit Manipulation
    - Math
    - String
---

<!-- problem:start -->

# [1256. Encode Number 🔒](https://leetcode.com/problems/encode-number)

[中文文档](/solution/1200-1299/1256.Encode%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên không âm <code>num</code>, hãy trả về chuỗi <em>mã hóa</em> của nó.</p>

<p>Việc mã hóa được thực hiện bằng cách chuyển số nguyên thành chuỗi theo một hàm bí mật mà bạn cần suy ra từ bảng sau:</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1256.Encode%20Number/images/encode_number.png" style="width: 164px; height: 360px;" /></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 23
<strong>Đầu ra:</strong> &quot;1000&quot;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 107
<strong>Đầu ra:</strong> &quot;101100&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= num &lt;= 10^9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bit Manipulation

<!-- thinking:start -->

> **Tư duy**
>
> Bảng lần lượt là $0,1,00,01,\ldots$, tức tất cả chuỗi nhị phân theo thứ tự. Các bit của $num+1$ sau khi bỏ bit $1$ ở đầu chính là chuỗi thứ $num$ trong dãy này. Vì $num$ có thể lên tới $10^9$, ta không thể liệt kê toàn bộ; chỉ cần một thao tác bit là đủ.

<!-- thinking:end -->

Ta cộng $1$ vào $num$, chuyển kết quả thành chuỗi nhị phân rồi bỏ bit $1$ cao nhất.

Độ phức tạp thời gian là $O(\log n)$ và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là giá trị của $num$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def encode(self, num: int) -> str:
        return bin(num + 1)[3:]
```

#### Java

```java
class Solution {
    public String encode(int num) {
        return Integer.toBinaryString(num + 1).substring(1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string encode(int num) {
        bitset<32> bs(++num);
        string ans = bs.to_string();
        int i = 0;
        while (ans[i] == '0') {
            ++i;
        }
        return ans.substr(i + 1);
    }
};
```

#### Go

```go
func encode(num int) string {
	num++
	s := strconv.FormatInt(int64(num), 2)
	return s[1:]
}
```

#### TypeScript

```ts
function encode(num: number): string {
    ++num;
    let s = num.toString(2);
    return s.slice(1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
