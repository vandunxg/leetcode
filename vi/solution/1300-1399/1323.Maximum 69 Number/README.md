---
comments: true
difficulty: Easy
rating: 1193
source: Weekly Contest 172 Q1
tags:
    - Greedy
    - Math
---

<!-- problem:start -->

# [1323. Maximum 69 Number](https://leetcode.com/problems/maximum-69-number)

[中文文档](/solution/1300-1399/1323.Maximum%2069%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên dương <code>num</code> chỉ gồm các chữ số <code>6</code> và <code>9</code>.</p>

<p>Trả về <em>số lớn nhất có thể tạo ra bằng cách đổi <strong>nhiều nhất</strong> một chữ số (</em><code>6</code><em> thành </em><code>9</code><em>, hoặc </em><code>9</code><em> thành </em><code>6</code><em>)</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> num = 9669
<strong>Output:</strong> 9969
<strong>Giải thích:</strong> 
Đổi chữ số đầu tiên sẽ được 6669.
Đổi chữ số thứ hai sẽ được 9969.
Đổi chữ số thứ ba sẽ được 9699.
Đổi chữ số thứ tư sẽ được 9666.
Số lớn nhất là 9969.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> num = 9996
<strong>Output:</strong> 9999
<strong>Giải thích:</strong> Đổi chữ số 6 cuối cùng thành 9 sẽ tạo ra số lớn nhất.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> num = 9999
<strong>Output:</strong> 9999
<strong>Giải thích:</strong> Tốt nhất là không thay đổi chữ số nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num &lt;= 10<sup>4</sup></code></li>
	<li><code>num</code>&nbsp;chỉ gồm các chữ số <code>6</code> và <code>9</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể đổi một chữ số $6$ thành $9$ để tạo số lớn nhất. Chữ số ở hàng cao hơn có giá trị lớn hơn mọi chữ số ở hàng thấp hơn, vì vậy đổi chữ số $6$ ngoài cùng bên trái là lựa chọn tốt nhất; nếu không có chữ số $6$ thì số đó đã lớn nhất. Đây chính là thao tác thay ký tự `'6'` đầu tiên trong chuỗi biểu diễn thập phân.

<!-- thinking:end -->

Ta chuyển số thành chuỗi, rồi duyệt chuỗi từ trái sang phải để tìm chữ số $6$ đầu tiên và đổi nó thành $9$. Sau đó, chuyển chuỗi đã thay đổi về số nguyên và trả về kết quả.

Độ phức tạp thời gian là $O(\log \textit{num})$, độ phức tạp không gian là $O(\log \textit{num})$, trong đó $\textit{num}$ là số nguyên được cho.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximum69Number(self, num: int) -> int:
        return int(str(num).replace("6", "9", 1))
```

#### Java

```java
class Solution {
    public int maximum69Number(int num) {
        return Integer.valueOf(String.valueOf(num).replaceFirst("6", "9"));
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximum69Number(int num) {
        string s = to_string(num);
        for (char& ch : s) {
            if (ch == '6') {
                ch = '9';
                break;
            }
        }
        return stoi(s);
    }
};
```

#### Go

```go
func maximum69Number(num int) int {
	s := strings.Replace(strconv.Itoa(num), "6", "9", 1)
	ans, _ := strconv.Atoi(s)
	return ans
}
```

#### TypeScript

```ts
function maximum69Number(num: number): number {
    return Number((num + '').replace('6', '9'));
}
```

#### Rust

```rust
impl Solution {
    pub fn maximum69_number(num: i32) -> i32 {
        num.to_string().replacen('6', "9", 1).parse().unwrap()
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param Integer $num
     * @return Integer
     */
    function maximum69Number($num) {
        $num = strval($num);
        $n = strpos($num, '6');
        $num[$n] = 9;
        return intval($num);
    }
}
```

#### C

```c
int maximum69Number(int num) {
    char buf[12];
    sprintf(buf, "%d", num);
    for (int i = 0; buf[i] != '\0'; i++) {
        if (buf[i] == '6') {
            buf[i] = '9';
            break;
        }
    }
    int ans;
    sscanf(buf, "%d", &ans);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
