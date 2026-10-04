---
comments: true
difficulty: Medium
rating: 1997
source: Biweekly Contest 149 Q3
tags:
    - Greedy
    - Array
    - Enumeration
---

<!-- problem:start -->

# [3440. Reschedule Meetings for Maximum Free Time II](https://leetcode.com/problems/reschedule-meetings-for-maximum-free-time-ii)

[中文文档](/solution/3400-3499/3440.Reschedule%20Meetings%20for%20Maximum%20Free%20Time%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>eventTime</code> biểu thị thời lượng của một sự kiện. Bạn cũng được cho hai mảng số nguyên <code>startTime</code> và <code>endTime</code>, mỗi mảng có độ dài <code>n</code>.</p>

<p>Các mảng này biểu thị thời gian bắt đầu và kết thúc của <code>n</code> cuộc họp <strong>không chồng lấn</strong> diễn ra trong sự kiện từ thời điểm <code>t = 0</code> đến thời điểm <code>t = eventTime</code>, trong đó cuộc họp thứ <code>i<sup>th</sup></code> diễn ra trong khoảng thời gian <code>[startTime[i], endTime[i]].</code></p>

<p>Bạn có thể lên lịch lại <strong>tối đa </strong>một cuộc họp bằng cách di chuyển thời gian bắt đầu của cuộc họp đó nhưng vẫn giữ <strong>nguyên thời lượng</strong>, sao cho các cuộc họp vẫn không chồng lấn, để <strong>tối đa hóa</strong> <strong>khoảng thời gian</strong> <em>liên tục không có cuộc họp dài nhất</em> trong sự kiện.</p>

<p>Hãy trả về lượng thời gian trống <strong>lớn nhất</strong> có thể đạt được sau khi sắp xếp lại các cuộc họp.</p>

<p><strong>Lưu ý</strong> rằng các cuộc họp <strong>không thể</strong> được lên lịch vào thời điểm nằm ngoài sự kiện và chúng vẫn phải không chồng lấn.</p>

<p><strong>Lưu ý:</strong> <em>Trong phiên bản này</em>, việc thứ tự tương đối của các cuộc họp thay đổi sau khi lên lịch lại một cuộc họp là <strong>hợp lệ</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">eventTime = 5, startTime = [1,3], endTime = [2,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3440.Reschedule%20Meetings%20for%20Maximum%20Free%20Time%20II/images/example0_rescheduled.png" style="width: 375px; height: 123px;" /></p>

<p>Lên lịch lại cuộc họp trong khoảng <code>[1, 2]</code> thành <code>[2, 3]</code>, để không có cuộc họp nào trong khoảng thời gian <code>[0, 2]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">eventTime = 10, startTime = [0,7,9], endTime = [1,8,10]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3440.Reschedule%20Meetings%20for%20Maximum%20Free%20Time%20II/images/rescheduled_example0.png" style="width: 375px; height: 125px;" /></p>

<p>Lên lịch lại cuộc họp trong khoảng <code>[0, 1]</code> thành <code>[8, 9]</code>, để không có cuộc họp nào trong khoảng thời gian <code>[0, 7]</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">eventTime = 10, startTime = [0,3,7,9], endTime = [1,4,8,10]</span></p>

<p><strong>Đầu ra:</strong> 6</p>

<p><strong>Giải thích:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3440.Reschedule%20Meetings%20for%20Maximum%20Free%20Time%20II/images/image3.png" style="width: 375px; height: 125px;" /></strong></p>

<p>Lên lịch lại cuộc họp trong khoảng <code>[3, 4]</code> thành <code>[8, 9]</code>, để không có cuộc họp nào trong khoảng thời gian <code>[1, 7]</code>.</p>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">eventTime = 5, startTime = [0,1,2,3,4], endTime = [1,2,3,4,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có thời gian nào trong sự kiện không bị các cuộc họp chiếm dụng.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= eventTime &lt;= 10<sup>9</sup></code></li>
	<li><code>n == startTime.length == endTime.length</code></li>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= startTime[i] &lt; endTime[i] &lt;= eventTime</code></li>
	<li><code>endTime[i] &lt;= startTime[i + 1]</code> với <code>i</code> thuộc khoảng <code>[0, n - 2]</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Khác với phần I, chỉ được di chuyển một cuộc họp và cuộc họp đó phải vừa vào một khoảng trống có sẵn. Với $n\le 10^5$, ta cần một lượt duyệt tuyến tính.
>
> Di chuyển cuộc họp $i$ trong vùng lân cận của chính nó tạo ra khoảng thời gian trống $r_i-l_i-w_i$. Nếu toàn bộ khối vừa trong một khoảng trống nằm hoàn toàn bên trái hoặc bên phải, khoảng $[l_i,r_i]$ sẽ trở thành khoảng trống.
>
> Các giá trị lớn nhất theo tiền tố và hậu tố $\textit{pre}[i]$, $\textit{suf}[i]$ lưu khoảng trống lớn nhất ở mỗi phía. Với mỗi $i$, ta lấy $r_i-l_i$ khi có thể di chuyển, nếu không thì lấy $r_i-l_i-w_i$.

<!-- thinking:end -->

Theo mô tả bài toán, với cuộc họp $i$, gọi $l_i$ là vị trí không trống ở bên trái, $r_i$ là vị trí không trống ở bên phải và thời lượng của cuộc họp $i$ là $w_i = \text{endTime}[i] - \text{startTime}[i]$. Khi đó:

$$
l_i = \begin{cases}
0 & i = 0 \\\\
\text{endTime}[i - 1] & i > 0
\end{cases}
$$

$$
r_i = \begin{cases}
\text{eventTime} & i = n - 1 \\\\
\text{startTime}[i + 1] & i < n - 1
\end{cases}
$$

Cuộc họp có thể được di chuyển sang trái hoặc sang phải, và khoảng thời gian trống trong trường hợp này là:

$$
r_i - l_i - w_i
$$

Nếu tồn tại khoảng thời gian trống lớn nhất ở bên trái, $\text{pre}_{i - 1}$, sao cho $\text{pre}_{i - 1} \geq w_i$, thì cuộc họp $i$ có thể được di chuyển đến vị trí đó ở bên trái, tạo ra khoảng thời gian trống mới:

$$
r_i - l_i
$$

Tương tự, nếu tồn tại khoảng thời gian trống lớn nhất ở bên phải, $\text{suf}_{i + 1}$, sao cho $\text{suf}_{i + 1} \geq w_i$, thì cuộc họp $i$ có thể được di chuyển đến vị trí đó ở bên phải, tạo ra khoảng thời gian trống mới:

$$
r_i - l_i
$$

Do đó, trước tiên ta tiền xử lý hai mảng $\text{pre}$ và $\text{suf}$, trong đó $\text{pre}[i]$ biểu thị khoảng thời gian trống lớn nhất trong đoạn $[0, i]$, còn $\text{suf}[i]$ biểu thị khoảng thời gian trống lớn nhất trong đoạn $[i, n - 1]$. Sau đó, với mỗi cuộc họp $i$, ta tính khoảng thời gian trống lớn nhất sau khi di chuyển cuộc họp đó và lấy giá trị lớn nhất.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số cuộc họp.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxFreeTime(
        self, eventTime: int, startTime: List[int], endTime: List[int]
    ) -> int:
        n = len(startTime)
        pre = [0] * n
        suf = [0] * n
        pre[0] = startTime[0]
        suf[n - 1] = eventTime - endTime[-1]
        for i in range(1, n):
            pre[i] = max(pre[i - 1], startTime[i] - endTime[i - 1])
        for i in range(n - 2, -1, -1):
            suf[i] = max(suf[i + 1], startTime[i + 1] - endTime[i])
        ans = 0
        for i in range(n):
            l = 0 if i == 0 else endTime[i - 1]
            r = eventTime if i == n - 1 else startTime[i + 1]
            w = endTime[i] - startTime[i]
            ans = max(ans, r - l - w)
            if i and pre[i - 1] >= w:
                ans = max(ans, r - l)
            elif i + 1 < n and suf[i + 1] >= w:
                ans = max(ans, r - l)
        return ans
```

#### Java

```java
class Solution {
    public int maxFreeTime(int eventTime, int[] startTime, int[] endTime) {
        int n = startTime.length;
        int[] pre = new int[n];
        int[] suf = new int[n];

        pre[0] = startTime[0];
        suf[n - 1] = eventTime - endTime[n - 1];

        for (int i = 1; i < n; i++) {
            pre[i] = Math.max(pre[i - 1], startTime[i] - endTime[i - 1]);
        }

        for (int i = n - 2; i >= 0; i--) {
            suf[i] = Math.max(suf[i + 1], startTime[i + 1] - endTime[i]);
        }

        int ans = 0;
        for (int i = 0; i < n; i++) {
            int l = (i == 0) ? 0 : endTime[i - 1];
            int r = (i == n - 1) ? eventTime : startTime[i + 1];
            int w = endTime[i] - startTime[i];
            ans = Math.max(ans, r - l - w);

            if (i > 0 && pre[i - 1] >= w) {
                ans = Math.max(ans, r - l);
            } else if (i + 1 < n && suf[i + 1] >= w) {
                ans = Math.max(ans, r - l);
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
    int maxFreeTime(int eventTime, vector<int>& startTime, vector<int>& endTime) {
        int n = startTime.size();
        vector<int> pre(n), suf(n);

        pre[0] = startTime[0];
        suf[n - 1] = eventTime - endTime[n - 1];

        for (int i = 1; i < n; ++i) {
            pre[i] = max(pre[i - 1], startTime[i] - endTime[i - 1]);
        }

        for (int i = n - 2; i >= 0; --i) {
            suf[i] = max(suf[i + 1], startTime[i + 1] - endTime[i]);
        }

        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int l = (i == 0) ? 0 : endTime[i - 1];
            int r = (i == n - 1) ? eventTime : startTime[i + 1];
            int w = endTime[i] - startTime[i];
            ans = max(ans, r - l - w);

            if (i > 0 && pre[i - 1] >= w) {
                ans = max(ans, r - l);
            } else if (i + 1 < n && suf[i + 1] >= w) {
                ans = max(ans, r - l);
            }
        }

        return ans;
    }
};
```

#### Go

```go
func maxFreeTime(eventTime int, startTime []int, endTime []int) int {
	n := len(startTime)
	pre := make([]int, n)
	suf := make([]int, n)

	pre[0] = startTime[0]
	suf[n-1] = eventTime - endTime[n-1]

	for i := 1; i < n; i++ {
		pre[i] = max(pre[i-1], startTime[i]-endTime[i-1])
	}

	for i := n - 2; i >= 0; i-- {
		suf[i] = max(suf[i+1], startTime[i+1]-endTime[i])
	}

	ans := 0
	for i := 0; i < n; i++ {
		l := 0
		if i > 0 {
			l = endTime[i-1]
		}
		r := eventTime
		if i < n-1 {
			r = startTime[i+1]
		}
		w := endTime[i] - startTime[i]
		ans = max(ans, r-l-w)

		if i > 0 && pre[i-1] >= w {
			ans = max(ans, r-l)
		} else if i+1 < n && suf[i+1] >= w {
			ans = max(ans, r-l)
		}
	}

	return ans
}
```

#### TypeScript

```ts
function maxFreeTime(eventTime: number, startTime: number[], endTime: number[]): number {
    const n = startTime.length;
    const pre: number[] = Array(n).fill(0);
    const suf: number[] = Array(n).fill(0);

    pre[0] = startTime[0];
    suf[n - 1] = eventTime - endTime[n - 1];

    for (let i = 1; i < n; i++) {
        pre[i] = Math.max(pre[i - 1], startTime[i] - endTime[i - 1]);
    }

    for (let i = n - 2; i >= 0; i--) {
        suf[i] = Math.max(suf[i + 1], startTime[i + 1] - endTime[i]);
    }

    let ans = 0;
    for (let i = 0; i < n; i++) {
        const l = i === 0 ? 0 : endTime[i - 1];
        const r = i === n - 1 ? eventTime : startTime[i + 1];
        const w = endTime[i] - startTime[i];

        ans = Math.max(ans, r - l - w);

        if (i > 0 && pre[i - 1] >= w) {
            ans = Math.max(ans, r - l);
        } else if (i + 1 < n && suf[i + 1] >= w) {
            ans = Math.max(ans, r - l);
        }
    }

    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_free_time(event_time: i32, start_time: Vec<i32>, end_time: Vec<i32>) -> i32 {
        let n = start_time.len();
        let mut pre = vec![0; n];
        let mut suf = vec![0; n];

        pre[0] = start_time[0];
        suf[n - 1] = event_time - end_time[n - 1];

        for i in 1..n {
            pre[i] = pre[i - 1].max(start_time[i] - end_time[i - 1]);
        }

        for i in (0..n - 1).rev() {
            suf[i] = suf[i + 1].max(start_time[i + 1] - end_time[i]);
        }

        let mut ans = 0;
        for i in 0..n {
            let l = if i == 0 { 0 } else { end_time[i - 1] };
            let r = if i == n - 1 { event_time } else { start_time[i + 1] };
            let w = end_time[i] - start_time[i];
            ans = ans.max(r - l - w);

            if i > 0 && pre[i - 1] >= w {
                ans = ans.max(r - l);
            } else if i + 1 < n && suf[i + 1] >= w {
                ans = ans.max(r - l);
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
