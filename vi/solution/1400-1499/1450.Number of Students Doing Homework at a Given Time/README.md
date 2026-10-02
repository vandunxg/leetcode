---
comments: true
difficulty: Easy
rating: 1129
source: Weekly Contest 189 Q1
tags:
    - Array
---

<!-- problem:start -->

# [1450. Number of Students Doing Homework at a Given Time](https://leetcode.com/problems/number-of-students-doing-homework-at-a-given-time)

[中文文档](/solution/1400-1499/1450.Number%20of%20Students%20Doing%20Homework%20at%20a%20Given%20Time/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>startTime</code> và <code>endTime</code>, cùng một số nguyên <code>queryTime</code>.</p>

<p>Sinh viên thứ <code>ith</code> bắt đầu làm bài tập vào thời điểm <code>startTime[i]</code> và hoàn thành vào thời điểm <code>endTime[i]</code>.</p>

<p>Trả về <em>số lượng sinh viên</em> đang làm bài tập vào thời điểm <code>queryTime</code>. Cụ thể hơn, hãy trả về số lượng sinh viên mà <code>queryTime</code> nằm trong đoạn <code>[startTime[i], endTime[i]]</code>, bao gồm cả hai đầu mút.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> startTime = [1,2,3], endTime = [3,2,7], queryTime = 4
<strong>Output:</strong> 1
<strong>Explanation:</strong> Có 3 sinh viên:
Sinh viên thứ nhất bắt đầu làm bài tập vào thời điểm 1, hoàn thành vào thời điểm 3 và không làm gì vào thời điểm 4.
Sinh viên thứ hai bắt đầu làm bài tập vào thời điểm 2, hoàn thành vào thời điểm 2 và cũng không làm gì vào thời điểm 4.
Sinh viên thứ ba bắt đầu làm bài tập vào thời điểm 3, hoàn thành vào thời điểm 7 và là sinh viên duy nhất đang làm bài tập vào thời điểm 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> startTime = [4], endTime = [4], queryTime = 4
<strong>Output:</strong> 1
<strong>Explanation:</strong> Sinh viên duy nhất đang làm bài tập tại queryTime.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>startTime.length == endTime.length</code></li>
	<li><code>1 &lt;= startTime.length &lt;= 100</code></li>
	<li><code>1 &lt;= startTime[i] &lt;= endTime[i] &lt;= 1000</code></li>
	<li><code>1 &lt;= queryTime &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt trực tiếp

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 100$. Đếm số sinh viên có đoạn thời gian chứa $\textit{queryTime}$.

<!-- thinking:end -->

Ta có thể duyệt trực tiếp qua hai mảng. Với mỗi sinh viên, kiểm tra xem $\textit{queryTime}$ có nằm trong đoạn thời gian làm bài tập của họ hay không. Nếu có, tăng đáp án lên một.

Sau khi duyệt xong, trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số lượng sinh viên. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def busyStudent(
        self, startTime: List[int], endTime: List[int], queryTime: int
    ) -> int:
        return sum(x <= queryTime <= y for x, y in zip(startTime, endTime))
```

#### Java

```java
class Solution {
    public int busyStudent(int[] startTime, int[] endTime, int queryTime) {
        int ans = 0;
        for (int i = 0; i < startTime.length; ++i) {
            if (startTime[i] <= queryTime && queryTime <= endTime[i]) {
                ++ans;
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
    int busyStudent(vector<int>& startTime, vector<int>& endTime, int queryTime) {
        int ans = 0;
        for (int i = 0; i < startTime.size(); ++i) {
            ans += startTime[i] <= queryTime && queryTime <= endTime[i];
        }
        return ans;
    }
};
```

#### Go

```go
func busyStudent(startTime []int, endTime []int, queryTime int) (ans int) {
	for i, x := range startTime {
		if x <= queryTime && queryTime <= endTime[i] {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function busyStudent(startTime: number[], endTime: number[], queryTime: number): number {
    const n = startTime.length;
    let ans = 0;
    for (let i = 0; i < n; i++) {
        if (startTime[i] <= queryTime && queryTime <= endTime[i]) {
            ans++;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn busy_student(start_time: Vec<i32>, end_time: Vec<i32>, query_time: i32) -> i32 {
        let mut ans = 0;
        for i in 0..start_time.len() {
            if start_time[i] <= query_time && end_time[i] >= query_time {
                ans += 1;
            }
        }
        ans
    }
}
```

#### C

```c
int busyStudent(int* startTime, int startTimeSize, int* endTime, int endTimeSize, int queryTime) {
    int ans = 0;
    for (int i = 0; i < startTimeSize; i++) {
        if (startTime[i] <= queryTime && endTime[i] >= queryTime) {
            ans++;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
