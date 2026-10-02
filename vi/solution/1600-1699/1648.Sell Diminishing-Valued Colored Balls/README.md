---
comments: true
difficulty: Medium
rating: 2050
source: Weekly Contest 214 Q3
tags:
    - Greedy
    - Array
    - Math
    - Binary Search
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1648. Sell Diminishing-Valued Colored Balls](https://leetcode.com/problems/sell-diminishing-valued-colored-balls)

[中文文档](/solution/1600-1699/1648.Sell%20Diminishing-Valued%20Colored%20Balls/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có <code>inventory</code> các quả bóng nhiều màu khác nhau, và một khách hàng muốn mua <code>orders</code> quả bóng có <strong>bất kỳ</strong> màu nào.</p>

<p>Khách hàng định giá các quả bóng theo cách khá đặc biệt. Giá trị của mỗi quả bóng là số quả bóng <strong>cùng màu&nbsp;</strong>hiện có trong <code>inventory</code>. Ví dụ, nếu bạn có <code>6</code> quả bóng vàng, khách hàng trả <code>6</code> cho quả đầu tiên. Sau giao dịch, chỉ còn <code>5</code> quả bóng vàng, nên quả tiếp theo có giá <code>5</code> (tức là giá trị giảm dần khi bán thêm bóng cho khách hàng).</p>

<p>Cho mảng số nguyên <code>inventory</code>, trong đó <code>inventory[i]</code> là số quả bóng màu thứ <code>i<sup>th</sup></code> ban đầu bạn có. Bạn cũng được cho số nguyên <code>orders</code>, là tổng số quả bóng khách hàng muốn mua. Bạn có thể bán bóng theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Trả về <em>tổng giá trị <strong>lớn nhất</strong> có thể đạt được sau khi bán </em><code>orders</code><em> quả bóng màu</em>. Vì đáp án có thể rất lớn, hãy trả về kết quả <strong>modulo </strong><code>10<sup>9 </sup>+ 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1648.Sell%20Diminishing-Valued%20Colored%20Balls/images/jj.gif" style="width: 480px; height: 270px;" />
<pre>
<strong>Input:</strong> inventory = [2,5], orders = 4
<strong>Output:</strong> 14
<strong>Explanation:</strong> Bán màu thứ nhất 1 lần (2) và màu thứ hai 3 lần (5 + 4 + 3).
Tổng giá trị lớn nhất là 2 + 5 + 4 + 3 = 14.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> inventory = [3,5], orders = 6
<strong>Output:</strong> 19
<strong>Explanation: </strong>Bán màu thứ nhất 2 lần (3 + 2) và màu thứ hai 4 lần (5 + 4 + 3 + 2).
Tổng giá trị lớn nhất là 3 + 2 + 5 + 4 + 3 + 2 = 19.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= inventory.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= inventory[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= orders &lt;= min(sum(inventory[i]), 10<sup>9</sup>)</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta luôn nên bán màu hiện có nhiều bóng nhất. $\textit{orders}$ có thể bằng $10^9$, nên không thể bán từng quả; hãy bán cả một tầng các mức cao nhất bằng nhau cùng lúc.
>
> Sau khi sắp xếp số lượng giảm dần, khoảng cách đến mức khác nhau kế tiếp tạo thành một lô có kích thước $(\textit{count of that height})\times(\textit{gap})$. Nếu lô này lớn hơn số orders còn lại, tính tổng cấp số cộng cho các vòng đầy đủ và phần dư.
>
> Hạ mức cao nhất xuống mức kế tiếp và lặp lại cho đến khi hết orders, lấy modulo $10^9+7$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxProfit(self, inventory: List[int], orders: int) -> int:
        inventory.sort(reverse=True)
        mod = 10**9 + 7
        ans = i = 0
        n = len(inventory)
        while orders > 0:
            while i < n and inventory[i] >= inventory[0]:
                i += 1
            nxt = 0
            if i < n:
                nxt = inventory[i]
            cnt = i
            x = inventory[0] - nxt
            tot = cnt * x
            if tot > orders:
                decr = orders // cnt
                a1, an = inventory[0] - decr + 1, inventory[0]
                ans += (a1 + an) * decr // 2 * cnt
                ans += (inventory[0] - decr) * (orders % cnt)
            else:
                a1, an = nxt + 1, inventory[0]
                ans += (a1 + an) * x // 2 * cnt
                inventory[0] = nxt
            orders -= tot
            ans %= mod
        return ans
```

#### Java

```java
class Solution {
    private static final int MOD = (int) 1e9 + 7;

    public int maxProfit(int[] inventory, int orders) {
        Arrays.sort(inventory);
        int n = inventory.length;
        for (int i = 0, j = n - 1; i < j; ++i, --j) {
            int t = inventory[i];
            inventory[i] = inventory[j];
            inventory[j] = t;
        }
        long ans = 0;
        int i = 0;
        while (orders > 0) {
            while (i < n && inventory[i] >= inventory[0]) {
                ++i;
            }
            int nxt = i < n ? inventory[i] : 0;
            int cnt = i;
            long x = inventory[0] - nxt;
            long tot = cnt * x;
            if (tot > orders) {
                int decr = orders / cnt;
                long a1 = inventory[0] - decr + 1, an = inventory[0];
                ans += (a1 + an) * decr / 2 * cnt;
                ans += (a1 - 1) * (orders % cnt);
            } else {
                long a1 = nxt + 1, an = inventory[0];
                ans += (a1 + an) * x / 2 * cnt;
                inventory[0] = nxt;
            }
            orders -= tot;
            ans %= MOD;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxProfit(vector<int>& inventory, int orders) {
        long ans = 0, mod = 1e9 + 7;
        int i = 0, n = inventory.size();
        sort(inventory.rbegin(), inventory.rend());
        while (orders > 0) {
            while (i < n && inventory[i] >= inventory[0]) {
                ++i;
            }
            int nxt = i < n ? inventory[i] : 0;
            int cnt = i;
            long x = inventory[0] - nxt;
            long tot = cnt * x;
            if (tot > orders) {
                int decr = orders / cnt;
                long a1 = inventory[0] - decr + 1, an = inventory[0];
                ans += (a1 + an) * decr / 2 * cnt;
                ans += (a1 - 1) * (orders % cnt);
            } else {
                long a1 = nxt + 1, an = inventory[0];
                ans += (a1 + an) * x / 2 * cnt;
                inventory[0] = nxt;
            }
            orders -= tot;
            ans %= mod;
        }
        return ans;
    }
};
```

#### Go

```go
func maxProfit(inventory []int, orders int) int {
	var mod int = 1e9 + 7
	i, n, ans := 0, len(inventory), 0
	sort.Ints(inventory)
	for i, j := 0, n-1; i < j; i, j = i+1, j-1 {
		inventory[i], inventory[j] = inventory[j], inventory[i]
	}
	for orders > 0 {
		for i < n && inventory[i] >= inventory[0] {
			i++
		}
		nxt := 0
		if i < n {
			nxt = inventory[i]
		}
		cnt := i
		x := inventory[0] - nxt
		tot := cnt * x
		if tot > orders {
			decr := orders / cnt
			a1, an := inventory[0]-decr+1, inventory[0]
			ans += (a1 + an) * decr / 2 * cnt
			ans += (a1 - 1) * (orders % cnt)
		} else {
			a1, an := nxt+1, inventory[0]
			ans += (a1 + an) * x / 2 * cnt
			inventory[0] = nxt
		}
		orders -= tot
		ans %= mod
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
