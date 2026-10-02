---
comments: true
difficulty: Hard
rating: 1885
source: Weekly Contest 172 Q4
tags:
    - Greedy
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [1326. Minimum Number of Taps to Open to Water a Garden](https://leetcode.com/problems/minimum-number-of-taps-to-open-to-water-a-garden)

[中文文档](/solution/1300-1399/1326.Minimum%20Number%20of%20Taps%20to%20Open%20to%20Water%20a%20Garden/README.md)

## Mô tả

<!-- description:start -->

<p>Có một khu vườn một chiều nằm trên trục x. Khu vườn bắt đầu tại điểm <code>0</code> và kết thúc tại điểm <code>n</code> (tức là độ dài khu vườn bằng <code>n</code>).</p>

<p>Có <code>n + 1</code> vòi nước đặt tại các điểm <code>[0, 1, ..., n]</code> trong khu vườn.</p>

<p>Cho số nguyên <code>n</code> và mảng số nguyên <code>ranges</code> có độ dài <code>n + 1</code>, trong đó <code>ranges[i]</code> (đánh chỉ số từ 0) cho biết vòi thứ <code>i-th</code> có thể tưới vùng <code>[i - ranges[i], i + ranges[i]]</code> nếu được mở.</p>

<p>Hãy trả về <em>số vòi ít nhất cần mở</em> để tưới toàn bộ khu vườn. Nếu không thể tưới hết khu vườn, trả về <strong>-1</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1326.Minimum%20Number%20of%20Taps%20to%20Open%20to%20Water%20a%20Garden/images/1685_example_1.png" style="width: 525px; height: 255px;" />
<pre>
<strong>Đầu vào:</strong> n = 5, ranges = [3,4,1,1,0,0]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Vòi tại điểm 0 có thể tưới đoạn [-3,3]
Vòi tại điểm 1 có thể tưới đoạn [-3,5]
Vòi tại điểm 2 có thể tưới đoạn [1,3]
Vòi tại điểm 3 có thể tưới đoạn [2,4]
Vòi tại điểm 4 có thể tưới đoạn [4,4]
Vòi tại điểm 5 có thể tưới đoạn [5,5]
Chỉ cần mở vòi thứ hai là có thể tưới toàn bộ khu vườn [0,5]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, ranges = [0,0,0,0]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Dù mở cả bốn vòi, bạn vẫn không thể tưới toàn bộ khu vườn.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>ranges.length == n + 1</code></li>
	<li><code>0 &lt;= ranges[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi vòi tưới được đoạn $[i-r_i,i+r_i]$; ta cần ít vòi nhất để phủ $[0,n]$. Với $n \le 10^4$, không thể thử mọi tập con. Trong số các vòi có cùng đầu trái, vòi vươn xa nhất về bên phải là lựa chọn tốt nhất; đây là dạng bài giống Jump Game: lưu trong $\textit{last}[l]$ điểm xa nhất bên phải có thể đến từ $l$.
>
> Khi duyệt các vị trí, ta theo dõi phạm vi hiện tại $mx$ và điểm kết thúc của đoạn trước $\textit{pre}$; khi đến $\textit{pre}$, ta mở thêm một vòi. Nếu tại bất kỳ thời điểm nào $mx \le i$, nghĩa là không thể phủ hết khu vườn.

<!-- thinking:end -->

Ta nhận thấy rằng trong số các vòi có thể phủ một đầu trái nhất định, chọn vòi vươn xa nhất về bên phải là tối ưu.

Vì vậy, ta có thể tiền xử lý mảng $ranges$. Vòi thứ $i$ có thể phủ đầu trái $l = \max(0, i - ranges[i])$ và đầu phải $r = i + ranges[i]$. Với mỗi đầu trái $l$, ta tìm vòi có đầu phải xa nhất và ghi lại vị trí đó trong mảng $last[i]$.

Sau đó, ta định nghĩa ba biến sau:

- Biến $ans$ là đáp án cuối cùng, tức số vòi ít nhất cần mở;
- Biến $mx$ là đầu phải xa nhất hiện có thể được phủ;
- Biến $pre$ là đầu phải xa nhất được vòi trước đó phủ.

Ta duyệt mọi vị trí trong đoạn $[0, \ldots, n-1]$. Với vị trí hiện tại $i$, ta dùng $last[i]$ để cập nhật $mx$, tức là $mx = \max(mx, last[i])$.

- Nếu $mx \leq i$, vị trí tiếp theo không thể được phủ, nên ta trả về $-1$.
- Nếu $pre = i$, ta cần dùng thêm một đoạn con, nên tăng $ans$ thêm $1$ và cập nhật $pre = mx$.

Sau khi duyệt xong, ta trả về $ans$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài khu vườn.

Các bài tương tự:

- [45. Jump Game II](https://github.com/doocs/leetcode/blob/main/solution/0000-0099/0045.Jump%20Game%20II/README.md)
- [55. Jump Game](https://github.com/doocs/leetcode/blob/main/solution/0000-0099/0055.Jump%20Game/README.md)
- [1024. Video Stitching](https://github.com/doocs/leetcode/blob/main/solution/1000-1099/1024.Video%20Stitching/README.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minTaps(self, n: int, ranges: List[int]) -> int:
        last = [0] * (n + 1)
        for i, x in enumerate(ranges):
            l, r = max(0, i - x), i + x
            last[l] = max(last[l], r)

        ans = mx = pre = 0
        for i in range(n):
            mx = max(mx, last[i])
            if mx <= i:
                return -1
            if pre == i:
                ans += 1
                pre = mx
        return ans
```

#### Java

```java
class Solution {
    public int minTaps(int n, int[] ranges) {
        int[] last = new int[n + 1];
        for (int i = 0; i < n + 1; ++i) {
            int l = Math.max(0, i - ranges[i]), r = i + ranges[i];
            last[l] = Math.max(last[l], r);
        }
        int ans = 0, mx = 0, pre = 0;
        for (int i = 0; i < n; ++i) {
            mx = Math.max(mx, last[i]);
            if (mx <= i) {
                return -1;
            }
            if (pre == i) {
                ++ans;
                pre = mx;
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
    int minTaps(int n, vector<int>& ranges) {
        vector<int> last(n + 1);
        for (int i = 0; i < n + 1; ++i) {
            int l = max(0, i - ranges[i]), r = i + ranges[i];
            last[l] = max(last[l], r);
        }
        int ans = 0, mx = 0, pre = 0;
        for (int i = 0; i < n; ++i) {
            mx = max(mx, last[i]);
            if (mx <= i) {
                return -1;
            }
            if (pre == i) {
                ++ans;
                pre = mx;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minTaps(n int, ranges []int) (ans int) {
	last := make([]int, n+1)
	for i, x := range ranges {
		l, r := max(0, i-x), i+x
		last[l] = max(last[l], r)
	}
	var pre, mx int
	for i, j := range last[:n] {
		mx = max(mx, j)
		if mx <= i {
			return -1
		}
		if pre == i {
			ans++
			pre = mx
		}
	}
	return
}
```

#### TypeScript

```ts
function minTaps(n: number, ranges: number[]): number {
    const last = new Array(n + 1).fill(0);
    for (let i = 0; i < n + 1; ++i) {
        const l = Math.max(0, i - ranges[i]);
        const r = i + ranges[i];
        last[l] = Math.max(last[l], r);
    }
    let ans = 0;
    let mx = 0;
    let pre = 0;
    for (let i = 0; i < n; ++i) {
        mx = Math.max(mx, last[i]);
        if (mx <= i) {
            return -1;
        }
        if (pre == i) {
            ++ans;
            pre = mx;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    #[allow(dead_code)]
    pub fn min_taps(n: i32, ranges: Vec<i32>) -> i32 {
        let mut last = vec![0; (n + 1) as usize];
        let mut ans = 0;
        let mut mx = 0;
        let mut pre = 0;

        // Initialize the last vector
        for (i, &r) in ranges.iter().enumerate() {
            if (i as i32) - r >= 0 {
                last[((i as i32) - r) as usize] =
                    std::cmp::max(last[((i as i32) - r) as usize], (i as i32) + r);
            } else {
                last[0] = std::cmp::max(last[0], (i as i32) + r);
            }
        }

        for i in 0..n as usize {
            mx = std::cmp::max(mx, last[i]);
            if mx <= (i as i32) {
                return -1;
            }
            if pre == (i as i32) {
                ans += 1;
                pre = mx;
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
