---
comments: true
difficulty: Easy
rating: 1198
source: Weekly Contest 467 Q1
---

<!-- problem:start -->

# [3683. Earliest Time to Finish One Task](https://leetcode.com/problems/earliest-time-to-finish-one-task)

[中文文档](/solution/3600-3699/3683.Earliest%20Time%20to%20Finish%20One%20Task/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên 2 chiều <code>tasks</code> trong đó <code>tasks[i] = [s<sub>i</sub>, t<sub>i</sub>]</code>.</p>

<p>Mỗi <code>[s<sub>i</sub>, t<sub>i</sub>]</code> trong <code>tasks</code> biểu diễn một task có thời điểm bắt đầu là <code>s<sub>i</sub></code> và cần <code>t<sub>i</sub></code> đơn vị thời gian để hoàn thành.</p>

<p>Hãy trả về thời điểm sớm nhất mà ít nhất một task hoàn thành.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">tasks = [[1,6],[2,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Task đầu tiên bắt đầu tại thời điểm <code>t = 1</code> và hoàn thành tại thời điểm <code>1 + 6 = 7</code>. Task thứ hai hoàn thành tại thời điểm <code>2 + 3 = 5</code>. Có thể hoàn thành một task tại thời điểm 5.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">tasks = [[100,100],[100,100],[100,100]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">200</span></p>

<p><strong>Giải thích:</strong></p>

<p>Cả ba task đều hoàn thành tại thời điểm <code>100 + 100 = 200</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= tasks.length &lt;= 100</code></li>
	<li><code>tasks[i] = [s<sub>i</sub>, t<sub>i</sub>]</code></li>
	<li><code>1 &lt;= s<sub>i</sub>, t<sub>i</sub> &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lần

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ cần hoàn thành một task; thời điểm hoàn thành task đó là $s_i+t_i$. Các task độc lập với nhau, nên đáp án là giá trị nhỏ nhất.
>
> $n\le 100$ nên chỉ cần duyệt một lần. Không có vấn đề về thứ tự hay chạy song song.

<!-- thinking:end -->

Ta duyệt qua mảng $\textit{tasks}$ và tính thời điểm hoàn thành $s_i + t_i$ cho từng task. Giá trị nhỏ nhất trong tất cả thời điểm hoàn thành của các task là thời điểm sớm nhất để hoàn thành ít nhất một task.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{tasks}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def earliestTime(self, tasks: List[List[int]]) -> int:
        return min(s + t for s, t in tasks)
```

#### Java

```java
class Solution {
    public int earliestTime(int[][] tasks) {
        int ans = 200;
        for (var task : tasks) {
            ans = Math.min(ans, task[0] + task[1]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int earliestTime(vector<vector<int>>& tasks) {
        int ans = 200;
        for (const auto& task : tasks) {
            ans = min(ans, task[0] + task[1]);
        }
        return ans;
    }
};
```

#### Go

```go
func earliestTime(tasks [][]int) int {
	ans := 200
	for _, task := range tasks {
		ans = min(ans, task[0]+task[1])
	}
	return ans
}
```

#### TypeScript

```ts
function earliestTime(tasks: number[][]): number {
    return Math.min(...tasks.map(task => task[0] + task[1]));
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
