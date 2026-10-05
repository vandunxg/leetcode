---
comments: true
difficulty: Medium
rating: 1192
source: Weekly Contest 514 Q1
---

<!-- problem:start -->

# [4014. Minimum Total Price After Applying Discounts](https://leetcode.com/problems/minimum-total-price-after-applying-discounts)

[中文文档](/solution/4000-4099/4014.Minimum%20Total%20Price%20After%20Applying%20Discounts/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng số nguyên <code>prices</code> và <code>discounts</code>.</p>

<p>Giá trị <code>prices[i]</code> biểu thị giá của mặt hàng thứ <code>i<sup>th</sup></code>, còn <code>discounts[j]</code> biểu thị phần trăm giảm giá.</p>

<p>Bạn có thể áp dụng các mức giảm giá theo những quy tắc sau:</p>

<ul>
	<li>Mỗi mức giảm giá có thể được áp dụng cho <strong>nhiều nhất</strong> một mặt hàng.</li>
	<li>Mỗi mặt hàng nhận được <strong>nhiều nhất</strong> một mức giảm giá.</li>
	<li>Một mặt hàng cũng có thể không nhận mức giảm giá nào.</li>
</ul>

<p>Nếu mức giảm giá <code>d</code> phần trăm được áp dụng cho mặt hàng có giá <code>p</code>, giá cuối cùng của mặt hàng đó là <code>(p * (100 - d)) / 100</code>. Giá cuối cùng <strong>không</strong> được làm tròn.</p>

<p>Hãy trả về <strong>tổng nhỏ nhất</strong> có thể có của các giá cuối cùng sau khi phân bổ các mức giảm giá một cách tối ưu. Các đáp án nằm trong khoảng <code>10<sup>-5</sup></code> so với đáp án thực tế đều được chấp nhận.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">prices = [10,30,21], discounts = [50,60]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">32.50000</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Áp dụng <code>discounts[1] = 60</code> cho <code>prices[1] = 30</code>, khi đó <code>30 * (100 - 60) / 100 = 12</code>.</li>
	<li>Áp dụng <code>discounts[0] = 50</code> cho <code>prices[2] = 21</code>, khi đó <code>21 * (100 - 50) / 100 = 10.5</code>.</li>
	<li><code>prices[0] = 10</code> không nhận mức giảm giá nào nên vẫn là 10.</li>
</ul>

<p>Tổng là <code>12 + 10.5 + 10 = 32.50000</code>, đây là giá trị nhỏ nhất có thể.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">prices = [100,70], discounts = [10,40,50]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">92.00000</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<ul>
	<li>Áp dụng <code>discounts[2] = 50</code> cho <code>prices[0] = 100</code>, khi đó <code>100 * (100 - 50) / 100 = 50</code>.</li>
	<li>Áp dụng <code>discounts[1] = 40</code> cho <code>prices[1] = 70</code>, khi đó <code>70 * (100 - 40) / 100 = 42</code>.</li>
</ul>

<p>Tổng là <code>50 + 42 = 92.00000</code>, đây là giá trị nhỏ nhất có thể.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">prices = [7,3,9], discounts = [100,100]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3.00000</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Áp dụng <code>discounts[0] = 100</code> cho <code>prices[2] = 9</code>, khi đó <code>9 * (100 - 100) / 100 = 0</code>.</li>
	<li>Áp dụng <code>discounts[1] = 100</code> cho <code>prices[0] = 7</code>, khi đó <code>7 * (100 - 100) / 100 = 0</code>.</li>
	<li><code>prices[1] = 3</code> không nhận mức giảm giá nào nên vẫn là 3.</li>
</ul>

<p>Tổng là <code>0 + 0 + 3 = 3.00000</code>, đây là giá trị nhỏ nhất có thể.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= prices.length, discounts.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= prices[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= discounts[j] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Mức giảm giá $d$ trên giá $p$ giúp tiết kiệm $p\times d/100$, vì vậy cùng một mức giảm giá sẽ tiết kiệm nhiều hơn trên mặt hàng đắt hơn. Nếu liên tục chọn cặp tốt nhất hiện tại, chúng ta sẽ phải quét lại các phần tử còn lại sau mỗi lần chọn.
>
> Sắp xếp cả hai mảng rồi ghép chúng từ lớn đến nhỏ sẽ phân bổ các mức giảm giá lớn hơn cho những mức giá cao hơn, tương đương với lựa chọn tham lam đó.
>
> Sau khi đã dùng hết các mức giảm giá, cộng các mặt hàng còn lại với giá ban đầu.

<!-- thinking:end -->

Để tối thiểu hóa tổng giá, chúng ta cần tối đa hóa tổng số tiền tiết kiệm được từ các mức giảm giá. Áp dụng mức giảm giá $d$ cho mặt hàng có giá $p$ giúp tiết kiệm $p \times d / 100$. Theo bất đẳng thức sắp xếp lại, áp dụng các mức giảm giá lớn hơn cho những mặt hàng đắt hơn sẽ tối đa hóa tổng số tiền tiết kiệm.

Vì vậy, chúng ta sắp xếp cả hai mảng $\textit{prices}$ và $\textit{discounts}$ theo thứ tự tăng dần, sau đó dùng hai con trỏ bắt đầu từ cuối hai mảng. Ở mỗi bước, áp dụng mức giảm giá lớn nhất hiện tại cho mặt hàng đắt nhất hiện tại và cộng giá sau giảm vào kết quả. Khi đã dùng hết các mức giảm giá, cộng các mặt hàng còn lại với giá ban đầu.

Độ phức tạp thời gian là $O(n \times \log n + m \times \log m)$, còn độ phức tạp không gian là $O(\log n + \log m)$. Trong đó, $n$ và $m$ lần lượt là độ dài của các mảng $\textit{prices}$ và $\textit{discounts}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minPrice(self, prices: list[int], discounts: list[int]) -> float:
        prices.sort()
        discounts.sort()
        i, j = len(prices) - 1, len(discounts) - 1
        ans = 0
        while i >= 0 and j >= 0:
            ans += prices[i] * (100 - discounts[j]) / 100
            i -= 1
            j -= 1
        while i >= 0:
            ans += prices[i]
            i -= 1
        return ans
```

#### Java

```java
class Solution {
    public double minPrice(int[] prices, int[] discounts) {
        Arrays.sort(prices);
        Arrays.sort(discounts);

        int i = prices.length - 1;
        int j = discounts.length - 1;

        double ans = 0;

        while (i >= 0 && j >= 0) {
            ans += prices[i] * (100 - discounts[j]) / 100.0;
            i--;
            j--;
        }

        while (i >= 0) {
            ans += prices[i];
            i--;
        }

        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    double minPrice(vector<int>& prices, vector<int>& discounts) {
        sort(prices.begin(), prices.end());
        sort(discounts.begin(), discounts.end());

        int i = prices.size() - 1;
        int j = discounts.size() - 1;

        double ans = 0;

        while (i >= 0 && j >= 0) {
            ans += prices[i] * (100 - discounts[j]) / 100.0;
            i--;
            j--;
        }

        while (i >= 0) {
            ans += prices[i];
            i--;
        }

        return ans;
    }
};
```

#### Go

```go
func minPrice(prices []int, discounts []int) float64 {
	sort.Ints(prices)
	sort.Ints(discounts)

	i := len(prices) - 1
	j := len(discounts) - 1

	var ans float64

	for i >= 0 && j >= 0 {
		ans += float64(prices[i]) * float64(100-discounts[j]) / 100.0
		i--
		j--
	}

	for i >= 0 {
		ans += float64(prices[i])
		i--
	}

	return ans
}
```

#### TypeScript

```ts
function minPrice(prices: number[], discounts: number[]): number {
    prices.sort((a, b) => a - b);
    discounts.sort((a, b) => a - b);

    let i = prices.length - 1;
    let j = discounts.length - 1;

    let ans = 0;

    while (i >= 0 && j >= 0) {
        ans += (prices[i] * (100 - discounts[j])) / 100;
        i--;
        j--;
    }

    while (i >= 0) {
        ans += prices[i];
        i--;
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
