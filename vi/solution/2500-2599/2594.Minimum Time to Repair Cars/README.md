---
comments: true
difficulty: Medium
rating: 1915
source: Biweekly Contest 100 Q4
tags:
    - Array
    - Binary Search
---

<!-- problem:start -->

# [2594. Minimum Time to Repair Cars](https://leetcode.com/problems/minimum-time-to-repair-cars)

[中文文档](/solution/2500-2599/2594.Minimum%20Time%20to%20Repair%20Cars/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>ranks</code> biểu thị <strong>cấp bậc</strong> của một số thợ sửa xe. <font face="monospace">ranks<sub>i</sub></font> là cấp bậc của thợ sửa xe thứ <font face="monospace">i<sup>th</sup></font><font face="monospace">.</font> Thợ sửa xe có cấp bậc <code>r</code> có thể sửa <font face="monospace">n</font> chiếc xe trong <code>r * n<sup>2</sup></code> phút.</p>

<p>Bạn cũng được cho một số nguyên <code>cars</code> biểu thị tổng số xe đang chờ được sửa trong gara.</p>

<p>Hãy trả về <em><strong>thời gian nhỏ nhất</strong> cần để sửa tất cả số xe.</em></p>

<p><strong>Lưu ý:</strong> Tất cả thợ sửa xe có thể sửa xe đồng thời.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> ranks = [4,2,3,1], cars = 10
<strong>Đầu ra:</strong> 16
<strong>Giải thích:</strong>
- Thợ sửa xe đầu tiên sửa hai chiếc xe. Thời gian cần thiết là 4 * 2 * 2 = 16 phút.
- Thợ sửa xe thứ hai sửa hai chiếc xe. Thời gian cần thiết là 2 * 2 * 2 = 8 phút.
- Thợ sửa xe thứ ba sửa hai chiếc xe. Thời gian cần thiết là 3 * 2 * 2 = 12 phút.
- Thợ sửa xe thứ tư sửa bốn chiếc xe. Thời gian cần thiết là 1 * 4 * 4 = 16 phút.
Có thể chứng minh rằng không thể sửa xe trong thời gian dưới 16 phút.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> ranks = [5,1,8], cars = 6
<strong>Đầu ra:</strong> 16
<strong>Giải thích:</strong>
- Thợ sửa xe đầu tiên sửa một chiếc xe. Thời gian cần thiết là 5 * 1 * 1 = 5 phút.
- Thợ sửa xe thứ hai sửa bốn chiếc xe. Thời gian cần thiết là 1 * 4 * 4 = 16 phút.
- Thợ sửa xe thứ ba sửa một chiếc xe. Thời gian cần thiết là 8 * 1 * 1 = 8 phút.
Có thể chứng minh rằng không thể sửa xe trong thời gian dưới 16 phút.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= ranks.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= ranks[i] &lt;= 100</code></li>
	<li><code>1 &lt;= cars &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Binary Search

<!-- thinking:start -->

> **Tư duy**
>
> Thợ sửa xe $i$ sửa $x$ chiếc xe trong $r_i x^2$ phút; các thợ làm việc song song. Số cách phân công rất nhiều và khoảng thời gian cần xét là $r\cdot cars^2$.
>
> Thời gian càng dài thì càng sửa được nhiều xe, nên ta có thể tìm kiếm nhị phân $t$. Trong thời gian $t$, thợ có cấp bậc $r$ sửa được $\lfloor\sqrt{t/r}\rfloor$ chiếc xe; điều kiện khả thi là tổng số xe này so với $\textit{cars}$. $\textit{bisect\_left}$ trả về $t$ nhỏ nhất.

<!-- thinking:end -->

Ta nhận thấy thời gian sửa càng dài thì số xe được sửa càng nhiều. Vì vậy, ta có thể dùng thời gian sửa làm mục tiêu tìm kiếm nhị phân và tìm thời gian sửa nhỏ nhất.

Ta đặt cận trái và cận phải của phép tìm kiếm nhị phân lần lượt là $left=0$, $right=ranks[0] \times cars \times cars$. Tiếp theo, ta tìm kiếm nhị phân thời gian sửa $mid$; số xe mỗi thợ có thể sửa là $\lfloor \sqrt{\frac{mid}{r}} \rfloor$, trong đó $\lfloor x \rfloor$ biểu thị phép làm tròn xuống. Nếu số xe đã sửa lớn hơn hoặc bằng $cars$, điều đó có nghĩa thời gian sửa $mid$ là khả thi, nên ta giảm cận phải xuống $mid$; ngược lại, ta tăng cận trái lên $mid+1$.

Cuối cùng, ta trả về cận trái.

Độ phức tạp thời gian là $O(n \times \log M)$, và độ phức tạp không gian là $O(1)$. Ở đây, $n$ là số thợ sửa xe và $M$ là cận trên của phép tìm kiếm nhị phân.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def repairCars(self, ranks: List[int], cars: int) -> int:
        def check(t: int) -> bool:
            return sum(int(sqrt(t // r)) for r in ranks) >= cars

        return bisect_left(range(ranks[0] * cars * cars), True, key=check)
```

#### Java

```java
class Solution {
    public long repairCars(int[] ranks, int cars) {
        long left = 0, right = 1L * ranks[0] * cars * cars;
        while (left < right) {
            long mid = (left + right) >> 1;
            long cnt = 0;
            for (int r : ranks) {
                cnt += Math.sqrt(mid / r);
            }
            if (cnt >= cars) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long repairCars(vector<int>& ranks, int cars) {
        long long left = 0, right = 1LL * ranks[0] * cars * cars;
        while (left < right) {
            long long mid = (left + right) >> 1;
            long long cnt = 0;
            for (int r : ranks) {
                cnt += sqrt(mid / r);
            }
            if (cnt >= cars) {
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
func repairCars(ranks []int, cars int) int64 {
	return int64(sort.Search(ranks[0]*cars*cars, func(t int) bool {
		cnt := 0
		for _, r := range ranks {
			cnt += int(math.Sqrt(float64(t / r)))
		}
		return cnt >= cars
	}))
}
```

#### TypeScript

```ts
function repairCars(ranks: number[], cars: number): number {
    let left = 0;
    let right = ranks[0] * cars * cars;
    while (left < right) {
        const mid = left + Math.floor((right - left) / 2);
        let cnt = 0;
        for (const r of ranks) {
            cnt += Math.floor(Math.sqrt(mid / r));
        }
        if (cnt >= cars) {
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
