---
comments: true
difficulty: Medium
rating: 1728
source: Biweekly Contest 149 Q2
tags:
    - Greedy
    - Array
    - Sliding Window
---

<!-- problem:start -->

# [3439. Reschedule Meetings for Maximum Free Time I](https://leetcode.com/problems/reschedule-meetings-for-maximum-free-time-i)

[中文文档](/solution/3400-3499/3439.Reschedule%20Meetings%20for%20Maximum%20Free%20Time%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>eventTime</code> biểu thị thời lượng của một sự kiện, trong đó sự kiện diễn ra từ thời điểm <code>t = 0</code> đến thời điểm <code>t = eventTime</code>.</p>

<p>Bạn cũng được cho hai mảng số nguyên <code>startTime</code> và <code>endTime</code>, mỗi mảng có độ dài <code>n</code>. Các mảng này biểu thị thời gian bắt đầu và kết thúc của <code>n</code> cuộc họp <strong>không chồng lấn</strong>, trong đó cuộc họp thứ <code>i<sup>th</sup></code> diễn ra trong khoảng thời gian <code>[startTime[i], endTime[i]]</code>.</p>

<p>Bạn có thể lên lịch lại <strong>tối đa</strong> <code>k</code> cuộc họp bằng cách thay đổi thời gian bắt đầu của chúng nhưng vẫn giữ <strong>nguyên thời lượng</strong>, để <strong>tối đa hóa</strong> <em>khoảng thời gian liên tục không có cuộc họp</em> <strong>dài nhất</strong> trong sự kiện.</p>

<p>Thứ tự <strong>tương đối</strong> của tất cả các cuộc họp phải được giữ<em> nguyên</em> và chúng vẫn phải không chồng lấn.</p>

<p>Hãy trả về lượng thời gian trống <strong>lớn nhất</strong> có thể đạt được sau khi sắp xếp lại các cuộc họp.</p>

<p><strong>Lưu ý</strong> rằng các cuộc họp <strong>không thể</strong> được lên lịch vào thời điểm nằm ngoài sự kiện.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">eventTime = 5, k = 1, startTime = [1,3], endTime = [2,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3439.Reschedule%20Meetings%20for%20Maximum%20Free%20Time%20I/images/example0_rescheduled.png" style="width: 375px; height: 123px;" /></p>

<p>Lên lịch lại cuộc họp trong khoảng <code>[1, 2]</code> thành <code>[2, 3]</code>, để không có cuộc họp nào trong khoảng thời gian <code>[0, 2]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">eventTime = 10, k = 1, startTime = [0,2,9], endTime = [1,4,10]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3439.Reschedule%20Meetings%20for%20Maximum%20Free%20Time%20I/images/example1_rescheduled.png" style="width: 375px; height: 125px;" /></p>

<p>Lên lịch lại cuộc họp trong khoảng <code>[2, 4]</code> thành <code>[1, 3]</code>, để không có cuộc họp nào trong khoảng thời gian <code>[3, 9]</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">eventTime = 5, k = 2, startTime = [0,1,2,3,4], endTime = [1,2,3,4,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có khoảng thời gian nào trong sự kiện không bị các cuộc họp chiếm dụng.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= eventTime &lt;= 10<sup>9</sup></code></li>
    <li><code>n == startTime.length == endTime.length</code></li>
    <li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= k &lt;= n</code></li>
    <li><code>0 &lt;= startTime[i] &lt; endTime[i] &lt;= eventTime</code></li>
    <li><code>endTime[i] &lt;= startTime[i + 1]</code> với <code>i</code> thuộc khoảng <code>[0, n - 2]</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sliding Window

<!-- thinking:start -->

> **Tư duy**
>
> Các cuộc họp đã được sắp xếp và không giao nhau. Khi lên lịch lại tối đa $k$ cuộc họp, ta xếp chúng liền nhau để gộp các khoảng trống ở hai bên.
>
> Vì $n\le 10^5$, ta không thể xét tường minh từng tập con các cuộc họp được di chuyển. Có $n+1$ khoảng trống liền kề, và việc di chuyển $k$ cuộc họp sẽ nối tối đa $k+1$ khoảng trống liên tiếp.
>
> Ta lưu độ dài các khoảng trống trong $\textit{nums}$ và tìm tổng lớn nhất của một cửa sổ có độ dài $k+1$. Tổng này chính là khoảng thời gian trống dài nhất mà một lần lên lịch lại hợp lệ có thể tạo ra.

<!-- thinking:end -->

Về cơ bản, bài toán yêu cầu gộp các khoảng thời gian trống liền kề thành một khoảng thời gian trống dài hơn. Tổng cộng có $n + 1$ khoảng thời gian trống:

- Khoảng trống đầu tiên nằm từ đầu sự kiện đến thời điểm bắt đầu cuộc họp đầu tiên;
- $n - 1$ khoảng trống ở giữa nằm giữa mỗi cặp cuộc họp liền kề;
- Khoảng trống cuối cùng nằm từ thời điểm kết thúc cuộc họp cuối cùng đến cuối sự kiện.

Ta có thể lên lịch lại tối đa $k$ cuộc họp, tương đương với việc gộp tối đa $k + 1$ khoảng thời gian trống. Ta cần tìm độ dài lớn nhất trong tất cả các khoảng gồm $k + 1$ khoảng trống được gộp lại.

Ta có thể lưu độ dài của các khoảng thời gian trống này vào một mảng $\textit{nums}$. Sau đó, ta dùng cửa sổ trượt có độ dài $k + 1$ để duyệt mảng, tính tổng của từng cửa sổ và tìm tổng lớn nhất. Đây chính là lượng thời gian trống lớn nhất cần tìm.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là số cuộc họp.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxFreeTime(
        self, eventTime: int, k: int, startTime: List[int], endTime: List[int]
    ) -> int:
        nums = [startTime[0]]
        for i in range(1, len(endTime)):
            nums.append(startTime[i] - endTime[i - 1])
        nums.append(eventTime - endTime[-1])
        ans = s = 0
        for i, x in enumerate(nums):
            s += x
            if i >= k:
                ans = max(ans, s)
                s -= nums[i - k]
        return ans
```

#### Java

```java
class Solution {
    public int maxFreeTime(int eventTime, int k, int[] startTime, int[] endTime) {
        int n = endTime.length;
        int[] nums = new int[n + 1];
        nums[0] = startTime[0];
        for (int i = 1; i < n; ++i) {
            nums[i] = startTime[i] - endTime[i - 1];
        }
        nums[n] = eventTime - endTime[n - 1];
        int ans = 0, s = 0;
        for (int i = 0; i <= n; ++i) {
            s += nums[i];
            if (i >= k) {
                ans = Math.max(ans, s);
                s -= nums[i - k];
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
    int maxFreeTime(int eventTime, int k, vector<int>& startTime, vector<int>& endTime) {
        int n = endTime.size();
        vector<int> nums(n + 1);
        nums[0] = startTime[0];
        for (int i = 1; i < n; ++i) {
            nums[i] = startTime[i] - endTime[i - 1];
        }
        nums[n] = eventTime - endTime[n - 1];

        int ans = 0, s = 0;
        for (int i = 0; i <= n; ++i) {
            s += nums[i];
            if (i >= k) {
                ans = max(ans, s);
                s -= nums[i - k];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxFreeTime(eventTime int, k int, startTime []int, endTime []int) int {
    n := len(endTime)
    nums := make([]int, n+1)
    nums[0] = startTime[0]
    for i := 1; i < n; i++ {
        nums[i] = startTime[i] - endTime[i-1]
    }
    nums[n] = eventTime - endTime[n-1]

    ans, s := 0, 0
    for i := 0; i <= n; i++ {
        s += nums[i]
        if i >= k {
            ans = max(ans, s)
            s -= nums[i-k]
        }
    }
    return ans
}
```

#### TypeScript

```ts
function maxFreeTime(eventTime: number, k: number, startTime: number[], endTime: number[]): number {
    const n = endTime.length;
    const nums: number[] = new Array(n + 1);
    nums[0] = startTime[0];
    for (let i = 1; i < n; i++) {
        nums[i] = startTime[i] - endTime[i - 1];
    }
    nums[n] = eventTime - endTime[n - 1];

    let [ans, s] = [0, 0];
    for (let i = 0; i <= n; i++) {
        s += nums[i];
        if (i >= k) {
            ans = Math.max(ans, s);
            s -= nums[i - k];
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_free_time(event_time: i32, k: i32, start_time: Vec<i32>, end_time: Vec<i32>) -> i32 {
        let n = end_time.len();
        let mut nums = vec![0; n + 1];
        nums[0] = start_time[0];
        for i in 1..n {
            nums[i] = start_time[i] - end_time[i - 1];
        }
        nums[n] = event_time - end_time[n - 1];

        let mut ans = 0;
        let mut s = 0;
        for i in 0..=n {
            s += nums[i];
            if i as i32 >= k {
                ans = ans.max(s);
                s -= nums[i - k as usize];
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Sliding Window (Tối ưu không gian)

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 đã tính đúng tổng các cửa sổ trong thời gian tuyến tính, nhưng phải lưu mọi khoảng trống.
>
> Khoảng trống thứ $i$ có thể được tính trực tiếp từ các điểm đầu mút, nên không cần mảng này.
>
> Tính $f(i)$ ngay trong cửa sổ trượt giúp giảm không gian bổ sung xuống còn $O(1)$ mà không thay đổi thời gian chạy hay đáp án.

<!-- thinking:end -->

Ở Lời giải 1, ta dùng một mảng để lưu độ dài của các khoảng thời gian trống. Thực ra, ta không cần lưu toàn bộ mảng; có thể dùng một hàm $f(i)$ để biểu diễn độ dài của khoảng trống thứ $i$. Nhờ đó, ta tiết kiệm được không gian.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số cuộc họp. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxFreeTime(
        self, eventTime: int, k: int, startTime: List[int], endTime: List[int]
    ) -> int:
        def f(i: int) -> int:
            if i == 0:
                return startTime[0]
            if i == len(endTime):
                return eventTime - endTime[-1]
            return startTime[i] - endTime[i - 1]

        ans = s = 0
        for i in range(len(endTime) + 1):
            s += f(i)
            if i >= k:
                ans = max(ans, s)
                s -= f(i - k)
        return ans
```

#### Java

```java
class Solution {
    public int maxFreeTime(int eventTime, int k, int[] startTime, int[] endTime) {
        int n = endTime.length;
        IntUnaryOperator f = i -> {
            if (i == 0) {
                return startTime[0];
            }
            if (i == n) {
                return eventTime - endTime[n - 1];
            }
            return startTime[i] - endTime[i - 1];
        };
        int ans = 0, s = 0;
        for (int i = 0; i <= n; i++) {
            s += f.applyAsInt(i);
            if (i >= k) {
                ans = Math.max(ans, s);
                s -= f.applyAsInt(i - k);
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
    int maxFreeTime(int eventTime, int k, vector<int>& startTime, vector<int>& endTime) {
        int n = endTime.size();
        auto f = [&](int i) -> int {
            if (i == 0) {
                return startTime[0];
            }
            if (i == n) {
                return eventTime - endTime[n - 1];
            }
            return startTime[i] - endTime[i - 1];
        };
        int ans = 0, s = 0;
        for (int i = 0; i <= n; ++i) {
            s += f(i);
            if (i >= k) {
                ans = max(ans, s);
                s -= f(i - k);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxFreeTime(eventTime int, k int, startTime []int, endTime []int) int {
    n := len(endTime)
    f := func(i int) int {
        if i == 0 {
            return startTime[0]
        }
        if i == n {
            return eventTime - endTime[n-1]
        }
        return startTime[i] - endTime[i-1]
    }
    ans, s := 0, 0
    for i := 0; i <= n; i++ {
        s += f(i)
        if i >= k {
            ans = max(ans, s)
            s -= f(i - k)
        }
    }
    return ans
}
```

#### TypeScript

```ts
function maxFreeTime(eventTime: number, k: number, startTime: number[], endTime: number[]): number {
    const n = endTime.length;
    const f = (i: number): number => {
        if (i === 0) {
            return startTime[0];
        }
        if (i === n) {
            return eventTime - endTime[n - 1];
        }
        return startTime[i] - endTime[i - 1];
    };
    let ans = 0;
    let s = 0;
    for (let i = 0; i <= n; i++) {
        s += f(i);
        if (i >= k) {
            ans = Math.max(ans, s);
            s -= f(i - k);
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_free_time(event_time: i32, k: i32, start_time: Vec<i32>, end_time: Vec<i32>) -> i32 {
        let n = end_time.len();
        let f = |i: usize| -> i32 {
            if i == 0 {
                start_time[0]
            } else if i == n {
                event_time - end_time[n - 1]
            } else {
                start_time[i] - end_time[i - 1]
            }
        };
        let mut ans = 0;
        let mut s = 0;
        for i in 0..=n {
            s += f(i);
            if i >= k as usize {
                ans = ans.max(s);
                s -= f(i - k as usize);
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
