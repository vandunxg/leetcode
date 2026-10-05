---
comments: true
difficulty: Medium
rating: 1593
source: Weekly Contest 482 Q3
tags:
    - Hash Table
    - Math
---

<!-- problem:start -->

# [3790. Smallest All-Ones Multiple](https://leetcode.com/problems/smallest-all-ones-multiple)

[中文文档](/solution/3700-3799/3790.Smallest%20All-Ones%20Multiple/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên dương <code>k</code>.</p>

<p>Hãy tìm số nguyên <code>n</code> <strong>nhỏ nhất</strong> chia hết cho <code>k</code> và trong biểu diễn thập phân <strong>chỉ gồm chữ số 1</strong> (ví dụ: 1, 11, 111, ...).</p>

<p>Trả về một số nguyên biểu thị <strong>số chữ số</strong> trong biểu diễn thập phân của <code>n</code>. Nếu không tồn tại <code>n</code> như vậy, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>n = 111</code> vì 111 chia hết cho 3, còn 1 và 11 thì không. Độ dài của <code>n = 111</code> là 3.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">k = 7</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>n = 111111</code>. Độ dài của <code>n = 111111</code> là 6.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không tồn tại <code>n</code> hợp lệ nào là bội của 2.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= k &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng + Phép toán modulo

<!-- thinking:start -->

> **Tư duy**
>
> Một số nguyên chỉ gồm các chữ số 1 không bao giờ là bội của $k$ chẵn. Nếu không, phần dư biến đổi theo $x\leftarrow 10x+1\pmod k$. Chỉ có $k$ phần dư, nên nếu gặp phần dư 0 thì ta tìm được số chữ số, còn $k$ bước không thành công nghĩa là không tồn tại số nguyên như vậy.

<!-- thinking:end -->

Trước tiên, nếu $k$ là số chẵn, không tồn tại $n$ hợp lệ thỏa mãn điều kiện, nên ta trả về $-1$ ngay.

Tiếp theo, ta có thể mô phỏng quá trình xây dựng một số $n$ chỉ gồm các chữ số 1, đồng thời lấy phần dư khi chia cho $k$ để xác định có tồn tại $n$ hợp lệ hay không.

Ta lặp $k$ lần để kiểm tra xem có số $n$ chỉ gồm các chữ số 1 chia hết cho $k$ trong $k$ lần lặp này hay không. Ở mỗi lần lặp, ta nhân phần dư hiện tại với $10$, cộng $1$, rồi lấy modulo với $k$. Nếu phần dư trở thành $0$ ở một lần lặp nào đó, nghĩa là ta đã tìm thấy một $n$ hợp lệ và trả về số lần lặp hiện tại (tức là số chữ số của số chỉ gồm các chữ số 1). Nếu kết thúc vòng lặp mà không tìm thấy $n$ hợp lệ, ta trả về $-1$.

Độ phức tạp thời gian là $O(k)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minAllOneMultiple(self, k: int) -> int:
        if k % 2 == 0:
            return -1
        x = 1 % k
        ans = 1
        for _ in range(k):
            x = (x * 10 + 1) % k
            ans += 1
            if x == 0:
                return ans
        return -1
```

#### Java

```java
class Solution {
    public int minAllOneMultiple(int k) {
        if ((k & 1) == 0) {
            return -1;
        }

        int x = 1 % k;
        int ans = 1;

        for (int i = 0; i < k; i++) {
            x = (x * 10 + 1) % k;
            ans++;
            if (x == 0) {
                return ans;
            }
        }

        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minAllOneMultiple(int k) {
        if ((k & 1) == 0) {
            return -1;
        }

        int x = 1 % k;
        int ans = 1;

        for (int i = 0; i < k; ++i) {
            x = (x * 10 + 1) % k;
            ++ans;
            if (x == 0) {
                return ans;
            }
        }

        return -1;
    }
};
```

#### Go

```go
func minAllOneMultiple(k int) int {
	if k&1 == 0 {
		return -1
	}

	x := 1 % k
	ans := 1

	for i := 0; i < k; i++ {
		x = (x*10 + 1) % k
		ans++
		if x == 0 {
			return ans
		}
	}

	return -1
}
```

#### TypeScript

```ts
function minAllOneMultiple(k: number): number {
    if ((k & 1) === 0) {
        return -1;
    }

    let x = 1 % k;
    let ans = 1;

    for (let i = 0; i < k; i++) {
        x = (x * 10 + 1) % k;
        ans++;
        if (x === 0) {
            return ans;
        }
    }

    return -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
