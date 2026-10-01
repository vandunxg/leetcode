---
comments: true
difficulty: Hard
tags:
    - Geometry
    - Array
    - Math
---

<!-- problem:start -->

# [335. Self Crossing](https://leetcode.com/problems/self-crossing)

[中文文档](/solution/0300-0399/0335.Self%20Crossing/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho mảng số nguyên <code>distance</code>.</p>

<p>Bạn bắt đầu tại điểm <code>(0, 0)</code> trên <strong>mặt phẳng X-Y,</strong> đi <code>distance[0]</code> mét về phía bắc, sau đó đi <code>distance[1]</code> mét về phía tây, <code>distance[2]</code> mét về phía nam, <code>distance[3]</code> mét về phía đông, rồi tiếp tục như vậy. Nói cách khác, sau mỗi lần di chuyển, hướng đi của bạn đổi ngược chiều kim đồng hồ.</p>

<p>Hãy trả về <code>true</code> <em>nếu đường đi tự cắt qua chính nó, nếu không thì trả về </em><code>false</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0335.Self%20Crossing/images/11.jpg" style="width: 400px; height: 413px;" />
<pre>
<strong>Đầu vào:</strong> distance = [2,1,1,2]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Đường đi tự cắt tại điểm (0, 1).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0335.Self%20Crossing/images/22.jpg" style="width: 400px; height: 413px;" />
<pre>
<strong>Đầu vào:</strong> distance = [1,2,3,4]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Đường đi không tự cắt tại bất kỳ điểm nào.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0335.Self%20Crossing/images/33.jpg" style="width: 400px; height: 413px;" />
<pre>
<strong>Đầu vào:</strong> distance = [1,1,1,2,1]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Đường đi tự cắt tại điểm (0, 0).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;=&nbsp;distance.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;=&nbsp;distance[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đi lần lượt về phía bắc, tây, nam rồi đông và kiểm tra xem đường đi có tự cắt hay không. Lưu tọa độ rồi kiểm tra từng đoạn sẽ tốn $O(n^2)$ với $n\le 5\times 10^4$.
>
> Một giao điểm chỉ liên quan đến vài đoạn cuối: đoạn thứ tư giao đoạn thứ nhất, đoạn thứ năm giao đoạn thứ hai, hoặc đoạn thứ sáu giao đoạn thứ ba và đoạn thứ nhất. Với $i\ge 3$, chỉ cần kiểm tra ba trường hợp cục bộ này; không cần xét toàn bộ đường đi.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isSelfCrossing(self, distance: List[int]) -> bool:
        d = distance
        for i in range(3, len(d)):
            if d[i] >= d[i - 2] and d[i - 1] <= d[i - 3]:
                return True
            if i >= 4 and d[i - 1] == d[i - 3] and d[i] + d[i - 4] >= d[i - 2]:
                return True
            if (
                i >= 5
                and d[i - 2] >= d[i - 4]
                and d[i - 1] <= d[i - 3]
                and d[i] >= d[i - 2] - d[i - 4]
                and d[i - 1] + d[i - 5] >= d[i - 3]
            ):
                return True
        return False
```

#### Java

```java
class Solution {
    public boolean isSelfCrossing(int[] distance) {
        int[] d = distance;
        for (int i = 3; i < d.length; ++i) {
            if (d[i] >= d[i - 2] && d[i - 1] <= d[i - 3]) {
                return true;
            }
            if (i >= 4 && d[i - 1] == d[i - 3] && d[i] + d[i - 4] >= d[i - 2]) {
                return true;
            }
            if (i >= 5 && d[i - 2] >= d[i - 4] && d[i - 1] <= d[i - 3]
                && d[i] >= d[i - 2] - d[i - 4] && d[i - 1] + d[i - 5] >= d[i - 3]) {
                return true;
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isSelfCrossing(vector<int>& distance) {
        vector<int> d = distance;
        for (int i = 3; i < d.size(); ++i) {
            if (d[i] >= d[i - 2] && d[i - 1] <= d[i - 3]) return true;
            if (i >= 4 && d[i - 1] == d[i - 3] && d[i] + d[i - 4] >= d[i - 2]) return true;
            if (i >= 5 && d[i - 2] >= d[i - 4] && d[i - 1] <= d[i - 3] && d[i] >= d[i - 2] - d[i - 4] && d[i - 1] + d[i - 5] >= d[i - 3]) return true;
        }
        return false;
    }
};
```

#### Go

```go
func isSelfCrossing(distance []int) bool {
	d := distance
	for i := 3; i < len(d); i++ {
		if d[i] >= d[i-2] && d[i-1] <= d[i-3] {
			return true
		}
		if i >= 4 && d[i-1] == d[i-3] && d[i]+d[i-4] >= d[i-2] {
			return true
		}
		if i >= 5 && d[i-2] >= d[i-4] && d[i-1] <= d[i-3] && d[i] >= d[i-2]-d[i-4] && d[i-1]+d[i-5] >= d[i-3] {
			return true
		}
	}
	return false
}
```

#### C#

```cs
public class Solution {
    public bool IsSelfCrossing(int[] x) {
        for (var i = 3; i < x.Length; ++i) {
            if (x[i] >= x[i - 2] && x[i - 1] <= x[i - 3]) return true;
            if (i > 3 && x[i] + x[i - 4] >= x[i - 2]) {
                if (x[i - 1] == x[i - 3]) return true;
                if (i > 4 && x[i - 2] >= x[i - 4] && x[i - 1] <= x[i - 3] && x[i - 1] + x[i - 5] >= x[i - 3]) return true;
            }
        }
        return false;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
