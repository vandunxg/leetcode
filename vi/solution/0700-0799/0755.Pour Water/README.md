---
comments: true
difficulty: Medium
tags:
    - Array
    - Simulation
---

<!-- problem:start -->

# [755. Pour Water 🔒](https://leetcode.com/problems/pour-water)

[中文文档](/solution/0700-0799/0755.Pour%20Water/README.md)

## Mô tả

<!-- description:start -->

<p>Cho bản đồ độ cao được biểu diễn bằng mảng số nguyên <code>heights</code>, trong đó <code>heights[i]</code> là độ cao địa hình tại chỉ số <code>i</code>. Chiều rộng tại mỗi chỉ số là <code>1</code>. Ngoài ra, cho hai số nguyên <code>volume</code> và <code>k</code>; <code>volume</code> đơn vị nước sẽ rơi tại chỉ số <code>k</code>.</p>

<p>Nước ban đầu rơi tại chỉ số <code>k</code> và đọng trên địa hình hoặc lượng nước cao nhất ở chỉ số đó. Sau đó, nước chảy theo các quy tắc sau:</p>

<ul>
	<li>Nếu giọt nước cuối cùng sẽ rơi xuống thấp hơn khi di chuyển sang trái, hãy cho nó di chuyển sang trái.</li>
	<li>Nếu không, nhưng giọt nước cuối cùng sẽ rơi xuống thấp hơn khi di chuyển sang phải, hãy cho nó di chuyển sang phải.</li>
	<li>Nếu không, giọt nước đọng lại tại vị trí hiện tại.</li>
</ul>

<p>Ở đây, <strong>&quot;rơi xuống thấp hơn&quot;</strong> nghĩa là cuối cùng giọt nước sẽ đến một mức thấp hơn nếu di chuyển theo hướng đó. Mức được tính bằng độ cao địa hình cộng với lượng nước trong cột.</p>

<p>Có thể giả sử địa hình bên ngoài hai đầu mảng cao vô hạn. Nước không thể chia nhỏ và phân tán đều trên nhiều ô; mỗi đơn vị nước phải nằm trọn trong đúng một ô.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0755.Pour%20Water/images/pour11-grid.jpg" style="width: 450px; height: 303px;" />
<pre>
<strong>Đầu vào:</strong> heights = [2,1,1,2,1,2,2], volume = 4, k = 3
<strong>Đầu ra:</strong> [2,2,2,3,2,2,2]
<strong>Giải thích:</strong>
Giọt nước đầu tiên rơi tại chỉ số k = 3. Khi di chuyển sang trái hoặc phải, nước chỉ có thể đi đến vị trí có cùng mức hoặc thấp hơn. (Mức ở đây là tổng độ cao địa hình và lượng nước trong cột.)
Vì di chuyển sang trái cuối cùng sẽ khiến giọt nước xuống thấp hơn nên nó đi sang trái. (Giọt nước &quot;rơi xuống&quot; nghĩa là đến độ cao thấp hơn vị trí trước đó.) Vì di chuyển tiếp sang trái sẽ không làm nó xuống thấp hơn, giọt nước đọng lại tại đó.
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0755.Pour%20Water/images/pour12-grid.jpg" style="width: 400px; height: 269px;" />
Giọt nước tiếp theo rơi tại chỉ số k = 3. Vì di chuyển sang trái cuối cùng sẽ khiến giọt nước xuống thấp hơn nên nó đi sang trái. Lưu ý, giọt nước vẫn ưu tiên đi sang trái dù nó cũng có thể đi sang phải (và đi sang phải sẽ khiến nó rơi nhanh hơn).
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0755.Pour%20Water/images/pour13-grid.jpg" style="width: 400px; height: 269px;" />
Giọt nước thứ ba rơi tại chỉ số k = 3. Vì di chuyển sang trái sẽ không khiến nó xuống thấp hơn, giọt nước thử đi sang phải. Do di chuyển sang phải cuối cùng sẽ làm nó xuống thấp hơn, nó đi sang phải.
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0755.Pour%20Water/images/pour14-grid.jpg" style="width: 400px; height: 269px;" />
Cuối cùng, giọt nước thứ tư rơi tại chỉ số k = 3. Vì di chuyển sang trái sẽ không khiến nó xuống thấp hơn, giọt nước thử đi sang phải. Di chuyển sang phải cũng không làm nó xuống thấp hơn, nên nó đọng lại tại chỗ.
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0755.Pour%20Water/images/pour15-grid.jpg" style="width: 400px; height: 269px;" />
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> heights = [1,2,3,4], volume = 2, k = 2
<strong>Đầu ra:</strong> [2,3,3,4]
<strong>Giải thích:</strong> Giọt nước cuối cùng đọng tại chỉ số 1 vì di chuyển xa hơn về bên trái cũng không khiến nó rơi xuống thấp hơn.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> heights = [3,1,3], volume = 5, k = 1
<strong>Đầu ra:</strong> [4,4,4]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= heights.length &lt;= 100</code></li>
	<li><code>0 &lt;= heights[i] &lt;= 99</code></li>
	<li><code>0 &lt;= volume &lt;= 2000</code></li>
	<li><code>0 &lt;= k &lt; heights.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Thả từng đơn vị nước tại chỉ số $k$: nếu có thể thì cho chảy sang trái đến chỗ trũng thấp hơn; nếu không thì thử sang phải; nếu cũng không được thì để yên. Độ cao và lượng nước đều nhỏ.
