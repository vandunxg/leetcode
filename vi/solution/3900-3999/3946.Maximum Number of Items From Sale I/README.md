---
comments: true
difficulty: Medium
rating: 1728
source: Weekly Contest 504 Q2
tags:
    - Greedy
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3946. Maximum Number of Items From Sale I](https://leetcode.com/problems/maximum-number-of-items-from-sale-i)

[中文文档](/solution/3900-3999/3946.Maximum%20Number%20of%20Items%20From%20Sale%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên 2 chiều <code>items</code>, trong đó <code>items[i] = [factor<sub>i</sub>, price<sub>i</sub>]</code> biểu diễn vật phẩm thứ <code>i<sup>th</sup></code>. Ngoài ra, cho một số nguyên <code>budget</code>.</p>

<p>Có vô hạn bản sao của mỗi vật phẩm để mua. Bạn có thể mua bao nhiêu bản sao của bất kỳ vật phẩm nào tùy ý sao cho tổng chi phí của các bản sao đã mua không vượt quá <code>budget</code>.</p>

<p>Sau khi mua vật phẩm, bạn có thể nhận các bản sao miễn phí theo những quy tắc sau:</p>

<ul>
	<li>Với mỗi vật phẩm <code>i</code> mà bạn đã mua <strong>ít nhất một bản sao</strong>, bạn nhận được <strong>một bản sao miễn phí</strong> của mọi vật phẩm <code>j</code> sao cho <code>j != i</code> và <code>factor<sub>i</sub></code> chia hết <code>factor<sub>j</sub></code>.</li>
	<li>Mua nhiều bản sao của cùng vật phẩm <code>i</code> <strong>không</strong> mang lại thêm bản sao miễn phí thông qua vật phẩm <code>i</code>.</li>
	<li>Cùng một vật phẩm <code>j</code> có thể được nhận nhiều lần miễn phí nếu nó được nhận từ việc mua các loại vật phẩm khác nhau.</li>
</ul>

<p>Trả về <strong>tổng số bản sao vật phẩm lớn nhất</strong> mà bạn có thể nhận được, bao gồm cả bản sao đã mua và bản sao miễn phí, khi chi phí cho các vật phẩm đã mua không vượt quá <code>budget</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">items = [[6,2],[2,6],[3,4]], budget = 9</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Bạn có thể mua 2 bản sao vật phẩm 0 và 1 bản sao vật phẩm 2 với tổng chi phí <code>2 * 2 + 4 = 8</code>, không vượt quá <code>budget = 9</code>.</li>
	<li>Mua vật phẩm 2 cho 1 bản sao miễn phí của vật phẩm 0, vì <code>factor<sub>2</sub> = 3</code> chia hết <code>factor<sub>0</sub> = 6</code>.</li>
	<li>Bạn có 3 bản sao đã mua và 1 bản sao miễn phí, tổng cộng là 4 bản sao vật phẩm.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">items = [[2,4],[3,2],[4,1],[6,4],[12,4]], budget = 8</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">10</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Bạn có thể mua 1 bản sao vật phẩm 0, 1 bản sao vật phẩm 1 và 2 bản sao vật phẩm 2 với tổng chi phí <code>4 + 2 + 2 * 1 = 8</code>.</li>
	<li>Mua vật phẩm 0 cho 1 bản sao miễn phí của các vật phẩm 2, 3 và 4.</li>
	<li>Mua vật phẩm 1 cho 1 bản sao miễn phí của các vật phẩm 3 và 4.</li>
	<li>Mua vật phẩm 2 cho 1 bản sao miễn phí của vật phẩm 4.</li>
	<li>Vì vậy, bạn nhận được 6 bản sao miễn phí. Bạn có 4 bản sao đã mua và 6 bản sao miễn phí, tổng cộng là 10 bản sao vật phẩm.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= items.length &lt;= 1000</code></li>
	<li><code>items[i] = [factor<sub>i</sub>, price<sub>i</sub>]</code></li>
	<li><code>1 &lt;= factor<sub>i</sub>, price<sub>i</sub> &lt;= 1500</code></li>
	<li><code>1 &lt;= budget &lt;= 1500</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động (Ba lô 0-1)

<!-- thinking:start -->

> **Tư duy**
>
> $n$ và budget tối đa là $1500$. Lần mua đầu tiên nhận được vật phẩm miễn phí nhờ tính chia hết; các lần mua sau chỉ dùng số tiền còn lại để mua vật phẩm có giá thấp nhất. Hai giai đoạn này cần được tách riêng.
>
> Bài toán ba lô $0$-$1$ mô hình hóa lần mua đầu tiên: vật phẩm $i$ có chi phí $\textit{price}$ và mang lại $1$ cộng với số loại vật phẩm mà nó chia hết. Sau đó, với mỗi khoản chi $i$ trong ba lô, ta cộng thêm $\lfloor(\textit{budget}-i)/\textit{mn}\rfloor$ vật phẩm rẻ nhất.
>
> Giá trị lớn nhất trong các tổng đó là đáp án.

<!-- thinking:end -->

Vì mua bản sao đầu tiên của một loại vật phẩm là đặc biệt và mang lại các vật phẩm miễn phí, ta xét riêng vật phẩm đầu tiên được mua và các lần mua sau đó.

Với vật phẩm đầu tiên được mua, giả sử ta chi ngân sách $i$ và nhận được tổng cộng $f[i]$ vật phẩm, bao gồm cả vật phẩm đã mua và vật phẩm miễn phí. Với các lần mua sau đó, ta có thể dùng ngân sách còn lại $\text{budget} - i$ để mua vật phẩm rẻ nhất, nhận được $\lfloor \frac{\text{budget} - i}{\text{mn}} \rfloor$ vật phẩm, trong đó $\text{mn}$ là giá nhỏ nhất của mọi vật phẩm. Vì vậy, ta duyệt khoản ngân sách $i$ dùng cho lần mua đầu tiên và tính giá trị lớn nhất của $f[i] + \lfloor \frac{\text{budget} - i}{\text{mn}} \rfloor$, đây là đáp án cuối cùng.

Độ phức tạp thời gian là $O(n^2 + n \times m)$, trong đó $n$ là số vật phẩm và $m$ là ngân sách.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumSaleItems(self, items: List[List[int]], budget: int) -> int:
        f = [0] * (budget + 1)
        mn = inf
        for factor, price in items:
            mn = min(mn, price)
            cnt = sum(factor_j % factor == 0 for factor_j, _ in items)
            for j in range(budget, price - 1, -1):
                f[j] = max(f[j], f[j - price] + cnt)
        return max(x + (budget - i) // mn for i, x in enumerate(f))
```

#### Java

```java
class Solution {
    public int maximumSaleItems(int[][] items, int budget) {
        int[] f = new int[budget + 1];
        int mn = Integer.MAX_VALUE;

        for (int[] item : items) {
            int factor = item[0];
            int price = item[1];

            mn = Math.min(mn, price);

            int cnt = 0;
            for (int[] jItem : items) {
                if (jItem[0] % factor == 0) {
                    cnt++;
                }
            }

            for (int j = budget; j >= price; j--) {
                f[j] = Math.max(f[j], f[j - price] + cnt);
            }
        }

        int ans = 0;
        for (int i = 0; i <= budget; i++) {
            ans = Math.max(ans, f[i] + (budget - i) / mn);
        }

        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumSaleItems(vector<vector<int>>& items, int budget) {
        vector<int> f(budget + 1, 0);
        int mn = INT_MAX;

        for (const auto& item : items) {
            int factor = item[0];
            int price = item[1];

            mn = min(mn, price);

            int cnt = 0;
            for (const auto& jItem : items) {
                if (jItem[0] % factor == 0) {
                    cnt++;
                }
            }

            for (int j = budget; j >= price; --j) {
                f[j] = max(f[j], f[j - price] + cnt);
            }
        }

        int ans = 0;
        for (int i = 0; i <= budget; ++i) {
            ans = max(ans, f[i] + (budget - i) / mn);
        }

        return ans;
    }
};
```

#### Go

```go
func maximumSaleItems(items [][]int, budget int) int {
	f := make([]int, budget+1)
	mn := math.MaxInt32

	for _, item := range items {
		factor := item[0]
		price := item[1]
		mn = min(mn, price)

		cnt := 0
		for _, jItem := range items {
			if jItem[0]%factor == 0 {
				cnt++
			}
		}

		for j := budget; j >= price; j-- {
			if f[j-price]+cnt > f[j] {
				f[j] = f[j-price] + cnt
			}
		}
	}

	ans := 0
	for i, x := range f {
		extra := (budget - i) / mn
		ans = max(ans, x+extra)
	}

	return ans
}
```

#### TypeScript

```ts
function maximumSaleItems(items: number[][], budget: number): number {
    const f: number[] = new Array(budget + 1).fill(0);
    let mn: number = Infinity;

    for (const [factor, price] of items) {
        mn = Math.min(mn, price);

        let cnt = 0;
        for (const [factor_j, _] of items) {
            if (factor_j % factor === 0) {
                cnt++;
            }
        }

        for (let j = budget; j >= price; j--) {
            f[j] = Math.max(f[j], f[j - price] + cnt);
        }
    }

    let ans = 0;
    for (let i = 0; i <= budget; i++) {
        ans = Math.max(ans, f[i] + Math.floor((budget - i) / mn));
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
