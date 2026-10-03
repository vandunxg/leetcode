---
comments: true
difficulty: Medium
tags:
    - Array
    - Binary Search
---

<!-- problem:start -->

# [2137. Pour Water Between Buckets to Make Water Levels Equal 🔒](https://leetcode.com/problems/pour-water-between-buckets-to-make-water-levels-equal)

[中文文档](/solution/2100-2199/2137.Pour%20Water%20Between%20Buckets%20to%20Make%20Water%20Levels%20Equal/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có <code>n</code> xô, mỗi xô chứa một lượng nước nhất định tính theo gallon, được biểu diễn bằng một mảng số nguyên <strong>đánh chỉ số từ 0</strong> là <code>buckets</code>, trong đó xô thứ <code>i<sup>th</sup></code> chứa <code>buckets[i]</code> gallon nước. Bạn cũng được cho một số nguyên <code>loss</code>.</p>

<p>Bạn muốn làm cho lượng nước trong mỗi xô bằng nhau. Bạn có thể rót một lượng nước bất kỳ từ xô này sang xô khác (không nhất thiết là số nguyên). Tuy nhiên, mỗi khi rót <code>k</code> gallon nước, <code>loss</code> <strong>phần trăm</strong> của <code>k</code> sẽ bị đổ ra ngoài.</p>

<p>Trả về <em><strong>lượng nước lớn nhất</strong> trong mỗi xô sau khi đã làm cho lượng nước bằng nhau.</em> Các đáp án nằm trong khoảng <code>10<sup>-5</sup></code> so với đáp án thực tế đều được chấp nhận.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> buckets = [1,2,7], loss = 80
<strong>Đầu ra:</strong> 2.00000
<strong>Giải thích:</strong> Rót 5 gallon nước từ buckets[2] sang buckets[0].
5 * 80% = 4 gallon nước bị đổ ra ngoài và buckets[0] chỉ nhận được 5 - 4 = 1 gallon nước.
Tất cả các xô đều có 2 gallon nước, nên trả về 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> buckets = [2,4,6], loss = 50
<strong>Đầu ra:</strong> 3.50000
<strong>Giải thích:</strong> Rót 0.5 gallon nước từ buckets[1] sang buckets[0].
0.5 * 50% = 0.25 gallon nước bị đổ ra ngoài và buckets[0] chỉ nhận được 0.5 - 0.25 = 0.25 gallon nước.
Bây giờ, buckets = [2.25, 3.5, 6].
Rót 2.5 gallon nước từ buckets[2] sang buckets[0].
2.5 * 50% = 1.25 gallon nước bị đổ ra ngoài và buckets[0] chỉ nhận được 2.5 - 1.25 = 1.25 gallon nước.
Tất cả các xô đều có 3.5 gallon nước, nên trả về 3.5.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> buckets = [3,3,3,3], loss = 40
<strong>Đầu ra:</strong> 3.00000
<strong>Giải thích:</strong> Tất cả các xô đã có cùng một lượng nước.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= buckets.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= buckets[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= loss &lt;= 99</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân trên số thực

<!-- thinking:start -->

> **Tư duy**
>
> Vì nước bị hao hụt khi rót, mực nước chung càng cao thì càng khó đạt được; miền khả thi là một đoạn đầu của trục số thực. Mực nước là liên tục nên không thể liệt kê từng giá trị, còn việc mô phỏng một chuỗi rót cụ thể thì khó duy trì độ chính xác.
>
> Với một giá trị ứng viên $v$, lượng nước dư có tổng là $a$, còn lượng thiếu hụt cần bù, sau khi tính phần hao hụt, có tổng là $b$. Mực nước đó khả thi khi và chỉ khi $a\ge b$. Ta tìm kiếm nhị phân $v$ trong đoạn $[0,\max\textit{buckets}]$.
>
> $\texttt{check}$ duyệt qua mọi xô; dừng khi độ rộng của khoảng tìm kiếm nhỏ hơn $10^{-5}$.

<!-- thinking:end -->

Ta nhận thấy nếu một lượng nước $x$ thỏa mãn điều kiện, thì mọi lượng nước nhỏ hơn $x$ cũng thỏa mãn điều kiện. Vì vậy, ta có thể sử dụng tìm kiếm nhị phân để tìm lượng nước lớn nhất thỏa mãn điều kiện.

Ta đặt cận trái của tìm kiếm nhị phân là $l=0$ và cận phải là $r=\max(buckets)$. Trong mỗi vòng lặp tìm kiếm nhị phân, ta lấy trung điểm $mid$ của $l$ và $r$, rồi kiểm tra xem $mid$ có thỏa mãn điều kiện hay không. Nếu có, ta cập nhật $l$ thành $mid$; nếu không, ta cập nhật $r$ thành $mid$. Sau khi kết thúc tìm kiếm nhị phân, lượng nước lớn nhất thỏa mãn điều kiện là $l$.

Mấu chốt của bài toán là xác định một lượng nước $v$ có thỏa mãn điều kiện hay không. Ta có thể duyệt qua tất cả các xô. Với mỗi xô, nếu lượng nước của nó lớn hơn $v$, ta cần rót ra một lượng nước $x-v$; nếu lượng nước của nó nhỏ hơn $v$, ta cần rót vào một lượng nước $(v-x)\times\frac{100}{100-\textit{loss}}$. Nếu tổng lượng nước rót ra lớn hơn hoặc bằng tổng lượng nước cần rót vào, thì $v$ thỏa mãn điều kiện.

Độ phức tạp thời gian là $O(n \times \log M)$, trong đó $n$ và $M$ lần lượt là độ dài và giá trị lớn nhất của mảng $buckets$. Độ phức tạp thời gian của tìm kiếm nhị phân là $O(\log M)$, và mỗi vòng lặp tìm kiếm nhị phân cần duyệt qua mảng $buckets$, với độ phức tạp thời gian là $O(n)$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def equalizeWater(self, buckets: List[int], loss: int) -> float:
        def check(v):
            a = b = 0
            for x in buckets:
                if x >= v:
                    a += x - v
                else:
                    b += (v - x) * 100 / (100 - loss)
            return a >= b

        l, r = 0, max(buckets)
        while r - l > 1e-5:
            mid = (l + r) / 2
            if check(mid):
                l = mid
            else:
                r = mid
        return l
```

#### Java

```java
class Solution {
    public double equalizeWater(int[] buckets, int loss) {
        double l = 0, r = Arrays.stream(buckets).max().getAsInt();
        while (r - l > 1e-5) {
            double mid = (l + r) / 2;
            if (check(buckets, loss, mid)) {
                l = mid;
            } else {
                r = mid;
            }
        }
        return l;
    }

    private boolean check(int[] buckets, int loss, double v) {
        double a = 0;
        double b = 0;
        for (int x : buckets) {
            if (x > v) {
                a += x - v;
            } else {
                b += (v - x) * 100 / (100 - loss);
            }
        }
        return a >= b;
    }
}
```

#### C++

```cpp
class Solution {
public:
    double equalizeWater(vector<int>& buckets, int loss) {
        double l = 0, r = *max_element(buckets.begin(), buckets.end());
        auto check = [&](double v) {
            double a = 0, b = 0;
            for (int x : buckets) {
                if (x > v) {
                    a += x - v;
                } else {
                    b += (v - x) * 100 / (100 - loss);
                }
            }
            return a >= b;
        };
        while (r - l > 1e-5) {
            double mid = (l + r) / 2;
            if (check(mid)) {
                l = mid;
            } else {
                r = mid;
            }
        }
        return l;
    }
};
```

#### Go

```go
func equalizeWater(buckets []int, loss int) float64 {
	check := func(v float64) bool {
		var a, b float64
		for _, x := range buckets {
			if float64(x) >= v {
				a += float64(x) - v
			} else {
				b += (v - float64(x)) * 100 / float64(100-loss)
			}
		}
		return a >= b
	}

	l, r := float64(0), float64(slices.Max(buckets))
	for r-l > 1e-5 {
		mid := (l + r) / 2
		if check(mid) {
			l = mid
		} else {
			r = mid
		}
	}
	return l
}
```

#### TypeScript

```ts
function equalizeWater(buckets: number[], loss: number): number {
    let l = 0;
    let r = Math.max(...buckets);
    const check = (v: number): boolean => {
        let [a, b] = [0, 0];
        for (const x of buckets) {
            if (x >= v) {
                a += x - v;
            } else {
                b += ((v - x) * 100) / (100 - loss);
            }
        }
        return a >= b;
    };
    while (r - l > 1e-5) {
        const mid = (l + r) / 2;
        if (check(mid)) {
            l = mid;
        } else {
            r = mid;
        }
    }
    return l;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