>
> Khi đi theo hướng $d$, tiếp tục đi nếu cột kế tiếp không cao hơn cột hiện tại, đồng thời lưu lại chỉ số cuối cùng có độ cao thấp hơn hẳn—đáy chỗ trũng.
>
> Thử $d=-1$ rồi $d=1$; nếu giọt nước di chuyển thì tăng độ cao tại $j$, nếu không thì tăng tại $k$.

<!-- thinking:end -->

Ta mô phỏng quá trình rơi của từng đơn vị nước. Mỗi lần nước rơi, trước tiên thử di chuyển sang trái. Nếu có thể đến vị trí thấp hơn, nước đi đến vị trí thấp nhất; nếu không, ta thử di chuyển sang phải. Nếu có thể đến vị trí thấp hơn, nước đi đến vị trí thấp nhất; nếu không, nước đọng tại vị trí hiện tại.

Độ phức tạp thời gian là $O(v \times n)$ và độ phức tạp không gian là $O(1)$, trong đó $v$ là số đơn vị nước được thả và $n$ là độ dài mảng độ cao.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def pourWater(self, heights: List[int], volume: int, k: int) -> List[int]:
        for _ in range(volume):
            for d in (-1, 1):
                i = j = k
                while 0 <= i + d < len(heights) and heights[i + d] <= heights[i]:
                    if heights[i + d] < heights[i]:
                        j = i + d
                    i += d
                if j != k:
                    heights[j] += 1
                    break
            else:
                heights[k] += 1
        return heights
```

#### Java

```java
class Solution {
    public int[] pourWater(int[] heights, int volume, int k) {
        while (volume-- > 0) {
            boolean find = false;
            for (int d = -1; d < 2 && !find; d += 2) {
                int i = k, j = k;
                while (i + d >= 0 && i + d < heights.length && heights[i + d] <= heights[i]) {
                    if (heights[i + d] < heights[i]) {
                        j = i + d;
                    }
                    i += d;
                }
                if (j != k) {
                    find = true;
                    ++heights[j];
                }
            }
            if (!find) {
                ++heights[k];
            }
        }
        return heights;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> pourWater(vector<int>& heights, int volume, int k) {
        while (volume--) {
            bool find = false;
            for (int d = -1; d < 2 && !find; d += 2) {
                int i = k, j = k;
                while (i + d >= 0 && i + d < heights.size() && heights[i + d] <= heights[i]) {
                    if (heights[i + d] < heights[i]) {
                        j = i + d;
                    }
                    i += d;
                }
                if (j != k) {
                    find = true;
                    ++heights[j];
                }
            }
            if (!find) {
                ++heights[k];
            }
        }
        return heights;
    }
};
```

#### Go

```go
func pourWater(heights []int, volume int, k int) []int {
	for ; volume > 0; volume-- {
		find := false
		for _, d := range [2]int{-1, 1} {
			i, j := k, k
			for i+d >= 0 && i+d < len(heights) && heights[i+d] <= heights[i] {
				if heights[i+d] < heights[i] {
					j = i + d
				}
				i += d
			}
			if j != k {
				find = true
				heights[j]++
				break
			}
		}
		if !find {
			heights[k]++
		}
	}
	return heights
}
```

#### TypeScript

```ts
function pourWater(heights: number[], volume: number, k: number): number[] {
    while (volume-- > 0) {
        let find = false;
        for (let d = -1; d < 2 && !find; d += 2) {
            let i = k,
                j = k;
            while (i + d >= 0 && i + d < heights.length && heights[i + d] <= heights[i]) {
                if (heights[i + d] < heights[i]) {
                    j = i + d;
                }
                i += d;
            }
            if (j !== k) {
                find = true;
                ++heights[j];
            }
        }
        if (!find) {
            ++heights[k];
        }
    }
    return heights;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
