---
comments: true
difficulty: Medium
rating: 1704
source: Weekly Contest 437 Q2
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [3457. Eat Pizzas!](https://leetcode.com/problems/eat-pizzas)

[中文文档](/solution/3400-3499/3457.Eat%20Pizzas%21/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>pizzas</code> có kích thước <code>n</code>, trong đó <code>pizzas[i]</code> biểu thị khối lượng của chiếc pizza thứ <code>i<sup>th</sup></code>. Mỗi ngày, bạn ăn <strong>chính xác</strong> 4 chiếc pizza. Nhờ khả năng trao đổi chất đáng kinh ngạc, khi bạn ăn các chiếc pizza có khối lượng <code>W</code>, <code>X</code>, <code>Y</code> và <code>Z</code>, trong đó <code>W &lt;= X &lt;= Y &lt;= Z</code>, bạn chỉ tăng cân bằng khối lượng của 1 chiếc pizza!</p>

<ul>
	<li>Vào những ngày <strong><span style="box-sizing: border-box; margin: 0px; padding: 0px;">số lẻ</span></strong> (tính từ 1), bạn tăng cân bằng khối lượng <code>Z</code>.</li>
	<li>Vào những ngày <strong>số chẵn</strong>, bạn tăng cân bằng khối lượng <code>Y</code>.</li>
</ul>

<p>Hãy tìm <strong>tổng khối lượng</strong> <strong>lớn nhất</strong> mà bạn có thể tăng thêm bằng cách ăn <strong>tất cả</strong> pizza một cách tối ưu.</p>

<p><strong>Lưu ý</strong>: Đảm bảo rằng <code>n</code> là bội số của 4 và mỗi chiếc pizza chỉ được ăn một lần.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">pizzas = [1,2,3,4,5,6,7,8]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">14</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Vào ngày 1, bạn ăn các chiếc pizza tại các chỉ số <code>[1, 2, 4, 7] = [2, 3, 5, 8]</code>. Bạn tăng cân bằng 8.</li>
	<li>Vào ngày 2, bạn ăn các chiếc pizza tại các chỉ số <code>[0, 3, 5, 6] = [1, 4, 6, 7]</code>. Bạn tăng cân bằng 6.</li>
</ul>

<p>Tổng khối lượng tăng thêm sau khi ăn tất cả pizza là <code>8 + 6 = 14</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">pizzas = [2,1,1,1,1,1,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Vào ngày 1, bạn ăn các chiếc pizza tại các chỉ số <code>[4, 5, 6, 0] = [1, 1, 1, 2]</code>. Bạn tăng cân bằng 2.</li>
	<li>Vào ngày 2, bạn ăn các chiếc pizza tại các chỉ số <code>[1, 2, 3, 7] = [1, 1, 1, 1]</code>. Bạn tăng cân bằng 1.</li>
</ul>

<p>Tổng khối lượng tăng thêm sau khi ăn tất cả pizza là <code>2 + 1 = 3.</code></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>4 &lt;= n == pizzas.length &lt;= 2 * 10<sup><span style="font-size: 10.8333px;">5</span></sup></code></li>
	<li><code>1 &lt;= pizzas[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>n</code> là bội số của 4.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi ngày ta ăn bốn chiếc pizza: lấy chiếc nặng nhất vào ngày lẻ, chiếc nặng thứ hai vào ngày chẵn. $n$ là bội số của $4$ và có thể lên đến $2\times 10^5$, nên không thể tìm kiếm các cách chia nhóm.
>
> Các ngày lẻ nên lấy những chiếc pizza lớn nhất; vào ngày chẵn, ta phải “bỏ phí” một chiếc lớn hơn để lấy chiếc lớn thứ hai. Sau khi sắp xếp, các giá trị đóng góp này nằm ở những chỉ số cố định.
>
> Đặt $\textit{odd}=\lceil\textit{days}/2\rceil$. Ta cộng $\textit{odd}$ chiếc pizza lớn nhất, sau đó từ phần còn lại lấy cách một chiếc để làm giá trị lớn thứ hai của các ngày chẵn.

<!-- thinking:end -->

Theo đề bài, mỗi ngày chúng ta có thể ăn $4$ chiếc pizza. Vào ngày lẻ, chúng ta nhận được giá trị lớn nhất trong $4$ chiếc pizza này, còn vào ngày chẵn, chúng ta nhận được giá trị lớn thứ hai trong $4$ chiếc pizza.

Do đó, ta có thể sắp xếp các chiếc pizza theo khối lượng tăng dần. Ta có thể ăn trong $\textit{days} = n / 4$ ngày, trong đó có $\textit{odd} = (\textit{days} + 1) / 2$ ngày lẻ và $\textit{even} = \textit{days} - \textit{odd}$ ngày chẵn.

Vào các ngày lẻ, ta có thể chọn $\textit{odd}$ chiếc pizza lớn nhất và $\textit{odd} \times 3$ chiếc nhỏ nhất, khi đó khối lượng tăng thêm là $\sum_{i = n - \textit{odd}}^{n - 1} \textit{pizzas}[i]$.

Vào các ngày chẵn, trong số những chiếc pizza còn lại, mỗi lần ta tham lam chọn hai chiếc lớn nhất và hai chiếc nhỏ nhất, khi đó khối lượng tăng thêm là $\sum_{i = n - \textit{odd} - 2}^{n - \textit{odd} - 2 \times \textit{even}} \textit{pizzas}[i]$.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(\log n)$. Ở đây, $n$ là độ dài của mảng $\textit{pizzas}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxWeight(self, pizzas: List[int]) -> int:
        days = len(pizzas) // 4
        pizzas.sort()
        odd = (days + 1) // 2
        even = days - odd
        ans = sum(pizzas[-odd:])
        i = len(pizzas) - odd - 2
        for _ in range(even):
            ans += pizzas[i]
            i -= 2
        return ans
```

#### Java

```java
class Solution {
    public long maxWeight(int[] pizzas) {
        int n = pizzas.length;
        int days = n / 4;
        Arrays.sort(pizzas);
        int odd = (days + 1) / 2;
        int even = days / 2;
        long ans = 0;
        for (int i = n - odd; i < n; ++i) {
            ans += pizzas[i];
        }
        for (int i = n - odd - 2; even > 0; --even) {
            ans += pizzas[i];
            i -= 2;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxWeight(vector<int>& pizzas) {
        int n = pizzas.size();
        int days = pizzas.size() / 4;
        ranges::sort(pizzas);
        int odd = (days + 1) / 2;
        int even = days - odd;
        long long ans = accumulate(pizzas.begin() + n - odd, pizzas.end(), 0LL);
        for (int i = n - odd - 2; even; --even) {
            ans += pizzas[i];
            i -= 2;
        }
        return ans;
    }
};
```

#### Go

```go
func maxWeight(pizzas []int) (ans int64) {
	n := len(pizzas)
	days := n / 4
	sort.Ints(pizzas)
	odd := (days + 1) / 2
	even := days - odd
	for i := n - odd; i < n; i++ {
		ans += int64(pizzas[i])
	}
	for i := n - odd - 2; even > 0; even-- {
		ans += int64(pizzas[i])
		i -= 2
	}
	return
}
```

#### TypeScript

```ts
function maxWeight(pizzas: number[]): number {
    const n = pizzas.length;
    const days = n >> 2;
    pizzas.sort((a, b) => a - b);
    const odd = (days + 1) >> 1;
    let even = days - odd;
    let ans = 0;
    for (let i = n - odd; i < n; ++i) {
        ans += pizzas[i];
    }
    for (let i = n - odd - 2; even; --even) {
        ans += pizzas[i];
        i -= 2;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
