---
comments: true
difficulty: Easy
rating: 1283
source: Weekly Contest 297 Q1
tags:
    - Array
    - Simulation
---

<!-- problem:start -->

# [2303. Calculate Amount Paid in Taxes](https://leetcode.com/problems/calculate-amount-paid-in-taxes)

[中文文档](/solution/2300-2399/2303.Calculate%20Amount%20Paid%20in%20Taxes/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên 2D <code>brackets</code> được đánh chỉ số từ <strong>0</strong>, trong đó <code>brackets[i] = [upper<sub>i</sub>, percent<sub>i</sub>]</code> cho biết khung thuế thứ <code>i<sup>th</sup></code> có giới hạn trên là <code>upper<sub>i</sub></code> và được áp dụng mức thuế <code>percent<sub>i</sub></code>. Các khung thuế được <strong>sắp xếp</strong> theo giới hạn trên (tức là <code>upper<sub>i-1</sub> &lt; upper<sub>i</sub></code> với <code>0 &lt; i &lt; brackets.length</code>).</p>

<p>Thuế được tính như sau:</p>

<ul>
	<li><code>upper<sub>0</sub></code> dollar đầu tiên kiếm được chịu mức thuế <code>percent<sub>0</sub></code>.</li>
	<li><code>upper<sub>1</sub> - upper<sub>0</sub></code> dollar tiếp theo kiếm được chịu mức thuế <code>percent<sub>1</sub></code>.</li>
	<li><code>upper<sub>2</sub> - upper<sub>1</sub></code> dollar tiếp theo kiếm được chịu mức thuế <code>percent<sub>2</sub></code>.</li>
	<li>Và tiếp tục như vậy.</li>
</ul>

<p>Cho một số nguyên <code>income</code> biểu diễn số tiền bạn kiếm được. Hãy trả về <em>số tiền thuế bạn phải nộp.</em> Các đáp án có sai số không quá <code>10<sup>-5</sup></code> so với đáp án thực tế đều được chấp nhận.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> brackets = [[3,50],[7,10],[12,25]], income = 10
<strong>Đầu ra:</strong> 2.65000
<strong>Giải thích:</strong>
Dựa trên thu nhập của bạn, có 3 dollar thuộc khung thuế thứ <sup>1</sup>, 4 dollar thuộc khung thuế thứ <sup>2</sup> và 3 dollar thuộc khung thuế thứ <sup>3</sup>.
Mức thuế của ba khung lần lượt là 50%, 10% và 25%.
Tổng cộng, bạn phải nộp $3 * 50% + $4 * 10% + $3 * 25% = $2.65 tiền thuế.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> brackets = [[1,0],[4,25],[5,50]], income = 2
<strong>Đầu ra:</strong> 0.25000
<strong>Giải thích:</strong>
Dựa trên thu nhập của bạn, có 1 dollar thuộc khung thuế thứ <sup>1</sup> và 1 dollar thuộc khung thuế thứ <sup>2</sup>.
Mức thuế của hai khung lần lượt là 0% và 25%.
Tổng cộng, bạn phải nộp $1 * 0% + $1 * 25% = $0.25 tiền thuế.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> brackets = [[2,50]], income = 0
<strong>Đầu ra:</strong> 0.00000
<strong>Giải thích:</strong>
Bạn không có thu nhập chịu thuế, nên tổng số thuế phải nộp là $0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= brackets.length &lt;= 100</code></li>
	<li><code>1 &lt;= upper<sub>i</sub> &lt;= 1000</code></li>
	<li><code>0 &lt;= percent<sub>i</sub> &lt;= 100</code></li>
	<li><code>0 &lt;= income &lt;= 1000</code></li>
	<li><code>upper<sub>i</sub></code> được sắp xếp theo thứ tự tăng dần.</li>
	<li>Tất cả giá trị của <code>upper<sub>i</sub></code> đều <strong>khác nhau</strong>.</li>
	<li>Giới hạn trên của khung thuế cuối cùng lớn hơn hoặc bằng <code>income</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Các khung thuế tăng dần theo giới hạn trên và khung cuối cùng bao phủ toàn bộ thu nhập, nên tiền thuế được quyết định bởi phần của $income$ nằm trong mỗi khung. Có nhiều nhất $100$ khung thuế, vì vậy chỉ cần duyệt qua một lần.
>
> Phần thu nhập chịu thuế của khung $i$ là $\min(income, upper_i)$ trừ đi giới hạn trên trước đó (hoặc bằng 0 nếu chưa chạm đến khung này). Ta nhân với mức thuế, cộng dồn kết quả rồi chia cho $100$. Không cần quay lui hay tính toán trước.

<!-- thinking:end -->

Ta duyệt qua `brackets`, với mỗi khung thuế, tính số tiền thuế của khung đó rồi cộng dồn vào kết quả.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của `brackets`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def calculateTax(self, brackets: List[List[int]], income: int) -> float:
        ans = prev = 0
        for upper, percent in brackets:
            ans += max(0, min(income, upper) - prev) * percent
            prev = upper
        return ans / 100
```

#### Java

```java
class Solution {
    public double calculateTax(int[][] brackets, int income) {
        int ans = 0, prev = 0;
        for (var e : brackets) {
            int upper = e[0], percent = e[1];
            ans += Math.max(0, Math.min(income, upper) - prev) * percent;
            prev = upper;
        }
        return ans / 100.0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    double calculateTax(vector<vector<int>>& brackets, int income) {
        int ans = 0, prev = 0;
        for (auto& e : brackets) {
            int upper = e[0], percent = e[1];
            ans += max(0, min(income, upper) - prev) * percent;
            prev = upper;
        }
        return ans / 100.0;
    }
};
```

#### Go

```go
func calculateTax(brackets [][]int, income int) float64 {
	var ans, prev int
	for _, e := range brackets {
		upper, percent := e[0], e[1]
		ans += max(0, min(income, upper)-prev) * percent
		prev = upper
	}
	return float64(ans) / 100.0
}
```

#### TypeScript

```ts
function calculateTax(brackets: number[][], income: number): number {
    let ans = 0;
    let prev = 0;
    for (const [upper, percent] of brackets) {
        ans += Math.max(0, Math.min(income, upper) - prev) * percent;
        prev = upper;
    }
    return ans / 100;
}
```

#### Rust

```rust
impl Solution {
    pub fn calculate_tax(brackets: Vec<Vec<i32>>, income: i32) -> f64 {
        let mut res = 0f64;
        let mut pre = 0i32;
        for bracket in brackets.iter() {
            res += f64::from(income.min(bracket[0]) - pre) * f64::from(bracket[1]) * 0.01;
            if income <= bracket[0] {
                break;
            }
            pre = bracket[0];
        }
        res
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
