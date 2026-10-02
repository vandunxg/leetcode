---
comments: true
difficulty: Medium
rating: 1725
source: Weekly Contest 128 Q3
tags:
    - Array
    - Binary Search
---

<!-- problem:start -->

# [1011. Capacity To Ship Packages Within D Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days)

[中文文档](/solution/1000-1099/1011.Capacity%20To%20Ship%20Packages%20Within%20D%20Days/README.md)

## Mô tả

<!-- description:start -->

<p>Có các kiện hàng trên băng chuyền cần được vận chuyển từ cảng này đến cảng khác trong vòng <code>days</code> ngày.</p>

<p>Kiện hàng thứ <code>i<sup>th</sup></code> trên băng chuyền có khối lượng <code>weights[i]</code>. Mỗi ngày, ta xếp các kiện lên tàu theo đúng thứ tự trong <code>weights</code>. Tổng khối lượng hàng không được vượt quá tải trọng tối đa của tàu.</p>

<p>Trả về tải trọng nhỏ nhất của tàu để vận chuyển hết các kiện hàng trên băng chuyền trong vòng <code>days</code> ngày.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> weights = [1,2,3,4,5,6,7,8,9,10], days = 5
<strong>Đầu ra:</strong> 15
<strong>Giải thích:</strong> Tải trọng 15 là nhỏ nhất để vận chuyển hết hàng trong 5 ngày như sau:
Ngày 1: 1, 2, 3, 4, 5
Ngày 2: 6, 7
Ngày 3: 8
Ngày 4: 9
Ngày 5: 10

Lưu ý, hàng phải được vận chuyển theo đúng thứ tự đã cho. Vì vậy, không được dùng tàu tải trọng 14 rồi chia kiện hàng thành các nhóm như (2, 3, 4, 5), (1, 6, 7), (8), (9), (10).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> weights = [3,2,2,4,1,4], days = 3
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Tải trọng 6 là nhỏ nhất để vận chuyển hết hàng trong 3 ngày như sau:
Ngày 1: 3, 2
Ngày 2: 2, 4
Ngày 3: 1, 4
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> weights = [1,2,3,1,1], days = 4
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Ngày 1: 1
Ngày 2: 2
Ngày 3: 3
Ngày 4: 1, 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= days &lt;= weights.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= weights[i] &lt;= 500</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tải trọng ít nhất phải bằng kiện nặng nhất và nhiều nhất bằng tổng khối lượng hàng. Thử mọi giá trị trong khoảng này sẽ quá chậm vì $n\le 5\times 10^4$ và tổng khối lượng có thể lên đến hàng triệu.
>
> Tải trọng càng lớn thì số ngày cần dùng không tăng, nên tính khả thi có tính đơn điệu; có thể tìm tải trọng nhỏ nhất thỏa mãn bằng tìm kiếm nhị phân.
>
> Với tải trọng thử $x$, ta xếp hàng từ trái sang phải và bắt đầu ngày mới khi tổng khối lượng vượt quá $x$. Dùng tìm kiếm nhị phân để tìm giá trị nhỏ nhất khiến phép kiểm tra đúng trong khoảng $[\max w_i,\sum w_i]$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shipWithinDays(self, weights: List[int], days: int) -> int:
        def check(mx):
            ws, cnt = 0, 1
            for w in weights:
                ws += w
                if ws > mx:
                    cnt += 1
                    ws = w
            return cnt <= days

        left, right = max(weights), sum(weights) + 1
        return left + bisect_left(range(left, right), True, key=check)
```

#### Java

```java
class Solution {
    public int shipWithinDays(int[] weights, int days) {
        int left = 0, right = 0;
        for (int w : weights) {
            left = Math.max(left, w);
            right += w;
        }
        while (left < right) {
            int mid = (left + right) >> 1;
            if (check(mid, weights, days)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }

    private boolean check(int mx, int[] weights, int days) {
        int ws = 0, cnt = 1;
        for (int w : weights) {
            ws += w;
            if (ws > mx) {
                ws = w;
                ++cnt;
            }
        }
        return cnt <= days;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int shipWithinDays(vector<int>& weights, int days) {
        int left = 0, right = 0;
        for (auto& w : weights) {
            left = max(left, w);
            right += w;
        }
        auto check = [&](int mx) {
            int ws = 0, cnt = 1;
            for (auto& w : weights) {
                ws += w;
                if (ws > mx) {
                    ws = w;
                    ++cnt;
                }
            }
            return cnt <= days;
        };
        while (left < right) {
            int mid = (left + right) >> 1;
            if (check(mid)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
};
```

#### Go

```go
func shipWithinDays(weights []int, days int) int {
	var left, right int
	for _, w := range weights {
		if left < w {
			left = w
		}
		right += w
	}
	return left + sort.Search(right, func(mx int) bool {
		mx += left
		ws, cnt := 0, 1
		for _, w := range weights {
			ws += w
			if ws > mx {
				ws = w
				cnt++
			}
		}
		return cnt <= days
	})
}
```

#### TypeScript

```ts
function shipWithinDays(weights: number[], days: number): number {
    let left = 0;
    let right = 0;
    for (const w of weights) {
        left = Math.max(left, w);
        right += w;
    }
    const check = (mx: number) => {
        let ws = 0;
        let cnt = 1;
        for (const w of weights) {
            ws += w;
            if (ws > mx) {
                ws = w;
                ++cnt;
            }
        }
        return cnt <= days;
    };
    while (left < right) {
        const mid = (left + right) >> 1;
        if (check(mid)) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    return left;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
