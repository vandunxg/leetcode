---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [2323. Find Minimum Time to Finish All Jobs II 🔒](https://leetcode.com/problems/find-minimum-time-to-finish-all-jobs-ii)

[中文文档](/solution/2300-2399/2323.Find%20Minimum%20Time%20to%20Finish%20All%20Jobs%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>jobs</code> và <code>workers</code> có <strong>độ dài bằng nhau</strong>, trong đó <code>jobs[i]</code> là thời gian cần để hoàn thành công việc thứ <code>i<sup>th</sup></code>, còn <code>workers[j]</code> là số giờ mà người thợ thứ <code>j<sup>th</sup></code> có thể làm việc mỗi ngày.</p>

<p>Mỗi công việc phải được giao cho <strong>chính xác</strong> một người thợ, sao cho mỗi người thợ hoàn thành <strong>chính xác</strong> một công việc.</p>

<p>Hãy trả về <em>số ngày <strong>nhỏ nhất</strong> cần để hoàn thành tất cả công việc sau khi phân công.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> jobs = [5,2,4], workers = [1,7,5]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
- Giao người thợ thứ 2<sup>nd</sup> cho công việc thứ 0<sup>th</sup>. Người thợ này mất 1 ngày để hoàn thành công việc.
- Giao người thợ thứ 0<sup>th</sup> cho công việc thứ 1<sup>st</sup>. Người thợ này mất 2 ngày để hoàn thành công việc.
- Giao người thợ thứ 1<sup>st</sup> cho công việc thứ 2<sup>nd</sup>. Người thợ này mất 1 ngày để hoàn thành công việc.
Mất 2 ngày để hoàn thành tất cả công việc, nên trả về 2.
Có thể chứng minh rằng 2 ngày là số ngày nhỏ nhất cần thiết.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> jobs = [3,18,15,9], workers = [6,5,1,3]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
- Giao người thợ thứ 2<sup>nd</sup> cho công việc thứ 0<sup>th</sup>. Người thợ này mất 3 ngày để hoàn thành công việc.
- Giao người thợ thứ 0<sup>th</sup> cho công việc thứ 1<sup>st</sup>. Người thợ này mất 3 ngày để hoàn thành công việc.
- Giao người thợ thứ 1<sup>st</sup> cho công việc thứ 2<sup>nd</sup>. Người thợ này mất 3 ngày để hoàn thành công việc.
- Giao người thợ thứ 3<sup>rd</sup> cho công việc thứ 3<sup>rd</sup>. Người thợ này mất 3 ngày để hoàn thành công việc.
Mất 3 ngày để hoàn thành tất cả công việc, nên trả về 3.
Có thể chứng minh rằng 3 ngày là số ngày nhỏ nhất cần thiết.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == jobs.length == workers.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= jobs[i], workers[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi người thợ làm chính xác một công việc; thời gian hoàn thành là $\lceil jobs_i / workers_j \rceil$. Vì số lượng bằng nhau, điều quan trọng chỉ là cách ghép cặp.
>
> Ghép một công việc nặng với người thợ chậm sẽ làm tăng giá trị lớn nhất. Sắp xếp cả hai mảng rồi ghép các phần tử cùng chỉ số để người thợ nhanh hơn làm các công việc nặng hơn, sau đó lấy giá trị lớn nhất của các phép chia làm tròn lên.

<!-- thinking:end -->

Để giảm số ngày cần thiết nhằm hoàn thành tất cả công việc, ta có thể thử giao các công việc mất nhiều thời gian hơn cho những người thợ có thể làm việc nhiều giờ hơn.

Vì vậy, trước tiên ta sắp xếp $\textit{jobs}$ và $\textit{workers}$, sau đó giao công việc cho người thợ dựa trên chỉ số của chúng. Cuối cùng, ta tính giá trị lớn nhất của tỉ lệ thời gian công việc trên thời gian làm việc của người thợ.

Độ phức tạp thời gian là $O(n \log n)$, còn độ phức tạp không gian là $O(\log n)$. Ở đây, $n$ là số lượng công việc.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumTime(self, jobs: List[int], workers: List[int]) -> int:
        jobs.sort()
        workers.sort()
        return max((a + b - 1) // b for a, b in zip(jobs, workers))
```

#### Java

```java
class Solution {
    public int minimumTime(int[] jobs, int[] workers) {
        Arrays.sort(jobs);
        Arrays.sort(workers);
        int ans = 0;
        for (int i = 0; i < jobs.length; ++i) {
            ans = Math.max(ans, (jobs[i] + workers[i] - 1) / workers[i]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumTime(vector<int>& jobs, vector<int>& workers) {
        ranges::sort(jobs);
        ranges::sort(workers);
        int ans = 0;
        int n = jobs.size();
        for (int i = 0; i < n; ++i) {
            ans = max(ans, (jobs[i] + workers[i] - 1) / workers[i]);
        }
        return ans;
    }
};
```

#### Go

```go
func minimumTime(jobs []int, workers []int) (ans int) {
	sort.Ints(jobs)
	sort.Ints(workers)
	for i, a := range jobs {
		b := workers[i]
		ans = max(ans, (a+b-1)/b)
	}
	return
}
```

#### TypeScript

```ts
function minimumTime(jobs: number[], workers: number[]): number {
    jobs.sort((a, b) => a - b);
    workers.sort((a, b) => a - b);
    let ans = 0;
    const n = jobs.length;
    for (let i = 0; i < n; ++i) {
        ans = Math.max(ans, Math.ceil(jobs[i] / workers[i]));
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn minimum_time(mut jobs: Vec<i32>, mut workers: Vec<i32>) -> i32 {
        jobs.sort();
        workers.sort();
        jobs.iter()
            .zip(workers.iter())
            .map(|(a, b)| (a + b - 1) / b)
            .max()
            .unwrap()
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} jobs
 * @param {number[]} workers
 * @return {number}
 */
var minimumTime = function (jobs, workers) {
    jobs.sort((a, b) => a - b);
    workers.sort((a, b) => a - b);
    let ans = 0;
    const n = jobs.length;
    for (let i = 0; i < n; ++i) {
        ans = Math.max(ans, Math.ceil(jobs[i] / workers[i]));
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
