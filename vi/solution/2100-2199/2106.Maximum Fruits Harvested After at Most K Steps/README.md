---
comments: true
difficulty: Hard
rating: 2062
source: Weekly Contest 271 Q4
tags:
    - Array
    - Binary Search
    - Prefix Sum
    - Sliding Window
---

<!-- problem:start -->

# [2106. Maximum Fruits Harvested After at Most K Steps](https://leetcode.com/problems/maximum-fruits-harvested-after-at-most-k-steps)

[中文文档](/solution/2100-2199/2106.Maximum%20Fruits%20Harvested%20After%20at%20Most%20K%20Steps/README.md)

## Mô tả

<!-- description:start -->

<p>Trên một trục x vô hạn, có trái cây ở một số vị trí. Cho mảng số nguyên 2 chiều <code>fruits</code>, trong đó <code>fruits[i] = [position<sub>i</sub>, amount<sub>i</sub>]</code> biểu diễn có <code>amount<sub>i</sub></code> trái cây tại vị trí <code>position<sub>i</sub></code>. <code>fruits</code> đã được <strong>sắp xếp</strong> theo <code>position<sub>i</sub></code> theo <strong>thứ tự tăng dần</strong>, và mỗi <code>position<sub>i</sub></code> là <strong>duy nhất</strong>.</p>

<p>Đồng thời, bạn được cho một số nguyên <code>startPos</code> và một số nguyên <code>k</code>. Ban đầu, bạn ở vị trí <code>startPos</code>. Từ bất kỳ vị trí nào, bạn có thể đi bộ sang <strong>trái hoặc phải</strong>. Mỗi bước đi một <strong>đơn vị</strong> trên trục x mất <strong>một bước</strong>, và tổng cộng bạn có thể đi bộ <strong>nhiều nhất</strong> <code>k</code> bước. Tại mỗi vị trí bạn đi qua, bạn thu hoạch toàn bộ trái cây ở vị trí đó, và trái cây sẽ biến mất khỏi vị trí.</p>

<p>Trả về <em><strong>tổng số</strong> trái cây lớn nhất mà bạn có thể thu hoạch</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2106.Maximum%20Fruits%20Harvested%20After%20at%20Most%20K%20Steps/images/1.png" style="width: 472px; height: 115px;" />
<pre>
<strong>Đầu vào:</strong> fruits = [[2,8],[6,3],[8,6]], startPos = 5, k = 4
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong>
Cách tối ưu là:
- Đi sang phải đến vị trí 6 và thu hoạch 3 trái cây
- Đi sang phải đến vị trí 8 và thu hoạch 6 trái cây
Bạn đã đi 3 bước và thu hoạch tổng cộng 3 + 6 = 9 trái cây.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2106.Maximum%20Fruits%20Harvested%20After%20at%20Most%20K%20Steps/images/2.png" style="width: 512px; height: 129px;" />
<pre>
<strong>Đầu vào:</strong> fruits = [[0,9],[4,1],[5,7],[6,2],[7,4],[10,9]], startPos = 5, k = 4
<strong>Đầu ra:</strong> 14
<strong>Giải thích:</strong>
Bạn chỉ có thể đi nhiều nhất k = 4 bước, nên không thể đến vị trí 0 hoặc 10.
Cách tối ưu là:
- Thu hoạch 7 trái cây tại vị trí ban đầu 5
- Đi sang trái đến vị trí 4 và thu hoạch 1 trái cây
- Đi sang phải đến vị trí 6 và thu hoạch 2 trái cây
- Đi sang phải đến vị trí 7 và thu hoạch 4 trái cây
Bạn đã đi 1 + 3 = 4 bước và thu hoạch 7 + 1 + 2 + 4 = 14 trái cây.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2106.Maximum%20Fruits%20Harvested%20After%20at%20Most%20K%20Steps/images/3.png" style="width: 476px; height: 100px;" />
<pre>
<strong>Đầu vào:</strong> fruits = [[0,3],[6,4],[8,5]], startPos = 3, k = 2
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
Bạn chỉ có thể đi nhiều nhất k = 2 bước và không thể đến vị trí nào có trái cây.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= fruits.length &lt;= 10<sup>5</sup></code></li>
	<li><code>fruits[i].length == 2</code></li>
	<li><code>0 &lt;= startPos, position<sub>i</sub> &lt;= 2 * 10<sup>5</sup></code></li>
	<li><code>position<sub>i-1</sub> &lt; position<sub>i</sub></code> với mọi <code>i &gt; 0</code>&nbsp;(<strong>đánh chỉ số từ 0</strong>)</li>
	<li><code>1 &lt;= amount<sub>i</sub> &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= k &lt;= 2 * 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Các vị trí có trái cây đã được sắp xếp, nên một hành trình tối ưu sẽ bao phủ một đoạn đóng $[l,r]$: đi đến đầu mút gần hơn trước, rồi quay lại để đi qua đầu mút còn lại. Số bước là $r-l+\min(|\textit{startPos}-l|,|r-\textit{startPos}|)$. Liệt kê mọi cặp và tính tổng trái cây sẽ tốn $O(n^2)$, không phù hợp với $n\le 10^5$.
