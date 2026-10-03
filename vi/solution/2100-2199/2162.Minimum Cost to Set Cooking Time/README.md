---
comments: true
difficulty: Medium
rating: 1851
source: Biweekly Contest 71 Q3
tags:
    - Math
    - Enumeration
---

<!-- problem:start -->

# [2162. Minimum Cost to Set Cooking Time](https://leetcode.com/problems/minimum-cost-to-set-cooking-time)

[中文文档](/solution/2100-2199/2162.Minimum%20Cost%20to%20Set%20Cooking%20Time/README.md)

## Mô tả

<!-- description:start -->

<p>Một lò vi sóng thông thường hỗ trợ thời gian nấu:</p>

<ul>
	<li>ít nhất <code>1</code> giây.</li>
	<li>nhiều nhất <code>99</code> phút và <code>99</code> giây.</li>
</ul>

<p>Để cài đặt thời gian nấu, bạn nhấn <strong>tối đa bốn chữ số</strong>. Lò vi sóng chuẩn hóa các chữ số bạn nhấn thành bốn chữ số bằng cách <strong>thêm các số 0 vào đầu</strong>. Lò hiểu hai chữ số <strong>đầu tiên</strong> là số phút và hai chữ số <strong>cuối cùng</strong> là số giây. Sau đó, lò <strong>cộng</strong> chúng lại để được thời gian nấu. Ví dụ:</p>

<ul>
	<li>Bạn nhấn <code>9</code> <code>5</code> <code>4</code> (ba chữ số). Lò chuẩn hóa thành <code>0954</code> và hiểu là <code>9</code> phút và <code>54</code> giây.</li>
	<li>Bạn nhấn <code>0</code> <code>0</code> <code>0</code> <code>8</code> (bốn chữ số). Lò hiểu là <code>0</code> phút và <code>8</code> giây.</li>
	<li>Bạn nhấn <code>8</code> <code>0</code> <code>9</code> <code>0</code>. Lò hiểu là <code>80</code> phút và <code>90</code> giây.</li>
	<li>Bạn nhấn <code>8</code> <code>1</code> <code>3</code> <code>0</code>. Lò hiểu là <code>81</code> phút và <code>30</code> giây.</li>
</ul>

<p>Bạn được cho các số nguyên <code>startAt</code>, <code>moveCost</code>, <code>pushCost</code> và <code>targetSeconds</code>. <strong>Ban đầu</strong>, ngón tay của bạn đặt trên chữ số <code>startAt</code>. Di chuyển ngón tay đến <strong>bất kỳ chữ số cụ thể nào</strong> sẽ tốn <code>moveCost</code> đơn vị mệt mỏi. Nhấn chữ số bên dưới ngón tay <strong>một lần</strong> sẽ tốn <code>pushCost</code> đơn vị mệt mỏi.</p>

<p>Có thể có nhiều cách cài đặt lò vi sóng để nấu trong <code>targetSeconds</code> giây, nhưng bạn muốn tìm cách có chi phí nhỏ nhất.</p>

<p>Trả về <em>chi phí <strong>nhỏ nhất</strong> để cài đặt</em> <code>targetSeconds</code> <em>giây thời gian nấu</em>.</p>

<p>Nhớ rằng một phút gồm <code>60</code> giây.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2162.Minimum%20Cost%20to%20Set%20Cooking%20Time/images/1.png" style="width: 506px; height: 210px;" />
<pre>
<strong>Đầu vào:</strong> startAt = 1, moveCost = 2, pushCost = 1, targetSeconds = 600
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Có những cách sau để cài đặt thời gian nấu.
- 1 0 0 0, được hiểu là 10 phút và 0 giây.
&nbsp; Ngón tay đã ở trên chữ số 1, nhấn 1 (chi phí 1), di chuyển đến 0 (chi phí 2), nhấn 0 (chi phí 1), nhấn 0 (chi phí 1) và nhấn 0 (chi phí 1).
&nbsp; Chi phí là: 1 + 2 + 1 + 1 + 1 = 6. Đây là chi phí nhỏ nhất.
- 0 9 6 0, được hiểu là 9 phút và 60 giây. Cách này cũng tương đương 600 giây.
&nbsp; Ngón tay di chuyển đến 0 (chi phí 2), nhấn 0 (chi phí 1), di chuyển đến 9 (chi phí 2), nhấn 9 (chi phí 1), di chuyển đến 6 (chi phí 2), nhấn 6 (chi phí 1), di chuyển đến 0 (chi phí 2) và nhấn 0 (chi phí 1).
&nbsp; Chi phí là: 2 + 1 + 2 + 1 + 2 + 1 + 2 + 1 = 12.
- 9 6 0, được chuẩn hóa thành 0960 và hiểu là 9 phút và 60 giây.
&nbsp; Ngón tay di chuyển đến 9 (chi phí 2), nhấn 9 (chi phí 1), di chuyển đến 6 (chi phí 2), nhấn 6 (chi phí 1), di chuyển đến 0 (chi phí 2) và nhấn 0 (chi phí 1).
&nbsp; Chi phí là: 2 + 1 + 2 + 1 + 2 + 1 = 9.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2162.Minimum%20Cost%20to%20Set%20Cooking%20Time/images/2.png" style="width: 505px; height: 73px;" />
<pre>
<strong>Đầu vào:</strong> startAt = 0, moveCost = 1, pushCost = 2, targetSeconds = 76
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Cách tối ưu là nhấn hai chữ số: 7 6, được hiểu là 76 giây.
Ngón tay di chuyển đến 7 (chi phí 1), nhấn 7 (chi phí 2), di chuyển đến 6 (chi phí 1) và nhấn 6 (chi phí 2). Tổng chi phí là: 1 + 2 + 1 + 2 = 6
Lưu ý các cách khác là 0076, 076, 0116 và 116, nhưng không cách nào trong số đó cho chi phí nhỏ nhất.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= startAt &lt;= 9</code></li>
	<li><code>1 &lt;= moveCost, pushCost &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= targetSeconds &lt;= 6039</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Lò hiển thị bốn chữ số cho phút và giây. Cùng một khoảng thời gian có thể được biểu diễn là $m$ phút $s$ giây hoặc $m-1$ phút $s+60$ giây, miễn là cả hai giá trị đều nằm trong phạm vi hai chữ số. Chi phí gồm số lần di chuyển ngón tay và số lần nhấn phím, nên ta đánh giá các cách biểu diễn hợp lệ.
