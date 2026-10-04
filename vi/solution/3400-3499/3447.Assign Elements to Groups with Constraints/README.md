---
comments: true
difficulty: Medium
rating: 1730
source: Weekly Contest 436 Q2
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [3447. Assign Elements to Groups with Constraints](https://leetcode.com/problems/assign-elements-to-groups-with-constraints)

[中文文档](/solution/3400-3499/3447.Assign%20Elements%20to%20Groups%20with%20Constraints/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>groups</code>, trong đó <code>groups[i]</code> biểu thị kích thước của nhóm thứ <code>i<sup>th</sup></code>. Bạn cũng được cho một mảng số nguyên <code>elements</code>.</p>

<p>Nhiệm vụ của bạn là gán <strong>một</strong> phần tử cho mỗi nhóm dựa trên các quy tắc sau:</p>

<ul>
	<li>Phần tử tại chỉ số <code>j</code> có thể được gán cho nhóm <code>i</code> nếu <code>groups[i]</code> <strong>chia hết</strong> cho <code>elements[j]</code>.</li>
	<li>Nếu có nhiều phần tử có thể được gán, hãy gán phần tử có <strong>chỉ số nhỏ nhất</strong> <code>j</code>.</li>
	<li>Nếu không có phần tử nào thỏa mãn điều kiện của một nhóm, hãy gán -1 cho nhóm đó.</li>
</ul>

<p>Trả về một mảng số nguyên <code>assigned</code>, trong đó <code>assigned[i]</code> là chỉ số của phần tử được chọn cho nhóm <code>i</code>, hoặc -1 nếu không tồn tại phần tử phù hợp.</p>

<p><strong>Lưu ý</strong>: Một phần tử có thể được gán cho nhiều nhóm.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">groups = [8,4,3,2,4], elements = [4,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,0,-1,1,0]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><code>elements[0] = 4</code> được gán cho các nhóm 0, 1 và 4.</li>
	<li><code>elements[1] = 2</code> được gán cho nhóm 3.</li>
	<li>Không thể gán phần tử nào cho nhóm 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">groups = [2,3,5,7], elements = [5,3,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[-1,1,0,-1]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><code>elements[1] = 3</code> được gán cho nhóm 1.</li>
	<li><code>elements[0] = 5</code> được gán cho nhóm 2.</li>
	<li>Không thể gán phần tử nào cho các nhóm 0 và 3.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">groups = [10,21,30,41], elements = [2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,1,0,1]</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>elements[0] = 2</code> được gán cho các nhóm có giá trị chẵn, còn <code>elements[1] = 1</code> được gán cho các nhóm có giá trị lẻ.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= groups.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= elements.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= groups[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= elements[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi $\textit{groups}[i]$ cần chỉ số nhỏ nhất $j$ sao cho $\textit{elements}[j]$ là ước của nó. Duyệt qua mọi phần tử cho từng nhóm là quá chậm khi $n,m\le 10^5$.
>
> Các giá trị không vượt quá $10^5$. Đánh dấu các bội số của từng ước theo kiểu sàng sẽ truy cập mỗi số một số lần có tổng bằng một số điều hòa.
>
> Duyệt từ trái sang phải, một $x$ chưa được đánh dấu sẽ ghi chỉ số của nó vào $x,2x,\ldots\le M$. Đáp án của một nhóm là $\textit{d}[\textit{groups}[i]]$. Các phần tử trùng lặp của $x$ về sau sẽ bị bỏ qua, nên chỉ số lớn hơn không thể ghi đè.

<!-- thinking:end -->

Đầu tiên, ta tìm giá trị lớn nhất trong mảng $\textit{groups}$, ký hiệu là $\textit{mx}$. Ta dùng một mảng $\textit{d}$ để ghi lại chỉ số tương ứng với mỗi phần tử. Ban đầu, $\textit{d}[x] = -1$ cho biết phần tử $x$ chưa được gán.

Sau đó, ta duyệt qua mảng $\textit{elements}$. Với mỗi phần tử $x$, nếu $x > \textit{mx}$ hoặc $\textit{d}[x] \neq -1$, điều đó có nghĩa là phần tử $x$ không thể được gán hoặc đã được xử lý, nên ta bỏ qua ngay. Nếu không, bắt đầu từ $x$, mỗi lần tăng thêm $x$ và gán $\textit{d}[y]$ bằng $j$, cho biết phần tử $y$ được gán cho chỉ số $j$.

Cuối cùng, ta duyệt qua mảng $\textit{groups}$ và lấy đáp án dựa trên các giá trị được ghi trong mảng $\textit{d}$.

Độ phức tạp thời gian là $O(M \times \log m + n)$, còn độ phức tạp không gian là $O(M)$. Ở đây, $n$ và $m$ lần lượt là độ dài của các mảng $\textit{groups}$ và $\textit{elements}$, còn $M$ là giá trị lớn nhất trong mảng $\textit{groups}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def assignElements(self, groups: List[int], elements: List[int]) -> List[int]:
        mx = max(groups)
        d = [-1] * (mx + 1)
        for j, x in enumerate(elements):
            if x > mx or d[x] != -1:
                continue
            for y in range(x, mx + 1, x):
                if d[y] == -1:
                    d[y] = j
        return [d[x] for x in groups]
```

#### Java

```java
class Solution {
    public int[] assignElements(int[] groups, int[] elements) {
        int mx = Arrays.stream(groups).max().getAsInt();
        int[] d = new int[mx + 1];
        Arrays.fill(d, -1);
        for (int j = 0; j < elements.length; ++j) {
            int x = elements[j];
            if (x > mx || d[x] != -1) {
                continue;
            }
            for (int y = x; y <= mx; y += x) {
                if (d[y] == -1) {
                    d[y] = j;
                }
            }
        }
        int n = groups.length;
        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            ans[i] = d[groups[i]];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> assignElements(vector<int>& groups, vector<int>& elements) {
        int mx = ranges::max(groups);
        vector<int> d(mx + 1, -1);

        for (int j = 0; j < elements.size(); ++j) {
            int x = elements[j];
            if (x > mx || d[x] != -1) {
                continue;
            }
            for (int y = x; y <= mx; y += x) {
                if (d[y] == -1) {
                    d[y] = j;
                }
            }
        }

        vector<int> ans(groups.size());
        for (int i = 0; i < groups.size(); ++i) {
            ans[i] = d[groups[i]];
        }

        return ans;
    }
};
```

#### Go

```go
func assignElements(groups []int, elements []int) (ans []int) {
	mx := slices.Max(groups)
	d := make([]int, mx+1)
	for i := range d {
		d[i] = -1
	}
	for j, x := range elements {
		if x > mx || d[x] != -1 {
			continue
		}
		for y := x; y <= mx; y += x {
			if d[y] == -1 {
				d[y] = j
			}
		}
	}
	for _, x := range groups {
		ans = append(ans, d[x])
	}
	return
}
```

#### TypeScript

```ts
function assignElements(groups: number[], elements: number[]): number[] {
    const mx = Math.max(...groups);
    const d: number[] = Array(mx + 1).fill(-1);
    for (let j = 0; j < elements.length; ++j) {
        const x = elements[j];
        if (x > mx || d[x] !== -1) {
            continue;
        }
        for (let y = x; y <= mx; y += x) {
            if (d[y] === -1) {
                d[y] = j;
            }
        }
    }
    return groups.map(x => d[x]);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
