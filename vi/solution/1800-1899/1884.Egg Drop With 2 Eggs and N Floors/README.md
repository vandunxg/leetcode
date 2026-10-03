---
comments: true
difficulty: Medium
tags:
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [1884. Egg Drop With 2 Eggs and N Floors](https://leetcode.com/problems/egg-drop-with-2-eggs-and-n-floors)

[中文文档](/solution/1800-1899/1884.Egg%20Drop%20With%202%20Eggs%20and%20N%20Floors/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <strong>hai quả trứng giống hệt nhau</strong> và một tòa nhà có <code>n</code> tầng được đánh số từ <code>1</code> đến <code>n</code>.</p>

<p>Biết rằng tồn tại một tầng <code>f</code> với <code>0 &lt;= f &lt;= n</code> sao cho mọi quả trứng thả từ tầng <strong>cao hơn</strong> <code>f</code> đều sẽ <strong>vỡ</strong>, còn mọi quả trứng thả từ tầng <strong>không cao hơn</strong> <code>f</code> đều sẽ <strong>không vỡ</strong>.</p>

<p>Trong mỗi lượt, bạn có thể lấy một quả trứng <strong>chưa vỡ</strong> và thả nó từ bất kỳ tầng nào <code>x</code> (với <code>1 &lt;= x &lt;= n</code>). Nếu trứng vỡ, bạn không thể sử dụng nó nữa. Tuy nhiên, nếu trứng không vỡ, bạn có thể <strong>tái sử dụng</strong> nó trong các lượt sau.</p>

<p>Trả về <em><strong>số lượt nhỏ nhất</strong> cần dùng để xác định <strong>chắc chắn</strong> giá trị của </em><code>f</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta có thể thả quả trứng đầu tiên từ tầng 1 và quả trứng thứ hai từ tầng 2.
Nếu quả trứng đầu tiên vỡ, ta biết rằng f = 0.
Nếu quả trứng thứ hai vỡ nhưng quả trứng đầu tiên không vỡ, ta biết rằng f = 1.
Ngược lại, nếu cả hai quả trứng đều còn nguyên, ta biết rằng f = 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 100
<strong>Đầu ra:</strong> 14
<strong>Giải thích:</strong> Một chiến lược tối ưu là:
- Thả quả trứng thứ 1 ở tầng 9. Nếu trứng vỡ, ta biết f nằm trong đoạn từ 0 đến 8. Thả quả trứng thứ 2 từ tầng 1 và tăng dần từng tầng để tìm f trong tối đa 8 lần thả nữa. Tổng số lần thả là 1 + 8 = 9.
- Nếu quả trứng thứ 1 không vỡ, thả lại quả trứng thứ 1 ở tầng 22. Nếu trứng vỡ, ta biết f nằm trong đoạn từ 9 đến 21. Thả quả trứng thứ 2 từ tầng 10 và tăng dần từng tầng để tìm f trong tối đa 12 lần thả nữa. Tổng số lần thả là 2 + 12 = 14.
- Nếu quả trứng thứ 1 tiếp tục không vỡ, thực hiện tương tự bằng cách thả quả trứng thứ 1 từ các tầng 34, 45, 55, 64, 72, 79, 85, 90, 94, 97, 99 và 100.
Bất kể kết quả thế nào, cần nhiều nhất 14 lần thả để xác định f.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Với hai quả trứng, ta phải tìm tầng giới hạn trong trường hợp xấu nhất. Một lần thả đầu tiên cố định rồi quét tuyến tính không phải lúc nào cũng tối ưu. Vì $n\le 1000$, ta có thể dùng quy hoạch động trên số tầng.
>
> $f[i]$ là chi phí min-max cho $i$ tầng. Thả quả trứng đầu tiên từ tầng $j$ tốn $1+\max(j-1,f[i-j])$: nếu trứng vỡ, phần bên dưới phải được tìm tuyến tính; nếu trứng còn nguyên, ta còn bài toán con gồm $i-j$ tầng. Ta lấy giá trị nhỏ nhất theo $j$.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là số thao tác nhỏ nhất để xác định $f$ trong $i$ tầng khi có hai quả trứng. Ban đầu, $f[0] = 0$, các giá trị còn lại $f[i] = +\infty$. Đáp án là $f[n]$.

Xét $f[i]$, ta duyệt tầng đầu tiên $j$ mà quả trứng đầu tiên được thả, với $1 \leq j \leq i$. Khi đó có hai trường hợp:

- Trứng vỡ. Lúc này ta còn một quả trứng và cần xác định $f$ trong $j - 1$ tầng, cần $j - 1$ thao tác. Vì vậy, tổng số thao tác là $1 + (j - 1)$;
- Trứng không vỡ. Lúc này ta còn hai quả trứng và cần xác định $f$ trong $i - j$ tầng, cần $f[i - j]$ thao tác. Vì vậy, tổng số thao tác là $1 + f[i - j]$.

Tóm lại, ta có phương trình chuyển trạng thái:

$$
f[i] = \min_{1 \leq j \leq i} \{1 + \max(j - 1, f[i - j])\}
$$

Cuối cùng, ta trả về $f[n]$.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số tầng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def twoEggDrop(self, n: int) -> int:
        f = [0] + [inf] * n
        for i in range(1, n + 1):
            for j in range(1, i + 1):
                f[i] = min(f[i], 1 + max(j - 1, f[i - j]))
        return f[n]
```

#### Java

```java
class Solution {
    public int twoEggDrop(int n) {
        int[] f = new int[n + 1];
        Arrays.fill(f, 1 << 29);
        f[0] = 0;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= i; j++) {
                f[i] = Math.min(f[i], 1 + Math.max(j - 1, f[i - j]));
            }
        }
        return f[n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int twoEggDrop(int n) {
        int f[n + 1];
        memset(f, 0x3f, sizeof(f));
        f[0] = 0;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= i; j++) {
                f[i] = min(f[i], 1 + max(j - 1, f[i - j]));
            }
        }
        return f[n];
    }
};
```

#### Go

```go
func twoEggDrop(n int) int {
	f := make([]int, n+1)
	for i := range f {
		f[i] = 1 << 29
	}
	f[0] = 0
	for i := 1; i <= n; i++ {
		for j := 1; j <= i; j++ {
			f[i] = min(f[i], 1+max(j-1, f[i-j]))
		}
	}
	return f[n]
}
```

#### TypeScript

```ts
function twoEggDrop(n: number): number {
    const f: number[] = Array(n + 1).fill(Infinity);
    f[0] = 0;
    for (let i = 1; i <= n; ++i) {
        for (let j = 1; j <= i; ++j) {
            f[i] = Math.min(f[i], 1 + Math.max(j - 1, f[i - j]));
        }
    }
    return f[n];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
