---
comments: true
difficulty: Medium
tags:
    - Array
    - Matrix
---

<!-- problem:start -->

# [835. Image Overlap](https://leetcode.com/problems/image-overlap)

[中文文档](/solution/0800-0899/0835.Image%20Overlap/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai ảnh <code>img1</code> và <code>img2</code>, được biểu diễn bằng các ma trận nhị phân vuông kích thước <code>n x n</code>. Ma trận nhị phân chỉ chứa các giá trị <code>0</code> và <code>1</code>.</p>

<p>Ta có thể <strong>dịch chuyển</strong> một ảnh tùy ý bằng cách di chuyển tất cả bit <code>1</code> sang trái, phải, lên và/hoặc xuống một số đơn vị bất kỳ. Sau đó, đặt ảnh này chồng lên ảnh kia. Ta tính độ <strong>chồng lấp</strong> bằng cách đếm số vị trí có giá trị <code>1</code> trong <strong>cả hai</strong> ảnh.</p>

<p>Lưu ý, dịch chuyển <strong>không</strong> bao gồm phép xoay. Bit <code>1</code> nào bị dịch ra ngoài biên ma trận sẽ bị xóa.</p>

<p>Hãy trả về <em>độ chồng lấp lớn nhất có thể</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0835.Image%20Overlap/images/overlap1.jpg" style="width: 450px; height: 231px;" />
<pre>
<strong>Đầu vào:</strong> img1 = [[1,1,0],[0,1,0],[0,1,0]], img2 = [[0,0,0],[0,1,1],[0,0,1]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta dịch chuyển img1 sang phải 1 đơn vị và xuống dưới 1 đơn vị.
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0835.Image%20Overlap/images/overlap_step1.jpg" style="width: 450px; height: 105px;" />
Có 3 vị trí chứa 1 trong cả hai ảnh (được tô đỏ).
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0835.Image%20Overlap/images/overlap_step2.jpg" style="width: 450px; height: 231px;" />
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> img1 = [[1]], img2 = [[1]]
<strong>Đầu ra:</strong> 1
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> img1 = [[0]], img2 = [[0]]
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == img1.length == img1[i].length</code></li>
	<li><code>n == img2.length == img2[i].length</code></li>
	<li><code>1 &lt;= n &lt;= 30</code></li>
	<li><code>img1[i][j]</code> là <code>0</code> hoặc <code>1</code>.</li>
	<li><code>img2[i][j]</code> là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Ta dịch chuyển $img1$ trên $img2$ sao cho số bit 1 chồng lên nhau là lớn nhất. Vì $n\le 30$, có thể liệt kê các độ dịch chuyển; tuy nhiên, chỉ cần ghép từng bit 1 của hai ảnh với nhau.
>
> Mỗi cặp bit 1 xác định một vector dịch chuyển. Đếm số lần xuất hiện của từng vector sẽ cho độ chồng lấp lớn nhất; nếu không có vector nào thì đáp án là $0$.

<!-- thinking:end -->

Ta có thể liệt kê từng vị trí có giá trị $1$ trong $\textit{img1}$ và $\textit{img2}$, lần lượt ký hiệu là $(i, j)$ và $(h, k)$. Sau đó, tính độ lệch $(i - h, j - k)$, ký hiệu là $(dx, dy)$, rồi dùng hash table $\textit{cnt}$ để đếm số lần xuất hiện của mỗi độ lệch. Cuối cùng, duyệt hash table $\textit{cnt}$ để tìm độ lệch xuất hiện nhiều nhất; đó là đáp án.

Độ phức tạp thời gian là $O(n^4)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là độ dài cạnh của $\textit{img1}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestOverlap(self, img1: List[List[int]], img2: List[List[int]]) -> int:
        n = len(img1)
        cnt = Counter()
        for i in range(n):
            for j in range(n):
                if img1[i][j]:
                    for h in range(n):
                        for k in range(n):
                            if img2[h][k]:
                                cnt[(i - h, j - k)] += 1
        return max(cnt.values()) if cnt else 0
```

#### Java

```java
class Solution {
    public int largestOverlap(int[][] img1, int[][] img2) {
        int n = img1.length;
        Map<List<Integer>, Integer> cnt = new HashMap<>();
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (img1[i][j] == 1) {
                    for (int h = 0; h < n; ++h) {
                        for (int k = 0; k < n; ++k) {
                            if (img2[h][k] == 1) {
                                List<Integer> t = List.of(i - h, j - k);
                                ans = Math.max(ans, cnt.merge(t, 1, Integer::sum));
                            }
                        }
                    }
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
    int largestOverlap(vector<vector<int>>& img1, vector<vector<int>>& img2) {
        int n = img1.size();
        map<pair<int, int>, int> cnt;
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (img1[i][j]) {
                    for (int h = 0; h < n; ++h) {
                        for (int k = 0; k < n; ++k) {
                            if (img2[h][k]) {
                                ans = max(ans, ++cnt[{i - h, j - k}]);
                            }
                        }
                    }
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func largestOverlap(img1 [][]int, img2 [][]int) (ans int) {
	type pair struct{ x, y int }
	cnt := map[pair]int{}
	for i, row1 := range img1 {
		for j, x1 := range row1 {
			if x1 == 1 {
				for h, row2 := range img2 {
					for k, x2 := range row2 {
						if x2 == 1 {
							t := pair{i - h, j - k}
							cnt[t]++
							ans = max(ans, cnt[t])
						}
					}
				}
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function largestOverlap(img1: number[][], img2: number[][]): number {
    const n = img1.length;
    const cnt: Map<number, number> = new Map();
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < n; ++j) {
            if (img1[i][j] === 1) {
                for (let h = 0; h < n; ++h) {
                    for (let k = 0; k < n; ++k) {
                        if (img2[h][k] === 1) {
                            const t = (i - h) * 200 + (j - k);
                            cnt.set(t, (cnt.get(t) ?? 0) + 1);
                            ans = Math.max(ans, cnt.get(t)!);
                        }
                    }
                }
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
