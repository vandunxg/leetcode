---
comments: true
difficulty: Medium
rating: 1436
source: Biweekly Contest 42 Q2
tags:
    - Array
    - Simulation
---

<!-- problem:start -->

# [1701. Average Waiting Time](https://leetcode.com/problems/average-waiting-time)

[中文文档](/solution/1700-1799/1701.Average%20Waiting%20Time/README.md)

## Mô tả

<!-- description:start -->

<p>Có một nhà hàng chỉ có một đầu bếp. Cho mảng <code>customers</code>, trong đó <code>customers[i] = [arrival<sub>i</sub>, time<sub>i</sub>]:</code></p>

<ul>
	<li><code>arrival<sub>i</sub></code> là thời điểm đến của khách hàng thứ <code>i<sup>th</sup></code>. Các thời điểm đến được sắp xếp theo thứ tự <strong>không giảm</strong>.</li>
	<li><code>time<sub>i</sub></code> là thời gian cần để chuẩn bị món của khách hàng thứ <code>i<sup>th</sup></code>.</li>
</ul>

<p>Khi khách hàng đến, họ đưa món cho đầu bếp, và đầu bếp bắt đầu chuẩn bị ngay khi rảnh. Khách hàng phải chờ đến khi đầu bếp chuẩn bị xong món. Đầu bếp không chuẩn bị món cho nhiều hơn một khách hàng cùng lúc. Đầu bếp phục vụ khách hàng <strong>theo thứ tự xuất hiện trong input</strong>.</p>

<p>Hãy trả về <em>thời gian chờ <strong>trung bình</strong> của tất cả khách hàng</em>. Lời giải có sai số không quá <code>10<sup>-5</sup></code> so với đáp án thực được chấp nhận.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> customers = [[1,2],[2,5],[4,3]]
<strong>Đầu ra:</strong> 5.00000
<strong>Giải thích:
</strong>1) Khách hàng đầu tiên đến lúc 1, đầu bếp nhận món và bắt đầu chuẩn bị ngay lúc 1, hoàn thành lúc 3, nên thời gian chờ là 3 - 1 = 2.
2) Khách hàng thứ hai đến lúc 2, đầu bếp nhận món và bắt đầu chuẩn bị lúc 3, hoàn thành lúc 8, nên thời gian chờ là 8 - 2 = 6.
3) Khách hàng thứ ba đến lúc 4, đầu bếp nhận món và bắt đầu chuẩn bị lúc 8, hoàn thành lúc 11, nên thời gian chờ là 11 - 4 = 7.
Vậy thời gian chờ trung bình = (2 + 6 + 7) / 3 = 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> customers = [[5,2],[5,4],[10,3],[20,1]]
<strong>Đầu ra:</strong> 3.25000
<strong>Giải thích:
</strong>1) Khách hàng đầu tiên đến lúc 5, đầu bếp nhận món và bắt đầu chuẩn bị ngay lúc 5, hoàn thành lúc 7, nên thời gian chờ là 7 - 5 = 2.
2) Khách hàng thứ hai đến lúc 5, đầu bếp nhận món và bắt đầu chuẩn bị lúc 7, hoàn thành lúc 11, nên thời gian chờ là 11 - 5 = 6.
3) Khách hàng thứ ba đến lúc 10, đầu bếp nhận món và bắt đầu chuẩn bị lúc 11, hoàn thành lúc 14, nên thời gian chờ là 14 - 10 = 4.
4) Khách hàng thứ tư đến lúc 20, đầu bếp nhận món và bắt đầu chuẩn bị ngay lúc 20, hoàn thành lúc 21, nên thời gian chờ là 21 - 20 = 1.
Vậy thời gian chờ trung bình = (2 + 6 + 4 + 1) / 4 = 3.25.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= customers.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= arrival<sub>i</sub>, time<sub>i</sub> &lt;= 10<sup>4</sup></code></li>
	<li><code>arrival<sub>i&nbsp;</sub>&lt;= arrival<sub>i+1</sub></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Thời gian chờ của mỗi khách hàng phụ thuộc vào lúc đầu bếp rảnh và lúc khách đến. Thời điểm đến đã theo thứ tự không giảm, đầu bếp cũng phục vụ theo thứ tự, nên không cần sắp xếp lại mà chỉ cần mô phỏng tuần tự.
