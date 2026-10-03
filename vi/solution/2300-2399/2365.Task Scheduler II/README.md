---
comments: true
difficulty: Medium
rating: 1622
source: Biweekly Contest 84 Q3
tags:
    - Array
    - Hash Table
    - Simulation
---

<!-- problem:start -->

# [2365. Task Scheduler II](https://leetcode.com/problems/task-scheduler-ii)

[中文文档](/solution/2300-2399/2365.Task%20Scheduler%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên dương <code>tasks</code> được đánh chỉ số từ <strong>0</strong>, đại diện cho các task cần được hoàn thành <strong>theo thứ tự</strong>, trong đó <code>tasks[i]</code> biểu thị <strong>loại</strong> của task thứ <code>i<sup>th</sup></code>.</p>

<p>Bạn cũng được cho một số nguyên dương <code>space</code>, biểu thị số ngày <strong>tối thiểu</strong> phải trôi qua <strong>sau khi</strong> hoàn thành một task trước khi có thể thực hiện một task khác cùng <strong>loại</strong>.</p>

<p>Mỗi ngày, cho đến khi hoàn thành tất cả task, bạn phải thực hiện một trong hai việc:</p>

<ul>
	<li>Hoàn thành task tiếp theo trong <code>tasks</code>, hoặc</li>
	<li>Nghỉ.</li>
</ul>

<p>Trả về <em>số ngày <strong>nhỏ nhất</strong> cần để hoàn thành tất cả task</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> tasks = [1,2,1,2,3,1], space = 3
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong>
Một cách để hoàn thành tất cả task trong 9 ngày là:
Ngày 1: Hoàn thành task thứ 0.
Ngày 2: Hoàn thành task thứ 1.
Ngày 3: Nghỉ.
Ngày 4: Nghỉ.
Ngày 5: Hoàn thành task thứ 2.
Ngày 6: Hoàn thành task thứ 3.
Ngày 7: Nghỉ.
Ngày 8: Hoàn thành task thứ 4.
Ngày 9: Hoàn thành task thứ 5.
Có thể chứng minh rằng không thể hoàn thành các task trong chưa đến 9 ngày.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> tasks = [5,8,8,5], space = 2
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong>
Một cách để hoàn thành tất cả task trong 6 ngày là:
Ngày 1: Hoàn thành task thứ 0.
Ngày 2: Hoàn thành task thứ 1.
Ngày 3: Nghỉ.
Ngày 4: Nghỉ.
Ngày 5: Hoàn thành task thứ 2.
Ngày 6: Hoàn thành task thứ 3.
Có thể chứng minh rằng không thể hoàn thành các task trong chưa đến 6 ngày.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= tasks.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= tasks[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= space &lt;= tasks.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Các task cùng loại cần cách nhau ít nhất $space$ ngày và phải được thực hiện theo thứ tự đã cho. Vì $n \le 10^5$, ta không nên chèn các ngày nghỉ vào một lịch biểu tường minh.
>
> Một map lưu ngày sớm nhất mà mỗi task có thể được thực hiện. Tăng thêm một ngày, lấy giá trị lớn hơn giữa ngày hiện tại và ngày được phép, rồi ghi thời điểm tiếp theo là $ans+space+1$.

<!-- thinking:end -->

Ta có thể dùng một hash table $day$ để lưu thời điểm tiếp theo mà mỗi task có thể được thực hiện. Ban đầu, tất cả giá trị trong $day$ đều là $0$. Ta dùng biến $ans$ để lưu thời điểm hiện tại.

Ta duyệt qua mảng $tasks$. Với mỗi task $task$, ta tăng thời gian hiện tại $ans$ lên một, cho biết đã trôi qua một ngày kể từ lần thực hiện task trước đó. Nếu lúc này $day[task] > ans$, điều đó có nghĩa là task $task$ chỉ có thể được thực hiện vào ngày $day[task]$. Vì vậy, ta cập nhật thời gian hiện tại $ans = \max(ans, day[task])$. Sau đó, ta cập nhật giá trị của $day[task]$ thành $ans + space + 1$, cho biết thời điểm tiếp theo task $task$ có thể được thực hiện là $ans + space + 1$.

Sau khi duyệt xong, ta trả về $ans$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $tasks$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def taskSchedulerII(self, tasks: List[int], space: int) -> int:
        day = defaultdict(int)
        ans = 0
        for task in tasks:
            ans += 1
            ans = max(ans, day[task])
            day[task] = ans + space + 1
        return ans
```

#### Java

```java
class Solution {
    public long taskSchedulerII(int[] tasks, int space) {
        Map<Integer, Long> day = new HashMap<>();
        long ans = 0;
        for (int task : tasks) {
            ++ans;
            ans = Math.max(ans, day.getOrDefault(task, 0L));
            day.put(task, ans + space + 1);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long taskSchedulerII(vector<int>& tasks, int space) {
        unordered_map<int, long long> day;
        long long ans = 0;
        for (int& task : tasks) {
            ++ans;
            ans = max(ans, day[task]);
            day[task] = ans + space + 1;
        }
        return ans;
    }
};
```

#### Go

```go
func taskSchedulerII(tasks []int, space int) (ans int64) {
	day := map[int]int64{}
	for _, task := range tasks {
		ans++
		if ans < day[task] {
			ans = day[task]
		}
		day[task] = ans + int64(space) + 1
	}
	return
}
```

#### TypeScript

```ts
function taskSchedulerII(tasks: number[], space: number): number {
    const day = new Map<number, number>();
    let ans = 0;
    for (const task of tasks) {
        ++ans;
        ans = Math.max(ans, day.get(task) ?? 0);
        day.set(task, ans + space + 1);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
