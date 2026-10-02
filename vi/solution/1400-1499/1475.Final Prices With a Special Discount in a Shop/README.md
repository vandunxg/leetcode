---
comments: true
difficulty: Easy
rating: 1212
source: Biweekly Contest 28 Q1
tags:
    - Stack
    - Array
    - Monotonic Stack
---

<!-- problem:start -->

# [1475. Final Prices With a Special Discount in a Shop](https://leetcode.com/problems/final-prices-with-a-special-discount-in-a-shop)

[中文文档](/solution/1400-1499/1475.Final%20Prices%20With%20a%20Special%20Discount%20in%20a%20Shop/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>prices</code>, trong đó <code>prices[i]</code> là giá của mặt hàng thứ <code>i</code> trong cửa hàng.</p>

<p>Cửa hàng có một chương trình giảm giá đặc biệt. Nếu bạn mua mặt hàng thứ <code>i</code>, bạn sẽ được giảm một khoản tương đương với <code>prices[j]</code>, trong đó <code>j</code> là chỉ số nhỏ nhất sao cho <code>j &gt; i</code> và <code>prices[j] &lt;= prices[i]</code>. Nếu không, bạn sẽ không được giảm giá.</p>

<p>Hãy trả về một mảng số nguyên <code>answer</code>, trong đó <code>answer[i]</code> là giá cuối cùng bạn phải trả cho mặt hàng thứ <code>i</code> trong cửa hàng, sau khi áp dụng chương trình giảm giá đặc biệt.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> prices = [8,4,6,2,3]
<strong>Đầu ra:</strong> [4,2,4,2,3]
<strong>Giải thích:</strong>
Với mặt hàng 0 có price[0]=8, bạn sẽ được giảm một khoản tương đương với prices[1]=4, do đó giá cuối cùng bạn phải trả là 8 - 4 = 4.
Với mặt hàng 1 có price[1]=4, bạn sẽ được giảm một khoản tương đương với prices[3]=2, do đó giá cuối cùng bạn phải trả là 4 - 2 = 2.
Với mặt hàng 2 có price[2]=6, bạn sẽ được giảm một khoản tương đương với prices[3]=2, do đó giá cuối cùng bạn phải trả là 6 - 2 = 4.
Với các mặt hàng 3 và 4, bạn sẽ không được giảm giá.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> prices = [1,2,3,4,5]
<strong>Đầu ra:</strong> [1,2,3,4,5]
<strong>Giải thích:</strong> Trong trường hợp này, tất cả các mặt hàng đều không được giảm giá.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> prices = [10,1,1,6]
<strong>Đầu ra:</strong> [9,0,1,6]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= prices.length &lt;= 500</code></li>
	<li><code>1 &lt;= prices[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Monotonic Stack

<!-- thinking:start -->

> **Tư duy**
>
> Khoản giảm giá là mức giá tiếp theo ở bên phải không lớn hơn mức giá hiện tại. Với $n\le 500$, ta có thể dùng hai vòng lặp; monotonic stack giúp tìm phần tử tiếp theo nhỏ hơn hoặc bằng trong thời gian tuyến tính. Duyệt từ phải sang trái trên một stack tăng dần và trừ trực tiếp vào mảng.

<!-- thinking:end -->

Về bản chất, bài toán là tìm phần tử đầu tiên ở bên phải nhỏ hơn mỗi phần tử. Ta có thể dùng monotonic stack để giải quyết bài toán này.

Ta duyệt mảng $\textit{prices}$ theo thứ tự ngược, dùng monotonic stack để tìm phần tử nhỏ hơn gần nhất ở bên trái của phần tử hiện tại, sau đó tính khoản giảm giá.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $\textit{prices}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def finalPrices(self, prices: List[int]) -> List[int]:
        stk = []
        for i in reversed(range(len(prices))):
            x = prices[i]
            while stk and x < stk[-1]:
                stk.pop()
            if stk:
                prices[i] -= stk[-1]
            stk.append(x)
        return prices
```

#### Java

```java
class Solution {
    public int[] finalPrices(int[] prices) {
        int n = prices.length;
        Deque<Integer> stk = new ArrayDeque<>();
        for (int i = n - 1; i >= 0; --i) {
            int x = prices[i];
            while (!stk.isEmpty() && stk.peek() > x) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                prices[i] -= stk.peek();
            }
            stk.push(x);
        }
        return prices;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> finalPrices(vector<int>& prices) {
        stack<int> stk;
        for (int i = prices.size() - 1; ~i; --i) {
            int x = prices[i];
            while (!stk.empty() && stk.top() > x) {
                stk.pop();
            }
            if (!stk.empty()) {
                prices[i] -= stk.top();
            }
            stk.push(x);
        }
        return prices;
    }
};
```

#### Go

```go
func finalPrices(prices []int) []int {
	stk := []int{}
	for i := len(prices) - 1; i >= 0; i-- {
		x := prices[i]
		for len(stk) > 0 && stk[len(stk)-1] > x {
			stk = stk[:len(stk)-1]
		}
		if len(stk) > 0 {
			prices[i] -= stk[len(stk)-1]
		}
		stk = append(stk, x)
	}
	return prices
}
```

#### TypeScript

```ts
function finalPrices(prices: number[]): number[] {
    const stk: number[] = [];
    for (let i = prices.length - 1; ~i; --i) {
        const x = prices[i];
        while (stk.length && stk.at(-1)! > x) {
            stk.pop();
        }
        prices[i] -= stk.at(-1) || 0;
        stk.push(x);
    }
    return prices;
}
```

#### Rust

```rust
impl Solution {
    pub fn final_prices(mut prices: Vec<i32>) -> Vec<i32> {
        let mut stk: Vec<i32> = Vec::new();
        for i in (0..prices.len()).rev() {
            let x = prices[i];
            while !stk.is_empty() && x < *stk.last().unwrap() {
                stk.pop();
            }
            if let Some(&top) = stk.last() {
                prices[i] -= top;
            }
            stk.push(x);
        }
        prices
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} prices
 * @return {number[]}
 */
var finalPrices = function (prices) {
    const stk = [];
    for (let i = prices.length - 1; ~i; --i) {
        const x = prices[i];
        while (stk.length && stk.at(-1) > x) {
            stk.pop();
        }
        prices[i] -= stk.at(-1) || 0;
        stk.push(x);
    }
    return prices;
};
```

#### PHP

```php
class Solution {
    /**
     * @param Integer[] $prices
     * @return Integer[]
     */
    function finalPrices($prices) {
        $stk = [];
        $n = count($prices);

        for ($i = $n - 1; $i >= 0; $i--) {
            $x = $prices[$i];
            while (!empty($stk) && $x < end($stk)) {
                array_pop($stk);
            }
            if (!empty($stk)) {
                $prices[$i] -= end($stk);
            }
            $stk[] = $x;
        }

        return $prices;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
