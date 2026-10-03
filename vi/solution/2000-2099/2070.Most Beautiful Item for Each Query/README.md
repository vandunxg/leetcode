---
comments: true
difficulty: Medium
rating: 1724
source: Biweekly Contest 65 Q3
tags:
    - Array
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [2070. Most Beautiful Item for Each Query](https://leetcode.com/problems/most-beautiful-item-for-each-query)

[中文文档](/solution/2000-2099/2070.Most%20Beautiful%20Item%20for%20Each%20Query/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên 2 chiều <code>items</code>, trong đó <code>items[i] = [price<sub>i</sub>, beauty<sub>i</sub>]</code> lần lượt biểu thị <strong>giá</strong> và <strong>độ đẹp</strong> của một mặt hàng.</p>

<p>Bạn cũng được cho một mảng số nguyên <code>queries</code>, được <strong>đánh chỉ số từ 0</strong>. Với mỗi <code>queries[j]</code>, hãy xác định <strong>độ đẹp lớn nhất</strong> của một mặt hàng có <strong>giá</strong> <strong>nhỏ hơn hoặc bằng</strong> <code>queries[j]</code>. Nếu không có mặt hàng nào như vậy, đáp án của truy vấn là <code>0</code>.</p>

<p>Trả về <em>một mảng </em><code>answer</code><em> có cùng độ dài với </em><code>queries</code><em>, trong đó </em><code>answer[j]</code><em> là đáp án của </em><code>j<sup>th</sup></code><em> truy vấn</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> items = [[1,2],[3,2],[2,4],[5,6],[3,5]], queries = [1,2,3,4,5,6]
<strong>Đầu ra:</strong> [2,4,5,5,6,6]
<strong>Giải thích:</strong>
- Với queries[0]=1, [1,2] là mặt hàng duy nhất có giá &lt;= 1. Vì vậy, đáp án của truy vấn này là 2.
- Với queries[1]=2, các mặt hàng có thể được xét là [1,2] và [2,4].
  Độ đẹp lớn nhất trong số đó là 4.
- Với queries[2]=3 và queries[3]=4, các mặt hàng có thể được xét là [1,2], [3,2], [2,4] và [3,5].
  Độ đẹp lớn nhất trong số đó là 5.
- Với queries[4]=5 và queries[5]=6, tất cả các mặt hàng đều có thể được xét.
  Vì vậy, đáp án của chúng là độ đẹp lớn nhất của tất cả các mặt hàng, tức là 6.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> items = [[1,2],[1,2],[1,3],[1,4]], queries = [1]
<strong>Đầu ra:</strong> [4]
<strong>Giải thích:</strong>
Giá của mọi mặt hàng đều bằng 1, nên ta chọn mặt hàng có độ đẹp lớn nhất là 4.
Lưu ý rằng nhiều mặt hàng có thể có cùng giá và/hoặc cùng độ đẹp.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> items = [[10,1000]], queries = [5]
<strong>Đầu ra:</strong> [0]
<strong>Giải thích:</strong>
Không có mặt hàng nào có giá nhỏ hơn hoặc bằng 5, nên không thể chọn mặt hàng nào.
Vì vậy, đáp án của truy vấn là 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= items.length, queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>items[i].length == 2</code></li>
	<li><code>1 &lt;= price<sub>i</sub>, beauty<sub>i</sub>, queries[j] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Truy vấn offline

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi truy vấn yêu cầu độ đẹp lớn nhất trong các mặt hàng có giá $\le q$. Duyệt trực tiếp cho từng truy vấn không phù hợp khi $n,m \le 10^5$. Xử lý truy vấn offline: sắp xếp các truy vấn và mặt hàng theo giá để con trỏ chỉ cần tiến về phía trước.
>
> Khi một mặt hàng có giá phù hợp, cập nhật độ đẹp lớn nhất hiện tại và ghi kết quả vào chỉ số ban đầu của truy vấn.

<!-- thinking:end -->

Với mỗi truy vấn, ta cần tìm giá trị độ đẹp lớn nhất trong các mặt hàng có giá nhỏ hơn hoặc bằng giá truy vấn. Ta có thể dùng phương pháp truy vấn offline: trước tiên sắp xếp các mặt hàng theo giá, sau đó sắp xếp các truy vấn theo giá.

Tiếp theo, ta duyệt các truy vấn từ nhỏ đến lớn. Với mỗi truy vấn, ta dùng một con trỏ $i$ trỏ vào mảng mặt hàng. Nếu giá của mặt hàng nhỏ hơn hoặc bằng giá truy vấn, ta cập nhật giá trị độ đẹp lớn nhất hiện tại và dịch con trỏ $i$ sang phải cho đến khi giá của mặt hàng lớn hơn giá truy vấn. Ta ghi lại giá trị độ đẹp lớn nhất hiện tại, đây là đáp án cho truy vấn hiện tại. Tiếp tục duyệt truy vấn tiếp theo cho đến khi xử lý hết tất cả các truy vấn.

Độ phức tạp thời gian là $O(n \times \log n + m \times \log m)$, và độ phức tạp không gian là $O(\log n + m)$. Trong đó, $n$ và $m$ lần lượt là độ dài của mảng mặt hàng và mảng truy vấn.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumBeauty(self, items: List[List[int]], queries: List[int]) -> List[int]:
        items.sort()
        n, m = len(items), len(queries)
        ans = [0] * len(queries)
        i = mx = 0
        for q, j in sorted(zip(queries, range(m))):
            while i < n and items[i][0] <= q:
                mx = max(mx, items[i][1])
                i += 1
            ans[j] = mx
        return ans
```

#### Java

```java
class Solution {
    public int[] maximumBeauty(int[][] items, int[] queries) {
        Arrays.sort(items, (a, b) -> a[0] - b[0]);
        int n = items.length;
        int m = queries.length;
        int[] ans = new int[m];
        Integer[] idx = new Integer[m];
        for (int i = 0; i < m; ++i) {
            idx[i] = i;
        }
        Arrays.sort(idx, (i, j) -> queries[i] - queries[j]);
        int i = 0, mx = 0;
        for (int j : idx) {
            while (i < n && items[i][0] <= queries[j]) {
                mx = Math.max(mx, items[i][1]);
                ++i;
            }
            ans[j] = mx;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> maximumBeauty(vector<vector<int>>& items, vector<int>& queries) {
        sort(items.begin(), items.end());
        int n = items.size();
        int m = queries.size();
        vector<int> idx(m);
        iota(idx.begin(), idx.end(), 0);
        sort(idx.begin(), idx.end(), [&](int i, int j) {
            return queries[i] < queries[j];
        });
        int mx = 0, i = 0;
        vector<int> ans(m);
        for (int j : idx) {
            while (i < n && items[i][0] <= queries[j]) {
                mx = max(mx, items[i][1]);
                ++i;
            }
            ans[j] = mx;
        }
        return ans;
    }
};
```

#### Go

```go
func maximumBeauty(items [][]int, queries []int) []int {
	sort.Slice(items, func(i, j int) bool {
		return items[i][0] < items[j][0]
	})
	n, m := len(items), len(queries)
	idx := make([]int, m)
	for i := range idx {
		idx[i] = i
	}
	sort.Slice(idx, func(i, j int) bool { return queries[idx[i]] < queries[idx[j]] })
	ans := make([]int, m)
	i, mx := 0, 0
	for _, j := range idx {
		for i < n && items[i][0] <= queries[j] {
			mx = max(mx, items[i][1])
			i++
		}
		ans[j] = mx
	}
	return ans
}
```

#### TypeScript

```ts
function maximumBeauty(items: number[][], queries: number[]): number[] {
    const n = items.length;
    const m = queries.length;
    items.sort((a, b) => a[0] - b[0]);
    const idx: number[] = Array(m)
        .fill(0)
        .map((_, i) => i);
    idx.sort((i, j) => queries[i] - queries[j]);
    let [i, mx] = [0, 0];
    const ans: number[] = Array(m).fill(0);
    for (const j of idx) {
        while (i < n && items[i][0] <= queries[j]) {
            mx = Math.max(mx, items[i][1]);
            ++i;
        }
        ans[j] = mx;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Sắp xếp + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 sắp xếp lại các truy vấn. Để trả lời trực tiếp, ta sắp xếp các mặt hàng theo giá, xây dựng các giá trị lớn nhất tiền tố của độ đẹp, rồi tìm kiếm nhị phân cho từng $q$.
>
> `mx` là đơn điệu; `bisect_right` cho ta chỉ số cuối cùng của mặt hàng có thể mua được.

<!-- thinking:end -->

Ta có thể sắp xếp các mặt hàng theo giá, sau đó tiền xử lý giá trị độ đẹp lớn nhất của các mặt hàng có giá nhỏ hơn hoặc bằng từng mức giá, rồi lưu các giá trị đó trong mảng $mx$ hoặc ngay trong mảng $items$ ban đầu.

Với mỗi truy vấn, ta có thể dùng tìm kiếm nhị phân để tìm chỉ số $j$ của mặt hàng đầu tiên có giá lớn hơn giá truy vấn. Khi đó, $j - 1$ là chỉ số của mặt hàng có giá trị độ đẹp lớn nhất và giá nhỏ hơn hoặc bằng giá truy vấn, rồi ta thêm giá trị đó vào đáp án.

Độ phức tạp thời gian là $O((m + n) \times \log n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ và $m$ lần lượt là độ dài của mảng mặt hàng và mảng truy vấn.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumBeauty(self, items: List[List[int]], queries: List[int]) -> List[int]:
        items.sort()
        prices = [p for p, _ in items]
        n = len(items)
        mx = [items[0][1]]
        for i in range(1, n):
            mx.append(max(mx[i - 1], items[i][1]))
        ans = []
        for q in queries:
            j = bisect_right(prices, q) - 1
            ans.append(0 if j < 0 else mx[j])
        return ans
```

#### Java

```java
class Solution {
    public int[] maximumBeauty(int[][] items, int[] queries) {
        Arrays.sort(items, (a, b) -> a[0] - b[0]);
        int n = items.length;
        int m = queries.length;
        int[] prices = new int[n];
        prices[0] = items[0][0];
        for (int i = 1; i < n; ++i) {
            prices[i] = items[i][0];
            items[i][1] = Math.max(items[i - 1][1], items[i][1]);
        }
        int[] ans = new int[m];
        for (int i = 0; i < m; ++i) {
            int j = Arrays.binarySearch(prices, queries[i] + 1);
            j = j < 0 ? -j - 2 : j - 1;
            ans[i] = j < 0 ? 0 : items[j][1];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> maximumBeauty(vector<vector<int>>& items, vector<int>& queries) {
        sort(items.begin(), items.end());
        int n = items.size();
        int m = queries.size();
        vector<int> prices(n, items[0][0]);
        for (int i = 1; i < n; ++i) {
            prices[i] = items[i][0];
            items[i][1] = max(items[i - 1][1], items[i][1]);
        }
        vector<int> ans;
        for (int q : queries) {
            int j = upper_bound(prices.begin(), prices.end(), q) - prices.begin() - 1;
            ans.push_back(j < 0 ? 0 : items[j][1]);
        }
        return ans;
    }
};
```

#### Go

```go
func maximumBeauty(items [][]int, queries []int) []int {
	sort.Slice(items, func(i, j int) bool {
		return items[i][0] < items[j][0]
	})
	n, m := len(items), len(queries)
	prices := make([]int, n)
	prices[0] = items[0][0]
	for i := 1; i < n; i++ {
		prices[i] = items[i][0]
		items[i][1] = max(items[i][1], items[i-1][1])
	}
	ans := make([]int, m)
	for i, q := range queries {
		j := sort.SearchInts(prices, q+1) - 1
		if j >= 0 {
			ans[i] = items[j][1]
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maximumBeauty(items: number[][], queries: number[]): number[] {
    items.sort((a, b) => a[0] - b[0]);
    const n = items.length;
    for (let i = 1; i < n; ++i) {
        items[i][1] = Math.max(items[i][1], items[i - 1][1]);
    }
    const ans: number[] = [];
    for (const q of queries) {
        let l = 0,
            r = n;
        while (l < r) {
            const mid = (l + r) >> 1;
            if (items[mid][0] > q) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        ans.push(--l >= 0 ? items[l][1] : 0);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
