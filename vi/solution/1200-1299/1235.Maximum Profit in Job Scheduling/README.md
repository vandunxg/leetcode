---
comments: true
difficulty: Hard
rating: 2022
source: Weekly Contest 159 Q4
tags:
    - Array
    - Binary Search
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [1235. Maximum Profit in Job Scheduling](https://leetcode.com/problems/maximum-profit-in-job-scheduling)

[中文文档](/solution/1200-1299/1235.Maximum%20Profit%20in%20Job%20Scheduling/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> công việc; mỗi công việc được lên lịch từ <code>startTime[i]</code> đến <code>endTime[i]</code> và mang lại lợi nhuận <code>profit[i]</code>.</p>

<p>Cho các mảng <code>startTime</code>, <code>endTime</code> và <code>profit</code>. Hãy trả về lợi nhuận tối đa có thể đạt được sao cho không có hai công việc nào trong tập được chọn bị trùng thời gian.</p>

<p>Nếu chọn công việc kết thúc tại thời điểm <code>X</code>, bạn có thể bắt đầu một công việc khác cũng bắt đầu tại thời điểm <code>X</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1235.Maximum%20Profit%20in%20Job%20Scheduling/images/sample1_1584.png" style="width: 380px; height: 154px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> startTime = [1,2,3,3], endTime = [3,4,5,6], profit = [50,10,40,70]
<strong>Đầu ra:</strong> 120
<strong>Giải thích:</strong> Tập công việc được chọn gồm công việc thứ nhất và thứ tư. 
Các khoảng thời gian [1-3]+[3-6], lợi nhuận thu được là 120 = 50 + 70.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1235.Maximum%20Profit%20in%20Job%20Scheduling/images/sample22_1584.png" style="width: 600px; height: 112px;" /> </strong></p>

<pre>
<strong>Đầu vào:</strong> startTime = [1,2,3,4,6], endTime = [3,5,10,6,9], profit = [20,20,100,70,60]
<strong>Đầu ra:</strong> 150
<strong>Giải thích:</strong> Tập công việc được chọn gồm công việc thứ nhất, thứ tư và thứ năm. 
Lợi nhuận thu được là 150 = 20 + 70 + 60.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1235.Maximum%20Profit%20in%20Job%20Scheduling/images/sample3_1584.png" style="width: 400px; height: 112px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> startTime = [1,1,1], endTime = [2,3,4], profit = [5,6,4]
<strong>Đầu ra:</strong> 6
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= startTime.length == endTime.length == profit.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= startTime[i] &lt; endTime[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= profit[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ + tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Các công việc không được trùng thời gian. Vì $n \le 5\times 10^4$, không thể thử mọi tập con. Việc chọn công việc $i$ chỉ ảnh hưởng đến những công việc bắt đầu không sớm hơn thời điểm nó kết thúc.
>
> Sau khi sắp xếp theo thời điểm bắt đầu, $dfs(i)$ chọn phương án tốt hơn giữa bỏ qua công việc $i$ và nhận công việc $i$ rồi chuyển đến công việc đầu tiên có $start\ge end_i$. Ta tìm chỉ số này bằng tìm kiếm nhị phân trên danh sách thời điểm bắt đầu đã sắp xếp. Memoization chỉ tính mỗi $i$ một lần.

<!-- thinking:end -->

Trước tiên, sắp xếp công việc theo thời điểm bắt đầu tăng dần, rồi định nghĩa hàm $dfs(i)$ là lợi nhuận tối đa có thể đạt được khi xét từ công việc thứ $i$. Đáp án là $dfs(0)$.

Cách tính hàm $dfs(i)$ như sau:

Với công việc thứ $i$, ta có thể chọn làm hoặc bỏ qua. Nếu bỏ qua, lợi nhuận tối đa là $dfs(i + 1)$. Nếu chọn làm, dùng tìm kiếm nhị phân để tìm công việc đầu tiên bắt đầu tại hoặc sau thời điểm kết thúc của công việc thứ $i$, gọi chỉ số đó là $j$; khi ấy lợi nhuận tối đa là $profit[i] + dfs(j)$. Ta lấy giá trị lớn hơn trong hai phương án. Tức là:

$$
dfs(i)=\max(dfs(i+1),profit[i]+dfs(j))
$$

Trong đó, $j$ là chỉ số nhỏ nhất thỏa mãn $startTime[j] \ge endTime[i]$.

Trong quá trình này, ta dùng memoization để lưu kết quả của từng state, tránh tính toán lặp lại.

Độ phức tạp thời gian là $O(n \times \log n)$, trong đó $n$ là số công việc.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def jobScheduling(
        self, startTime: List[int], endTime: List[int], profit: List[int]
    ) -> int:
        @cache
        def dfs(i):
            if i >= n:
                return 0
            _, e, p = jobs[i]
            j = bisect_left(jobs, e, lo=i + 1, key=lambda x: x[0])
            return max(dfs(i + 1), p + dfs(j))

        jobs = sorted(zip(startTime, endTime, profit))
        n = len(profit)
        return dfs(0)
```

#### Java

```java
class Solution {
    private int[][] jobs;
    private int[] f;
    private int n;

    public int jobScheduling(int[] startTime, int[] endTime, int[] profit) {
        n = profit.length;
        jobs = new int[n][3];
        for (int i = 0; i < n; ++i) {
            jobs[i] = new int[] {startTime[i], endTime[i], profit[i]};
        }
        Arrays.sort(jobs, (a, b) -> a[0] - b[0]);
        f = new int[n];
        return dfs(0);
    }

    private int dfs(int i) {
        if (i >= n) {
            return 0;
        }
        if (f[i] != 0) {
            return f[i];
        }
        int e = jobs[i][1], p = jobs[i][2];
        int j = search(jobs, e, i + 1);
        int ans = Math.max(dfs(i + 1), p + dfs(j));
        f[i] = ans;
        return ans;
    }

    private int search(int[][] jobs, int x, int i) {
        int left = i, right = n;
        while (left < right) {
            int mid = (left + right) >> 1;
            if (jobs[mid][0] >= x) {
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
    int jobScheduling(vector<int>& startTime, vector<int>& endTime, vector<int>& profit) {
        int n = profit.size();
        vector<tuple<int, int, int>> jobs(n);
        for (int i = 0; i < n; ++i) jobs[i] = {startTime[i], endTime[i], profit[i]};
        sort(jobs.begin(), jobs.end());
        vector<int> f(n);
        function<int(int)> dfs = [&](int i) -> int {
            if (i >= n) return 0;
            if (f[i]) return f[i];
            auto [_, e, p] = jobs[i];
            tuple<int, int, int> t{e, 0, 0};
            int j = lower_bound(jobs.begin() + i + 1, jobs.end(), t, [&](auto& l, auto& r) -> bool { return get<0>(l) < get<0>(r); }) - jobs.begin();
            int ans = max(dfs(i + 1), p + dfs(j));
            f[i] = ans;
            return ans;
        };
        return dfs(0);
    }
};
```

#### Go

```go
func jobScheduling(startTime []int, endTime []int, profit []int) int {
	n := len(profit)
	type tuple struct{ s, e, p int }
	jobs := make([]tuple, n)
	for i, p := range profit {
		jobs[i] = tuple{startTime[i], endTime[i], p}
	}
	sort.Slice(jobs, func(i, j int) bool { return jobs[i].s < jobs[j].s })
	f := make([]int, n)
	var dfs func(int) int
	dfs = func(i int) int {
		if i >= n {
			return 0
		}
		if f[i] != 0 {
			return f[i]
		}
		j := sort.Search(n, func(j int) bool { return jobs[j].s >= jobs[i].e })
		ans := max(dfs(i+1), jobs[i].p+dfs(j))
		f[i] = ans
		return ans
	}
	return dfs(0)
}
```

#### TypeScript

```ts
function jobScheduling(startTime: number[], endTime: number[], profit: number[]): number {
    const n = startTime.length;
    const f = new Array(n).fill(0);
    const idx = new Array(n).fill(0).map((_, i) => i);
    idx.sort((i, j) => startTime[i] - startTime[j]);
    const search = (x: number) => {
        let l = 0;
        let r = n;
        while (l < r) {
            const mid = (l + r) >> 1;
            if (startTime[idx[mid]] >= x) {
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
        if (f[i] !== 0) {
            return f[i];
        }
        const j = search(endTime[idx[i]]);
        return (f[i] = Math.max(dfs(i + 1), dfs(j) + profit[idx[i]]));
    };
    return dfs(0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động + tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Memoization chuyển tiếp theo thời điểm bắt đầu. Thay vào đó, nếu sắp xếp theo thời điểm kết thúc thì $dp[i]$ là lợi nhuận tốt nhất trong $i$ công việc đầu tiên: bỏ qua công việc hiện tại thì giữ $dp[i-1]$; chọn nó thì cộng lợi nhuận với $dp[j]$, trong đó $j$ là công việc cuối cùng kết thúc trước thời điểm bắt đầu này và vẫn được tìm bằng tìm kiếm nhị phân. Cách tính bottom-up loại bỏ đệ quy nhưng giữ cùng ý nghĩa với lời giải 1.

<!-- thinking:end -->

Ta cũng có thể chuyển lời giải dùng memoization ở lời giải 1 thành quy hoạch động.

Trước tiên, sắp xếp công việc theo thời điểm kết thúc tăng dần, rồi định nghĩa $dp[i]$ là lợi nhuận tối đa có thể đạt được từ $i$ công việc đầu tiên. Đáp án là $dp[n]$. Khởi tạo $dp[0]=0$.

Với công việc thứ $i$, ta có thể chọn làm hoặc bỏ qua. Nếu bỏ qua, lợi nhuận tối đa vẫn là $dp[i]$. Nếu chọn làm, dùng tìm kiếm nhị phân để tìm công việc cuối cùng kết thúc không muộn hơn thời điểm bắt đầu của công việc thứ $i$, gọi chỉ số đó là $j$; khi ấy lợi nhuận tối đa là $profit[i] + dp[j]$. Ta lấy giá trị lớn hơn trong hai phương án. Tức là:

$$
dp[i+1] = \max(dp[i], profit[i] + dp[j])
$$

Trong đó, $j$ là chỉ số lớn nhất thỏa mãn $endTime[j] \leq startTime[i]$.

Độ phức tạp thời gian là $O(n \times \log n)$, trong đó $n$ là số công việc.

Bài toán tương tự:

- [2008. Maximum Earnings From Taxi](https://github.com/doocs/leetcode/blob/main/solution/2000-2099/2008.Maximum%20Earnings%20From%20Taxi/README.md)
- [1751. Maximum Number of Events That Can Be Attended II](https://github.com/doocs/leetcode/blob/main/solution/1700-1799/1751.Maximum%20Number%20of%20Events%20That%20Can%20Be%20Attended%20II/README.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def jobScheduling(
        self, startTime: List[int], endTime: List[int], profit: List[int]
    ) -> int:
        jobs = sorted(zip(endTime, startTime, profit))
        n = len(profit)
        dp = [0] * (n + 1)
        for i, (_, s, p) in enumerate(jobs):
            j = bisect_right(jobs, s, hi=i, key=lambda x: x[0])
            dp[i + 1] = max(dp[i], dp[j] + p)
        return dp[n]
```

#### Java

```java
class Solution {
    public int jobScheduling(int[] startTime, int[] endTime, int[] profit) {
        int n = profit.length;
        int[][] jobs = new int[n][3];
        for (int i = 0; i < n; ++i) {
            jobs[i] = new int[] {startTime[i], endTime[i], profit[i]};
        }
        Arrays.sort(jobs, (a, b) -> a[1] - b[1]);
        int[] dp = new int[n + 1];
        for (int i = 0; i < n; ++i) {
            int j = search(jobs, jobs[i][0], i);
            dp[i + 1] = Math.max(dp[i], dp[j] + jobs[i][2]);
        }
        return dp[n];
    }

    private int search(int[][] jobs, int x, int n) {
        int left = 0, right = n;
        while (left < right) {
            int mid = (left + right) >> 1;
            if (jobs[mid][1] > x) {
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
    int jobScheduling(vector<int>& startTime, vector<int>& endTime, vector<int>& profit) {
        int n = profit.size();
        vector<tuple<int, int, int>> jobs(n);
        for (int i = 0; i < n; ++i) jobs[i] = {endTime[i], startTime[i], profit[i]};
        sort(jobs.begin(), jobs.end());
        vector<int> dp(n + 1);
        for (int i = 0; i < n; ++i) {
            auto [_, s, p] = jobs[i];
            int j = upper_bound(jobs.begin(), jobs.begin() + i, s, [&](int x, auto& job) -> bool { return x < get<0>(job); }) - jobs.begin();
            dp[i + 1] = max(dp[i], dp[j] + p);
        }
        return dp[n];
    }
};
```

#### Go

```go
func jobScheduling(startTime []int, endTime []int, profit []int) int {
	n := len(profit)
	type tuple struct{ s, e, p int }
	jobs := make([]tuple, n)
	for i, p := range profit {
		jobs[i] = tuple{startTime[i], endTime[i], p}
	}
	sort.Slice(jobs, func(i, j int) bool { return jobs[i].e < jobs[j].e })
	dp := make([]int, n+1)
	for i, job := range jobs {
		j := sort.Search(i, func(k int) bool { return jobs[k].e > job.s })
		dp[i+1] = max(dp[i], dp[j]+job.p)
	}
	return dp[n]
}
```

#### TypeScript

```ts
function jobScheduling(startTime: number[], endTime: number[], profit: number[]): number {
    const n = profit.length;
    const jobs: [number, number, number][] = Array.from({ length: n }, (_, i) => [
        startTime[i],
        endTime[i],
        profit[i],
    ]);
    jobs.sort((a, b) => a[1] - b[1]);
    const dp: number[] = Array.from({ length: n + 1 }, () => 0);
    const search = (x: number, right: number): number => {
        let left = 0;
        while (left < right) {
            const mid = (left + right) >> 1;
            if (jobs[mid][1] > x) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    };
    for (let i = 0; i < n; ++i) {
        const j = search(jobs[i][0], i);
        dp[i + 1] = Math.max(dp[i], dp[j] + jobs[i][2]);
    }
    return dp[n];
}
```

#### Swift

```swift
class Solution {

    func binarySearch<T: Comparable>(inputArr: [T], searchItem: T) -> Int? {
        var lowerIndex = 0
        var upperIndex = inputArr.count - 1

        while lowerIndex < upperIndex {
            let currentIndex = (lowerIndex + upperIndex) / 2
            if inputArr[currentIndex] <= searchItem {
                lowerIndex = currentIndex + 1
            } else {
                upperIndex = currentIndex
            }
        }

        if inputArr[upperIndex] <= searchItem {
            return upperIndex + 1
        }
        return lowerIndex
    }

    func jobScheduling(_ startTime: [Int], _ endTime: [Int], _ profit: [Int]) -> Int {
        let zipList = zip(zip(startTime, endTime), profit)
        var table: [(startTime: Int, endTime: Int, profit: Int, cumsum: Int)] = []

        for ((x, y), z) in zipList {
            table.append((x, y, z, 0))
        }
        table.sort(by: { $0.endTime < $1.endTime })
        let sortedEndTime = endTime.sorted()

        var profits: [Int] = [0]
        for iJob in table {
            let index: Int! = binarySearch(inputArr: sortedEndTime, searchItem: iJob.startTime)
            if profits.last! < profits[index] + iJob.profit {
                profits.append(profits[index] + iJob.profit)
            } else {
                profits.append(profits.last!)
            }
        }
        return profits.last!
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
