---
comments: true
difficulty: Medium
rating: 1885
source: Weekly Contest 266 Q3
tags:
    - Greedy
    - Array
    - Binary Search
---

<!-- problem:start -->

# [2064. Minimized Maximum of Products Distributed to Any Store](https://leetcode.com/problems/minimized-maximum-of-products-distributed-to-any-store)

[中文文档](/solution/2000-2099/2064.Minimized%20Maximum%20of%20Products%20Distributed%20to%20Any%20Store/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code> biểu thị có <code>n</code> cửa hàng bán lẻ chuyên biệt. Có <code>m</code> loại sản phẩm với số lượng khác nhau, được cho bởi một mảng số nguyên <code>quantities</code> đánh chỉ số từ <strong>0</strong>, trong đó <code>quantities[i]</code> biểu thị số sản phẩm của loại sản phẩm thứ <code>i<sup>th</sup></code>.</p>

<p>Bạn cần phân phối <strong>tất cả sản phẩm</strong> cho các cửa hàng bán lẻ theo các quy tắc sau:</p>

<ul>
	<li>Một cửa hàng chỉ có thể được nhận <strong>nhiều nhất một loại sản phẩm</strong>, nhưng có thể nhận <strong>bất kỳ</strong> số lượng nào của loại đó.</li>
	<li>Sau khi phân phối, mỗi cửa hàng sẽ nhận một số sản phẩm (có thể là <code>0</code>). Gọi <code>x</code> là số sản phẩm lớn nhất được phân phối cho bất kỳ cửa hàng nào. Bạn muốn <code>x</code> nhỏ nhất có thể, tức là muốn <strong>tối thiểu hóa</strong> <strong>số sản phẩm lớn nhất</strong> được phân phối cho một cửa hàng.</li>
</ul>

<p>Trả về <em>giá trị nhỏ nhất có thể</em> của <code>x</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 6, quantities = [11,6]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Một cách phân phối tối ưu là:
- 11 sản phẩm loại 0 được phân phối cho bốn cửa hàng đầu tiên với số lượng lần lượt là: 2, 3, 3, 3
- 6 sản phẩm loại 1 được phân phối cho hai cửa hàng còn lại với số lượng lần lượt là: 3, 3
Số sản phẩm lớn nhất được phân phối cho bất kỳ cửa hàng nào là max(2, 3, 3, 3, 3, 3) = 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 7, quantities = [15,10,10]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Một cách phân phối tối ưu là:
- 15 sản phẩm loại 0 được phân phối cho ba cửa hàng đầu tiên với số lượng lần lượt là: 5, 5, 5
- 10 sản phẩm loại 1 được phân phối cho hai cửa hàng tiếp theo với số lượng lần lượt là: 5, 5
- 10 sản phẩm loại 2 được phân phối cho hai cửa hàng cuối cùng với số lượng lần lượt là: 5, 5
Số sản phẩm lớn nhất được phân phối cho bất kỳ cửa hàng nào là max(5, 5, 5, 5, 5, 5, 5) = 5.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1, quantities = [100000]
<strong>Đầu ra:</strong> 100000
<strong>Giải thích:</strong> Cách phân phối tối ưu duy nhất là:
- 100000 sản phẩm loại 0 được phân phối cho cửa hàng duy nhất.
Số sản phẩm lớn nhất được phân phối cho bất kỳ cửa hàng nào là max(100000) = 100000.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == quantities.length</code></li>
	<li><code>1 &lt;= m &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= quantities[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Phân phối từng loại sản phẩm cho các cửa hàng đồng thời tối thiểu hóa giới hạn số sản phẩm trên mỗi cửa hàng $x$. $x$ càng lớn thì điều kiện càng dễ thỏa mãn, nên mệnh đề kiểm tra có tính đơn điệu. Vì cả $m$ và $n$ đều có thể đạt $10^5$, mỗi lần kiểm tra phải có độ phức tạp $O(m)$.
>
> Loại $i$ cần $\lceil q_i/x \rceil$ cửa hàng; điều kiện khả thi là tổng số này $\le n$. Dùng tìm kiếm nhị phân để tìm $x$ nhỏ nhất thỏa mãn điều kiện.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimizedMaximum(self, n: int, quantities: List[int]) -> int:
        def check(x):
            return sum((v + x - 1) // x for v in quantities) <= n

        return 1 + bisect_left(range(1, 10**6), True, key=check)
```

#### Java

```java
class Solution {
    public int minimizedMaximum(int n, int[] quantities) {
        int left = 1, right = (int) 1e5;
        while (left < right) {
            int mid = (left + right) >> 1;
            int cnt = 0;
            for (int v : quantities) {
                cnt += (v + mid - 1) / mid;
            }
            if (cnt <= n) {
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
    int minimizedMaximum(int n, vector<int>& quantities) {
        int left = 1, right = 1e5;
        while (left < right) {
            int mid = (left + right) >> 1;
            int cnt = 0;
            for (int& v : quantities) {
                cnt += (v + mid - 1) / mid;
            }
            if (cnt <= n) {
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
func minimizedMaximum(n int, quantities []int) int {
	return 1 + sort.Search(1e5, func(x int) bool {
		x++
		cnt := 0
		for _, v := range quantities {
			cnt += (v + x - 1) / x
		}
		return cnt <= n
	})
}
```

#### TypeScript

```ts
function minimizedMaximum(n: number, quantities: number[]): number {
    let left = 1;
    let right = 1e5;
    while (left < right) {
        const mid = (left + right) >> 1;
        let cnt = 0;
        for (const v of quantities) {
            cnt += Math.ceil(v / mid);
        }
        if (cnt <= n) {
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
