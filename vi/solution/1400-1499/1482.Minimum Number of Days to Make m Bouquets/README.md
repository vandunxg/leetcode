---
comments: true
difficulty: Medium
rating: 1945
source: Weekly Contest 193 Q3
tags:
    - Array
    - Binary Search
---

<!-- problem:start -->

# [1482. Minimum Number of Days to Make m Bouquets](https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets)

[中文文档](/solution/1400-1499/1482.Minimum%20Number%20of%20Days%20to%20Make%20m%20Bouquets/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>bloomDay</code>, một số nguyên <code>m</code> và một số nguyên <code>k</code>.</p>

<p>Bạn muốn làm <code>m</code> bó hoa. Để làm một bó hoa, bạn cần sử dụng <code>k</code> <strong>bông hoa liền kề</strong> trong vườn.</p>

<p>Vườn có <code>n</code> bông hoa, bông hoa thứ <code>i</code> sẽ nở vào ngày <code>bloomDay[i]</code> và sau đó chỉ có thể được dùng trong <strong>đúng một</strong> bó hoa.</p>

<p>Trả về <em>số ngày ít nhất cần chờ để có thể làm </em><code>m</code><em> bó hoa từ khu vườn</em>. Nếu không thể làm được m bó hoa, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> bloomDay = [1,10,3,10,2], m = 3, k = 1
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Hãy xem điều gì xảy ra trong ba ngày đầu tiên. x biểu thị bông hoa đã nở và _ biểu thị bông hoa chưa nở trong vườn.
Chúng ta cần 3 bó hoa, mỗi bó chứa 1 bông hoa.
Sau ngày 1: [x, _, _, _, _]   // chúng ta chỉ có thể làm một bó hoa.
Sau ngày 2: [x, _, _, _, x]   // chúng ta chỉ có thể làm hai bó hoa.
Sau ngày 3: [x, _, x, _, x]   // chúng ta có thể làm 3 bó hoa. Đáp án là 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> bloomDay = [1,10,3,10,2], m = 3, k = 2
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Chúng ta cần 3 bó hoa, mỗi bó có 2 bông, tức là cần 6 bông hoa. Chúng ta chỉ có 5 bông hoa nên không thể làm đủ số bó hoa cần thiết và trả về -1.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> bloomDay = [7,7,7,7,12,7,7], m = 2, k = 3
<strong>Đầu ra:</strong> 12
<strong>Giải thích:</strong> Chúng ta cần 2 bó hoa, mỗi bó có 3 bông.
Đây là khu vườn sau ngày 7 và ngày 12:
Sau ngày 7: [x, x, x, x, _, x, x]
Chúng ta có thể làm một bó từ ba bông hoa đầu tiên đã nở. Chúng ta không thể làm thêm một bó từ ba bông hoa cuối cùng đã nở vì chúng không liền kề.
Sau ngày 12: [x, x, x, x, x, x, x]
Rõ ràng là chúng ta có thể làm hai bó hoa theo những cách khác nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>bloomDay.length == n</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= bloomDay[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= m &lt;= 10<sup>6</sup></code></li>
	<li><code>1 &lt;= k &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Binary Search

<!-- thinking:start -->

> **Tư duy**
>
> Càng nhiều ngày thì càng dễ tạo được $m$ bó hoa. Vì $n\le 10^5$ và số ngày nở có thể lên tới $10^9$, hãy dùng binary search trên số ngày. Phần kiểm tra đếm các đoạn hoa đã nở liền kề có độ dài $k$. Nếu không có ngày nào thỏa mãn, trả về $-1$.

<!-- thinking:end -->

Theo mô tả bài toán, nếu ngày $t$ có thể đáp ứng việc làm $m$ bó hoa, thì với mọi $t' > t$, ta cũng có thể làm được $m$ bó hoa. Vì vậy, chúng ta có thể dùng binary search để tìm ngày nhỏ nhất đáp ứng điều kiện này.

Gọi $mx$ là ngày nở lớn nhất trong khu vườn. Tiếp theo, ta xác định biên trái của binary search là $l = 1$ và biên phải là $r = mx + 1$.

Sau đó, ta thực hiện binary search. Với mỗi giá trị ở giữa $\textit{mid} = \frac{l + r}{2}$, ta kiểm tra xem có thể làm $m$ bó hoa hay không. Nếu có thể, ta cập nhật biên phải $r$ thành $\textit{mid}$; ngược lại, ta cập nhật biên trái $l$ thành $\textit{mid} + 1$.

Cuối cùng, khi $l = r$, binary search kết thúc. Lúc này, nếu $l > mx$, điều đó có nghĩa là không thể làm $m$ bó hoa, nên ta trả về $-1$; nếu không, ta trả về $l$.

Do đó, bài toán được rút gọn thành việc kiểm tra xem một ngày $\textit{days}$ có thể làm được $m$ bó hoa hay không.

Ta có thể dùng hàm $\text{check}(\textit{days})$ để xác định có thể làm $m$ bó hoa hay không. Cụ thể, ta duyệt từng bông hoa trong vườn từ trái sang phải. Nếu ngày nở của bông hoa hiện tại nhỏ hơn hoặc bằng $\textit{days}$, ta thêm bông hoa hiện tại vào bó hoa đang làm; nếu không, ta xóa số bông hoa trong bó hiện tại. Khi số bông hoa trong bó hiện tại bằng $k$, ta tăng số bó hoa lên một và xóa số bông hoa trong bó hiện tại. Cuối cùng, ta kiểm tra xem số bó hoa có lớn hơn hoặc bằng $m$ hay không. Nếu có, nghĩa là có thể làm $m$ bó hoa; ngược lại thì không thể.

Độ phức tạp thời gian là $O(n \times \log M)$, trong đó $n$ và $M$ lần lượt là số bông hoa trong vườn và ngày nở lớn nhất. Trong bài toán này, $M \leq 10^9$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minDays(self, bloomDay: List[int], m: int, k: int) -> int:
        def check(days: int) -> int:
            cnt = cur = 0
            for x in bloomDay:
                cur = cur + 1 if x <= days else 0
                if cur == k:
                    cnt += 1
                    cur = 0
            return cnt >= m

        mx = max(bloomDay)
        l = bisect_left(range(mx + 2), True, key=check)
        return -1 if l > mx else l
```

#### Java

```java
class Solution {
    private int[] bloomDay;
    private int m, k;

    public int minDays(int[] bloomDay, int m, int k) {
        this.bloomDay = bloomDay;
        this.m = m;
        this.k = k;
        final int mx = (int) 1e9;
        int l = 1, r = mx + 1;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (check(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l > mx ? -1 : l;
    }

    private boolean check(int days) {
        int cnt = 0, cur = 0;
        for (int x : bloomDay) {
            cur = x <= days ? cur + 1 : 0;
            if (cur == k) {
                ++cnt;
                cur = 0;
            }
        }
        return cnt >= m;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minDays(vector<int>& bloomDay, int m, int k) {
        int mx = ranges::max(bloomDay);
        int l = 1, r = mx + 1;
        auto check = [&](int days) {
            int cnt = 0, cur = 0;
            for (int x : bloomDay) {
                cur = x <= days ? cur + 1 : 0;
                if (cur == k) {
                    cnt++;
                    cur = 0;
                }
            }
            return cnt >= m;
        };
        while (l < r) {
            int mid = (l + r) >> 1;
            if (check(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l > mx ? -1 : l;
    }
};
```

#### Go

```go
func minDays(bloomDay []int, m int, k int) int {
	mx := slices.Max(bloomDay)
	if l := sort.Search(mx+2, func(days int) bool {
		cnt, cur := 0, 0
		for _, x := range bloomDay {
			if x <= days {
				cur++
				if cur == k {
					cnt++
					cur = 0
				}
			} else {
				cur = 0
			}
		}
		return cnt >= m
	}); l <= mx {
		return l
	}
	return -1

}
```

#### TypeScript

```ts
function minDays(bloomDay: number[], m: number, k: number): number {
    const mx = Math.max(...bloomDay);
    let [l, r] = [1, mx + 1];
    const check = (days: number): boolean => {
        let [cnt, cur] = [0, 0];
        for (const x of bloomDay) {
            cur = x <= days ? cur + 1 : 0;
            if (cur === k) {
                cnt++;
                cur = 0;
            }
        }
        return cnt >= m;
    };
    while (l < r) {
        const mid = (l + r) >> 1;
        if (check(mid)) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return l > mx ? -1 : l;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
