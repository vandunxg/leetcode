---
comments: true
difficulty: Easy
rating: 1227
source: Biweekly Contest 106 Q1
tags:
    - Hash Table
    - Math
---

<!-- problem:start -->

# [2729. Check if The Number is Fascinating](https://leetcode.com/problems/check-if-the-number-is-fascinating)

[Tài liệu tiếng Trung](/solution/2700-2799/2729.Check%20if%20The%20Number%20is%20Fascinating/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code> gồm đúng <code>3</code> chữ số.</p>

<p>Ta gọi số <code>n</code> là <strong>đặc biệt</strong> nếu sau khi thực hiện thay đổi dưới đây, số thu được chứa tất cả các chữ số từ <code>1</code> đến <code>9</code> <strong>đúng</strong> một lần và không chứa bất kỳ số <code>0</code> nào:</p>

<ul>
	<li><strong>Nối</strong> <code>n</code> với các số <code>2 * n</code> và <code>3 * n</code>.</li>
</ul>

<p>Trả về <code>true</code><em> nếu </em><code>n</code><em> là số đặc biệt, ngược lại trả về </em><code>false</code><em>.</em></p>

<p><strong>Nối</strong> hai số nghĩa là ghép chúng liền nhau. Ví dụ, nối <code>121</code> và <code>371</code> sẽ được <code>121371</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 192
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Ta nối các số n = 192, 2 * n = 384 và 3 * n = 576. Số thu được là 192384576. Số này chứa tất cả các chữ số từ 1 đến 9 đúng một lần.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 100
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Ta nối các số n = 100, 2 * n = 200 và 3 * n = 300. Số thu được là 100200300. Số này không thỏa mãn bất kỳ điều kiện nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>100 &lt;= n &lt;= 999</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Kiểm tra xem phép nối $n$, $2n$ và $3n$ có tạo thành một hoán vị của các chữ số từ $1$ đến $9$ hay không. Vì $n$ là số có ba chữ số nên chuỗi nối có độ dài cố định, không cần liệt kê các hoán vị.
>
> Sắp xếp các chữ số trong chuỗi nối rồi so sánh với $123456789$; cách này đồng thời loại bỏ các trường hợp thiếu chữ số, lặp chữ số hoặc có số $0$.

<!-- thinking:end -->

Theo mô tả bài toán, ta nối $n$, $2 \times n$ và $3 \times n$ thành một chuỗi $s$, sau đó kiểm tra xem $s$ có chứa mỗi chữ số từ $1$ đến $9$ đúng một lần và không chứa số $0$ hay không.

Độ phức tạp thời gian là $O(\log n)$, và độ phức tạp không gian là $O(\log n)$. Trong đó, $n$ là số nguyên được cho.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isFascinating(self, n: int) -> bool:
        s = str(n) + str(2 * n) + str(3 * n)
        return "".join(sorted(s)) == "123456789"
```

#### Java

```java
class Solution {
    public boolean isFascinating(int n) {
        String s = "" + n + (2 * n) + (3 * n);
        int[] cnt = new int[10];
        for (char c : s.toCharArray()) {
            if (++cnt[c - '0'] > 1) {
                return false;
            }
        }
        return cnt[0] == 0 && s.length() == 9;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isFascinating(int n) {
        string s = to_string(n) + to_string(n * 2) + to_string(n * 3);
        sort(s.begin(), s.end());
        return s == "123456789";
    }
};
```

#### Go

```go
func isFascinating(n int) bool {
	s := strconv.Itoa(n) + strconv.Itoa(n*2) + strconv.Itoa(n*3)
	cnt := [10]int{}
	for _, c := range s {
		cnt[c-'0']++
		if cnt[c-'0'] > 1 {
			return false
		}
	}
	return cnt[0] == 0 && len(s) == 9
}
```

#### TypeScript

```ts
function isFascinating(n: number): boolean {
    const s = `${n}${n * 2}${n * 3}`;
    return s.split('').sort().join('') === '123456789';
}
```

#### Rust

```rust
impl Solution {
    pub fn is_fascinating(n: i32) -> bool {
        let s = format!("{}{}{}", n, n * 2, n * 3);

        let mut cnt = vec![0; 10];
        for c in s.chars() {
            let t = (c as usize) - ('0' as usize);
            cnt[t] += 1;
            if cnt[t] > 1 {
                return false;
            }
        }

        cnt[0] == 0 && s.len() == 9
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
