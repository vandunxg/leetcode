---
comments: true
difficulty: Medium
tags:
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [2237. Count Positions on Street With Required Brightness 🔒](https://leetcode.com/problems/count-positions-on-street-with-required-brightness)

[中文文档](/solution/2200-2299/2237.Count%20Positions%20on%20Street%20With%20Required%20Brightness/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code>. Một con đường thẳng hoàn toàn được biểu diễn bằng trục số từ <code>0</code> đến <code>n - 1</code>. Bạn được cho một mảng số nguyên 2 chiều <code>lights</code> biểu diễn các đèn đường trên đường phố. Mỗi <code>lights[i] = [position<sub>i</sub>, range<sub>i</sub>]</code> cho biết có một đèn đường tại vị trí <code>position<sub>i</sub></code> chiếu sáng khu vực từ <code>[max(0, position<sub>i</sub> - range<sub>i</sub>), min(n - 1, position<sub>i</sub> + range<sub>i</sub>)]</code> (<strong>bao gồm cả hai đầu mút</strong>).</p>

<p><strong>Độ sáng</strong> của một vị trí <code>p</code> được định nghĩa là số lượng đèn đường chiếu sáng vị trí <code>p</code>. Bạn được cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>requirement</code> có kích thước <code>n</code>, trong đó <code>requirement[i]</code> là <strong>độ sáng</strong> tối thiểu của vị trí thứ <code>i<sup>th</sup></code> trên đường phố.</p>

<p>Trả về <em>số lượng vị trí </em><code>i</code><em> trên đường phố, nằm trong khoảng từ </em><code>0</code><em> đến </em><code>n - 1</code><em>, có <strong>độ sáng </strong></em><em><strong>ít nhất bằng </strong></em><code>requirement[i]</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2237.Count%20Positions%20on%20Street%20With%20Required%20Brightness/images/screenshot-2022-04-11-at-22-24-43-diagramdrawio-diagramsnet.png" style="height: 150px; width: 579px;" />
<pre>
<strong>Đầu vào:</strong> n = 5, lights = [[0,1],[2,1],[3,2]], requirement = [0,2,1,4,1]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong>
- Đèn đường đầu tiên chiếu sáng khu vực từ [max(0, 0 - 1), min(n - 1, 0 + 1)] = [0, 1] (bao gồm cả hai đầu mút).
- Đèn đường thứ hai chiếu sáng khu vực từ [max(0, 2 - 1), min(n - 1, 2 + 1)] = [1, 3] (bao gồm cả hai đầu mút).
- Đèn đường thứ ba chiếu sáng khu vực từ [max(0, 3 - 2), min(n - 1, 3 + 2)] = [1, 4] (bao gồm cả hai đầu mút).

- Vị trí 0 được đèn đường đầu tiên chiếu sáng. Có 1 đèn đường chiếu sáng vị trí này, lớn hơn requirement[0].
- Vị trí 1 được đèn đường thứ nhất, thứ hai và thứ ba chiếu sáng. Có 3 đèn đường chiếu sáng vị trí này, lớn hơn requirement[1].
- Vị trí 2 được đèn đường thứ hai và thứ ba chiếu sáng. Có 2 đèn đường chiếu sáng vị trí này, lớn hơn requirement[2].
- Vị trí 3 được đèn đường thứ hai và thứ ba chiếu sáng. Có 2 đèn đường chiếu sáng vị trí này, nhỏ hơn requirement[3].
- Vị trí 4 được đèn đường thứ ba chiếu sáng. Có 1 đèn đường chiếu sáng vị trí này, bằng requirement[4].

Các vị trí 0, 1, 2 và 4 thỏa mãn yêu cầu, nên ta trả về 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1, lights = [[0,1]], requirement = [2]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
- Đèn đường đầu tiên chiếu sáng khu vực từ [max(0, 0 - 1), min(n - 1, 0 + 1)] = [0, 0] (bao gồm cả hai đầu mút).
- Vị trí 0 được đèn đường đầu tiên chiếu sáng. Có 1 đèn đường chiếu sáng vị trí này, nhỏ hơn requirement[0].
- Ta trả về 0 vì không có vị trí nào đạt yêu cầu về độ sáng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= lights.length &lt;= 10<sup>5</sup></code></li>
    <li><code>0 &lt;= position<sub>i</sub> &lt; n</code></li>
    <li><code>0 &lt;= range<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
    <li><code>requirement.length == n</code></li>
    <li><code>0 &lt;= requirement[i] &lt;= 10<sup>5</sup></code></li>
 </ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng hiệu

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi đèn chiếu sáng một đoạn liên tiếp; ta đếm các vị trí có độ sáng đạt yêu cầu. Nếu cập nhật từng ô cho từng đèn, độ phức tạp có thể là bậc hai. Cập nhật trên một đoạn là thao tác cơ bản của mảng hiệu.
>
> Thêm một vào $d[\max(0,p-r)]$ và trừ một ngay sau đầu phải, tính tổng tiền tố của $d$, rồi so sánh từng ô với $\textit{requirement}$.

<!-- thinking:end -->

Để đồng thời cộng một giá trị $v$ vào một khoảng liên tiếp $[i, j]$, ta có thể sử dụng mảng hiệu.

Ta định nghĩa một mảng $\textit{d}$ có độ dài $n + 1$. Với mỗi đèn đường, ta tính biên trái $i = \max(0, p - r)$ và biên phải $j = \min(n - 1, p + r)$, sau đó cộng $1$ vào $\textit{d}[i]$ và trừ $1$ tại $\textit{d}[j + 1]$.

Tiếp theo, ta tính tổng tiền tố trên $\textit{d}$. Với mỗi vị trí $i$, nếu tổng tiền tố của $\textit{d}[i]$ lớn hơn hoặc bằng $\textit{requirement}[i]$, nghĩa là vị trí đó đạt yêu cầu, và ta tăng đáp án lên một.

Cuối cùng, trả về đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số lượng đèn đường.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def meetRequirement(
        self, n: int, lights: List[List[int]], requirement: List[int]
    ) -> int:
        d = [0] * (n + 1)
        for p, r in lights:
            i, j = max(0, p - r), min(n - 1, p + r)
            d[i] += 1
            d[j + 1] -= 1
        return sum(s >= r for s, r in zip(accumulate(d), requirement))
```

#### Java

```java
class Solution {
    public int meetRequirement(int n, int[][] lights, int[] requirement) {
        int[] d = new int[n + 1];
        for (int[] e : lights) {
            int i = Math.max(0, e[0] - e[1]);
            int j = Math.min(n - 1, e[0] + e[1]);
            ++d[i];
            --d[j + 1];
        }
        int s = 0;
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            s += d[i];
            if (s >= requirement[i]) {
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
    int meetRequirement(int n, vector<vector<int>>& lights, vector<int>& requirement) {
        vector<int> d(n + 1);
        for (const auto& e : lights) {
            int i = max(0, e[0] - e[1]), j = min(n - 1, e[0] + e[1]);
            ++d[i];
            --d[j + 1];
        }
        int s = 0, ans = 0;
        for (int i = 0; i < n; ++i) {
            s += d[i];
            if (s >= requirement[i]) {
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func meetRequirement(n int, lights [][]int, requirement []int) (ans int) {
	d := make([]int, n+1)
	for _, e := range lights {
		i, j := max(0, e[0]-e[1]), min(n-1, e[0]+e[1])
		d[i]++
		d[j+1]--
	}
	s := 0
	for i, r := range requirement {
		s += d[i]
		if s >= r {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function meetRequirement(n: number, lights: number[][], requirement: number[]): number {
    const d: number[] = Array(n + 1).fill(0);
    for (const [p, r] of lights) {
        const [i, j] = [Math.max(0, p - r), Math.min(n - 1, p + r)];
        ++d[i];
        --d[j + 1];
    }
    let [ans, s] = [0, 0];
    for (let i = 0; i < n; ++i) {
        s += d[i];
        if (s >= requirement[i]) {
            ++ans;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
