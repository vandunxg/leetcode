---
comments: true
difficulty: Easy
rating: 1175
source: Weekly Contest 473 Q1
tags:
    - Math
    - Simulation
---

<!-- problem:start -->

# [3726. Remove Zeros in Decimal Representation](https://leetcode.com/problems/remove-zeros-in-decimal-representation)

[中文文档](/solution/3700-3799/3726.Remove%20Zeros%20in%20Decimal%20Representation/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <strong>dương</strong> <code>n</code>.</p>

<p>Trả về số nguyên nhận được sau khi xóa tất cả các số 0 khỏi biểu diễn thập phân của <code>n</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 1020030</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">123</span></p>

<p><strong>Giải thích:</strong></p>

<p>Sau khi xóa tất cả các số 0 khỏi 1<strong><u>0</u></strong>2<strong><u>00</u></strong>3<strong><u>0</u></strong>, ta nhận được 123.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>1 không có số 0 trong biểu diễn thập phân. Do đó, đáp án là 1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>15</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> $n$ có thể lớn tới $10^{15}$. Chuyển sang chuỗi là một cách, nhưng ta có thể làm việc trực tiếp với số nguyên: tách chữ số cuối, giữ lại các chữ số khác 0 với giá trị hàng $k$ đang được theo dõi, bỏ qua các số 0 và chỉ nhân $k$ với $10$ khi ghi một chữ số.

<!-- thinking:end -->

Ta bắt đầu từ chữ số thấp nhất của $n$ và kiểm tra từng chữ số. Nếu chữ số khác 0, ta thêm nó vào kết quả. Đồng thời, ta cần một biến để theo dõi vị trí hiện tại của chữ số nhằm xây dựng số nguyên cuối cùng một cách chính xác.

Cụ thể, ta có thể dùng biến $k$ để biểu diễn vị trí chữ số hiện tại, sau đó kiểm tra từng chữ số từ thấp nhất đến cao nhất. Nếu chữ số khác 0, ta nhân nó với $k$ rồi cộng vào kết quả, sau đó nhân $k$ với 10 để xử lý chữ số tiếp theo.

Cuối cùng, ta thu được một số nguyên không chứa số 0 nào.

Độ phức tạp thời gian là $O(d)$, trong đó $d$ là số chữ số của $n$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def removeZeros(self, n: int) -> int:
        k = 1
        ans = 0
        while n:
            x = n % 10
            if x:
                ans = k * x + ans
                k *= 10
            n //= 10
        return ans
```

#### Java

```java
class Solution {
    public long removeZeros(long n) {
        long k = 1;
        long ans = 0;
        while (n > 0) {
            long x = n % 10;
            if (x > 0) {
                ans = k * x + ans;
                k *= 10;
            }
            n /= 10;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long removeZeros(long long n) {
        long long k = 1;
        long long ans = 0;
        while (n > 0) {
            long x = n % 10;
            if (x > 0) {
                ans = k * x + ans;
                k *= 10;
            }
            n /= 10;
        }
        return ans;
    }
};
```

#### Go

```go
func removeZeros(n int64) (ans int64) {
	k := int64(1)
	for n > 0 {
		x := n % 10
		if x > 0 {
			ans = k*x + ans
			k *= 10
		}
		n /= 10
	}
	return
}
```

#### TypeScript

```ts
function removeZeros(n: number): number {
    let k = 1;
    let ans = 0;
    while (n) {
        const x = n % 10;
        if (x) {
            ans = k * x + ans;
            k *= 10;
        }
        n = Math.floor(n / 10);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
