---
comments: true
difficulty: Easy
rating: 1255
source: Weekly Contest 428 Q1
tags:
    - Array
---

<!-- problem:start -->

# [3386. Button with Longest Push Time](https://leetcode.com/problems/button-with-longest-push-time)

[中文文档](/solution/3300-3399/3386.Button%20with%20Longest%20Push%20Time/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng 2D <code>events</code> biểu diễn một chuỗi sự kiện, trong đó một đứa trẻ nhấn liên tiếp một loạt nút trên bàn phím.</p>

<p>Mỗi <code>events[i] = [index<sub>i</sub>, time<sub>i</sub>]</code> cho biết nút có chỉ số <code>index<sub>i</sub></code> được nhấn tại thời điểm <code>time<sub>i</sub></code>.</p>

<ul>
	<li>Mảng được <strong>sắp xếp</strong> theo thứ tự tăng dần của <code>time</code>.</li>
	<li>Thời gian nhấn một nút là hiệu thời gian giữa hai lần nhấn nút liên tiếp. Thời gian của nút đầu tiên chính là thời điểm nó được nhấn.</li>
</ul>

<p>Trả về <code>index</code> của nút mất <strong>nhiều thời gian nhất</strong> để nhấn. Nếu có nhiều nút có cùng thời gian nhấn dài nhất, hãy trả về nút có <code>index</code> <strong>nhỏ nhất</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">events = [[1,2],[2,5],[3,9],[1,15]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Nút có chỉ số 1 được nhấn tại thời điểm 2.</li>
	<li>Nút có chỉ số 2 được nhấn tại thời điểm 5, nên mất <code>5 - 2 = 3</code> đơn vị thời gian.</li>
	<li>Nút có chỉ số 3 được nhấn tại thời điểm 9, nên mất <code>9 - 5 = 4</code> đơn vị thời gian.</li>
	<li>Nút có chỉ số 1 được nhấn lại tại thời điểm 15, nên mất <code>15 - 9 = 6</code> đơn vị thời gian.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">events = [[10,5],[1,7]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">10</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Nút có chỉ số 10 được nhấn tại thời điểm 5.</li>
	<li>Nút có chỉ số 1 được nhấn tại thời điểm 7, nên mất <code>7 - 5 = 2</code> đơn vị thời gian.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= events.length &lt;= 1000</code></li>
	<li><code>events[i] == [index<sub>i</sub>, time<sub>i</sub>]</code></li>
	<li><code>1 &lt;= index<sub>i</sub>, time<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
	<li>Đầu vào được tạo sao cho <code>events</code> được sắp xếp theo thứ tự tăng dần của <code>time<sub>i</sub></code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lần

<!-- thinking:start -->

> **Tư duy**
>
> Một lần nhấn kéo dài bằng khoảng cách giữa hai sự kiện liên tiếp; nút đầu tiên có thời gian nhấn bằng timestamp của nó. Vì $n \le 1000$, chỉ cần duyệt một lần.
>
> Lưu thời gian và chỉ số tốt nhất; thay thế chúng khi thời gian nhấn lớn hơn, hoặc bằng nhau nhưng chỉ số nhỏ hơn.
>
> Các sự kiện đã được sắp xếp theo thời gian.

<!-- thinking:end -->

Ta định nghĩa hai biến $\textit{ans}$ và $t$, lần lượt biểu diễn chỉ số của nút có thời gian nhấn dài nhất và thời gian nhấn.

Tiếp theo, ta bắt đầu duyệt mảng $\textit{events}$ từ chỉ số $k = 1$. Với mỗi $k$, ta tính thời gian nhấn của nút hiện tại $d = t2 - t1$, trong đó $t2$ là thời điểm nhấn của nút hiện tại và $t1$ là thời điểm nhấn của nút trước đó. Nếu $d > t$ hoặc $d = t$ và chỉ số $i$ của nút hiện tại nhỏ hơn $\textit{ans}$, ta cập nhật $\textit{ans} = i$ và $t = d$.

Cuối cùng, ta trả về $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{events}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def buttonWithLongestTime(self, events: List[List[int]]) -> int:
        ans, t = events[0]
        for (_, t1), (i, t2) in pairwise(events):
            d = t2 - t1
            if d > t or (d == t and i < ans):
                ans, t = i, d
        return ans
```

#### Java

```java
class Solution {
    public int buttonWithLongestTime(int[][] events) {
        int ans = events[0][0], t = events[0][1];
        for (int k = 1; k < events.length; ++k) {
            int i = events[k][0], t2 = events[k][1], t1 = events[k - 1][1];
            int d = t2 - t1;
            if (d > t || (d == t && ans > i)) {
                ans = i;
                t = d;
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
    int buttonWithLongestTime(vector<vector<int>>& events) {
        int ans = events[0][0], t = events[0][1];
        for (int k = 1; k < events.size(); ++k) {
            int i = events[k][0], t2 = events[k][1], t1 = events[k - 1][1];
            int d = t2 - t1;
            if (d > t || (d == t && ans > i)) {
                ans = i;
                t = d;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func buttonWithLongestTime(events [][]int) int {
	ans, t := events[0][0], events[0][1]
	for k, e := range events[1:] {
		i, t2, t1 := e[0], e[1], events[k][1]
		d := t2 - t1
		if d > t || (d == t && i < ans) {
			ans, t = i, d
		}
	}
	return ans
}
```

#### TypeScript

```ts
function buttonWithLongestTime(events: number[][]): number {
    let [ans, t] = events[0];
    for (let k = 1; k < events.length; ++k) {
        const [i, t2] = events[k];
        const d = t2 - events[k - 1][1];
        if (d > t || (d === t && i < ans)) {
            ans = i;
            t = d;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
