---
comments: true
difficulty: Medium
rating: 1366
source: Weekly Contest 391 Q2
tags:
    - Math
    - Simulation
---

<!-- problem:start -->

# [3100. Water Bottles II](https://leetcode.com/problems/water-bottles-ii)

[中文文档](/solution/3100-3199/3100.Water%20Bottles%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai số nguyên <code>numBottles</code> và <code>numExchange</code>.</p>

<p><code>numBottles</code> là số chai nước đầy ban đầu bạn có. Trong một thao tác, bạn có thể thực hiện một trong các thao tác sau:</p>

<ul>
	<li>Uống bất kỳ số chai nước đầy nào, biến chúng thành chai rỗng.</li>
	<li>Đổi <code>numExchange</code> chai rỗng lấy một chai nước đầy. Sau đó, tăng <code>numExchange</code> lên một.</li>
</ul>

<p>Lưu ý rằng bạn không thể đổi nhiều lô chai rỗng với cùng một giá trị <code>numExchange</code>. Ví dụ, nếu <code>numBottles == 3</code> và <code>numExchange == 1</code>, bạn không thể đổi <code>3</code> chai nước rỗng lấy <code>3</code> chai đầy.</p>

<p>Trả về <em>số chai nước <strong>tối đa</strong> bạn có thể uống</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3100.Water%20Bottles%20II/images/exampleone1.png" style="width: 948px; height: 482px; padding: 10px; background: #fff; border-radius: .5rem;" />
<pre>
<strong>Đầu vào:</strong> numBottles = 13, numExchange = 6
<strong>Đầu ra:</strong> 15
<strong>Giải thích:</strong> Bảng trên cho biết số chai nước đầy, số chai nước rỗng, giá trị của numExchange và số chai đã uống.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3100.Water%20Bottles%20II/images/example231.png" style="width: 990px; height: 642px; padding: 10px; background: #fff; border-radius: .5rem;" />
<pre>
<strong>Đầu vào:</strong> numBottles = 10, numExchange = 3
<strong>Đầu ra:</strong> 13
<strong>Giải thích:</strong> Bảng trên cho biết số chai nước đầy, số chai nước rỗng, giá trị của numExchange và số chai đã uống.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= numBottles &lt;= 100 </code></li>
	<li><code>1 &lt;= numExchange &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lần đổi thành công, ngưỡng đổi tăng thêm một, nên nếu muốn dùng công thức đóng thì phải theo dõi quan hệ bậc hai giữa số chai rỗng và chi phí tăng dần. Cả $n$ và $\textit{numExchange}$ đều không vượt quá $100$, còn vòng lặp trực tiếp chỉ chạy trong $O(\sqrt{n})$, nên đáp ứng giới hạn.
>
> Ta có thể uống hết các chai đầy trước. Sau đó, câu hỏi duy nhất là số chai rỗng hiện có có ít nhất bằng ngưỡng hiện tại hay không. Mỗi lần đổi một chai, uống chai đó rồi tăng ngưỡng, số chai rỗng giảm $\textit{numExchange}-1$.
>
> Cộng $\textit{numBottles}$ vào đáp án, rồi khi số chai rỗng đủ thì trừ ngưỡng, tăng ngưỡng và cộng thêm một chai đã uống. Số lượng tích lũy là số chai tối đa có thể uống.

<!-- thinking:end -->

Ta có thể uống hết các chai nước đầy ngay từ đầu, nên số chai nước uống được ban đầu là $\textit{numBottles}$. Sau đó, ta lặp lại các thao tác sau:

- Nếu hiện có $\textit{numExchange}$ chai nước rỗng, ta có thể đổi chúng lấy một chai nước đầy. Sau khi đổi, giá trị của $\textit{numExchange}$ tăng thêm $1$. Tiếp theo, ta uống chai này, số chai nước đã uống tăng thêm $1$ và số chai nước rỗng tăng thêm $1$.
- Nếu không có $\textit{numExchange}$ chai nước rỗng, ta không thể đổi thêm nước và nên dừng lại.

Ta lặp lại quy trình trên cho đến khi không thể đổi thêm chai nào. Tổng số chai nước đã uống là đáp án.

Độ phức tạp thời gian là $O(\sqrt{n})$, trong đó $n$ là số chai nước đầy ban đầu. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxBottlesDrunk(self, numBottles: int, numExchange: int) -> int:
        ans = numBottles
        while numBottles >= numExchange:
            numBottles -= numExchange
            numExchange += 1
            ans += 1
            numBottles += 1
        return ans
```

#### Java

```java
class Solution {
    public int maxBottlesDrunk(int numBottles, int numExchange) {
        int ans = numBottles;
        while (numBottles >= numExchange) {
            numBottles -= numExchange;
            ++numExchange;
            ++ans;
            ++numBottles;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxBottlesDrunk(int numBottles, int numExchange) {
        int ans = numBottles;
        while (numBottles >= numExchange) {
            numBottles -= numExchange;
            ++numExchange;
            ++ans;
            ++numBottles;
        }
        return ans;
    }
};
```

#### Go

```go
func maxBottlesDrunk(numBottles int, numExchange int) int {
	ans := numBottles
	for numBottles >= numExchange {
		numBottles -= numExchange
		numExchange++
		ans++
		numBottles++
	}
	return ans
}
```

#### TypeScript

```ts
function maxBottlesDrunk(numBottles: number, numExchange: number): number {
    let ans = numBottles;
    while (numBottles >= numExchange) {
        numBottles -= numExchange;
        ++numExchange;
        ++ans;
        ++numBottles;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_bottles_drunk(mut num_bottles: i32, mut num_exchange: i32) -> i32 {
        let mut ans = num_bottles;

        while num_bottles >= num_exchange {
            num_bottles -= num_exchange;
            num_exchange += 1;
            ans += 1;
            num_bottles += 1;
        }

        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int MaxBottlesDrunk(int numBottles, int numExchange) {
        int ans = numBottles;
        while (numBottles >= numExchange) {
            numBottles -= numExchange;
            ++numExchange;
            ++ans;
            ++numBottles;
        }
        return ans;
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param Integer $numBottles
     * @param Integer $numExchange
     * @return Integer
     */
    function maxBottlesDrunk($numBottles, $numExchange) {
        $ans = $numBottles;
        while ($numBottles >= $numExchange) {
            $numBottles -= $numExchange;
            $numExchange++;
            $ans++;
            $numBottles++;
        }
        return $ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
