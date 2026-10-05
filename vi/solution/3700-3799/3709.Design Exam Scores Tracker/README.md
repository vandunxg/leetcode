---
comments: true
difficulty: Medium
rating: 1647
source: Biweekly Contest 167 Q3
tags:
    - Design
    - Array
    - Binary Search
    - Prefix Sum
---

<!-- problem:start -->

# [3709. Design Exam Scores Tracker](https://leetcode.com/problems/design-exam-scores-tracker)

[中文文档](/solution/3700-3799/3709.Design%20Exam%20Scores%20Tracker/README.md)

## Mô tả

<!-- description:start -->

<p>Alice thường xuyên làm bài thi và muốn theo dõi điểm số cũng như tính tổng điểm trong các khoảng thời gian cụ thể.</p>

<p>Hãy cài đặt lớp <code>ExamTracker</code>:</p>

<ul>
	<li><code>ExamTracker()</code>: Khởi tạo đối tượng <code>ExamTracker</code>.</li>
	<li><code>void record(int time, int score)</code>: Alice làm một bài thi mới tại thời điểm <code>time</code> và đạt điểm <code>score</code>.</li>
	<li><code>long long totalScore(int startTime, int endTime)</code>: Trả về một số nguyên biểu thị <strong>tổng</strong> điểm của tất cả các bài thi Alice đã làm trong khoảng từ <code>startTime</code> đến <code>endTime</code> (bao gồm cả hai đầu). Nếu Alice không làm bài thi nào trong khoảng thời gian đã cho, trả về 0.</li>
</ul>

<p>Các lời gọi hàm được đảm bảo diễn ra theo thứ tự thời gian. Cụ thể:</p>

<ul>
	<li>Các lời gọi <code>record()</code> sẽ được thực hiện với <strong>tăng nghiêm ngặt</strong> <code>time</code>.</li>
	<li>Alice sẽ không bao giờ yêu cầu tổng điểm cần thông tin từ tương lai. Nghĩa là, nếu <code>record()</code> gần nhất được gọi với <code>time = t</code>, thì <code>totalScore()</code> luôn được gọi với <code>startTime &lt;= endTime &lt;= t</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong><br />
<span class="example-io">[&quot;ExamTracker&quot;, &quot;record&quot;, &quot;totalScore&quot;, &quot;record&quot;, &quot;totalScore&quot;, &quot;totalScore&quot;, &quot;totalScore&quot;, &quot;totalScore&quot;]<br />
[[], [1, 98], [1, 1], [5, 99], [1, 3], [1, 5], [3, 4], [2, 5]]</span></p>

<p><strong>Đầu ra:</strong><br />
<span class="example-io">[null, null, 98, null, 98, 197, 0, 99] </span></p>

<p><strong>Giải thích</strong></p>
ExamTracker examTracker = new ExamTracker();<br />
examTracker.record(1, 98); // Alice takes a new exam at time 1, scoring 98.<br />
examTracker.totalScore(1, 1); // Between time 1 and time 1, Alice took 1 exam at time 1, scoring 98. The total score is 98.<br />
examTracker.record(5, 99); // Alice takes a new exam at time 5, scoring 99.<br />
examTracker.totalScore(1, 3); // Between time 1 and time 3, Alice took 1 exam at time 1, scoring 98. The total score is 98.<br />
examTracker.totalScore(1, 5); // Between time 1 and time 5, Alice took 2 exams at time 1 and 5, scoring 98 and 99. The total score is <code>98 + 99 = 197</code>.<br />
examTracker.totalScore(3, 4); // Alice did not take any exam between time 3 and time 4. Therefore, the answer is 0.<br />
examTracker.totalScore(2, 5); // Between time 2 and time 5, Alice took 1 exam at time 5, scoring 99. The total score is 99.</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= time &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= score &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= startTime &lt;= endTime &lt;= t</code>, trong đó <code>t</code> là giá trị của <code>time</code> trong lời gọi <code>record()</code> gần nhất.</li>
	<li>Các lời gọi <code>record()</code> sẽ được thực hiện với <strong>tăng nghiêm ngặt</strong> <code>time</code>.</li>
	<li>Sau <code>ExamTracker()</code>, lời gọi hàm đầu tiên luôn là <code>record()</code>.</li>
	<li>Tổng số lời gọi đến <code>record()</code> và <code>totalScore()</code> nhiều nhất là <code>10<sup>5</sup></code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum + Binary Search

<!-- thinking:start -->

> **Tư duy**
>
> `record` chèn các phần tử theo thời gian tăng dần, còn `totalScore` yêu cầu tính tổng trên một đoạn. Với các mốc thời gian đã được sắp xếp, tổng trên một đoạn có thể được tính bằng hiệu của hai prefix sum; binary search tìm vị trí hai đầu mút trên mảng thời gian, được duy trì đồng bộ với mảng prefix.

