---
comments: true
difficulty: Medium
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [983. Minimum Cost For Tickets](https://leetcode.com/problems/minimum-cost-for-tickets)

[中文文档](/solution/0900-0999/0983.Minimum%20Cost%20For%20Tickets/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn đã lên kế hoạch đi tàu trước một năm. Các ngày trong năm mà bạn sẽ đi được lưu trong mảng số nguyên <code>days</code>. Mỗi ngày là số nguyên từ <code>1</code> đến <code>365</code>.</p>

<p>Vé tàu được bán theo <strong>ba loại</strong>:</p>

<ul>
	<li>vé <strong>1 ngày</strong> có giá <code>costs[0]</code> đô la,</li>
	<li>vé <strong>7 ngày</strong> có giá <code>costs[1]</code> đô la,</li>
	<li>vé <strong>30 ngày</strong> có giá <code>costs[2]</code> đô la.</li>
</ul>

<p>Mỗi vé cho phép đi tàu trong số ngày liên tiếp tương ứng.</p>

<ul>
	<li>Ví dụ, nếu mua vé <strong>7 ngày</strong> vào ngày <code>2</code>, ta có thể đi tàu trong <code>7</code> ngày: <code>2</code>, <code>3</code>, <code>4</code>, <code>5</code>, <code>6</code>, <code>7</code> và <code>8</code>.</li>
</ul>

<p>Hãy trả về <em>số tiền tối thiểu cần trả để đi tàu vào tất cả các ngày trong danh sách đã cho</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> days = [1,4,6,7,8,20], costs = [2,7,15]
<strong>Output:</strong> 11
<strong>Giải thích:</strong> Ví dụ, có thể mua vé theo cách sau để đi được vào tất cả các ngày đã lên kế hoạch:
Vào ngày 1, bạn mua vé 1 ngày với giá costs[0] = $2, áp dụng cho ngày 1.
Vào ngày 3, bạn mua vé 7 ngày với giá costs[1] = $7, áp dụng cho các ngày 3, 4, ..., 9.
Vào ngày 20, bạn mua vé 1 ngày với giá costs[0] = $2, áp dụng cho ngày 20.
Tổng cộng, bạn trả $11 và đi được vào tất cả các ngày trong kế hoạch.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> days = [1,2,3,4,5,6,7,8,9,10,30,31], costs = [2,7,15]
<strong>Output:</strong> 17
<strong>Giải thích:</strong> Ví dụ, có thể mua vé theo cách sau để đi được vào tất cả các ngày đã lên kế hoạch:
Vào ngày 1, bạn mua vé 30 ngày với giá costs[2] = $15, áp dụng cho các ngày 1, 2, ..., 30.
Vào ngày 31, bạn mua vé 1 ngày với giá costs[0] = $2, áp dụng cho ngày 31.
Tổng cộng, bạn trả $17 và đi được vào tất cả các ngày trong kế hoạch.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= days.length &lt;= 365</code></li>
	<li><code>1 &lt;= days[i] &lt;= 365</code></li>
	<li><code>days</code> được sắp xếp theo thứ tự tăng nghiêm ngặt.</li>
	<li><code>costs.length == 3</code></li>
	<li><code>1 &lt;= costs[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Memoization Search + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi ngày đi tàu cần được phủ bằng vé $1$, $7$ hoặc $30$ ngày với chi phí thấp nhất. Sau khi mua vé cho chuyến thứ $i$, ta dùng tìm kiếm nhị phân để tìm chuyến tiếp theo chưa được vé phủ. $dfs(i)$ chọn phương án tốt nhất trong ba loại vé. Có nhiều nhất $365$ chuyến nên memoization là đủ.

<!-- thinking:end -->

Ta định nghĩa hàm $\textit{dfs(i)}$ là chi phí tối thiểu cần thiết từ chuyến thứ $i$ đến chuyến cuối cùng. Vì vậy, đáp án là $\textit{dfs(0)}$.

Hàm $\textit{dfs(i)}$ hoạt động như sau:

- Nếu $i \geq n$, nghĩa là đã hết các chuyến, trả về $0$;
- Ngược lại, ta xét ba cách mua vé: vé 1 ngày, vé 7 ngày và vé 30 ngày. Với mỗi cách, tính chi phí tương ứng rồi dùng tìm kiếm nhị phân để tìm chỉ số $j$ của chuyến tiếp theo, gọi đệ quy $\textit{dfs(j)}$, sau đó trả về chi phí nhỏ nhất trong ba phương án.

Để tránh tính toán lặp lại, ta dùng memoization để lưu các kết quả đã tính.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số chuyến đi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def mincostTickets(self, days: List[int], costs: List[int]) -> int:
        @cache
        def dfs(i: int) -> int:
            if i >= n:
                return 0
            ans = inf
            for c, v in zip(costs, valid):
                j = bisect_left(days, days[i] + v)
                ans = min(ans, c + dfs(j))
            return ans

        n = len(days)
        valid = [1, 7, 30]
        return dfs(0)
```

#### Java

```java
class Solution {
    private final int[] valid = {1, 7, 30};
    private int[] days;
    private int[] costs;
    private Integer[] f;
    private int n;

    public int mincostTickets(int[] days, int[] costs) {
        n = days.length;
        f = new Integer[n];
        this.days = days;
        this.costs = costs;
        return dfs(0);
    }

    private int dfs(int i) {
        if (i >= n) {
            return 0;
        }
        if (f[i] != null) {
            return f[i];
        }
        f[i] = Integer.MAX_VALUE;
        for (int k = 0; k < 3; ++k) {
            int j = Arrays.binarySearch(days, days[i] + valid[k]);
            j = j < 0 ? -j - 1 : j;
            f[i] = Math.min(f[i], dfs(j) + costs[k]);
        }
        return f[i];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int mincostTickets(vector<int>& days, vector<int>& costs) {
        int valid[3] = {1, 7, 30};
        int n = days.size();
        int f[n];
        memset(f, 0, sizeof(f));
        function<int(int)> dfs = [&](int i) {
            if (i >= n) {
                return 0;
            }
            if (f[i]) {
                return f[i];
            }
            f[i] = INT_MAX;
            for (int k = 0; k < 3; ++k) {
                int j = lower_bound(days.begin(), days.end(), days[i] + valid[k]) - days.begin();
                f[i] = min(f[i], dfs(j) + costs[k]);
            }
            return f[i];
        };
        return dfs(0);
    }
};
```

#### Go

```go
func mincostTickets(days []int, costs []int) int {
	valid := [3]int{1, 7, 30}
	n := len(days)
	f := make([]int, n)
	var dfs func(int) int
	dfs = func(i int) int {
		if i >= n {
			return 0
		}
		if f[i] > 0 {
			return f[i]
		}
		f[i] = 1 << 30
		for k := 0; k < 3; k++ {
			j := sort.SearchInts(days, days[i]+valid[k])
			f[i] = min(f[i], dfs(j)+costs[k])
		}
		return f[i]
	}
	return dfs(0)
}
```

#### TypeScript

```ts
function mincostTickets(days: number[], costs: number[]): number {
    const n = days.length;
    const f: number[] = Array(n).fill(0);
    const valid: number[] = [1, 7, 30];
    const search = (x: number): number => {
        let [l, r] = [0, n];
        while (l < r) {
            const mid = (l + r) >> 1;
            if (days[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    };
    const dfs = (i: number): number => {
        if (i >= n) {
            return 0;
        }
        if (f[i]) {
            return f[i];
        }
        f[i] = Infinity;
        for (let k = 0; k < 3; ++k) {
            const j = search(days[i] + valid[k]);
            f[i] = Math.min(f[i], dfs(j) + costs[k]);
        }
        return f[i];
    };
    return dfs(0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Tìm theo chỉ số chuyến cần dùng tìm kiếm nhị phân. Thay vào đó, ta làm DP theo ngày trong năm: $f[i]$ là chi phí thấp nhất để hoàn thành đến ngày $i$. Nếu ngày đó không đi tàu, lấy $f[i-1]$; nếu có đi, chọn chi phí nhỏ nhất trong ba phương án mua vé. Ngày cuối cùng không vượt quá $365$.

<!-- thinking:end -->

Gọi ngày cuối cùng trong mảng $\textit{days}$ là $m$. Ta định nghĩa mảng $f$ có độ dài $m + 1$, trong đó $f[i]$ là chi phí tối thiểu từ ngày $1$ đến ngày $i$.

Ta tính $f[i]$ theo thứ tự ngày tăng dần, bắt đầu từ ngày $1$. Nếu ngày $i$ là ngày đi tàu, ta xét ba phương án mua vé: vé 1 ngày, vé 7 ngày và vé 30 ngày. Tính chi phí cho từng phương án rồi lấy giá trị nhỏ nhất làm $f[i]$. Nếu ngày $i$ không đi tàu thì $f[i] = f[i - 1]$.

Đáp án cuối cùng là $f[m]$.

Độ phức tạp thời gian là $O(m)$ và độ phức tạp không gian là $O(m)$, trong đó $m$ là ngày đi tàu cuối cùng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def mincostTickets(self, days: List[int], costs: List[int]) -> int:
        m = days[-1]
        f = [0] * (m + 1)
        valid = [1, 7, 30]
        j = 0
        for i in range(1, m + 1):
            if i == days[j]:
                f[i] = inf
                for c, v in zip(costs, valid):
                    f[i] = min(f[i], f[max(0, i - v)] + c)
                j += 1
            else:
                f[i] = f[i - 1]
        return f[m]
```

#### Java

```java
class Solution {
    public int mincostTickets(int[] days, int[] costs) {
        int m = days[days.length - 1];
        int[] f = new int[m + 1];
        final int[] valid = {1, 7, 30};
        for (int i = 1, j = 0; i <= m; ++i) {
            if (i == days[j]) {
                f[i] = Integer.MAX_VALUE;
                for (int k = 0; k < 3; ++k) {
                    int c = costs[k], v = valid[k];
                    f[i] = Math.min(f[i], f[Math.max(0, i - v)] + c);
                }
                ++j;
            } else {
                f[i] = f[i - 1];
            }
        }
        return f[m];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int mincostTickets(vector<int>& days, vector<int>& costs) {
        int m = days.back();
        int f[m + 1];
        f[0] = 0;
        int valid[3] = {1, 7, 30};
        for (int i = 1, j = 0; i <= m; ++i) {
            if (i == days[j]) {
                f[i] = INT_MAX;
                for (int k = 0; k < 3; ++k) {
                    int c = costs[k], v = valid[k];
                    f[i] = min(f[i], f[max(0, i - v)] + c);
                }
                ++j;
            } else {
                f[i] = f[i - 1];
            }
        }
        return f[m];
    }
};
```

#### Go

```go
func mincostTickets(days []int, costs []int) int {
	m := days[len(days)-1]
	f := make([]int, m+1)
	valid := [3]int{1, 7, 30}
	for i, j := 1, 0; i <= m; i++ {
		if i == days[j] {
			f[i] = 1 << 30
			for k, v := range valid {
				c := costs[k]
				f[i] = min(f[i], f[max(0, i-v)]+c)
			}
			j++
		} else {
			f[i] = f[i-1]
		}
	}
	return f[m]
}
```

#### TypeScript

```ts
function mincostTickets(days: number[], costs: number[]): number {
    const m = days.at(-1)!;
    const f: number[] = Array(m).fill(0);
    const valid: number[] = [1, 7, 30];
    for (let i = 1, j = 0; i <= m; ++i) {
        if (i === days[j]) {
            f[i] = Infinity;
            for (let k = 0; k < 3; ++k) {
                const [c, v] = [costs[k], valid[k]];
                f[i] = Math.min(f[i], f[Math.max(0, i - v)] + c);
            }
            ++j;
        } else {
            f[i] = f[i - 1];
        }
    }
    return f[m];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
