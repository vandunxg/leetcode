---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Two Pointers
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [826. Most Profit Assigning Work](https://leetcode.com/problems/most-profit-assigning-work)

[中文文档](/solution/0800-0899/0826.Most%20Profit%20Assigning%20Work/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> công việc và <code>m</code> công nhân. Bạn được cho ba mảng: <code>difficulty</code>, <code>profit</code> và <code>worker</code>, trong đó:</p>

<ul>
	<li><code>difficulty[i]</code> và <code>profit[i]</code> lần lượt là độ khó và lợi nhuận của công việc thứ <code>i<sup>th</sup></code>, và</li>
	<li><code>worker[j]</code> là năng lực của công nhân thứ <code>j<sup>th</sup></code> (tức là công nhân thứ <code>j<sup>th</sup></code> chỉ có thể hoàn thành công việc có độ khó không vượt quá <code>worker[j]</code>).</li>
</ul>

<p>Mỗi công nhân được giao <strong>tối đa một công việc</strong>, nhưng một công việc có thể được <strong>hoàn thành nhiều lần</strong>.</p>

<ul>
	<li>Ví dụ, nếu ba công nhân cùng làm một công việc có lợi nhuận <code>$1</code>, tổng lợi nhuận sẽ là <code>$3</code>. Nếu công nhân không thể hoàn thành công việc nào, lợi nhuận của họ là <code>$0</code>.</li>
</ul>

<p>Hãy trả về lợi nhuận tối đa có thể đạt được sau khi phân công công nhân vào các công việc.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> difficulty = [2,4,6,8,10], profit = [10,20,30,40,50], worker = [4,5,6,7]
<strong>Đầu ra:</strong> 100
<strong>Giải thích:</strong> Các công nhân được giao những công việc có độ khó [4,4,6,6] và lần lượt nhận lợi nhuận [20,20,30,30].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> difficulty = [85,47,57], profit = [24,66,99], worker = [40,25,25]
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == difficulty.length</code></li>
	<li><code>n == profit.length</code></li>
	<li><code>m == worker.length</code></li>
	<li><code>1 &lt;= n, m &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= difficulty[i], profit[i], worker[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi công nhân chọn công việc có lợi nhuận cao nhất mà độ khó không vượt quá năng lực của họ. Duyệt mọi công việc cho từng công nhân sẽ chậm khi $n,m\le 10^4$.
>
> Sắp xếp công nhân theo năng lực và công việc theo độ khó, sau đó di chuyển một con trỏ: vì năng lực tăng dần nên ta có thể duy trì lợi nhuận tốt nhất đã gặp. Mỗi công nhân lấy giá trị lớn nhất này trong $O(1)$.

<!-- thinking:end -->

Ta có thể sắp xếp các công việc theo thứ tự năng lực tăng dần, rồi sắp xếp các công việc theo thứ tự độ khó tăng dần.

Sau đó, ta duyệt các công nhân. Với mỗi công nhân, tìm công việc có lợi nhuận cao nhất mà họ có thể hoàn thành, rồi cộng lợi nhuận đó vào đáp án.

Độ phức tạp thời gian là $O(n \times \log n + m \times \log m)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ và $m$ lần lượt là độ dài của các mảng `profit` và `worker`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxProfitAssignment(
        self, difficulty: List[int], profit: List[int], worker: List[int]
    ) -> int:
        worker.sort()
        jobs = sorted(zip(difficulty, profit))
        ans = mx = i = 0
        for w in worker:
            while i < len(jobs) and jobs[i][0] <= w:
                mx = max(mx, jobs[i][1])
                i += 1
            ans += mx
        return ans
```

#### Java

```java
class Solution {
    public int maxProfitAssignment(int[] difficulty, int[] profit, int[] worker) {
        Arrays.sort(worker);
        int n = profit.length;
        int[][] jobs = new int[n][0];
        for (int i = 0; i < n; ++i) {
            jobs[i] = new int[] {difficulty[i], profit[i]};
        }
        Arrays.sort(jobs, (a, b) -> a[0] - b[0]);
        int ans = 0, mx = 0, i = 0;
        for (int w : worker) {
            while (i < n && jobs[i][0] <= w) {
                mx = Math.max(mx, jobs[i++][1]);
            }
            ans += mx;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxProfitAssignment(vector<int>& difficulty, vector<int>& profit, vector<int>& worker) {
        sort(worker.begin(), worker.end());
        int n = profit.size();
        vector<pair<int, int>> jobs;
        for (int i = 0; i < n; ++i) {
            jobs.emplace_back(difficulty[i], profit[i]);
        }
        sort(jobs.begin(), jobs.end());
        int ans = 0, mx = 0, i = 0;
        for (int w : worker) {
            while (i < n && jobs[i].first <= w) {
                mx = max(mx, jobs[i++].second);
            }
            ans += mx;
        }
        return ans;
    }
};
```

#### Go

```go
func maxProfitAssignment(difficulty []int, profit []int, worker []int) (ans int) {
	sort.Ints(worker)
	n := len(profit)
	jobs := make([][2]int, n)
	for i, p := range profit {
		jobs[i] = [2]int{difficulty[i], p}
	}
	sort.Slice(jobs, func(i, j int) bool { return jobs[i][0] < jobs[j][0] })
	mx, i := 0, 0
	for _, w := range worker {
		for ; i < n && jobs[i][0] <= w; i++ {
			mx = max(mx, jobs[i][1])
		}
		ans += mx
	}
	return
}
```

#### TypeScript

```ts
function maxProfitAssignment(difficulty: number[], profit: number[], worker: number[]): number {
    const n = profit.length;
    worker.sort((a, b) => a - b);
    const jobs = Array.from({ length: n }, (_, i) => [difficulty[i], profit[i]]);
    jobs.sort((a, b) => a[0] - b[0]);
    let [ans, mx, i] = [0, 0, 0];
    for (const w of worker) {
        while (i < n && jobs[i][0] <= w) {
            mx = Math.max(mx, jobs[i++][1]);
        }
        ans += mx;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Cách dùng hai con trỏ cần sắp xếp cả hai mảng. Độ khó tối đa là $10^5$, nên $f[d]$ có thể lưu lợi nhuận cao nhất của công việc có độ khó đúng bằng $d$; sau đó, prefix maximum cho biết lợi nhuận cao nhất của công việc có độ khó $\le i$.
>
> Với mỗi công nhân, chỉ cần tra cứu trong bảng. Không cần sắp xếp công nhân, thuận tiện khi phạm vi độ khó không quá lớn.

<!-- thinking:end -->

Gọi $m = \max(\textit{difficulty})$ và định nghĩa mảng $f$ có độ dài $m + 1$, trong đó $f[i]$ là lợi nhuận lớn nhất trong các công việc có độ khó không vượt quá $i$; ban đầu, $f[i] = 0$.

Tiếp theo, duyệt các công việc. Với mỗi công việc $(d, p)$, nếu $d \leq m$ thì cập nhật $f[d] = \max(f[d], p)$.

Sau đó, duyệt từ $1$ đến $m$ và với mỗi $i$, cập nhật $f[i] = \max(f[i], f[i - 1])$.

Cuối cùng, duyệt các công nhân và với mỗi công nhân $w$, cộng $f[w]$ vào đáp án.

Độ phức tạp thời gian là $O(n + M)$ và độ phức tạp không gian là $O(M)$. Trong đó, $n$ là độ dài mảng `profit`, còn $M$ là giá trị lớn nhất trong mảng `difficulty`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxProfitAssignment(
        self, difficulty: List[int], profit: List[int], worker: List[int]
    ) -> int:
        m = max(difficulty)
        f = [0] * (m + 1)
        for d, p in zip(difficulty, profit):
            f[d] = max(f[d], p)
        for i in range(1, m + 1):
            f[i] = max(f[i], f[i - 1])
        return sum(f[min(w, m)] for w in worker)
```

#### Java

```java
class Solution {
    public int maxProfitAssignment(int[] difficulty, int[] profit, int[] worker) {
        int m = Arrays.stream(difficulty).max().getAsInt();
        int[] f = new int[m + 1];
        int n = profit.length;
        for (int i = 0; i < n; ++i) {
            int d = difficulty[i];
            f[d] = Math.max(f[d], profit[i]);
        }
        for (int i = 1; i <= m; ++i) {
            f[i] = Math.max(f[i], f[i - 1]);
        }
        int ans = 0;
        for (int w : worker) {
            ans += f[Math.min(w, m)];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxProfitAssignment(vector<int>& difficulty, vector<int>& profit, vector<int>& worker) {
        int m = *max_element(begin(difficulty), end(difficulty));
        int f[m + 1];
        memset(f, 0, sizeof(f));
        int n = profit.size();
        for (int i = 0; i < n; ++i) {
            int d = difficulty[i];
            f[d] = max(f[d], profit[i]);
        }
        for (int i = 1; i <= m; ++i) {
            f[i] = max(f[i], f[i - 1]);
        }
        int ans = 0;
        for (int w : worker) {
            ans += f[min(w, m)];
        }
        return ans;
    }
};
```

#### Go

```go
func maxProfitAssignment(difficulty []int, profit []int, worker []int) (ans int) {
	m := slices.Max(difficulty)
	f := make([]int, m+1)
	for i, d := range difficulty {
		f[d] = max(f[d], profit[i])
	}
	for i := 1; i <= m; i++ {
		f[i] = max(f[i], f[i-1])
	}
	for _, w := range worker {
		ans += f[min(w, m)]
	}
	return
}
```

#### TypeScript

```ts
function maxProfitAssignment(difficulty: number[], profit: number[], worker: number[]): number {
    const m = Math.max(...difficulty);
    const f = Array(m + 1).fill(0);
    const n = profit.length;
    for (let i = 0; i < n; ++i) {
        const d = difficulty[i];
        f[d] = Math.max(f[d], profit[i]);
    }
    for (let i = 1; i <= m; ++i) {
        f[i] = Math.max(f[i], f[i - 1]);
    }
    return worker.reduce((acc, w) => acc + f[Math.min(w, m)], 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
