---
comments: true
difficulty: Medium
rating: 1840
source: Biweekly Contest 82 Q2
tags:
    - Array
    - Two Pointers
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [2332. The Latest Time to Catch a Bus](https://leetcode.com/problems/the-latest-time-to-catch-a-bus)

[中文文档](/solution/2300-2399/2332.The%20Latest%20Time%20to%20Catch%20a%20Bus/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>buses</code> có độ dài <code>n</code>, <strong>được đánh chỉ số từ 0</strong>, trong đó <code>buses[i]</code> là thời điểm khởi hành của chuyến xe buýt thứ <code>i<sup>th</sup></code>. Bạn cũng được cho một mảng số nguyên <code>passengers</code> có độ dài <code>m</code>, <strong>được đánh chỉ số từ 0</strong>, trong đó <code>passengers[j]</code> là thời điểm đến của hành khách thứ <code>j<sup>th</sup></code>. Tất cả thời điểm khởi hành của xe buýt đều khác nhau. Tất cả thời điểm đến của hành khách đều khác nhau.</p>

<p>Bạn được cho một số nguyên <code>capacity</code>, biểu thị <strong>số lượng tối đa</strong> hành khách có thể lên mỗi xe buýt.</p>

<p>Khi một hành khách đến, họ sẽ xếp hàng chờ chuyến xe buýt khả dụng tiếp theo. Bạn có thể lên chuyến xe khởi hành tại <code>x</code> phút nếu bạn đến tại <code>y</code> phút với <code>y &lt;= x</code>, và xe buýt chưa đầy. Hành khách có thời điểm đến <strong>sớm nhất</strong> sẽ được lên xe trước.</p>

<p>Cụ thể hơn, khi một xe buýt đến, sẽ xảy ra một trong hai trường hợp:</p>

<ul>
	<li>Nếu có <code>capacity</code> hành khách hoặc ít hơn đang chờ xe buýt, <strong>tất cả</strong> họ sẽ lên xe, hoặc</li>
	<li><code>capacity</code> hành khách có thời điểm đến <strong>sớm nhất</strong> sẽ lên xe.</li>
</ul>

<p>Hãy trả về <em>thời điểm muộn nhất bạn có thể đến trạm xe buýt để bắt xe</em>. Bạn <strong>không được</strong> đến cùng thời điểm với một hành khách khác.</p>

<p><strong>Lưu ý: </strong>Các mảng <code>buses</code> và <code>passengers</code> không nhất thiết đã được sắp xếp.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> buses = [10,20], passengers = [2,17,18,19], capacity = 2
<strong>Đầu ra:</strong> 16
<strong>Giải thích:</strong> Giả sử bạn đến vào thời điểm 16.
Vào thời điểm 10, chuyến xe đầu tiên khởi hành cùng hành khách thứ 0<sup>th</sup>.
Vào thời điểm 20, chuyến xe thứ hai khởi hành cùng bạn và hành khách thứ 1<sup>st</sup>.
Lưu ý rằng bạn không được đến cùng thời điểm với một hành khách khác, vì vậy bạn phải đến trước hành khách thứ 1<sup>st</sup> để bắt xe.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> buses = [20,30,10], passengers = [19,13,26,4,25,11,21], capacity = 2
<strong>Đầu ra:</strong> 20
<strong>Giải thích:</strong> Giả sử bạn đến vào thời điểm 20.
Vào thời điểm 10, chuyến xe đầu tiên khởi hành cùng hành khách thứ 3<sup>rd</sup>.
Vào thời điểm 20, chuyến xe thứ hai khởi hành cùng hành khách thứ 5<sup>th</sup> và thứ 1<sup>st</sup>.
Vào thời điểm 30, chuyến xe thứ ba khởi hành cùng hành khách thứ 0<sup>th</sup> và bạn.
Lưu ý rằng nếu bạn đến muộn hơn, hành khách thứ 6<sup>th</sup> sẽ chiếm chỗ của bạn trên chuyến xe thứ ba.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == buses.length</code></li>
	<li><code>m == passengers.length</code></li>
	<li><code>1 &lt;= n, m, capacity &lt;= 10<sup>5</sup></code></li>
	<li><code>2 &lt;= buses[i], passengers[i] &lt;= 10<sup>9</sup></code></li>
	<li>Mỗi phần tử trong <code>buses</code> là <strong>duy nhất</strong>.</li>
	<li>Mỗi phần tử trong <code>passengers</code> là <strong>duy nhất</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm thời điểm đến muộn nhất mà không trùng với hành khách khác. Cả hai mảng đều có thể có tới $10^5$ phần tử, nên ta sắp xếp rồi mô phỏng quá trình lên xe.
>
> Xếp hành khách lên từng xe theo thứ tự đến và ghi nhận số ghế còn lại trên chuyến xe cuối cùng. Nếu còn ghế, lùi dần từ thời điểm khởi hành cuối cùng; nếu không, lùi dần từ hành khách cuối cùng đã lên xe, bỏ qua các thời điểm đã có người.

<!-- thinking:end -->

Trước tiên, chúng ta sắp xếp, sau đó dùng hai con trỏ để mô phỏng quá trình hành khách lên xe: duyệt các xe buýt $bus$, còn hành khách tuân theo nguyên tắc "đến trước, phục vụ trước".

Sau khi mô phỏng kết thúc, hãy kiểm tra xem chuyến xe cuối cùng còn chỗ hay không:

- Nếu còn chỗ, chúng ta có thể đến trạm xe buýt vào thời điểm chuyến xe khởi hành tại $bus[|bus|-1]$; nếu có người vào thời điểm này, chúng ta có thể tìm thời điểm không có ai đến bằng cách đi về phía trước.
- Nếu không còn chỗ, chúng ta có thể tìm hành khách cuối cùng đã lên xe, rồi tìm thời điểm không có ai đến bằng cách đi về phía trước từ thời điểm của hành khách đó.

Độ phức tạp thời gian là $O(n \times \log n + m \times \log m)$, và độ phức tạp không gian là $O(\log n + \log m)$. Trong đó $n$ và $m$ lần lượt là số lượng xe buýt và hành khách.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def latestTimeCatchTheBus(
        self, buses: List[int], passengers: List[int], capacity: int
    ) -> int:
        buses.sort()
        passengers.sort()
        j = 0
        for t in buses:
            c = capacity
            while c and j < len(passengers) and passengers[j] <= t:
                c, j = c - 1, j + 1
        j -= 1
        ans = buses[-1] if c else passengers[j]
        while ~j and passengers[j] == ans:
            ans, j = ans - 1, j - 1
        return ans
```

#### Java

```java
class Solution {
    public int latestTimeCatchTheBus(int[] buses, int[] passengers, int capacity) {
        Arrays.sort(buses);
        Arrays.sort(passengers);
        int j = 0, c = 0;
        for (int t : buses) {
            c = capacity;
            while (c > 0 && j < passengers.length && passengers[j] <= t) {
                --c;
                ++j;
            }
        }
        --j;
        int ans = c > 0 ? buses[buses.length - 1] : passengers[j];
        while (j >= 0 && ans == passengers[j]) {
            --ans;
            --j;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int latestTimeCatchTheBus(vector<int>& buses, vector<int>& passengers, int capacity) {
        sort(buses.begin(), buses.end());
        sort(passengers.begin(), passengers.end());
        int j = 0, c = 0;
        for (int t : buses) {
            c = capacity;
            while (c && j < passengers.size() && passengers[j] <= t) --c, ++j;
        }
        --j;
        int ans = c ? buses[buses.size() - 1] : passengers[j];
        while (~j && ans == passengers[j]) --j, --ans;
        return ans;
    }
};
```

#### Go

```go
func latestTimeCatchTheBus(buses []int, passengers []int, capacity int) int {
	sort.Ints(buses)
	sort.Ints(passengers)
	j, c := 0, 0
	for _, t := range buses {
		c = capacity
		for c > 0 && j < len(passengers) && passengers[j] <= t {
			j++
			c--
		}
	}
	j--
	ans := buses[len(buses)-1]
	if c == 0 {
		ans = passengers[j]
	}
	for j >= 0 && ans == passengers[j] {
		ans--
		j--
	}
	return ans
}
```

#### TypeScript

```ts
function latestTimeCatchTheBus(buses: number[], passengers: number[], capacity: number): number {
    buses.sort((a, b) => a - b);
    passengers.sort((a, b) => a - b);
    let [j, c] = [0, 0];
    for (const t of buses) {
        c = capacity;
        while (c && j < passengers.length && passengers[j] <= t) {
            --c;
            ++j;
        }
    }
    --j;
    let ans = c > 0 ? buses.at(-1)! : passengers[j];
    while (j >= 0 && passengers[j] === ans) {
        --ans;
        --j;
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {number[]} buses
 * @param {number[]} passengers
 * @param {number} capacity
 * @return {number}
 */
var latestTimeCatchTheBus = function (buses, passengers, capacity) {
    buses.sort((a, b) => a - b);
    passengers.sort((a, b) => a - b);
    let [j, c] = [0, 0];
    for (const t of buses) {
        c = capacity;
        while (c && j < passengers.length && passengers[j] <= t) {
            --c;
            ++j;
        }
    }
    --j;
    let ans = c > 0 ? buses.at(-1) : passengers[j];
    while (j >= 0 && passengers[j] === ans) {
        --ans;
        --j;
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
