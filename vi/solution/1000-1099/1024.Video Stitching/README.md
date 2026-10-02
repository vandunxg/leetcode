---
comments: true
difficulty: Medium
rating: 1746
source: Weekly Contest 131 Q4
tags:
    - Greedy
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [1024. Video Stitching](https://leetcode.com/problems/video-stitching)

[中文文档](/solution/1000-1099/1024.Video%20Stitching/README.md)

## Mô tả

<!-- description:start -->

<p>Cho các đoạn video ghi lại một sự kiện thể thao kéo dài <code>time</code> giây. Các đoạn video có thể chồng lấn và có độ dài khác nhau.</p>

<p>Mỗi đoạn video được biểu diễn bằng mảng <code>clips</code>, trong đó <code>clips[i] = [start<sub>i</sub>, end<sub>i</sub>]</code> cho biết đoạn thứ i bắt đầu tại <code>start<sub>i</sub></code> và kết thúc tại <code>end<sub>i</sub></code>.</p>

<p>Ta có thể tùy ý cắt các đoạn video thành những đoạn nhỏ hơn.</p>

<ul>
	<li>Ví dụ, đoạn <code>[0, 7]</code> có thể được cắt thành các đoạn <code>[0, 1] + [1, 3] + [3, 7]</code>.</li>
</ul>

<p>Hãy trả về <em>số đoạn video ít nhất cần dùng để cắt thành các phần phủ kín toàn bộ sự kiện thể thao</em> <code>[0, time]</code>. Nếu không thể thực hiện, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> clips = [[0,2],[4,6],[8,10],[1,9],[1,5],[5,9]], time = 10
<strong>Output:</strong> 3
<strong>Giải thích:</strong> Ta chọn các đoạn [0,2], [8,10], [1,9], tổng cộng 3 đoạn.
Sau đó, có thể ghép lại toàn bộ sự kiện như sau:
Ta cắt [1,9] thành các đoạn [1,2] + [2,8] + [8,9].
Khi đó, ta có các đoạn [0,2] + [2,8] + [8,10] phủ kín sự kiện [0, 10].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> clips = [[0,1],[1,2]], time = 5
<strong>Output:</strong> -1
<strong>Giải thích:</strong> Không thể phủ kín [0,5] chỉ bằng [0,1] và [1,2].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> clips = [[0,1],[6,8],[0,2],[5,6],[0,4],[0,3],[6,7],[1,3],[4,7],[1,4],[2,5],[2,6],[3,4],[4,5],[5,7],[6,9]], time = 9
<strong>Output:</strong> 3
<strong>Giải thích:</strong> Ta có thể chọn các đoạn [0,4], [4,7] và [6,9].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= clips.length &lt;= 100</code></li>
	<li><code>0 &lt;= start<sub>i</sub> &lt;= end<sub>i</sub> &lt;= 100</code></li>
	<li><code>1 &lt;= time &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Số đoạn video và giới hạn thời gian đều không quá $100$, nên có thể thử các tập con để phủ $[0,\textit{time}]$, nhưng số tập con tăng theo hàm mũ. Với một điểm bắt đầu cố định, ta chỉ cần đoạn có điểm kết thúc xa nhất; bài toán tương tự Jump Game.
>
> Lưu điểm kết thúc xa nhất $\textit{last}[i]$ của các đoạn bắt đầu tại $i$. Khi duyệt từ trái sang phải, biến $\textit{mx}$ lưu vị trí xa nhất có thể phủ tới; nếu tại $i$ mà $\textit{mx}$ không vượt qua được $i$, thì không thể phủ kín. Khi vượt qua điểm kết thúc của đoạn trước $\textit{pre}$, ta cần thêm một đoạn video.
>
> Lượt duyệt này cho số đoạn video ít nhất cần dùng, hoặc trả về $-1$.

<!-- thinking:end -->

Nếu có nhiều đoạn con cùng điểm bắt đầu, cách tối ưu là chọn đoạn có điểm kết thúc bên phải lớn nhất.

Do đó, ta tiền xử lý tất cả các đoạn con. Với mỗi vị trí $i$, tìm điểm kết thúc bên phải lớn nhất trong các đoạn bắt đầu tại $i$ rồi lưu vào mảng $last[i]$.

Ta định nghĩa biến `mx` là vị trí xa nhất hiện có thể phủ tới, biến `ans` là số đoạn con ít nhất đang cần dùng, và biến `pre` là điểm kết thúc bên phải của đoạn con được dùng gần nhất.

Tiếp theo, ta duyệt các vị trí $i$ bắt đầu từ $0$ và dùng $last[i]$ để cập nhật `mx`. Nếu sau khi cập nhật mà $mx = i$, nghĩa là không thể phủ vị trí tiếp theo, nên không thể hoàn thành yêu cầu; trả về $-1$.

Đồng thời, ta lưu điểm kết thúc bên phải `pre` của đoạn con vừa dùng. Nếu $pre = i$, ta cần dùng một đoạn con mới, nên tăng `ans` thêm $1$ và cập nhật `pre` thành `mx`.

Sau khi duyệt xong, trả về `ans`.

Độ phức tạp thời gian là $O(n+m)$ và độ phức tạp không gian là $O(m)$, trong đó $n$ là số phần tử của mảng `clips`, còn $m$ là giá trị `time`.

Bài toán tương tự:

- [45. Jump Game II](https://github.com/doocs/leetcode/blob/main/solution/0000-0099/0045.Jump%20Game%20II/README_EN.md)
- [55. Jump Game](https://github.com/doocs/leetcode/blob/main/solution/0000-0099/0055.Jump%20Game/README_EN.md)
- [1326. Minimum Number of Taps to Open to Water a Garden](https://github.com/doocs/leetcode/blob/main/solution/1300-1399/1326.Minimum%20Number%20of%20Taps%20to%20Open%20to%20Water%20a%20Garden/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def videoStitching(self, clips: List[List[int]], time: int) -> int:
        last = [0] * time
        for a, b in clips:
            if a < time:
                last[a] = max(last[a], b)
        ans = mx = pre = 0
        for i, v in enumerate(last):
            mx = max(mx, v)
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
    public int videoStitching(int[][] clips, int time) {
        int[] last = new int[time];
        for (var e : clips) {
            int a = e[0], b = e[1];
            if (a < time) {
                last[a] = Math.max(last[a], b);
            }
        }
        int ans = 0, mx = 0, pre = 0;
        for (int i = 0; i < time; ++i) {
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
    int videoStitching(vector<vector<int>>& clips, int time) {
        vector<int> last(time);
        for (auto& v : clips) {
            int a = v[0], b = v[1];
            if (a < time) {
                last[a] = max(last[a], b);
            }
        }
        int mx = 0, ans = 0;
        int pre = 0;
        for (int i = 0; i < time; ++i) {
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
func videoStitching(clips [][]int, time int) int {
	last := make([]int, time)
	for _, v := range clips {
		a, b := v[0], v[1]
		if a < time {
			last[a] = max(last[a], b)
		}
	}
	ans, mx, pre := 0, 0, 0
	for i, v := range last {
		mx = max(mx, v)
		if mx <= i {
			return -1
		}
		if pre == i {
			ans++
			pre = mx
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
