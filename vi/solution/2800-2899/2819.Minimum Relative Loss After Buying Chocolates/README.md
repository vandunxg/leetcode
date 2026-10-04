---
comments: true
difficulty: Hard
tags:
    - Array
    - Binary Search
    - Prefix Sum
    - Sorting
---

<!-- problem:start -->

# [2819. Minimum Relative Loss After Buying Chocolates 🔒](https://leetcode.com/problems/minimum-relative-loss-after-buying-chocolates)

[中文文档](/solution/2800-2899/2819.Minimum%20Relative%20Loss%20After%20Buying%20Chocolates/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>prices</code> biểu thị giá của các thanh chocolate và một mảng số nguyên 2D <code>queries</code>, trong đó <code>queries[i] = [k<sub>i</sub>, m<sub>i</sub>]</code>.</p>

<p>Alice và Bob đi mua một số thanh chocolate, Alice đề xuất một cách thanh toán và Bob đồng ý.</p>

<p>Các điều khoản cho mỗi truy vấn như sau:</p>

<ul>
	<li>Nếu giá của một thanh chocolate <strong>nhỏ hơn hoặc bằng</strong> <code>k<sub>i</sub></code>, Bob sẽ trả tiền cho thanh đó.</li>
	<li>Nếu không, Bob trả <code>k<sub>i</sub></code> cho thanh đó, còn Alice trả <strong>phần còn lại</strong>.</li>
</ul>

<p>Bob muốn chọn <strong>chính xác</strong> <code>m<sub>i</sub></code> thanh chocolate sao cho <strong>tổn thất tương đối</strong> của mình là <strong>nhỏ nhất</strong>. Cụ thể hơn, nếu tổng số tiền Alice trả là <code>a<sub>i</sub></code> và Bob trả là <code>b<sub>i</sub></code>, Bob muốn tối thiểu hóa <code>b<sub>i</sub> - a<sub>i</sub></code>.</p>

<p>Trả về <em>một mảng số nguyên</em> <code>ans</code>, <em>trong đó</em> <code>ans[i]</code> <em>là <strong>tổn thất tương đối nhỏ nhất </strong>của Bob có thể đạt được cho</em> <code>queries[i]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> prices = [1,9,22,10,19], queries = [[18,4],[5,2]]
<strong>Đầu ra:</strong> [34,-21]
<strong>Giải thích:</strong> Với truy vấn thứ 1<sup>st</sup>, Bob chọn các thanh chocolate có giá [1,9,10,22]. Bob trả 1 + 9 + 10 + 18 = 38 và Alice trả 0 + 0 + 0 + 4 = 4. Vì vậy, tổn thất tương đối của Bob là 38 - 4 = 34.
Với truy vấn thứ 2<sup>nd</sup>, Bob chọn các thanh chocolate có giá [19,22]. Bob trả 5 + 5 = 10 và Alice trả 14 + 17 = 31. Vì vậy, tổn thất tương đối của Bob là 10 - 31 = -21.
Có thể chứng minh đây là các tổn thất tương đối nhỏ nhất có thể đạt được.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> prices = [1,5,4,3,7,11,9], queries = [[5,4],[5,7],[7,3],[4,5]]
<strong>Đầu ra:</strong> [4,16,7,1]
<strong>Giải thích:</strong> Với truy vấn thứ 1<sup>st</sup>, Bob chọn các thanh chocolate có giá [1,3,9,11]. Bob trả 1 + 3 + 5 + 5 = 14 và Alice trả 0 + 0 + 4 + 6 = 10. Vì vậy, tổn thất tương đối của Bob là 14 - 10 = 4.
Với truy vấn thứ 2<sup>nd</sup>, Bob phải chọn tất cả các thanh chocolate. Bob trả 1 + 5 + 4 + 3 + 5 + 5 + 5 = 28 và Alice trả 0 + 0 + 0 + 0 + 2 + 6 + 4 = 12. Vì vậy, tổn thất tương đối của Bob là 28 - 12 = 16.
Với truy vấn thứ 3<sup>rd</sup>, Bob chọn các thanh chocolate có giá [1,3,11] và trả 1 + 3 + 7 = 11, còn Alice trả 0 + 0 + 4 = 4. Vì vậy, tổn thất tương đối của Bob là 11 - 4 = 7.
Với truy vấn thứ 4<sup>th</sup>, Bob chọn các thanh chocolate có giá [1,3,7,9,11] và trả 1 + 3 + 4 + 4 + 4 = 16, còn Alice trả 0 + 0 + 3 + 5 + 7 = 15. Vì vậy, tổn thất tương đối của Bob là 16 - 15 = 1.
Có thể chứng minh đây là các tổn thất tương đối nhỏ nhất có thể đạt được.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> prices = [5,6,7], queries = [[10,1],[5,3],[3,3]]
<strong>Đầu ra:</strong> [5,12,0]
<strong>Giải thích:</strong> Với truy vấn thứ 1<sup>st</sup>, Bob chọn thanh chocolate có giá 5, trả 5 và Alice trả 0. Vì vậy, tổn thất tương đối của Bob là 5 - 0 = 5.
Với truy vấn thứ 2<sup>nd</sup>, Bob phải chọn tất cả các thanh chocolate. Bob trả 5 + 5 + 5 = 15 và Alice trả 0 + 1 + 2 = 3. Vì vậy, tổn thất tương đối của Bob là 15 - 3 = 12.
Với truy vấn thứ 3<sup>rd</sup>, Bob phải chọn tất cả các thanh chocolate. Bob trả 3 + 3 + 3 = 9 và Alice trả 2 + 3 + 4 = 9. Vì vậy, tổn thất tương đối của Bob là 9 - 9 = 0.
Có thể chứng minh đây là các tổn thất tương đối nhỏ nhất có thể đạt được.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= prices.length == n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= prices[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>queries[i].length == 2</code></li>
	<li><code>1 &lt;= k<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= m<sub>i</sub> &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Tìm kiếm nhị phân + Tổng tiền tố

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi truy vấn, ta mua $m$ thanh chocolate ở ngưỡng $k$. Tổn thất tương đối là chính giá của thanh chocolate nếu $price\le k$, và là $2k-price$ nếu $price>k$. Một cách chọn tối ưu sẽ lấy một số thanh rẻ nhất và một số thanh đắt nhất. Sau khi sắp xếp và tính tổng tiền tố, ta dùng tìm kiếm nhị phân để tìm điểm chia cho mỗi truy vấn.

<!-- thinking:end -->

Dựa trên mô tả bài toán, ta biết rằng:

Nếu $prices[i] \leq k$, Bob cần trả $prices[i]$ và Alice không cần trả tiền. Do đó, tổn thất tương đối của Bob là $prices[i]$. Trong trường hợp này, Bob nên chọn thanh chocolate có giá thấp hơn để giảm tổn thất tương đối.

Nếu $prices[i] > k$, Bob cần trả $k$ và Alice cần trả $prices[i] - k$. Do đó, tổn thất tương đối của Bob là $k - (prices[i] - k) = 2k - prices[i]$. Trong trường hợp này, Bob nên chọn thanh chocolate có giá cao hơn để giảm tổn thất tương đối.

Vì vậy, trước tiên ta sắp xếp mảng giá $prices$, sau đó tiền xử lý mảng tổng tiền tố $s$, trong đó $s[i]$ biểu thị tổng giá của $i$ thanh chocolate đầu tiên.

Tiếp theo, với mỗi truy vấn $[k, m]$, trước tiên ta dùng tìm kiếm nhị phân để tìm chỉ số $r$ của thanh chocolate đầu tiên có giá lớn hơn $k$. Sau đó, ta lại dùng tìm kiếm nhị phân để tìm số thanh chocolate $l$ cần chọn ở bên trái; khi đó số thanh chocolate cần chọn ở bên phải là $m - l$. Lúc này, tổn thất tương đối của Bob là $s[l] + 2k(m - l) - (s[n] - s[n - (m - l)])$.

Trong quá trình tìm kiếm nhị phân thứ hai nói trên, ta cần kiểm tra liệu $prices[mid] < 2k - prices[n - (m - mid)]$ hay không, trong đó $right$ là số thanh chocolate cần chọn ở bên phải. Nếu bất đẳng thức này đúng, nghĩa là chọn thanh chocolate ở vị trí $mid$ có tổn thất tương đối thấp hơn, nên ta cập nhật $l = mid + 1$. Ngược lại, thanh chocolate ở vị trí $mid$ có tổn thất tương đối cao hơn, nên ta cập nhật $r = mid$.

Độ phức tạp thời gian là $O((n + m) \times \log n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ và $m$ lần lượt là độ dài của các mảng $prices$ và $queries$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumRelativeLosses(
        self, prices: List[int], queries: List[List[int]]
    ) -> List[int]:
        def f(k: int, m: int) -> int:
            l, r = 0, min(m, bisect_right(prices, k))
            while l < r:
                mid = (l + r) >> 1
                right = m - mid
                if prices[mid] < 2 * k - prices[n - right]:
                    l = mid + 1
                else:
                    r = mid
            return l

        prices.sort()
        s = list(accumulate(prices, initial=0))
        ans = []
        n = len(prices)
        for k, m in queries:
            l = f(k, m)
            r = m - l
            loss = s[l] + 2 * k * r - (s[n] - s[n - r])
            ans.append(loss)
        return ans
```

#### Java

```java
class Solution {
    private int n;
    private int[] prices;

    public long[] minimumRelativeLosses(int[] prices, int[][] queries) {
        n = prices.length;
        Arrays.sort(prices);
        this.prices = prices;
        long[] s = new long[n + 1];
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + prices[i];
        }
        int q = queries.length;
        long[] ans = new long[q];
        for (int i = 0; i < q; ++i) {
            int k = queries[i][0], m = queries[i][1];
            int l = f(k, m);
            int r = m - l;
            ans[i] = s[l] + 2L * k * r - (s[n] - s[n - r]);
        }
        return ans;
    }

    private int f(int k, int m) {
        int l = 0, r = Arrays.binarySearch(prices, k);
        if (r < 0) {
            r = -(r + 1);
        }
        r = Math.min(m, r);
        while (l < r) {
            int mid = (l + r) >> 1;
            int right = m - mid;
            if (prices[mid] < 2L * k - prices[n - right]) {
                l = mid + 1;
            } else {
                r = mid;
            }
        }
        return l;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<long long> minimumRelativeLosses(vector<int>& prices, vector<vector<int>>& queries) {
        int n = prices.size();
        sort(prices.begin(), prices.end());
        long long s[n + 1];
        s[0] = 0;
        for (int i = 1; i <= n; ++i) {
            s[i] = s[i - 1] + prices[i - 1];
        }
        auto f = [&](int k, int m) {
            int l = 0, r = upper_bound(prices.begin(), prices.end(), k) - prices.begin();
            r = min(r, m);
            while (l < r) {
                int mid = (l + r) >> 1;
                int right = m - mid;
                if (prices[mid] < 2LL * k - prices[n - right]) {
                    l = mid + 1;
                } else {
                    r = mid;
                }
            }
            return l;
        };
        vector<long long> ans;
        for (auto& q : queries) {
            int k = q[0], m = q[1];
            int l = f(k, m);
            int r = m - l;
            ans.push_back(s[l] + 2LL * k * r - (s[n] - s[n - r]));
        }
        return ans;
    }
};
```

#### Go

```go
func minimumRelativeLosses(prices []int, queries [][]int) []int64 {
	n := len(prices)
	sort.Ints(prices)
	s := make([]int, n+1)
	for i, x := range prices {
		s[i+1] = s[i] + x
	}
	f := func(k, m int) int {
		l, r := 0, sort.Search(n, func(i int) bool { return prices[i] > k })
		if r > m {
			r = m
		}
		for l < r {
			mid := (l + r) >> 1
			right := m - mid
			if prices[mid] < 2*k-prices[n-right] {
				l = mid + 1
			} else {
				r = mid
			}
		}
		return l
	}
	ans := make([]int64, len(queries))
	for i, q := range queries {
		k, m := q[0], q[1]
		l := f(k, m)
		r := m - l
		ans[i] = int64(s[l] + 2*k*r - (s[n] - s[n-r]))
	}
	return ans
}
```

#### TypeScript

```ts
function minimumRelativeLosses(prices: number[], queries: number[][]): number[] {
    const n = prices.length;
    prices.sort((a, b) => a - b);
    const s: number[] = Array(n).fill(0);
    for (let i = 0; i < n; ++i) {
        s[i + 1] = s[i] + prices[i];
    }

    const search = (x: number): number => {
        let l = 0;
        let r = n;
        while (l < r) {
            const mid = (l + r) >> 1;
            if (prices[mid] > x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    };

    const f = (k: number, m: number): number => {
        let l = 0;
        let r = Math.min(search(k), m);
        while (l < r) {
            const mid = (l + r) >> 1;
            const right = m - mid;
            if (prices[mid] < 2 * k - prices[n - right]) {
                l = mid + 1;
            } else {
                r = mid;
            }
        }
        return l;
    };
    const ans: number[] = [];
    for (const [k, m] of queries) {
        const l = f(k, m);
        const r = m - l;
        ans.push(s[l] + 2 * k * r - (s[n] - s[n - r]));
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
