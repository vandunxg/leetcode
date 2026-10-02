---
comments: true
difficulty: Medium
tags:
    - Hash Table
    - Math
    - Counting
    - Enumeration
    - Sorting
---

<!-- problem:start -->

# [869. Reordered Power of 2](https://leetcode.com/problems/reordered-power-of-2)

[中文文档](/solution/0800-0899/0869.Reordered%20Power%20of%202/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>n</code>. Ta có thể sắp xếp lại các chữ số theo bất kỳ thứ tự nào (kể cả giữ nguyên thứ tự ban đầu), miễn là chữ số đầu tiên khác 0.</p>

<p>Trả về <code>true</code> <em>khi và chỉ khi có thể sắp xếp như vậy để số thu được là lũy thừa của 2</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> n = 1
<strong>Output:</strong> true
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> n = 10
<strong>Output:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Có thể hoán vị các chữ số của $n$ để được lũy thừa của 2 không? Vì $n\le 10^9$, xét $10!$ hoán vị tốn công hơn nhiều so với khoảng ba mươi lũy thừa $2^0\ldots 2^{30}$.
>
> So sánh tần suất các chữ số của $n$ với từng lũy thừa của 2 không vượt quá $10^9$. Nếu trùng khớp thì trả lời có.

<!-- thinking:end -->

Ta có thể liệt kê các lũy thừa của 2 trong đoạn $[1, 10^9]$ rồi kiểm tra xem tần suất từng chữ số có giống với số đã cho hay không.

Định nghĩa hàm $f(x)$ biểu diễn thành phần chữ số của số $x$. Có thể chuyển $x$ thành mảng độ dài 10 lưu số lần xuất hiện của từng chữ số, hoặc thành chuỗi có các chữ số được sắp xếp.

Đầu tiên, tính thành phần chữ số của $n$ thành $\text{target} = f(n)$. Sau đó, bắt đầu với $i=1$ và mỗi lần dịch trái $i$ một bit (tương đương nhân đôi) cho đến khi $i$ vượt quá $10^9$. Với mỗi $i$, tính thành phần chữ số rồi so sánh với $\text{target}$. Nếu trùng khớp, trả về $\text{true}$; nếu duyệt hết mà không tìm thấy thành phần chữ số giống nhau, trả về $\text{false}$.

Độ phức tạp thời gian là $O(\log^2 M)$, độ phức tạp không gian là $O(\log M)$, với $M$ là giới hạn trên của miền đầu vào, ${10}^9$ trong bài này.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def reorderedPowerOf2(self, n: int) -> bool:
        def f(x: int) -> List[int]:
            cnt = [0] * 10
            while x:
                x, v = divmod(x, 10)
                cnt[v] += 1
            return cnt

        target = f(n)
        i = 1
        while i <= 10**9:
            if f(i) == target:
                return True
            i <<= 1
        return False
```

#### Java

```java
class Solution {
    public boolean reorderedPowerOf2(int n) {
        String target = f(n);
        for (int i = 1; i <= 1000000000; i <<= 1) {
            if (target.equals(f(i))) {
                return true;
            }
        }
        return false;
    }

    private String f(int x) {
        char[] cnt = new char[10];
        for (; x > 0; x /= 10) {
            cnt[x % 10]++;
        }
        return new String(cnt);
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool reorderedPowerOf2(int n) {
        string target = f(n);
        for (int i = 1; i <= 1000000000; i <<= 1) {
            if (target == f(i)) {
                return true;
            }
        }
        return false;
    }

private:
    string f(int x) {
        char cnt[10] = {};
        while (x > 0) {
            cnt[x % 10]++;
            x /= 10;
        }
        return string(cnt, cnt + 10);
    }
};
```

#### Go

```go
func reorderedPowerOf2(n int) bool {
	target := f(n)
	for i := 1; i <= 1000000000; i <<= 1 {
		if bytes.Equal(target, f(i)) {
			return true
		}
	}
	return false
}

func f(x int) []byte {
	cnt := make([]byte, 10)
	for x > 0 {
		cnt[x%10]++
		x /= 10
	}
	return cnt
}
```

#### TypeScript

```ts
function reorderedPowerOf2(n: number): boolean {
    const f = (x: number) => {
        const cnt = Array(10).fill(0);
        while (x > 0) {
            cnt[x % 10]++;
            x = (x / 10) | 0;
        }
        return cnt.join(',');
    };
    const target = f(n);
    for (let i = 1; i <= 1_000_000_000; i <<= 1) {
        if (target === f(i)) {
            return true;
        }
    }
    return false;
}
```

#### Rust

```rust
impl Solution {
    pub fn reordered_power_of2(n: i32) -> bool {
        fn f(mut x: i32) -> [u8; 10] {
            let mut cnt = [0u8; 10];
            while x > 0 {
                cnt[(x % 10) as usize] += 1;
                x /= 10;
            }
            cnt
        }

        let target = f(n);
        let mut i = 1i32;
        while i <= 1_000_000_000 {
            if target == f(i) {
                return true;
            }
            i <<= 1;
        }
        false
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