<!-- thinking:end -->

Ta dùng một mảng $\textit{times}$ để lưu thời điểm của mỗi bài thi và một mảng $\textit{pre}$ để lưu các prefix sum, trong đó $\textit{pre}[i]$ biểu thị tổng điểm của $i$ bài thi đầu tiên. Với mỗi lời gọi $\texttt{record}(time, score)$, ta thêm $time$ vào $\textit{times}$ và thêm phần tử cuối cùng của $\textit{pre}$ cộng với $score$ vào $\textit{pre}$.

Với mỗi lời gọi $\texttt{totalScore}(startTime, endTime)$, ta dùng binary search để tìm vị trí đầu tiên $l$ trong $\textit{times}$ có giá trị lớn hơn hoặc bằng $startTime$ và vị trí đầu tiên $r$ có giá trị lớn hơn $endTime$, sau đó trả về $\textit{pre}[r-1] - \textit{pre}[l-1]$.

Độ phức tạp thời gian là $O(\log n)$, trong đó $n$ là số bài thi. Độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class ExamTracker:

    def __init__(self):
        self.times = [0]
        self.pre = [0]

    def record(self, time: int, score: int) -> None:
        self.times.append(time)
        self.pre.append(self.pre[-1] + score)

    def totalScore(self, startTime: int, endTime: int) -> int:
        l = bisect_left(self.times, startTime) - 1
        r = bisect_left(self.times, endTime + 1) - 1
        return self.pre[r] - self.pre[l]


# Your ExamTracker object will be instantiated and called as such:
# obj = ExamTracker()
# obj.record(time,score)
# param_2 = obj.totalScore(startTime,endTime)
```

#### Java

```java
class ExamTracker {
    private List<Integer> times = new ArrayList<>();
    private List<Long> pre = new ArrayList<>();

    public ExamTracker() {
        times.add(0);
        pre.add(0L);
    }

    public void record(int time, int score) {
        times.add(time);
        pre.add(pre.getLast() + score);
    }

    public long totalScore(int startTime, int endTime) {
        int l = binarySearch(startTime) - 1;
        int r = binarySearch(endTime + 1) - 1;
        return pre.get(r) - pre.get(l);
    }

    private int binarySearch(int x) {
        int l = 0, r = times.size();
        while (l < r) {
            int mid = (l + r) >> 1;
            if (times.get(mid) >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
}

/**
 * Your ExamTracker object will be instantiated and called as such:
 * ExamTracker obj = new ExamTracker();
 * obj.record(time,score);
 * long param_2 = obj.totalScore(startTime,endTime);
 */
```

#### C++

```cpp
class ExamTracker {
public:
    ExamTracker() {
        times.push_back(0);
        pre.push_back(0LL);
    }

    void record(int time, int score) {
        times.push_back(time);
        pre.push_back(pre.back() + score);
    }

    long long totalScore(int startTime, int endTime) {
        int l = lower_bound(times.begin(), times.end(), startTime) - times.begin() - 1;
        int r = lower_bound(times.begin(), times.end(), endTime + 1) - times.begin() - 1;
        return pre[r] - pre[l];
    }

private:
    vector<int> times;
    vector<long long> pre;
};

/**
 * Your ExamTracker object will be instantiated and called as such:
 * ExamTracker* obj = new ExamTracker();
 * obj->record(time,score);
 * long long param_2 = obj->totalScore(startTime,endTime);
 */
```

#### Go

```go
type ExamTracker struct {
	times []int
	pre   []int64
}

func Constructor() ExamTracker {
	return ExamTracker{[]int{0}, []int64{int64(0)}}
}

func (this *ExamTracker) Record(time int, score int) {
	this.times = append(this.times, time)
	this.pre = append(this.pre, this.pre[len(this.pre)-1]+int64(score))
}

func (this *ExamTracker) TotalScore(startTime int, endTime int) int64 {
	l := sort.SearchInts(this.times, startTime) - 1
	r := sort.SearchInts(this.times, endTime+1) - 1
	return this.pre[r] - this.pre[l]
}

/**
 * Your ExamTracker object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Record(time,score);
 * param_2 := obj.TotalScore(startTime,endTime);
 */
```

#### TypeScript

```ts
class ExamTracker {
    private times: number[] = [0];
    private pre: number[] = [0];
    constructor() {}

    record(time: number, score: number): void {
        this.times.push(time);
        this.pre.push(this.pre.at(-1)! + score);
    }

    totalScore(startTime: number, endTime: number): number {
        const l = _.sortedIndex(this.times, startTime) - 1;
        const r = _.sortedIndex(this.times, endTime + 1) - 1;
        return this.pre[r] - this.pre[l];
    }
}

/**
 * Your ExamTracker object will be instantiated and called as such:
 * var obj = new ExamTracker()
 * obj.record(time,score)
 * var param_2 = obj.totalScore(startTime,endTime)
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
