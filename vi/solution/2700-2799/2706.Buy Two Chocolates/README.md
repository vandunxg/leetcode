---
comments: true
difficulty: Easy
rating: 1207
source: Biweekly Contest 105 Q1
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [2706. Buy Two Chocolates](https://leetcode.com/problems/buy-two-chocolates)

[中文文档](/solution/2700-2799/2706.Buy%20Two%20Chocolates/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>prices</code> biểu diễn giá của các loại chocolate trong cửa hàng. Bạn cũng được cho một số nguyên <code>money</code>, biểu diễn số tiền ban đầu của mình.</p>

<p>Bạn phải mua <strong>chính xác</strong> hai thanh chocolate sao cho vẫn còn một khoản tiền thừa <strong>không âm</strong>. Bạn muốn tổng giá của hai thanh chocolate đã mua là nhỏ nhất.</p>

<p>Trả về <em>số tiền còn lại sau khi mua hai thanh chocolate</em>. Nếu không có cách nào mua hai thanh chocolate mà không mắc nợ, trả về <code>money</code>. Lưu ý rằng số tiền còn lại phải không âm.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> prices = [1,2,2], money = 3
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Mua hai thanh chocolate có giá lần lượt là 1 và 2 đơn vị. Sau đó bạn còn 3 - 3 = 0 đơn vị tiền. Vì vậy, ta trả về 0.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> prices = [3,2,3], money = 3
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Bạn không thể mua 2 thanh chocolate mà không mắc nợ, nên ta trả về 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= prices.length &lt;= 50</code></li>
	<li><code>1 &lt;= prices[i] &lt;= 100</code></li>
	<li><code>1 &lt;= money &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Ta phải mua chính xác hai thanh chocolate mà không vượt quá $money$ và tối đa hóa số tiền còn lại. Với $n\le 50$, việc liệt kê các cặp là hoàn toàn phù hợp, nhưng đáp án chỉ phụ thuộc vào hai mức giá nhỏ nhất.
>
> Sắp xếp mảng rồi lấy tổng $cost$ của hai phần tử đầu tiên. Nếu $cost>money$ thì không mua gì; ngược lại, số tiền còn lại là $money-cost$.

<!-- thinking:end -->

Ta có thể sắp xếp giá của các thanh chocolate theo thứ tự tăng dần, sau đó cộng hai giá đầu tiên để nhận được chi phí nhỏ nhất $cost$ khi mua hai thanh chocolate. Nếu chi phí này lớn hơn số tiền hiện có, ta trả về `money`. Ngược lại, ta trả về `money - cost`.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(\log n)$. Trong đó $n$ là độ dài của mảng `prices`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def buyChoco(self, prices: List[int], money: int) -> int:
        prices.sort()
        cost = prices[0] + prices[1]
        return money if money < cost else money - cost
```

#### Java

```java
class Solution {
    public int buyChoco(int[] prices, int money) {
        Arrays.sort(prices);
        int cost = prices[0] + prices[1];
        return money < cost ? money : money - cost;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int buyChoco(vector<int>& prices, int money) {
        sort(prices.begin(), prices.end());
        int cost = prices[0] + prices[1];
        return money < cost ? money : money - cost;
    }
};
```

#### Go

```go
func buyChoco(prices []int, money int) int {
	sort.Ints(prices)
	cost := prices[0] + prices[1]
	if money < cost {
		return money
	}
	return money - cost
}
```

#### TypeScript

```ts
function buyChoco(prices: number[], money: number): number {
    prices.sort((a, b) => a - b);
    const cost = prices[0] + prices[1];
    return money < cost ? money : money - cost;
}
```

#### Rust

```rust
impl Solution {
    pub fn buy_choco(mut prices: Vec<i32>, money: i32) -> i32 {
        prices.sort();
        let cost = prices[0] + prices[1];
        if cost > money {
            return money;
        }
        money - cost
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Duyệt một lần

<!-- thinking:start -->

> **Tư duy**
>
> Việc sắp xếp tốn $O(n\log n)$ trong khi ta chỉ cần hai giá trị nhỏ nhất. Một lượt duyệt duy nhất để theo dõi giá trị nhỏ nhất và nhỏ thứ hai cũng cho cùng $cost$ trong thời gian tuyến tính.

<!-- thinking:end -->

Ta có thể tìm hai mức giá nhỏ nhất trong một lượt duyệt, sau đó tính chi phí.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng `prices`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def buyChoco(self, prices: List[int], money: int) -> int:
        a = b = inf
        for x in prices:
            if x < a:
                a, b = x, a
            elif x < b:
                b = x
        cost = a + b
        return money if money < cost else money - cost
```

#### Java

```java
class Solution {
    public int buyChoco(int[] prices, int money) {
        int a = 1000, b = 1000;
        for (int x : prices) {
            if (x < a) {
                b = a;
                a = x;
            } else if (x < b) {
                b = x;
            }
        }
        int cost = a + b;
        return money < cost ? money : money - cost;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int buyChoco(vector<int>& prices, int money) {
        int a = 1000, b = 1000;
        for (int x : prices) {
            if (x < a) {
                b = a;
                a = x;
            } else if (x < b) {
                b = x;
            }
        }
        int cost = a + b;
        return money < cost ? money : money - cost;
    }
};
```

#### Go

```go
func buyChoco(prices []int, money int) int {
	a, b := 1001, 1001
	for _, x := range prices {
		if x < a {
			a, b = x, a
		} else if x < b {
			b = x
		}
	}
	cost := a + b
	if money < cost {
		return money
	}
	return money - cost
}
```

#### TypeScript

```ts
function buyChoco(prices: number[], money: number): number {
    let [a, b] = [1000, 1000];
    for (const x of prices) {
        if (x < a) {
            b = a;
            a = x;
        } else if (x < b) {
            b = x;
        }
    }
    const cost = a + b;
    return money < cost ? money : money - cost;
}
```

#### Rust

```rust
impl Solution {
    pub fn buy_choco(prices: Vec<i32>, money: i32) -> i32 {
        let mut a = 1000;
        let mut b = 1000;
        for &x in prices.iter() {
            if x < a {
                b = a;
                a = x;
            } else if x < b {
                b = x;
            }
        }
        let cost = a + b;
        if money < cost {
            money
        } else {
            money - cost
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
