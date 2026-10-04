---
comments: true
difficulty: Medium
rating: 1851
source: Weekly Contest 359 Q3
tags:
    - Array
    - Hash Table
    - Binary Search
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [2830. Maximize the Profit as the Salesman](https://leetcode.com/problems/maximize-the-profit-as-the-salesman)

[中文文档](/solution/2800-2899/2830.Maximize%20the%20Profit%20as%20the%20Salesman/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code> biểu thị số lượng ngôi nhà trên một trục số, được đánh số từ <code>0</code> đến <code>n - 1</code>.</p>

<p>Ngoài ra, bạn được cho một mảng số nguyên 2 chiều <code>offers</code>, trong đó <code>offers[i] = [start<sub>i</sub>, end<sub>i</sub>, gold<sub>i</sub>]</code>, cho biết người mua thứ <code>i<sup>th</sup></code> muốn mua tất cả các ngôi nhà từ <code>start<sub>i</sub></code> đến <code>end<sub>i</sub></code> với số lượng vàng là <code>gold<sub>i</sub></code>.</p>

<p>Là một người bán hàng, mục tiêu của bạn là <strong>tối đa hóa</strong> thu nhập bằng cách lựa chọn và bán nhà cho người mua một cách hợp lý.</p>

<p>Trả về <em>lượng vàng tối đa bạn có thể kiếm được</em>.</p>

<p><strong>Lưu ý</strong> rằng những người mua khác nhau không thể mua cùng một ngôi nhà, và một số ngôi nhà có thể không được bán.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5, offers = [[0,0,1],[0,2,2],[1,3,2]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Có 5 ngôi nhà được đánh số từ 0 đến 4 và có 3 lời đề nghị mua.
Ta bán các ngôi nhà trong đoạn [0,0] cho người mua thứ 1<sup>st</sup> với giá 1 vàng và các ngôi nhà trong đoạn [1,3] cho người mua thứ 3<sup>rd</sup> với giá 2 vàng.
Có thể chứng minh rằng 3 là lượng vàng tối đa chúng ta có thể kiếm được.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5, offers = [[0,0,1],[0,2,10],[1,3,2]]
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Có 5 ngôi nhà được đánh số từ 0 đến 4 và có 3 lời đề nghị mua.
Ta bán các ngôi nhà trong đoạn [0,2] cho người mua thứ 2<sup>nd</sup> với giá 10 vàng.
Có thể chứng minh rằng 10 là lượng vàng tối đa chúng ta có thể kiếm được.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= offers.length &lt;= 10<sup>5</sup></code></li>
	<li><code>offers[i].length == 3</code></li>
	<li><code>0 &lt;= start<sub>i</sub> &lt;= end<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>1 &lt;= gold<sub>i</sub> &lt;= 10<sup>3</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Tìm kiếm nhị phân + Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Các lời đề nghị không thể chồng lấn, nên chúng tạo thành một chuỗi thứ tự. Sau khi sắp xếp theo vị trí kết thúc, $f[i]$ là lợi nhuận tốt nhất trong $i$ lời đề nghị đầu tiên: bỏ qua lời đề nghị thứ $i$, hoặc chọn nó cộng với lời đề nghị cuối cùng có vị trí kết thúc không vượt quá vị trí bắt đầu của nó. Vì các vị trí kết thúc tăng dần, ta có thể tìm lời đề nghị trước đó bằng tìm kiếm nhị phân.

<!-- thinking:end -->

Ta sắp xếp tất cả các lời đề nghị mua theo $end$ tăng dần, sau đó dùng quy hoạch động để giải bài toán.

Đặt $f[i]$ là lượng vàng tối đa có thể nhận được từ $i$ lời đề nghị mua đầu tiên. Đáp án là $f[n]$.

Với $f[i]$, ta có thể chọn không bán theo lời đề nghị mua thứ $i$, khi đó $f[i] = f[i - 1]$; hoặc chọn bán theo lời đề nghị mua thứ $i$, khi đó $f[i] = f[j] + gold_i$, trong đó $j$ là chỉ số lớn nhất thỏa mãn $end_j \leq start_i$.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là số lượng lời đề nghị mua.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximizeTheProfit(self, n: int, offers: List[List[int]]) -> int:
        offers.sort(key=lambda x: x[1])
        f = [0] * (len(offers) + 1)
        g = [x[1] for x in offers]
        for i, (s, _, v) in enumerate(offers, 1):
            j = bisect_left(g, s)
            f[i] = max(f[i - 1], f[j] + v)
        return f[-1]
```

#### Java

```java
class Solution {
    public int maximizeTheProfit(int n, List<List<Integer>> offers) {
        offers.sort((a, b) -> a.get(1) - b.get(1));
        n = offers.size();
        int[] f = new int[n + 1];
        int[] g = new int[n];
        for (int i = 0; i < n; ++i) {
            g[i] = offers.get(i).get(1);
        }
        for (int i = 1; i <= n; ++i) {
            var o = offers.get(i - 1);
            int j = search(g, o.get(0));
            f[i] = Math.max(f[i - 1], f[j] + o.get(2));
        }
        return f[n];
    }

    private int search(int[] nums, int x) {
        int l = 0, r = nums.length;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (nums[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
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
    int maximizeTheProfit(int n, vector<vector<int>>& offers) {
        sort(offers.begin(), offers.end(), [](const vector<int>& a, const vector<int>& b) {
            return a[1] < b[1];
        });
        n = offers.size();
        vector<int> f(n + 1);
        vector<int> g;
        for (auto& o : offers) {
            g.push_back(o[1]);
        }
        for (int i = 1; i <= n; ++i) {
            auto o = offers[i - 1];
            int j = lower_bound(g.begin(), g.end(), o[0]) - g.begin();
            f[i] = max(f[i - 1], f[j] + o[2]);
        }
        return f[n];
    }
};
```

#### Go

```go
func maximizeTheProfit(n int, offers [][]int) int {
	sort.Slice(offers, func(i, j int) bool { return offers[i][1] < offers[j][1] })
	n = len(offers)
	f := make([]int, n+1)
	g := []int{}
	for _, o := range offers {
		g = append(g, o[1])
	}
	for i := 1; i <= n; i++ {
		j := sort.SearchInts(g, offers[i-1][0])
		f[i] = max(f[i-1], f[j]+offers[i-1][2])
	}
	return f[n]
}
```

#### TypeScript

```ts
function maximizeTheProfit(n: number, offers: number[][]): number {
    offers.sort((a, b) => a[1] - b[1]);
    n = offers.length;
    const f: number[] = Array(n + 1).fill(0);
    const g = offers.map(x => x[1]);
    const search = (x: number) => {
        let l = 0;
        let r = n;
        while (l < r) {
            const mid = (l + r) >> 1;
            if (g[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    };
    for (let i = 1; i <= n; ++i) {
        const j = search(offers[i - 1][0]);
        f[i] = Math.max(f[i - 1], f[j] + offers[i - 1][2]);
    }
    return f[n];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
