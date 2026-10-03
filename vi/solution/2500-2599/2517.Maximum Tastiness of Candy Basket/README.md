---
comments: true
difficulty: Medium
rating: 2020
source: Weekly Contest 325 Q3
tags:
    - Greedy
    - Array
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [2517. Maximum Tastiness of Candy Basket](https://leetcode.com/problems/maximum-tastiness-of-candy-basket)

[中文文档](/solution/2500-2599/2517.Maximum%20Tastiness%20of%20Candy%20Basket/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên dương <code>price</code>, trong đó <code>price[i]</code> là giá của viên kẹo thứ <code>i<sup>th</sup></code>, cùng một số nguyên dương <code>k</code>.</p>

<p>Cửa hàng bán các giỏ gồm <code>k</code> viên kẹo <strong>khác nhau</strong>. Độ <strong>ngon</strong> của một giỏ kẹo là chênh lệch tuyệt đối nhỏ nhất giữa <strong>giá</strong> của bất kỳ hai viên kẹo nào trong giỏ.</p>

<p>Trả về <em>độ ngon <strong>lớn nhất</strong> của một giỏ kẹo.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> price = [13,5,1,8,21,2], k = 3
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Chọn các viên kẹo có giá [13,5,21].
Độ ngon của giỏ kẹo là: min(|13 - 5|, |13 - 21|, |5 - 21|) = min(8, 8, 16) = 8.
Có thể chứng minh rằng 8 là độ ngon lớn nhất có thể đạt được.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> price = [1,3,1], k = 2
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Chọn các viên kẹo có giá [1,3].
Độ ngon của giỏ kẹo là: min(|1 - 3|) = min(2) = 2.
Có thể chứng minh rằng 2 là độ ngon lớn nhất có thể đạt được.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> price = [7,7,7,7], k = 2
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Chọn bất kỳ hai viên kẹo khác nhau nào trong số các viên kẹo đã cho cũng cho độ ngon bằng 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= k &lt;= price.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= price[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Độ ngon là chênh lệch nhỏ nhất giữa từng cặp giá trong $k$ giá được chọn, và chúng ta muốn làm cho giá trị nhỏ nhất đó lớn nhất có thể. Việc liệt kê các tập con là bất khả thi khi $n\le 10^5$.
>
> Nếu đạt được độ ngon $x$ thì mọi giá trị nhỏ hơn cũng đạt được, nên ta có thể tìm kiếm nhị phân trên $x$. Sau khi sắp xếp, ta tham lam chọn từ trái sang phải mỗi khi khoảng cách với phần tử được chọn trước đó ít nhất là $x$; chọn được $k$ phần tử thì chứng tỏ điều kiện khả thi. Cận trên của khoảng tìm kiếm là $\max\textit{price}-\min\textit{price}$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumTastiness(self, price: List[int], k: int) -> int:
        def check(x: int) -> bool:
            cnt, pre = 0, -x
            for cur in price:
                if cur - pre >= x:
                    pre = cur
                    cnt += 1
            return cnt >= k

        price.sort()
        l, r = 0, price[-1] - price[0]
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
    public int maximumTastiness(int[] price, int k) {
        Arrays.sort(price);
        int l = 0, r = price[price.length - 1] - price[0];
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            if (check(price, k, mid)) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l;
    }

    private boolean check(int[] price, int k, int x) {
        int cnt = 0, pre = -x;
        for (int cur : price) {
            if (cur - pre >= x) {
                pre = cur;
                ++cnt;
            }
        }
        return cnt >= k;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumTastiness(vector<int>& price, int k) {
        sort(price.begin(), price.end());
        int l = 0, r = price.back() - price[0];
        auto check = [&](int x) -> bool {
            int cnt = 0, pre = -x;
            for (int& cur : price) {
                if (cur - pre >= x) {
                    pre = cur;
                    ++cnt;
                }
            }
            return cnt >= k;
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
func maximumTastiness(price []int, k int) int {
	sort.Ints(price)
	return sort.Search(price[len(price)-1], func(x int) bool {
		cnt, pre := 0, -x
		for _, cur := range price {
			if cur-pre >= x {
				pre = cur
				cnt++
			}
		}
		return cnt < k
	}) - 1
}
```

#### TypeScript

```ts
function maximumTastiness(price: number[], k: number): number {
    price.sort((a, b) => a - b);
    let l = 0;
    let r = price[price.length - 1] - price[0];
    const check = (x: number): boolean => {
        let [cnt, pre] = [0, -x];
        for (const cur of price) {
            if (cur - pre >= x) {
                pre = cur;
                ++cnt;
            }
        }
        return cnt >= k;
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

#### C#

```cs
public class Solution {
    public int MaximumTastiness(int[] price, int k) {
        Array.Sort(price);
        int l = 0, r = price[price.Length - 1] - price[0];
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            if (check(price, mid, k)) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l;
    }

    private bool check(int[] price, int x, int k) {
        int cnt = 0, pre = -x;
        foreach (int cur in price) {
            if (cur - pre >= x) {
                ++cnt;
                pre = cur;
            }
        }
        return cnt >= k;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
