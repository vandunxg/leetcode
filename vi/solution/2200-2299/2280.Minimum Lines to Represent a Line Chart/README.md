---
comments: true
difficulty: Medium
rating: 1680
source: Weekly Contest 294 Q3
tags:
    - Geometry
    - Array
    - Math
    - Number Theory
    - Sorting
---

<!-- problem:start -->

# [2280. Minimum Lines to Represent a Line Chart](https://leetcode.com/problems/minimum-lines-to-represent-a-line-chart)

[中文文档](/solution/2200-2299/2280.Minimum%20Lines%20to%20Represent%20a%20Line%20Chart/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên 2 chiều <code>stockPrices</code>, trong đó <code>stockPrices[i] = [day<sub>i</sub>, price<sub>i</sub>]</code> cho biết giá cổ phiếu vào ngày <code>day<sub>i</sub></code> là <code>price<sub>i</sub></code>. Một <strong>biểu đồ đường</strong> được tạo từ mảng này bằng cách biểu diễn các điểm trên mặt phẳng XY, với trục X biểu diễn ngày và trục Y biểu diễn giá, rồi nối các điểm kề nhau. Một ví dụ được hiển thị bên dưới:</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2280.Minimum%20Lines%20to%20Represent%20a%20Line%20Chart/images/1920px-pushkin_population_historysvg.png" style="width: 500px; height: 313px;" />
<p>Hãy trả về <em><strong>số đường thẳng nhỏ nhất</strong> cần thiết để biểu diễn biểu đồ đường</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2280.Minimum%20Lines%20to%20Represent%20a%20Line%20Chart/images/ex0.png" style="width: 400px; height: 400px;" />
<pre>
<strong>Đầu vào:</strong> stockPrices = [[1,7],[2,6],[3,5],[4,4],[5,4],[6,3],[7,2],[8,1]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Biểu đồ trên biểu diễn đầu vào, với trục X biểu diễn ngày và trục Y biểu diễn giá.
Có thể vẽ 3 đường thẳng sau để biểu diễn biểu đồ đường:
- Đường thẳng 1 (màu đỏ) từ (1,7) đến (4,4), đi qua (1,7), (2,6), (3,5) và (4,4).
- Đường thẳng 2 (màu xanh dương) từ (4,4) đến (5,4).
- Đường thẳng 3 (màu xanh lá) từ (5,4) đến (8,1), đi qua (5,4), (6,3), (7,2) và (8,1).
Có thể chứng minh rằng không thể biểu diễn biểu đồ đường bằng ít hơn 3 đường thẳng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2280.Minimum%20Lines%20to%20Represent%20a%20Line%20Chart/images/ex1.png" style="width: 325px; height: 325px;" />
<pre>
<strong>Đầu vào:</strong> stockPrices = [[3,4],[1,2],[7,8],[2,3]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
Như trong hình trên, biểu đồ đường có thể được biểu diễn bằng một đường thẳng duy nhất.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= stockPrices.length &lt;= 10<sup>5</sup></code></li>
	<li><code>stockPrices[i].length == 2</code></li>
	<li><code>1 &lt;= day<sub>i</sub>, price<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
	<li>Tất cả <code>day<sub>i</sub></code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta nối các mức giá theo ngày; các đoạn liên tiếp thẳng hàng nằm trên cùng một đường thẳng. Có $10^5$ điểm với các ngày đôi một khác nhau. Sắp xếp theo ngày rồi kiểm tra xem các hệ số góc kề nhau có bằng nhau không. Phép chia có thể gây sai số, nên ta so sánh bằng phép nhân chéo.
>
> Lưu $(\Delta x,\Delta y)$ trước đó; cần thêm một đường thẳng khi $\Delta y\cdot\Delta x_1 \ne \Delta x\cdot\Delta y_1$. Một điểm duy nhất cần 0 đường thẳng.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumLines(self, stockPrices: List[List[int]]) -> int:
        stockPrices.sort()
        dx, dy = 0, 1
        ans = 0
        for (x, y), (x1, y1) in pairwise(stockPrices):
            dx1, dy1 = x1 - x, y1 - y
            if dy * dx1 != dx * dy1:
                ans += 1
            dx, dy = dx1, dy1
        return ans
```

#### Java

```java
class Solution {
    public int minimumLines(int[][] stockPrices) {
        Arrays.sort(stockPrices, (a, b) -> a[0] - b[0]);
        int dx = 0, dy = 1;
        int ans = 0;
        for (int i = 1; i < stockPrices.length; ++i) {
            int x = stockPrices[i - 1][0], y = stockPrices[i - 1][1];
            int x1 = stockPrices[i][0], y1 = stockPrices[i][1];
            int dx1 = x1 - x, dy1 = y1 - y;
            if (dy * dx1 != dx * dy1) {
                ++ans;
            }
            dx = dx1;
            dy = dy1;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumLines(vector<vector<int>>& stockPrices) {
        sort(stockPrices.begin(), stockPrices.end());
        int dx = 0, dy = 1;
        int ans = 0;
        for (int i = 1; i < stockPrices.size(); ++i) {
            int x = stockPrices[i - 1][0], y = stockPrices[i - 1][1];
            int x1 = stockPrices[i][0], y1 = stockPrices[i][1];
            int dx1 = x1 - x, dy1 = y1 - y;
            if ((long long) dy * dx1 != (long long) dx * dy1) ++ans;
            dx = dx1;
            dy = dy1;
        }
        return ans;
    }
};
```

#### Go

```go
func minimumLines(stockPrices [][]int) int {
	ans := 0
	sort.Slice(stockPrices, func(i, j int) bool { return stockPrices[i][0] < stockPrices[j][0] })
	for i, dx, dy := 1, 0, 1; i < len(stockPrices); i++ {
		x, y := stockPrices[i-1][0], stockPrices[i-1][1]
		x1, y1 := stockPrices[i][0], stockPrices[i][1]
		dx1, dy1 := x1-x, y1-y
		if dy*dx1 != dx*dy1 {
			ans++
		}
		dx, dy = dx1, dy1
	}
	return ans
}
```

#### TypeScript

```ts
function minimumLines(stockPrices: number[][]): number {
    const n = stockPrices.length;
    stockPrices.sort((a, b) => a[0] - b[0]);
    let ans = 0;
    let pre = [BigInt(0), BigInt(0)];
    for (let i = 1; i < n; i++) {
        const [x1, y1] = stockPrices[i - 1];
        const [x2, y2] = stockPrices[i];
        const dx = BigInt(x2 - x1),
            dy = BigInt(y2 - y1);
        if (i == 1 || dx * pre[1] !== dy * pre[0]) ans++;
        pre = [dx, dy];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
