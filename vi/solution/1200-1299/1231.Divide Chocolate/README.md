---
comments: true
difficulty: Hard
rating: 2029
source: Biweekly Contest 11 Q4
tags:
    - Array
    - Binary Search
---

<!-- problem:start -->

# [1231. Divide Chocolate 🔒](https://leetcode.com/problems/divide-chocolate)

[中文文档](/solution/1200-1299/1231.Divide%20Chocolate/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có một thanh chocolate gồm nhiều miếng. Độ ngọt của mỗi miếng được cho trong mảng <code>sweetness</code>.</p>

<p>Bạn muốn chia chocolate cho <code>k</code> người bạn, nên cắt thanh chocolate thành <code>k + 1</code> phần bằng <code>k</code> nhát cắt; mỗi phần gồm một số miếng <strong>liên tiếp</strong>.</p>

<p>Để nhường phần ngon hơn cho bạn bè, bạn sẽ ăn phần có <strong>tổng độ ngọt thấp nhất</strong> và chia các phần còn lại cho họ.</p>

<p>Hãy tìm <strong>tổng độ ngọt lớn nhất</strong> của phần bạn nhận được khi cắt thanh chocolate tối ưu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> sweetness = [1,2,3,4,5,6,7,8,9], k = 5
<strong>Đầu ra:</strong> 6
<b>Giải thích: </b>Có thể chia chocolate thành [1,2,3], [4,5], [6], [7], [8], [9].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> sweetness = [5,6,7,8,9,1,2,3,4], k = 8
<strong>Đầu ra:</strong> 1
<b>Giải thích: </b>Chỉ có một cách cắt thanh chocolate thành 9 phần.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> sweetness = [1,2,2,1,2,2,1,2,2], k = 2
<strong>Đầu ra:</strong> 5
<b>Giải thích: </b>Có thể chia chocolate thành [1,2,2], [1,2,2], [1,2,2].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= k &lt; sweetness.length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= sweetness[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân + tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Ta chia thanh chocolate thành $k+1$ phần và tối đa hóa độ ngọt thấp nhất trong các phần. Vì $n \le 10^4$, không thể thử mọi cách cắt. Nếu ngưỡng độ ngọt $x$ khả thi thì mọi ngưỡng nhỏ hơn cũng khả thi, nên tính khả thi có tính đơn điệu.
>
> Khi kiểm tra, ta cộng dồn từ trái sang phải và cắt mỗi khi tổng đạt $x$; nếu tạo được hơn $k$ phần (một phần cho ta và $k$ phần cho bạn bè) thì $x$ khả thi. Ta tìm kiếm nhị phân giá trị $x$ khả thi lớn nhất trong $[0,\sum sweetness]$. Cắt ngay khi một phần đạt ngưỡng chỉ giúp phần còn lại dành cho các phần sau nhiều hơn.

<!-- thinking:end -->

Ta nhận thấy rằng nếu có thể nhận được một phần chocolate có độ ngọt $x$, thì cũng có thể nhận phần có độ ngọt nhỏ hơn hoặc bằng $x$. Tính đơn điệu này cho phép dùng tìm kiếm nhị phân để tìm $x$ lớn nhất thỏa mãn điều kiện.

Ta đặt biên trái tìm kiếm nhị phân là $l=0$ và biên phải là $r=\sum_{i=0}^{n-1} sweetness[i]$. Mỗi lần, lấy giá trị giữa $mid$ của $l$ và $r$, rồi kiểm tra xem có thể nhận phần chocolate có độ ngọt $mid$ hay không. Nếu được, ta thử độ ngọt lớn hơn bằng cách đặt $l=mid$; nếu không, ta thử độ ngọt nhỏ hơn bằng cách đặt $r=mid-1$. Khi tìm kiếm nhị phân kết thúc, trả về $l$.

Mấu chốt là xác định liệu ta có thể nhận phần chocolate có độ ngọt $x$ hay không. Dùng cách tham lam: duyệt mảng từ trái sang phải, cộng dồn độ ngọt; mỗi khi tổng đạt ít nhất $x$, tăng số phần chocolate $cnt$ thêm $1$ rồi đặt tổng tích lũy về 0. Cuối cùng, kiểm tra xem $cnt$ có lớn hơn $k$ hay không.

Độ phức tạp thời gian là $O(n \times \log \sum_{i=0}^{n-1} sweetness[i])$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximizeSweetness(self, sweetness: List[int], k: int) -> int:
        def check(x: int) -> bool:
            s = cnt = 0
            for v in sweetness:
                s += v
                if s >= x:
                    s = 0
                    cnt += 1
            return cnt > k

        l, r = 0, sum(sweetness)
        while l < r:
            mid = (l + r + 1) >> 1
            if check(mid):
                l = mid
            else:
                r = mid - 1
        return l
```

#### Java

```java
class Solution {
    public int maximizeSweetness(int[] sweetness, int k) {
        int l = 0, r = 0;
        for (int v : sweetness) {
            r += v;
        }
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            if (check(sweetness, mid, k)) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l;
    }

    private boolean check(int[] nums, int x, int k) {
        int s = 0, cnt = 0;
        for (int v : nums) {
            s += v;
            if (s >= x) {
                s = 0;
                ++cnt;
            }
        }
        return cnt > k;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximizeSweetness(vector<int>& sweetness, int k) {
        int l = 0, r = accumulate(sweetness.begin(), sweetness.end(), 0);
        auto check = [&](int x) {
            int s = 0, cnt = 0;
            for (int v : sweetness) {
                s += v;
                if (s >= x) {
                    s = 0;
                    ++cnt;
                }
            }
            return cnt > k;
        };
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            if (check(mid)) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l;
    }
};
```

#### Go

```go
func maximizeSweetness(sweetness []int, k int) int {
	l, r := 0, 0
	for _, v := range sweetness {
		r += v
	}
	check := func(x int) bool {
		s, cnt := 0, 0
		for _, v := range sweetness {
			s += v
			if s >= x {
				s = 0
				cnt++
			}
		}
		return cnt > k
	}
	for l < r {
		mid := (l + r + 1) >> 1
		if check(mid) {
			l = mid
		} else {
			r = mid - 1
		}
	}
	return l
}
```

#### TypeScript

```ts
function maximizeSweetness(sweetness: number[], k: number): number {
    let l = 0;
    let r = sweetness.reduce((a, b) => a + b);
    const check = (x: number): boolean => {
        let s = 0;
        let cnt = 0;
        for (const v of sweetness) {
            s += v;
            if (s >= x) {
                s = 0;
                ++cnt;
            }
        }
        return cnt > k;
    };
    while (l < r) {
        const mid = (l + r + 1) >> 1;
        if (check(mid)) {
            l = mid;
        } else {
            r = mid - 1;
        }
    }
    return l;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
