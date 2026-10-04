---
comments: true
difficulty: Medium
rating: 1351
source: Weekly Contest 366 Q2
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [2895. Minimum Processing Time](https://leetcode.com/problems/minimum-processing-time)

[中文文档](/solution/2800-2899/2895.Minimum%20Processing%20Time/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có một số lượng processor nhất định, mỗi processor có 4 core. Số task cần thực hiện gấp bốn lần số processor. Mỗi task phải được gán cho một core riêng, và mỗi core chỉ được sử dụng một lần.</p>

<p>Bạn được cho một mảng <code>processorTime</code> biểu thị thời điểm mỗi processor sẵn sàng và một mảng <code>tasks</code> biểu thị thời gian cần để hoàn thành mỗi task. Hãy trả về thời gian <em>nhỏ nhất</em> cần để hoàn thành tất cả task.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">processorTime = [8,10], tasks = [2,2,3,1,8,7,4,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">16</span></p>

<p><strong>Giải thích:</strong></p>

<p>Gán các task có chỉ số 4, 5, 6, 7 cho processor đầu tiên, processor này sẵn sàng tại <code>time = 8</code>, và các task có chỉ số 0, 1, 2, 3 cho processor thứ hai, processor này sẵn sàng tại <code>time = 10</code>.&nbsp;</p>

<p>Thời điểm processor đầu tiên hoàn thành tất cả task là&nbsp;<code>max(8 + 8, 8 + 7, 8 + 4, 8 + 5) = 16</code>.</p>

<p>Thời điểm processor thứ hai hoàn thành tất cả task là&nbsp;<code>max(10 + 2, 10 + 2, 10 + 3, 10 + 1) = 13</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">processorTime = [10,20], tasks = [2,3,1,2,5,8,4,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">23</span></p>

<p><strong>Giải thích:</strong></p>

<p>Gán các task có chỉ số 1, 4, 5, 6 cho processor đầu tiên và các task còn lại cho processor thứ hai.</p>

<p>Thời điểm processor đầu tiên hoàn thành tất cả task là <code>max(10 + 3, 10 + 5, 10 + 8, 10 + 4) = 18</code>.</p>

<p>Thời điểm processor thứ hai hoàn thành tất cả task là <code>max(20 + 2, 20 + 1, 20 + 2, 20 + 3) = 23</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == processorTime.length &lt;= 25000</code></li>
	<li><code>1 &lt;= tasks.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= processorTime[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= tasks[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>tasks.length == 4 * n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Sorting

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi processor có bốn core; thời điểm hoàn thành của nó bằng thời điểm sẵn sàng cộng với task dài nhất được gán. Sắp xếp thời điểm sẵn sàng tăng dần và các task giảm dần, gán bốn task dài nhất còn lại cho processor sẵn sàng sớm nhất, rồi lấy thời điểm hoàn thành lớn nhất.

<!-- thinking:end -->

Để giảm thời gian cần thiết để xử lý tất cả task, bốn task có thời gian xử lý dài nhất nên được gán cho các processor trở nên rảnh sớm nhất.

Vì vậy, ta sắp xếp các processor theo thời điểm rảnh và sắp xếp các task theo thời gian xử lý. Sau đó, ta gán bốn task có thời gian xử lý dài nhất cho processor rảnh sớm nhất và tính thời điểm hoàn thành lớn nhất.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(\log n)$. Ở đây, $n$ là số lượng task.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minProcessingTime(self, processorTime: List[int], tasks: List[int]) -> int:
        processorTime.sort()
        tasks.sort()
        ans = 0
        i = len(tasks) - 1
        for t in processorTime:
            ans = max(ans, t + tasks[i])
            i -= 4
        return ans
```

#### Java

```java
class Solution {
    public int minProcessingTime(List<Integer> processorTime, List<Integer> tasks) {
        processorTime.sort((a, b) -> a - b);
        tasks.sort((a, b) -> a - b);
        int ans = 0, i = tasks.size() - 1;
        for (int t : processorTime) {
            ans = Math.max(ans, t + tasks.get(i));
            i -= 4;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minProcessingTime(vector<int>& processorTime, vector<int>& tasks) {
        sort(processorTime.begin(), processorTime.end());
        sort(tasks.begin(), tasks.end());
        int ans = 0, i = tasks.size() - 1;
        for (int t : processorTime) {
            ans = max(ans, t + tasks[i]);
            i -= 4;
        }
        return ans;
    }
};
```

#### Go

```go
func minProcessingTime(processorTime []int, tasks []int) (ans int) {
	sort.Ints(processorTime)
	sort.Ints(tasks)
	i := len(tasks) - 1
	for _, t := range processorTime {
		ans = max(ans, t+tasks[i])
		i -= 4
	}
	return
}
```

#### TypeScript

```ts
function minProcessingTime(processorTime: number[], tasks: number[]): number {
    processorTime.sort((a, b) => a - b);
    tasks.sort((a, b) => a - b);
    let [ans, i] = [0, tasks.length - 1];
    for (const t of processorTime) {
        ans = Math.max(ans, t + tasks[i]);
        i -= 4;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