>
> Duy trì thời điểm $t$ khi món trước hoàn thành. Với thời điểm đến $a$ và thời gian nấu $b$, việc chuẩn bị bắt đầu tại $\max(t,a)$ và kết thúc tại $\max(t,a)+b$, nên thời gian chờ bằng thời điểm kết thúc trừ $a$.
>
> Cộng tất cả thời gian chờ rồi chia cho số khách hàng.

<!-- thinking:end -->

Ta dùng biến `tot` để ghi nhận tổng thời gian chờ của khách hàng, và biến `t` để ghi nhận thời điểm hoàn thành món của từng khách. Giá trị ban đầu của cả hai biến là $0$.

Ta duyệt mảng khách hàng `customers`. Với mỗi khách hàng:

Nếu thời điểm hiện tại `t` nhỏ hơn hoặc bằng thời điểm đến `customers[i][0]`, nghĩa là đầu bếp đang rảnh và có thể bắt đầu ngay. Thời điểm hoàn thành món là $t = customers[i][0] + customers[i][1]$, còn thời gian chờ của khách là `customers[i][1]`.

Ngược lại, đầu bếp đang bận, nên khách phải chờ đầu bếp hoàn thành các món trước rồi mới bắt đầu chuẩn bị món của mình. Thời điểm hoàn thành món là $t = t + customers[i][1]$, và thời gian chờ là $t - customers[i][0]$.

Độ phức tạp thời gian là $O(n)$, với $n$ là độ dài mảng `customers`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def averageWaitingTime(self, customers: List[List[int]]) -> float:
        tot = t = 0
        for a, b in customers:
            t = max(t, a) + b
            tot += t - a
        return tot / len(customers)
```

#### Java

```java
class Solution {
    public double averageWaitingTime(int[][] customers) {
        double tot = 0;
        int t = 0;
        for (var e : customers) {
            int a = e[0], b = e[1];
            t = Math.max(t, a) + b;
            tot += t - a;
        }
        return tot / customers.length;
    }
}
```

#### C++

```cpp
class Solution {
public:
    double averageWaitingTime(vector<vector<int>>& customers) {
        double tot = 0;
        int t = 0;
        for (auto& e : customers) {
            int a = e[0], b = e[1];
            t = max(t, a) + b;
            tot += t - a;
        }
        return tot / customers.size();
    }
};
```

#### Go

```go
func averageWaitingTime(customers [][]int) float64 {
	tot, t := 0, 0
	for _, e := range customers {
		a, b := e[0], e[1]
		t = max(t, a) + b
		tot += t - a
	}
	return float64(tot) / float64(len(customers))
}
```

#### TypeScript

```ts
function averageWaitingTime(customers: number[][]): number {
    let [tot, t] = [0, 0];
    for (const [a, b] of customers) {
        t = Math.max(t, a) + b;
        tot += t - a;
    }
    return tot / customers.length;
}
```

#### Rust

```rust
impl Solution {
    pub fn average_waiting_time(customers: Vec<Vec<i32>>) -> f64 {
        let mut tot = 0.0;
        let mut t = 0;

        for e in customers.iter() {
            let a = e[0];
            let b = e[1];
            t = t.max(a) + b;
            tot += (t - a) as f64;
        }

        tot / customers.len() as f64
    }
}
```

#### JavaScript

```js
function averageWaitingTime(customers) {
    let [tot, t] = [0, 0];
    for (const [a, b] of customers) {
        t = Math.max(t, a) + b;
        tot += t - a;
    }
    return tot / customers.length;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