>
> Khi cố định đầu phải, việc di chuyển đầu trái sang phải không bao giờ làm tăng số bước đó, nên ta có thể dùng hai con trỏ để tìm mọi cửa sổ cực đại có chi phí không vượt quá $k$, đồng thời cập nhật tổng trái cây tăng dần.
>
> Ta duyệt qua $\textit{fruits}$ bằng $i$ và $j$, cộng thêm $s$, thu hẹp từ bên trái khi đoạn vượt quá $k$, rồi ghi nhận giá trị lớn nhất của $s$.

<!-- thinking:end -->

Giả sử phạm vi di chuyển là $[l, r]$ và vị trí ban đầu là $\textit{startPos}$. Ta cần tính số bước ít nhất cần đi. Dựa trên vị trí của $\textit{startPos}$, ta có thể chia thành ba trường hợp:

1. Nếu $\textit{startPos} \leq l$, ta đi sang phải từ $\textit{startPos}$ đến $r$. Số bước ít nhất là $r - \textit{startPos}$;
2. Nếu $\textit{startPos} \geq r$, ta đi sang trái từ $\textit{startPos}$ đến $l$. Số bước ít nhất là $\textit{startPos} - l$;
3. Nếu $l < \textit{startPos} < r$, ta có thể đi sang trái từ $\textit{startPos}$ đến $l$ rồi sang phải đến $r$, hoặc đi sang phải từ $\textit{startPos}$ đến $r$ rồi sang trái đến $l$. Số bước ít nhất là $r - l + \min(\lvert \textit{startPos} - l \rvert, \lvert r - \textit{startPos} \rvert)$.

Cả ba trường hợp có thể được gộp bằng công thức $r - l + \min(\lvert \textit{startPos} - l \rvert, \lvert r - \textit{startPos} \rvert)$.

Giả sử ta cố định đầu phải $r$ của đoạn và di chuyển đầu trái $l$ sang phải. Hãy xem số bước ít nhất thay đổi như thế nào:

1. Nếu $\textit{startPos} \leq l$, khi $l$ tăng, số bước ít nhất không đổi.
2. Nếu $\textit{startPos} > l$, khi $l$ tăng, số bước ít nhất giảm.

Vì vậy, khi $l$ tăng, số bước ít nhất giảm không nghiêm ngặt. Dựa trên điều này, ta có thể dùng phương pháp hai con trỏ để tìm tất cả các đoạn lớn nhất thỏa mãn, sau đó chọn đoạn có tổng số trái cây lớn nhất trong số các đoạn đó làm đáp án.

Cụ thể, ta dùng hai con trỏ $i$ và $j$ trỏ đến chỉ số trái và phải của đoạn, ban đầu $i = j = 0$. Ta cũng dùng một biến $s$ để ghi nhận tổng số trái cây trong đoạn, ban đầu $s = 0$.

