---
comments: true
difficulty: Medium
rating: 1640
source: Weekly Contest 282 Q3
tags:
    - Array
    - Binary Search
---

<!-- problem:start -->

# [2187. Minimum Time to Complete Trips](https://leetcode.com/problems/minimum-time-to-complete-trips)

[中文文档](/solution/2100-2199/2187.Minimum%20Time%20to%20Complete%20Trips/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một mảng <code>time</code>, trong đó <code>time[i]</code> biểu thị thời gian mà xe buýt thứ <code>i<sup>th</sup></code> cần để hoàn thành <strong>một chuyến</strong>.</p>

<p>Mỗi xe buýt có thể thực hiện nhiều chuyến <strong>liên tiếp</strong>; nghĩa là chuyến tiếp theo có thể bắt đầu <strong>ngay sau khi</strong> hoàn thành chuyến hiện tại. Ngoài ra, mỗi xe buýt hoạt động <strong>độc lập</strong>; nghĩa là các chuyến của một xe buýt không ảnh hưởng đến bất kỳ xe buýt nào khác.</p>

<p>Bạn cũng được cung cấp một số nguyên <code>totalTrips</code>, biểu thị số chuyến mà tất cả xe buýt phải thực hiện <strong>tổng cộng</strong>. Hãy trả về <em><strong>thời gian nhỏ nhất</strong> cần thiết để tất cả xe buýt hoàn thành <strong>ít nhất</strong> </em><code>totalTrips</code><em> chuyến</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> time = [1,2,3], totalTrips = 5
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
- Tại thời điểm t = 1, số chuyến đã hoàn thành của mỗi xe buýt là [1,0,0].
  Tổng số chuyến đã hoàn thành là 1 + 0 + 0 = 1.
- Tại thời điểm t = 2, số chuyến đã hoàn thành của mỗi xe buýt là [2,1,0].
  Tổng số chuyến đã hoàn thành là 2 + 1 + 0 = 3.
- Tại thời điểm t = 3, số chuyến đã hoàn thành của mỗi xe buýt là [3,1,1].
  Tổng số chuyến đã hoàn thành là 3 + 1 + 1 = 5.
Vì vậy, thời gian nhỏ nhất cần thiết để tất cả xe buýt hoàn thành ít nhất 5 chuyến là 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> time = [2], totalTrips = 1
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Chỉ có một xe buýt và nó sẽ hoàn thành chuyến đầu tiên tại t = 2.
Vì vậy, thời gian nhỏ nhất cần thiết để hoàn thành 1 chuyến là 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= time.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= time[i], totalTrips &lt;= 10<sup>7</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Số chuyến hoàn thành đến thời điểm $t$ bằng $\sum \lfloor t/\textit{time}_i\rfloor$, đơn điệu theo $t$. Ta cần tìm $t$ nhỏ nhất đạt $\textit{totalTrips}$. Cận trên $\min(\textit{time})\times\textit{totalTrips}$ quá lớn để duyệt.
>
> Tìm kiếm nhị phân trên hàm đơn điệu đó trong $[0,\textit{mx})$; $\texttt{bisect\_left}$ trả về thời điểm khả thi đầu tiên.
>
> Mỗi lần kiểm tra cần tính tổng $n$ phép chia lấy phần nguyên.

<!-- thinking:end -->

Ta nhận thấy rằng nếu có thể hoàn thành ít nhất $totalTrips$ chuyến trong $t$ thời gian, thì cũng có thể hoàn thành ít nhất $totalTrips$ chuyến trong thời gian $t' > t$. Do đó, ta có thể sử dụng phương pháp tìm kiếm nhị phân để tìm $t$ nhỏ nhất.

Ta đặt biên trái của tìm kiếm nhị phân là $l = 1$, và biên phải là $r = \min(time) \times totalTrips$. Trong mỗi bước tìm kiếm nhị phân, ta tính giá trị ở giữa $\textit{mid} = \frac{l + r}{2}$, sau đó tính số chuyến có thể hoàn thành trong thời gian $\textit{mid}$. Nếu số này lớn hơn hoặc bằng $totalTrips$, ta giảm biên phải xuống $\textit{mid}$; ngược lại, ta tăng biên trái lên $\textit{mid} + 1$.

Cuối cùng, trả về biên trái.

Độ phức tạp thời gian là $O(n \times \log(m \times k))$, trong đó $n$ và $k$ lần lượt là độ dài của mảng $time$ và giá trị $totalTrips$, còn $m$ là giá trị nhỏ nhất trong mảng $time$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumTime(self, time: List[int], totalTrips: int) -> int:
        mx = min(time) * totalTrips
        return bisect_left(
            range(mx), totalTrips, key=lambda x: sum(x // v for v in time)
        )
```

#### Java

```java
class Solution {
    public long minimumTime(int[] time, int totalTrips) {
        int mi = time[0];
        for (int v : time) {
            mi = Math.min(mi, v);
        }
        long left = 1, right = (long) mi * totalTrips;
        while (left < right) {
            long cnt = 0;
            long mid = (left + right) >> 1;
            for (int v : time) {
                cnt += mid / v;
            }
            if (cnt >= totalTrips) {
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
    long long minimumTime(vector<int>& time, int totalTrips) {
        int mi = *min_element(time.begin(), time.end());
        long long left = 1, right = 1LL * mi * totalTrips;
        while (left < right) {
            long long cnt = 0;
            long long mid = (left + right) >> 1;
            for (int v : time) {
                cnt += mid / v;
            }
            if (cnt >= totalTrips) {
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
func minimumTime(time []int, totalTrips int) int64 {
	mx := slices.Min(time) * totalTrips
	return int64(sort.Search(mx, func(x int) bool {
		cnt := 0
		for _, v := range time {
			cnt += x / v
		}
		return cnt >= totalTrips
	}))
}
```

#### TypeScript

```ts
function minimumTime(time: number[], totalTrips: number): number {
    let left = 1n;
    let right = BigInt(Math.min(...time)) * BigInt(totalTrips);
    while (left < right) {
        const mid = (left + right) >> 1n;
        const cnt = time.reduce((acc, v) => acc + mid / BigInt(v), 0n);
        if (cnt >= BigInt(totalTrips)) {
            right = mid;
        } else {
            left = mid + 1n;
        }
    }
    return Number(left);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