>
> Với một cặp $(\textit{m},\textit{s})$, loại bỏ các số 0 ở đầu rồi duyệt các chữ số từ vị trí $\textit{startAt}$, cộng $\textit{moveCost}$ khi chữ số thay đổi và cộng $\textit{pushCost}$ cho mỗi lần nhấn.
>
> Trả về giá trị nhỏ hơn giữa $\texttt{f}(m,s)$ và $\texttt{f}(m-1,s+60)$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCostSetTime(
        self, startAt: int, moveCost: int, pushCost: int, targetSeconds: int
    ) -> int:
        def f(m, s):
            if not 0 <= m < 100 or not 0 <= s < 100:
                return inf
            arr = [m // 10, m % 10, s // 10, s % 10]
            i = 0
            while i < 4 and arr[i] == 0:
                i += 1
            t = 0
            prev = startAt
            for v in arr[i:]:
                if v != prev:
                    t += moveCost
                t += pushCost
                prev = v
            return t

        m, s = divmod(targetSeconds, 60)
        ans = min(f(m, s), f(m - 1, s + 60))
        return ans
```

#### Java

```java
class Solution {
    public int minCostSetTime(int startAt, int moveCost, int pushCost, int targetSeconds) {
        int m = targetSeconds / 60;
        int s = targetSeconds % 60;
        return Math.min(
            f(m, s, startAt, moveCost, pushCost), f(m - 1, s + 60, startAt, moveCost, pushCost));
    }

    private int f(int m, int s, int prev, int moveCost, int pushCost) {
        if (m < 0 || m > 99 || s < 0 || s > 99) {
            return Integer.MAX_VALUE;
        }
        int[] arr = new int[] {m / 10, m % 10, s / 10, s % 10};
        int i = 0;
        for (; i < 4 && arr[i] == 0; ++i)
            ;
        int t = 0;
        for (; i < 4; ++i) {
            if (arr[i] != prev) {
                t += moveCost;
            }
            t += pushCost;
            prev = arr[i];
        }
        return t;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minCostSetTime(int startAt, int moveCost, int pushCost, int targetSeconds) {
        int m = targetSeconds / 60, s = targetSeconds % 60;
        return min(f(m, s, startAt, moveCost, pushCost), f(m - 1, s + 60, startAt, moveCost, pushCost));
    }

    int f(int m, int s, int prev, int moveCost, int pushCost) {
        if (m < 0 || m > 99 || s < 0 || s > 99) return INT_MAX;
        vector<int> arr = {m / 10, m % 10, s / 10, s % 10};
        int i = 0;
        for (; i < 4 && arr[i] == 0; ++i)
            ;
        int t = 0;
        for (; i < 4; ++i) {
            if (arr[i] != prev) t += moveCost;
            t += pushCost;
            prev = arr[i];
        }
        return t;
    }
};
```

#### Go

```go
func minCostSetTime(startAt int, moveCost int, pushCost int, targetSeconds int) int {
	m, s := targetSeconds/60, targetSeconds%60
	f := func(m, s int) int {
		if m < 0 || m > 99 || s < 0 || s > 99 {
			return 0x3f3f3f3f
		}
		arr := []int{m / 10, m % 10, s / 10, s % 10}
		i := 0
		for ; i < 4 && arr[i] == 0; i++ {
		}
		t := 0
		prev := startAt
		for ; i < 4; i++ {
			if arr[i] != prev {
				t += moveCost
			}
			t += pushCost
			prev = arr[i]
		}
		return t
	}
	return min(f(m, s), f(m-1, s+60))
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
