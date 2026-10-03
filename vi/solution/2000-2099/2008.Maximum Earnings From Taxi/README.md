---
comments: true
difficulty: Medium
rating: 1871
source: Biweekly Contest 61 Q3
tags:
    - Array
    - Hash Table
    - Binary Search
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [2008. Maximum Earnings From Taxi](https://leetcode.com/problems/maximum-earnings-from-taxi)

[中文文档](/solution/2000-2099/2008.Maximum%20Earnings%20From%20Taxi/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> điểm trên con đường mà bạn đang lái taxi. Các <code>n</code> điểm trên đường được đánh số từ <code>1</code> đến <code>n</code> theo hướng bạn đang đi, và bạn muốn lái xe từ điểm <code>1</code> đến điểm <code>n</code> để kiếm tiền bằng cách đón hành khách. Bạn không thể đổi hướng của taxi.</p>

<p>Các hành khách được biểu diễn bằng một mảng số nguyên 2 chiều <strong>đánh chỉ số từ 0</strong> <code>rides</code>, trong đó <code>rides[i] = [start<sub>i</sub>, end<sub>i</sub>, tip<sub>i</sub>]</code> biểu thị hành khách thứ <code>i<sup>th</sup></code> yêu cầu đi từ điểm <code>start<sub>i</sub></code> đến điểm <code>end<sub>i</sub></code> và sẵn sàng trả thêm <code>tip<sub>i</sub></code> dollar tiền tip.</p>

<p>Với <strong>mỗi</strong> hành khách <code>i</code> mà bạn đón, bạn <strong>kiếm được</strong> <code>end<sub>i</sub> - start<sub>i</sub> + tip<sub>i</sub></code> dollar. Mỗi thời điểm bạn chỉ được chở <b>nhiều nhất một</b> hành khách.</p>

<p>Cho <code>n</code> và <code>rides</code>, hãy trả về <em>số dollar <strong>lớn nhất</strong> mà bạn có thể kiếm được bằng cách chọn hành khách một cách tối ưu.</em></p>

<p><strong>Lưu ý:</strong> Bạn có thể trả một hành khách và đón hành khách khác tại cùng một điểm.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5, rides = [<u>[2,5,4]</u>,[1,5,1]]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Ta có thể đón hành khách 0 và kiếm được 5 - 2 + 4 = 7 dollar.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 20, rides = [[1,6,1],<u>[3,10,2]</u>,<u>[10,12,3]</u>,[11,12,2],[12,15,2],<u>[13,18,1]</u>]
<strong>Đầu ra:</strong> 20
<strong>Giải thích:</strong> Ta sẽ đón các hành khách sau:
- Chở hành khách 1 từ điểm 3 đến điểm 10, thu được 10 - 3 + 2 = 9 dollar.
- Chở hành khách 2 từ điểm 10 đến điểm 12, thu được 12 - 10 + 3 = 5 dollar.
- Chở hành khách 5 từ điểm 13 đến điểm 18, thu được 18 - 13 + 1 = 6 dollar.
Tổng cộng ta kiếm được 9 + 5 + 6 = 20 dollar.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= rides.length &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>rides[i].length == 3</code></li>
	<li><code>1 &lt;= start<sub>i</sub> &lt; end<sub>i</sub> &lt;= n</code></li>
	<li><code>1 &lt;= tip<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Sau khi sắp xếp các chuyến đi theo điểm bắt đầu, chuyến đi $i$ chỉ có thể kết hợp với những chuyến đi phía sau có điểm bắt đầu $\ge end_i$. Việc xét mọi tập con là không khả thi với $m \le 3 \times 10^4$; trạng thái cần xét là lợi nhuận tốt nhất từ chỉ số $i$.
>
> Nhánh bỏ qua chuyển đến $i+1$; nhánh chọn dùng tìm kiếm nhị phân để tìm điểm bắt đầu đầu tiên $\ge end_i$, gọi là $j$, rồi cộng quãng đường, tiền tip và $dfs(j)$.
>
> Tìm kiếm có ghi nhớ tính mỗi $i$ đúng một lần trong $O(m \log m)$.

<!-- thinking:end -->

Trước tiên, ta sắp xếp $rides$ theo thứ tự tăng dần của $start$. Sau đó, ta định nghĩa hàm $dfs(i)$ biểu thị số tiền tip tối đa có thể nhận được khi xét các chuyến đi bắt đầu từ hành khách thứ $i$. Đáp án là $dfs(0)$.

Quá trình tính hàm $dfs(i)$ như sau:

Với hành khách thứ $i$, ta có thể chọn nhận hoặc không nhận chuyến đi. Nếu không nhận, số tiền tip tối đa có thể nhận được là $dfs(i + 1)$. Nếu nhận, ta có thể dùng tìm kiếm nhị phân để tìm hành khách đầu tiên xuất hiện sau điểm trả của hành khách thứ $i$, gọi là $j$. Số tiền tip tối đa có thể nhận được là $dfs(j) + end_i - start_i + tip_i$. Ta lấy giá trị lớn hơn trong hai lựa chọn. Cụ thể:

$$
dfs(i) = \max(dfs(i + 1), dfs(j) + end_i - start_i + tip_i)
$$

Trong đó, $j$ là chỉ số nhỏ nhất thỏa mãn $start_j \ge end_i$, có thể tìm được bằng tìm kiếm nhị phân.

Trong quá trình này, ta có thể dùng tìm kiếm có ghi nhớ để lưu đáp án của mỗi trạng thái, tránh tính toán lặp lại.

Độ phức tạp thời gian là $O(m \times \log m)$, và độ phức tạp không gian là $O(m)$. Ở đây, $m$ là độ dài của $rides$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxTaxiEarnings(self, n: int, rides: List[List[int]]) -> int:
        @cache
        def dfs(i: int) -> int:
            if i >= len(rides):
                return 0
            st, ed, tip = rides[i]
            j = bisect_left(rides, ed, lo=i + 1, key=lambda x: x[0])
            return max(dfs(i + 1), dfs(j) + ed - st + tip)

        rides.sort()
        return dfs(0)
```

#### Java

```java
class Solution {
    private int m;
    private int[][] rides;
    private Long[] f;

    public long maxTaxiEarnings(int n, int[][] rides) {
        Arrays.sort(rides, (a, b) -> a[0] - b[0]);
        m = rides.length;
        f = new Long[m];
        this.rides = rides;
        return dfs(0);
    }

    private long dfs(int i) {
        if (i >= m) {
            return 0;
        }
        if (f[i] != null) {
            return f[i];
        }
        int[] r = rides[i];
        int st = r[0], ed = r[1], tip = r[2];
        int j = search(ed, i + 1);
        return f[i] = Math.max(dfs(i + 1), dfs(j) + ed - st + tip);
    }

    private int search(int x, int l) {
        int r = m;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (rides[mid][0] >= x) {
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
    long long maxTaxiEarnings(int n, vector<vector<int>>& rides) {
        sort(rides.begin(), rides.end());
        int m = rides.size();
        long long f[m];
        memset(f, -1, sizeof(f));
        function<long long(int)> dfs = [&](int i) -> long long {
            if (i >= m) {
                return 0;
            }
            if (f[i] != -1) {
                return f[i];
            }
            auto& r = rides[i];
            int st = r[0], ed = r[1], tip = r[2];
            int j = lower_bound(rides.begin() + i + 1, rides.end(), ed, [](auto& a, int val) { return a[0] < val; }) - rides.begin();
            return f[i] = max(dfs(i + 1), dfs(j) + ed - st + tip);
        };
        return dfs(0);
    }
};
```

#### Go

```go
func maxTaxiEarnings(n int, rides [][]int) int64 {
	sort.Slice(rides, func(i, j int) bool { return rides[i][0] < rides[j][0] })
	m := len(rides)
	f := make([]int64, m)
	var dfs func(int) int64
	dfs = func(i int) int64 {
		if i >= m {
			return 0
		}
		if f[i] == 0 {
			st, ed, tip := rides[i][0], rides[i][1], rides[i][2]
			j := sort.Search(m, func(j int) bool { return rides[j][0] >= ed })
			f[i] = max(dfs(i+1), int64(ed-st+tip)+dfs(j))
		}
		return f[i]
	}
	return dfs(0)
}
```

#### TypeScript

```ts
function maxTaxiEarnings(n: number, rides: number[][]): number {
    rides.sort((a, b) => a[0] - b[0]);
    const m = rides.length;
    const f: number[] = Array(m).fill(-1);
    const search = (x: number, l: number): number => {
        let r = m;
        while (l < r) {
            const mid = (l + r) >> 1;
            if (rides[mid][0] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    };
    const dfs = (i: number): number => {
        if (i >= m) {
            return 0;
        }
        if (f[i] === -1) {
            const [st, ed, tip] = rides[i];
            const j = search(ed, i + 1);
            f[i] = Math.max(dfs(i + 1), dfs(j) + ed - st + tip);
        }
        return f[i];
    };
    return dfs(0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 đã có độ phức tạp $O(m \log m)$, nhưng đệ quy và `@cache` vẫn tạo thêm overhead. Sắp xếp theo điểm kết thúc cho phép $f[i]$ là lợi nhuận tốt nhất trong $i$ chuyến đi đầu tiên, không gây ảnh hưởng về sau.
>
> Bỏ qua là $f[i-1]$; chọn chuyến đi thì dùng tìm kiếm nhị phân để tìm điểm kết thúc cuối cùng $\le start_i$. Tính $f$ theo cách lặp giúp loại bỏ call stack.

<!-- thinking:end -->

Ta có thể thay thế tìm kiếm có ghi nhớ trong Lời giải 1 bằng quy hoạch động.

Trước tiên, sắp xếp $rides$, lần này theo thứ tự tăng dần của $end$. Sau đó, định nghĩa $f[i]$ là số tiền tip tối đa có thể nhận được từ $i$ hành khách đầu tiên. Ban đầu, $f[0] = 0$, và đáp án là $f[m]$.

Với hành khách thứ $i$, ta có thể chọn nhận hoặc không nhận chuyến đi. Nếu không nhận, số tiền tip tối đa có thể nhận được là $f[i-1]$. Nếu nhận, ta có thể dùng tìm kiếm nhị phân để tìm hành khách cuối cùng có điểm trả không lớn hơn $start_i$ trước khi đón hành khách thứ $i$, gọi là $j$. Số tiền tip tối đa có thể nhận được là $f[j] + end_i - start_i + tip_i$. Ta lấy giá trị lớn hơn trong hai lựa chọn. Cụ thể:

$$
f[i] = \max(f[i - 1], f[j] + end_i - start_i + tip_i)
$$

Trong đó, $j$ là chỉ số lớn nhất thỏa mãn $end_j \le start_i$, có thể tìm được bằng tìm kiếm nhị phân.

Độ phức tạp thời gian là $O(m \times \log m)$, và độ phức tạp không gian là $O(m)$. Ở đây, $m$ là độ dài của $rides$.

Các bài tương tự:

- [1235. Maximum Profit in Job Scheduling](https://github.com/doocs/leetcode/blob/main/solution/1200-1299/1235.Maximum%20Profit%20in%20Job%20Scheduling/README_EN.md)
- [1751. Maximum Number of Events That Can Be Attended II](https://github.com/doocs/leetcode/blob/main/solution/1700-1799/1751.Maximum%20Number%20of%20Events%20That%20Can%20Be%20Attended%20II/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxTaxiEarnings(self, n: int, rides: List[List[int]]) -> int:
        rides.sort(key=lambda x: x[1])
        f = [0] * (len(rides) + 1)
        for i, (st, ed, tip) in enumerate(rides, 1):
            j = bisect_left(rides, st + 1, hi=i, key=lambda x: x[1])
            f[i] = max(f[i - 1], f[j] + ed - st + tip)
        return f[-1]
```

#### Java

```java
class Solution {
    public long maxTaxiEarnings(int n, int[][] rides) {
        Arrays.sort(rides, (a, b) -> a[1] - b[1]);
        int m = rides.length;
        long[] f = new long[m + 1];
        for (int i = 1; i <= m; ++i) {
            int[] r = rides[i - 1];
            int st = r[0], ed = r[1], tip = r[2];
            int j = search(rides, st + 1, i);
            f[i] = Math.max(f[i - 1], f[j] + ed - st + tip);
        }
        return f[m];
    }

    private int search(int[][] nums, int x, int r) {
        int l = 0;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (nums[mid][1] >= x) {
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
    long long maxTaxiEarnings(int n, vector<vector<int>>& rides) {
        sort(rides.begin(), rides.end(), [](const vector<int>& a, const vector<int>& b) { return a[1] < b[1]; });
        int m = rides.size();
        vector<long long> f(m + 1);
        for (int i = 1; i <= m; ++i) {
            auto& r = rides[i - 1];
            int st = r[0], ed = r[1], tip = r[2];
            auto it = lower_bound(rides.begin(), rides.begin() + i, st + 1, [](auto& a, int val) { return a[1] < val; });
            int j = distance(rides.begin(), it);
            f[i] = max(f[i - 1], f[j] + ed - st + tip);
        }
        return f.back();
    }
};
```

#### Go

```go
func maxTaxiEarnings(n int, rides [][]int) int64 {
	sort.Slice(rides, func(i, j int) bool { return rides[i][1] < rides[j][1] })
	m := len(rides)
	f := make([]int64, m+1)
	for i := 1; i <= m; i++ {
		r := rides[i-1]
		st, ed, tip := r[0], r[1], r[2]
		j := sort.Search(m, func(j int) bool { return rides[j][1] >= st+1 })
		f[i] = max(f[i-1], f[j]+int64(ed-st+tip))
	}
	return f[m]
}
```

#### TypeScript

```ts
function maxTaxiEarnings(n: number, rides: number[][]): number {
    rides.sort((a, b) => a[1] - b[1]);
    const m = rides.length;
    const f: number[] = Array(m + 1).fill(0);
    const search = (x: number, r: number): number => {
        let l = 0;
        while (l < r) {
            const mid = (l + r) >> 1;
            if (rides[mid][1] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    };
    for (let i = 1; i <= m; ++i) {
        const [st, ed, tip] = rides[i - 1];
        const j = search(st + 1, i);
        f[i] = Math.max(f[i - 1], f[j] + ed - st + tip);
    }
    return f[m];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
