---
comments: true
difficulty: Hard
tags:
    - Array
    - Binary Search
---

<!-- problem:start -->

# [774. Minimize Max Distance to Gas Station 🔒](https://leetcode.com/problems/minimize-max-distance-to-gas-station)

[中文文档](/solution/0700-0799/0774.Minimize%20Max%20Distance%20to%20Gas%20Station/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>stations</code> biểu diễn vị trí các trạm xăng trên <strong>trục x</strong>. Bạn cũng được cho số nguyên <code>k</code>.</p>

<p>Bạn cần thêm <code>k</code> trạm xăng mới. Có thể đặt trạm ở bất kỳ vị trí nào trên <strong>trục x</strong>, không nhất thiết phải ở vị trí nguyên.</p>

<p>Gọi <code>penalty()</code> là khoảng cách lớn nhất giữa hai trạm xăng <strong>liền kề</strong> sau khi thêm <code>k</code> trạm mới.</p>

<p>Hãy trả về <em>giá trị nhỏ nhất có thể của </em><code>penalty()</code>. Các đáp án có sai số không quá <code>10<sup>-6</sup></code> so với đáp án thực tế sẽ được chấp nhận.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> stations = [1,2,3,4,5,6,7,8,9,10], k = 9
<strong>Đầu ra:</strong> 0.50000
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> stations = [23,24,36,39,46,56,57,65,84,98], k = 1
<strong>Đầu ra:</strong> 14.00000
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>10 &lt;= stations.length &lt;= 2000</code></li>
	<li><code>0 &lt;= stations[i] &lt;= 10<sup>8</sup></code></li>
	<li><code>stations</code> được sắp xếp theo thứ tự <strong>tăng nghiêm ngặt</strong>.</li>
	<li><code>1 &lt;= k &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Thêm tối đa $k$ trạm để giảm khoảng cách lớn nhất giữa các trạm. Vì $k$ có thể lên tới $10^6$, ta không thể thử mọi cách phân bổ.
>
> Tính khả thi đơn điệu theo giới hạn $x$: một khoảng cách $d$ cần $lfloor d/x\rfloor$ trạm mới. $x$ càng nhỏ thì càng cần nhiều trạm.
>
> Dùng tìm kiếm nhị phân trên $x$ kiểu số thực cho đến khi độ rộng khoảng còn $10^{-6}$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minmaxGasDist(self, stations: List[int], k: int) -> float:
        def check(x):
            return sum(int((b - a) / x) for a, b in pairwise(stations)) <= k

        left, right = 0, 1e8
        while right - left > 1e-6:
            mid = (left + right) / 2
            if check(mid):
                right = mid
            else:
                left = mid
        return left
```

#### Java

```java
class Solution {
    public double minmaxGasDist(int[] stations, int k) {
        double left = 0, right = 1e8;
        while (right - left > 1e-6) {
            double mid = (left + right) / 2.0;
            if (check(mid, stations, k)) {
                right = mid;
            } else {
                left = mid;
            }
        }
        return left;
    }

    private boolean check(double x, int[] stations, int k) {
        int s = 0;
        for (int i = 0; i < stations.length - 1; ++i) {
            s += (int) ((stations[i + 1] - stations[i]) / x);
        }
        return s <= k;
    }
}
```

#### C++

```cpp
class Solution {
public:
    double minmaxGasDist(vector<int>& stations, int k) {
        double left = 0, right = 1e8;
        auto check = [&](double x) {
            int s = 0;
            for (int i = 0; i < stations.size() - 1; ++i) {
                s += (int) ((stations[i + 1] - stations[i]) / x);
            }
            return s <= k;
        };
        while (right - left > 1e-6) {
            double mid = (left + right) / 2.0;
            if (check(mid)) {
                right = mid;
            } else {
                left = mid;
            }
        }
        return left;
    }
};
```

#### Go

```go
func minmaxGasDist(stations []int, k int) float64 {
	check := func(x float64) bool {
		s := 0
		for i, v := range stations[:len(stations)-1] {
			s += int(float64(stations[i+1]-v) / x)
		}
		return s <= k
	}
	var left, right float64 = 0, 1e8
	for right-left > 1e-6 {
		mid := (left + right) / 2.0
		if check(mid) {
			right = mid
		} else {
			left = mid
		}
	}
	return left
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
