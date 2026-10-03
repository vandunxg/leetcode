---
comments: true
difficulty: Easy
rating: 1325
source: Weekly Contest 267 Q1
tags:
    - Queue
    - Array
    - Simulation
---

<!-- problem:start -->

# [2073. Time Needed to Buy Tickets](https://leetcode.com/problems/time-needed-to-buy-tickets)

[中文文档](/solution/2000-2099/2073.Time%20Needed%20to%20Buy%20Tickets/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> người xếp hàng để mua vé, trong đó người thứ <code>0<sup>th</sup></code> đứng ở <strong>đầu</strong> hàng và người thứ <code>(n - 1)<sup>th</sup></code> đứng ở <strong>cuối</strong> hàng.</p>

<p>Bạn được cho một mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>tickets</code> có độ dài <code>n</code>, trong đó số vé mà người thứ <code>i<sup>th</sup></code> muốn mua là <code>tickets[i]</code>.</p>

<p>Mỗi người mất <strong>chính xác 1 giây</strong> để mua một vé. Mỗi lần một người chỉ có thể mua <strong>1 vé</strong> và phải quay lại <strong>cuối</strong> hàng (việc này diễn ra <strong>ngay lập tức</strong>) để mua thêm vé. Nếu một người không còn vé nào cần mua, người đó sẽ <strong>rời khỏi</strong> hàng.</p>

<p>Hãy trả về <strong>thời gian</strong> cần để người <strong>ban đầu</strong> đứng ở vị trí <strong>k</strong><strong> </strong>(đánh chỉ số từ 0) mua xong số vé của mình.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">tickets = [2,3,2], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Hàng bắt đầu là [2,3,<u>2</u>], trong đó người thứ k được gạch chân.</li>
	<li>Sau khi người ở đầu hàng mua một vé, hàng trở thành [3,<u>2</u>,1] ở thời điểm 1 giây.</li>
	<li>Tiếp tục quá trình này, hàng trở thành [<u>2</u>,1,2] ở thời điểm 2 giây.</li>
	<li>Tiếp tục quá trình này, hàng trở thành [1,2,<u>1</u>] ở thời điểm 3 giây.</li>
	<li>Tiếp tục quá trình này, hàng trở thành [2,<u>1</u>] ở thời điểm 4 giây. Lưu ý: người ở đầu hàng đã rời khỏi hàng.</li>
	<li>Tiếp tục quá trình này, hàng trở thành [<u>1</u>,1] ở thời điểm 5 giây.</li>
	<li>Tiếp tục quá trình này, hàng trở thành [1] ở thời điểm 6 giây. Người thứ k đã mua đủ số vé, nên trả về 6.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">tickets = [5,1,1,1], k = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Hàng bắt đầu là [<u>5</u>,1,1,1], trong đó người thứ k được gạch chân.</li>
	<li>Sau khi người ở đầu hàng mua một vé, hàng trở thành [1,1,1,<u>4</u>] ở thời điểm 1 giây.</li>
	<li>Tiếp tục quá trình này trong 3 giây, hàng trở thành [<u>4]</u> ở thời điểm 4 giây.</li>
	<li>Tiếp tục quá trình này trong 4 giây, hàng trở thành [] ở thời điểm 8 giây. Người thứ k đã mua đủ số vé, nên trả về 8.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == tickets.length</code></li>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= tickets[i] &lt;= 100</code></li>
	<li><code>0 &lt;= k &lt; n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lượt

<!-- thinking:start -->

> **Tư duy**
>
> Mô phỏng trực tiếp hàng đợi có độ phức tạp $O(\sum tickets)$. Khi người thứ $k$ mua xong, mỗi người đứng trước người này đã mua nhiều nhất là $tickets[k]$ vé, còn mỗi người đứng sau đã mua nhiều nhất là $tickets[k]-1$ vé.
>
> Trong một lượt duyệt, cộng $\min$ giữa giới hạn này và nhu cầu của từng người.

<!-- thinking:end -->

Theo mô tả bài toán, khi người thứ $k^{th}$ mua xong số vé của mình, tất cả những người đứng trước người thứ $k^{th}$ sẽ không mua nhiều vé hơn người thứ $k^{th}$, còn tất cả những người đứng sau người thứ $k^{th}$ sẽ không mua nhiều hơn người thứ $k^{th}$ $1$ vé.

Vì vậy, ta có thể duyệt toàn bộ hàng. Với người thứ $i^{th}$, nếu $i \leq k$, thời gian mua vé là $\min(\textit{tickets}[i], \textit{tickets}[k])$; ngược lại, thời gian mua vé là $\min(\textit{tickets}[i], \textit{tickets}[k] - 1)$. Cộng thời gian mua vé của tất cả mọi người để thu được kết quả.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài hàng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def timeRequiredToBuy(self, tickets: List[int], k: int) -> int:
        ans = 0
        for i, x in enumerate(tickets):
            ans += min(x, tickets[k] if i <= k else tickets[k] - 1)
        return ans
```

#### Java

```java
class Solution {
    public int timeRequiredToBuy(int[] tickets, int k) {
        int ans = 0;
        for (int i = 0; i < tickets.length; ++i) {
            ans += Math.min(tickets[i], i <= k ? tickets[k] : tickets[k] - 1);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int timeRequiredToBuy(vector<int>& tickets, int k) {
        int ans = 0;
        for (int i = 0; i < tickets.size(); ++i) {
            ans += min(tickets[i], i <= k ? tickets[k] : tickets[k] - 1);
        }
        return ans;
    }
};
```

#### Go

```go
func timeRequiredToBuy(tickets []int, k int) (ans int) {
	for i, x := range tickets {
		t := tickets[k]
		if i > k {
			t--
		}
		ans += min(x, t)
	}
	return
}
```

#### TypeScript

```ts
function timeRequiredToBuy(tickets: number[], k: number): number {
    let ans = 0;
    const n = tickets.length;
    for (let i = 0; i < n; ++i) {
        ans += Math.min(tickets[i], i <= k ? tickets[k] : tickets[k] - 1);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