Mỗi lần đưa $j$ vào đoạn, ta cập nhật $s = s + \textit{fruits}[j][1]$. Nếu số bước ít nhất trong đoạn hiện tại $\textit{fruits}[j][0] - \textit{fruits}[i][0] + \min(\lvert \textit{startPos} - \textit{fruits}[i][0] \rvert, \lvert \textit{startPos} - \textit{fruits}[j][0] \rvert)$ lớn hơn $k$, ta lặp để di chuyển $i$ sang phải cho đến khi $i > j$ hoặc số bước ít nhất trong đoạn nhỏ hơn hoặc bằng $k$. Khi đó, ta cập nhật đáp án $\textit{ans} = \max(\textit{ans}, s)$. Tiếp tục di chuyển $j$ cho đến khi $j$ đi đến cuối mảng.

Cuối cùng, trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxTotalFruits(self, fruits: List[List[int]], startPos: int, k: int) -> int:
        ans = i = s = 0
        for j, (pj, fj) in enumerate(fruits):
            s += fj
            while (
                i <= j
                and pj
                - fruits[i][0]
                + min(abs(startPos - fruits[i][0]), abs(startPos - fruits[j][0]))
                > k
            ):
                s -= fruits[i][1]
                i += 1
            ans = max(ans, s)
        return ans
```

#### Java

```java
class Solution {
    public int maxTotalFruits(int[][] fruits, int startPos, int k) {
        int ans = 0, s = 0;
        for (int i = 0, j = 0; j < fruits.length; ++j) {
            int pj = fruits[j][0], fj = fruits[j][1];
            s += fj;
            while (i <= j
                && pj - fruits[i][0]
                        + Math.min(Math.abs(startPos - fruits[i][0]), Math.abs(startPos - pj))
                    > k) {
                s -= fruits[i++][1];
            }
            ans = Math.max(ans, s);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxTotalFruits(vector<vector<int>>& fruits, int startPos, int k) {
        int ans = 0, s = 0;
        for (int i = 0, j = 0; j < fruits.size(); ++j) {
            int pj = fruits[j][0], fj = fruits[j][1];
            s += fj;
            while (i <= j && pj - fruits[i][0] + min(abs(startPos - fruits[i][0]), abs(startPos - pj)) > k) {
                s -= fruits[i++][1];
            }
            ans = max(ans, s);
        }
        return ans;
    }
};
```

#### Go

```go
func maxTotalFruits(fruits [][]int, startPos int, k int) (ans int) {
	var s, i int
	for j, f := range fruits {
		s += f[1]
		for i <= j && f[0]-fruits[i][0]+min(abs(startPos-fruits[i][0]), abs(startPos-f[0])) > k {
			s -= fruits[i][1]
			i += 1
		}
		ans = max(ans, s)
	}
	return
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function maxTotalFruits(fruits: number[][], startPos: number, k: number): number {
    let ans = 0;
    let s = 0;
    for (let i = 0, j = 0; j < fruits.length; ++j) {
        const [pj, fj] = fruits[j];
        s += fj;
        while (
            i <= j &&
            pj -
                fruits[i][0] +
                Math.min(Math.abs(startPos - fruits[i][0]), Math.abs(startPos - pj)) >
                k
        ) {
            s -= fruits[i++][1];
        }
        ans = Math.max(ans, s);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_total_fruits(fruits: Vec<Vec<i32>>, start_pos: i32, k: i32) -> i32 {
        let mut ans = 0;
        let mut s = 0;
        let mut i = 0;
        for j in 0..fruits.len() {
            let pj = fruits[j][0];
            let fj = fruits[j][1];
            s += fj;
            while i <= j && pj - fruits[i][0] + std::cmp::min((start_pos - fruits[i][0]).abs(), (start_pos - pj).abs()) > k {
                s -= fruits[i][1];
                i += 1;
            }
            ans = ans.max(s)
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
