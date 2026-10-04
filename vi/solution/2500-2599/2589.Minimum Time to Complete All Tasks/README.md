---
comments: true
difficulty: Hard
rating: 2380
source: Weekly Contest 336 Q4
tags:
    - Stack
    - Greedy
    - Array
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [2589. Minimum Time to Complete All Tasks](https://leetcode.com/problems/minimum-time-to-complete-all-tasks)

[中文文档](/solution/2500-2599/2589.Minimum%20Time%20to%20Complete%20All%20Tasks/README.md)

## Mô tả

<!-- description:start -->

<p>Có một máy tính có thể chạy vô hạn task <strong>đồng thời</strong>. Cho một mảng số nguyên 2 chiều <code>tasks</code>, trong đó <code>tasks[i] = [start<sub>i</sub>, end<sub>i</sub>, duration<sub>i</sub>]</code> cho biết task thứ <code>i<sup>th</sup></code> phải chạy tổng cộng <code>duration<sub>i</sub></code> giây (không nhất thiết liên tục) trong khoảng thời gian <strong>bao gồm cả hai đầu mút</strong> <code>[start<sub>i</sub>, end<sub>i</sub>]</code>.</p>

<p>Bạn chỉ được bật máy tính khi máy cần chạy task. Bạn cũng có thể tắt máy nếu máy đang rảnh.</p>

<p>Trả về <em>tổng thời gian nhỏ nhất mà máy tính cần được bật để hoàn thành tất cả task</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> tasks = [[2,3,1],[4,5,1],[1,5,2]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
- Task thứ nhất có thể chạy trong khoảng thời gian bao gồm cả hai đầu mút [2, 2].
- Task thứ hai có thể chạy trong khoảng thời gian bao gồm cả hai đầu mút [5, 5].
- Task thứ ba có thể chạy trong hai khoảng thời gian bao gồm cả hai đầu mút [2, 2] và [5, 5].
Tổng thời gian máy tính được bật là 2 giây.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> tasks = [[1,3,2],[2,5,3],[5,6,2]]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong>
- Task thứ nhất có thể chạy trong khoảng thời gian bao gồm cả hai đầu mút [2, 3].
- Task thứ hai có thể chạy trong hai khoảng thời gian bao gồm cả hai đầu mút [2, 3] và [5, 5].
- Task thứ ba có thể chạy trong khoảng thời gian bao gồm cả hai đầu mút [5, 6].
Tổng thời gian máy tính được bật là 4 giây.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= tasks.length &lt;= 2000</code></li>
	<li><code>tasks[i].length == 3</code></li>
	<li><code>1 &lt;= start<sub>i</sub>, end<sub>i</sub> &lt;= 2000</code></li>
	<li><code>1 &lt;= duration<sub>i</sub> &lt;= end<sub>i</sub> - start<sub>i</sub> + 1 </code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Sorting

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi task cần $\textit{duration}$ thời điểm nguyên trong đoạn đóng của nó, và máy tính chạy từng task một. Timeline chỉ dài $2000$, nhưng số lượng lần phân công thì lớn.
>
> Sắp xếp theo thời gian kết thúc và chiếm các thời điểm còn trống ở phía bên phải, vì các task sau có khả năng tái sử dụng chúng cao hơn. Trừ đi các điểm đã được chọn trong đoạn, sau đó điền phần còn thiếu từ phải sang trái.

<!-- thinking:end -->

Ta nhận thấy bài toán tương đương với việc chọn $duration$ điểm thời gian nguyên trong mỗi đoạn $[start,..,end]$, sao cho tổng số điểm thời gian nguyên được chọn là nhỏ nhất.

Vì vậy, trước tiên ta có thể sắp xếp $tasks$ theo thứ tự tăng dần của thời gian kết thúc $end$. Sau đó ta tham lam chọn các điểm. Với mỗi task, ta bắt đầu từ thời gian kết thúc $end$ và chọn các điểm muộn nhất có thể theo thứ tự từ phải sang trái. Nhờ đó, các điểm này có khả năng được các task sau tái sử dụng cao hơn.

Trong phần cài đặt, ta có thể dùng mảng $vis$ có độ dài $2010$ để ghi nhận mỗi điểm thời gian đã được chọn hay chưa. Với mỗi task, trước tiên ta đếm số điểm $cnt$ đã được chọn trong đoạn $[start,..,end]$, sau đó chọn thêm $duration - cnt$ điểm từ phải sang trái, đồng thời ghi nhận số điểm đã chọn $ans$ và cập nhật mảng $vis$.

Cuối cùng, ta trả về $ans$.

Độ phức tạp thời gian là $O(n \times \log n + n \times m)$, và độ phức tạp không gian là $O(m)$. Ở đây, $n$ và $m$ lần lượt là độ dài của $tasks$ và mảng $vis$. Trong bài toán này, $m = 2010$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMinimumTime(self, tasks: List[List[int]]) -> int:
        tasks.sort(key=lambda x: x[1])
        vis = [0] * 2010
        ans = 0
        for start, end, duration in tasks:
            duration -= sum(vis[start : end + 1])
            i = end
            while i >= start and duration > 0:
                if not vis[i]:
                    duration -= 1
                    vis[i] = 1
                    ans += 1
                i -= 1
        return ans
```

#### Java

```java
class Solution {
    public int findMinimumTime(int[][] tasks) {
        Arrays.sort(tasks, (a, b) -> a[1] - b[1]);
        int[] vis = new int[2010];
        int ans = 0;
        for (var task : tasks) {
            int start = task[0], end = task[1], duration = task[2];
            for (int i = start; i <= end; ++i) {
                duration -= vis[i];
            }
            for (int i = end; i >= start && duration > 0; --i) {
                if (vis[i] == 0) {
                    --duration;
                    ans += vis[i] = 1;
                }
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findMinimumTime(vector<vector<int>>& tasks) {
        sort(tasks.begin(), tasks.end(), [&](auto& a, auto& b) { return a[1] < b[1]; });
        bitset<2010> vis;
        int ans = 0;
        for (auto& task : tasks) {
            int start = task[0], end = task[1], duration = task[2];
            for (int i = start; i <= end; ++i) {
                duration -= vis[i];
            }
            for (int i = end; i >= start && duration > 0; --i) {
                if (!vis[i]) {
                    --duration;
                    ans += vis[i] = 1;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findMinimumTime(tasks [][]int) (ans int) {
	sort.Slice(tasks, func(i, j int) bool { return tasks[i][1] < tasks[j][1] })
	vis := [2010]int{}
	for _, task := range tasks {
		start, end, duration := task[0], task[1], task[2]
		for _, x := range vis[start : end+1] {
			duration -= x
		}
		for i := end; i >= start && duration > 0; i-- {
			if vis[i] == 0 {
				vis[i] = 1
				duration--
				ans++
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function findMinimumTime(tasks: number[][]): number {
    tasks.sort((a, b) => a[1] - b[1]);
    const vis: number[] = Array(2010).fill(0);
    let ans = 0;
    for (let [start, end, duration] of tasks) {
        for (let i = start; i <= end; ++i) {
            duration -= vis[i];
        }
        for (let i = end; i >= start && duration > 0; --i) {
            if (vis[i] === 0) {
                --duration;
                ans += vis[i] = 1;
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_minimum_time(tasks: Vec<Vec<i32>>) -> i32 {
        let mut tasks = tasks;
        tasks.sort_by(|a, b| a[1].cmp(&b[1]));
        let mut vis = vec![0; 2010];
        let mut ans = 0;

        for task in tasks {
            let start = task[0] as usize;
            let end = task[1] as usize;
            let mut duration = task[2] - vis[start..=end].iter().sum::<i32>();
            let mut i = end;

            while i >= start && duration > 0 {
                if vis[i] == 0 {
                    duration -= 1;
                    vis[i] = 1;
                    ans += 1;
                }
                i -= 1;
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
