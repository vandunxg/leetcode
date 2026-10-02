---
comments: true
difficulty: Medium
rating: 1574
source: Weekly Contest 205 Q3
tags:
    - Greedy
    - Array
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [1578. Minimum Time to Make Rope Colorful](https://leetcode.com/problems/minimum-time-to-make-rope-colorful)

[中文文档](/solution/1500-1599/1578.Minimum%20Time%20to%20Make%20Rope%20Colorful/README.md)

## Mô tả

<!-- description:start -->

<p>Alice có <code>n</code> quả bóng được xếp trên một sợi dây. Cho chuỗi <code>colors</code> <strong>đánh chỉ số từ 0</strong>, trong đó <code>colors[i]</code> là màu của quả bóng thứ <code>i<sup>th</sup></code>.</p>

<p>Alice muốn sợi dây có <strong>nhiều màu</strong>. Cô ấy không muốn <strong>hai quả bóng liên tiếp</strong> có cùng màu nên nhờ Bob giúp đỡ. Bob có thể tháo một số quả bóng khỏi dây để làm dây có <strong>nhiều màu</strong>. Cho mảng số nguyên <code>neededTime</code> <strong>đánh chỉ số từ 0</strong>, trong đó <code>neededTime[i]</code> là thời gian (tính bằng giây) Bob cần để tháo quả bóng thứ <code>i<sup>th</sup></code>.</p>

<p>Trả về <em><strong>thời gian nhỏ nhất</strong> Bob cần để làm sợi dây <strong>nhiều màu</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1578.Minimum%20Time%20to%20Make%20Rope%20Colorful/images/ballon1.jpg" style="width: 404px; height: 243px;" />
<pre>
<strong>Đầu vào:</strong> colors = &quot;abaac&quot;, neededTime = [1,2,3,4,5]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Trong hình trên, &#39;a&#39; là màu xanh dương, &#39;b&#39; là màu đỏ và &#39;c&#39; là màu xanh lá.
Bob có thể tháo quả bóng xanh dương ở chỉ số 2. Việc này mất 3 giây.
Không còn hai quả bóng liên tiếp cùng màu. Tổng thời gian = 3.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1578.Minimum%20Time%20to%20Make%20Rope%20Colorful/images/balloon2.jpg" style="width: 244px; height: 243px;" />
<pre>
<strong>Đầu vào:</strong> colors = &quot;abc&quot;, neededTime = [1,2,3]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Sợi dây đã có nhiều màu. Bob không cần tháo quả bóng nào.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1578.Minimum%20Time%20to%20Make%20Rope%20Colorful/images/balloon3.jpg" style="width: 404px; height: 243px;" />
<pre>
<strong>Đầu vào:</strong> colors = &quot;aabaa&quot;, neededTime = [1,2,3,4,1]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Bob sẽ tháo các quả bóng ở chỉ số 0 và 4. Mỗi quả bóng mất $1$ giây để tháo.
Không còn hai quả bóng liên tiếp cùng màu. Tổng thời gian = 1 + 1 = 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == colors.length == neededTime.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= neededTime[i] &lt;= 10<sup>4</sup></code></li>
	<li><code>colors</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ + Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Các màu giống nhau liền kề phải được giảm còn một quả bóng, với chi phí là thời gian tháo các quả bị xóa. Vì $n\le 10^5$ và các màu khác nhau không ảnh hưởng lẫn nhau, mỗi đoạn có thể xử lý độc lập.
>
> Hai con trỏ đánh dấu một đoạn cùng màu, đồng thời tính tổng thời gian và theo dõi giá trị lớn nhất. Giữ quả bóng tốn nhiều thời gian nhất và xóa phần còn lại, chi phí là tổng trừ đi giá trị lớn nhất. Mỗi đoạn chỉ cần duyệt một lần.

<!-- thinking:end -->

Ta có thể dùng hai con trỏ trỏ đến đầu và cuối nhóm quả bóng liên tiếp cùng màu, sau đó tính tổng thời gian $s$ và thời gian lớn nhất $mx$ của nhóm. Nếu nhóm có nhiều hơn một quả bóng, ta tham lam giữ quả bóng có thời gian lớn nhất và tháo các quả còn lại, tốn $s - mx$, rồi cộng vào đáp án. Tiếp tục duyệt cho đến hết các quả bóng.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là số quả bóng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCost(self, colors: str, neededTime: List[int]) -> int:
        ans = i = 0
        n = len(colors)
        while i < n:
            j = i
            s = mx = 0
            while j < n and colors[j] == colors[i]:
                s += neededTime[j]
                if mx < neededTime[j]:
                    mx = neededTime[j]
                j += 1
            if j - i > 1:
                ans += s - mx
            i = j
        return ans
```

#### Java

```java
class Solution {
    public int minCost(String colors, int[] neededTime) {
        int ans = 0;
        int n = neededTime.length;
        for (int i = 0, j = 0; i < n; i = j) {
            j = i;
            int s = 0, mx = 0;
            while (j < n && colors.charAt(j) == colors.charAt(i)) {
                s += neededTime[j];
                mx = Math.max(mx, neededTime[j]);
                ++j;
            }
            if (j - i > 1) {
                ans += s - mx;
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
    int minCost(string colors, vector<int>& neededTime) {
        int ans = 0;
        int n = colors.size();
        for (int i = 0, j = 0; i < n; i = j) {
            j = i;
            int s = 0, mx = 0;
            while (j < n && colors[j] == colors[i]) {
                s += neededTime[j];
                mx = max(mx, neededTime[j]);
                ++j;
            }
            if (j - i > 1) {
                ans += s - mx;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minCost(colors string, neededTime []int) (ans int) {
	n := len(colors)
	for i, j := 0, 0; i < n; i = j {
		j = i
		s, mx := 0, 0
		for j < n && colors[j] == colors[i] {
			s += neededTime[j]
			mx = max(mx, neededTime[j])
			j++
		}
		if j-i > 1 {
			ans += s - mx
		}
	}
	return
}
```

#### TypeScript

```ts
function minCost(colors: string, neededTime: number[]): number {
    let ans = 0;
    const n = neededTime.length;

    for (let i = 0, j = 0; i < n; i = j) {
        j = i;
        let [s, mx] = [0, 0];
        while (j < n && colors[j] === colors[i]) {
            s += neededTime[j];
            mx = Math.max(mx, neededTime[j]);
            ++j;
        }
        if (j - i > 1) {
            ans += s - mx;
        }
    }

    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_cost(colors: String, needed_time: Vec<i32>) -> i32 {
        let n = needed_time.len();
        let mut ans = 0;
        let bytes = colors.as_bytes();
        let mut i = 0;

        while i < n {
            let mut j = i;
            let mut s = 0;
            let mut mx = 0;

            while j < n && bytes[j] == bytes[i] {
                s += needed_time[j];
                mx = mx.max(needed_time[j]);
                j += 1;
            }

            if j - i > 1 {
                ans += s - mx;
            }
            i = j;
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
