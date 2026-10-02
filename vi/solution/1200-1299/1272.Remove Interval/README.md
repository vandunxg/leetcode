---
comments: true
difficulty: Medium
rating: 1524
source: Biweekly Contest 14 Q2
tags:
    - Array
---

<!-- problem:start -->

# [1272. Remove Interval 🔒](https://leetcode.com/problems/remove-interval)

[中文文档](/solution/1200-1299/1272.Remove%20Interval/README.md)

## Mô tả

<!-- description:start -->

<p>Một tập hợp số thực có thể được biểu diễn bằng hợp của nhiều khoảng rời nhau, mỗi khoảng có dạng <code>[a, b)</code>. Số thực <code>x</code> thuộc tập hợp nếu có một khoảng <code>[a, b)</code> chứa <code>x</code> (tức là <code>a &lt;= x &lt; b</code>).</p>

<p>Bạn được cho danh sách <strong>đã sắp xếp</strong> các khoảng rời nhau <code>intervals</code> biểu diễn một tập hợp số thực như mô tả ở trên, trong đó <code>intervals[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> biểu diễn khoảng <code>[a<sub>i</sub>, b<sub>i</sub>)</code>. Bạn cũng được cho một khoảng khác <code>toBeRemoved</code>.</p>

<p>Hãy trả về tập hợp số thực thu được sau khi <strong>loại bỏ</strong> khoảng <code>toBeRemoved</code> khỏi <code>intervals</code>. Nói cách khác, trả về tập hợp sao cho mọi <code>x</code> trong đó đều thuộc <code>intervals</code> nhưng <strong>không</strong> thuộc <code>toBeRemoved</code>. Đáp án cần là danh sách <strong>đã sắp xếp</strong> gồm các khoảng rời nhau như mô tả ở trên.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1272.Remove%20Interval/images/removeintervalex1.png" style="width: 510px; height: 319px;" />
<pre>
<strong>Đầu vào:</strong> intervals = [[0,2],[3,4],[5,7]], toBeRemoved = [1,6]
<strong>Đầu ra:</strong> [[0,1],[6,7]]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1272.Remove%20Interval/images/removeintervalex2.png" style="width: 410px; height: 318px;" />
<pre>
<strong>Đầu vào:</strong> intervals = [[0,5]], toBeRemoved = [2,3]
<strong>Đầu ra:</strong> [[0,2],[3,5]]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> intervals = [[-5,-4],[-3,-2],[1,2],[3,5],[8,9]], toBeRemoved = [-1,4]
<strong>Đầu ra:</strong> [[-5,-4],[-3,-2],[4,5],[8,9]]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= intervals.length &lt;= 10<sup>4</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= a<sub>i</sub> &lt; b<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Xét các trường hợp

<!-- thinking:start -->

> **Tư duy**
>
> Ta loại khoảng $[x,y)$ khỏi danh sách các khoảng rời nhau đã sắp xếp. Mỗi khoảng có thể không giao với khoảng bị loại bỏ, bị phủ hoàn toàn, hoặc bị tách thành phần bên trái và bên phải. Khi duyệt từ trái sang phải, ta giữ nguyên khoảng không giao nhau; nếu có giao thì có thể thêm $[a,x)$ và $[y,b)$ vào kết quả. Chỉ cần một lượt duyệt tuyến tính, không cần sắp xếp thêm.

<!-- thinking:end -->

Gọi khoảng cần loại bỏ là $[x, y)$. Ta duyệt danh sách khoảng; với mỗi khoảng $[a, b)$, có ba trường hợp:

- Nếu $a \geq y$ hoặc $b \leq x$, khoảng hiện tại không giao với khoảng cần loại bỏ, nên ta thêm nguyên khoảng này vào đáp án.
- Nếu $a \lt x$ và $b \gt y$, khoảng hiện tại giao với khoảng cần loại bỏ và bao trùm nó. Ta tách khoảng này thành hai khoảng rồi thêm vào đáp án.
- Nếu $a \geq x$ và $b \leq y$, khoảng hiện tại bị khoảng cần loại bỏ phủ hoàn toàn, nên ta không thêm vào đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số khoảng trong danh sách. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def removeInterval(
        self, intervals: List[List[int]], toBeRemoved: List[int]
    ) -> List[List[int]]:
        x, y = toBeRemoved
        ans = []
        for a, b in intervals:
            if a >= y or b <= x:
                ans.append([a, b])
            else:
                if a < x:
                    ans.append([a, x])
                if b > y:
                    ans.append([y, b])
        return ans
```

#### Java

```java
class Solution {
    public List<List<Integer>> removeInterval(int[][] intervals, int[] toBeRemoved) {
        int x = toBeRemoved[0], y = toBeRemoved[1];
        List<List<Integer>> ans = new ArrayList<>();
        for (var e : intervals) {
            int a = e[0], b = e[1];
            if (a >= y || b <= x) {
                ans.add(Arrays.asList(a, b));
            } else {
                if (a < x) {
                    ans.add(Arrays.asList(a, x));
                }
                if (b > y) {
                    ans.add(Arrays.asList(y, b));
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
    vector<vector<int>> removeInterval(vector<vector<int>>& intervals, vector<int>& toBeRemoved) {
        int x = toBeRemoved[0], y = toBeRemoved[1];
        vector<vector<int>> ans;
        for (auto& e : intervals) {
            int a = e[0], b = e[1];
            if (a >= y || b <= x) {
                ans.push_back(e);
            } else {
                if (a < x) {
                    ans.push_back({a, x});
                }
                if (b > y) {
                    ans.push_back({y, b});
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func removeInterval(intervals [][]int, toBeRemoved []int) (ans [][]int) {
	x, y := toBeRemoved[0], toBeRemoved[1]
	for _, e := range intervals {
		a, b := e[0], e[1]
		if a >= y || b <= x {
			ans = append(ans, e)
		} else {
			if a < x {
				ans = append(ans, []int{a, x})
			}
			if b > y {
				ans = append(ans, []int{y, b})
			}
		}
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
