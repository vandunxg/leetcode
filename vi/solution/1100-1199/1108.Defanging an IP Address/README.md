---
comments: true
difficulty: Easy
rating: 1084
source: Weekly Contest 144 Q1
tags:
    - String
---

<!-- problem:start -->

# [1108. Defanging an IP Address](https://leetcode.com/problems/defanging-an-ip-address)

[中文文档](/solution/1100-1199/1108.Defanging%20an%20IP%20Address/README.md)

## Mô tả

<!-- description:start -->

<p>Cho địa chỉ IP (IPv4) hợp lệ <code>address</code>, hãy trả về phiên bản đã làm biến dạng của địa chỉ đó.</p>

<p><em>Địa chỉ IP đã làm biến dạng</em> thay mọi dấu chấm <code>&quot;.&quot;</code> bằng <code>&quot;[.]&quot;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> address = "1.1.1.1"
<strong>Đầu ra:</strong> "1[.]1[.]1[.]1"
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> address = "255.100.50.0"
<strong>Đầu ra:</strong> "255[.]100[.]50[.]0"
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>address</code> là địa chỉ IPv4 hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Direct Replacement

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ cần thay mọi dấu `.` bằng `[.]`; không cần phân tích hay xác thực địa chỉ. Một lần gọi `replace` tuyến tính là đủ, với thời gian chạy tỷ lệ thuận với độ dài chuỗi.

<!-- thinking:end -->

Ta có thể thay trực tiếp `'.'` trong chuỗi bằng `'[.]'`.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi. Nếu không tính phần bộ nhớ dùng để lưu đáp án, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def defangIPaddr(self, address: str) -> str:
        return address.replace('.', '[.]')
```

#### Java

```java
class Solution {
    public String defangIPaddr(String address) {
        return address.replace(".", "[.]");
    }
}
```

#### C++

```cpp
class Solution {
public:
    string defangIPaddr(string address) {
        for (int i = address.size(); i >= 0; --i) {
            if (address[i] == '.') {
                address.replace(i, 1, "[.]");
            }
        }
        return address;
    }
};
```

#### Go

```go
func defangIPaddr(address string) string {
	return strings.Replace(address, ".", "[.]", -1)
}
```

#### TypeScript

```ts
function defangIPaddr(address: string): string {
    return address.split('.').join('[.]');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
