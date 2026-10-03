---
comments: true
difficulty: Medium
tags:
    - Array
---

<!-- problem:start -->

# [2113. Elements in Array After Removing and Replacing Elements 🔒](https://leetcode.com/problems/elements-in-array-after-removing-and-replacing-elements)

[中文文档](/solution/2100-2199/2113.Elements%20in%20Array%20After%20Removing%20and%20Replacing%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums</code>. Ban đầu tại phút <code>0</code>, mảng không thay đổi. Mỗi phút, phần tử <strong>ngoài cùng bên trái</strong> trong <code>nums</code> sẽ bị xóa cho đến khi không còn phần tử nào. Sau đó, mỗi phút, một phần tử được thêm vào <strong>cuối</strong> <code>nums</code> theo đúng thứ tự các phần tử đã bị xóa, cho đến khi mảng ban đầu được khôi phục. Quá trình này lặp lại vô hạn.</p>

<ul>
	<li>Ví dụ, mảng <code>[0,1,2]</code> sẽ thay đổi như sau: <code>[0,1,2] &rarr; [1,2] &rarr; [2] &rarr; [] &rarr; [0] &rarr; [0,1] &rarr; [0,1,2] &rarr; [1,2] &rarr; [2] &rarr; [] &rarr; [0] &rarr; [0,1] &rarr; [0,1,2] &rarr; ...</code></li>
</ul>

<p>Bạn cũng được cho một mảng số nguyên 2 chiều <code>queries</code> có kích thước <code>n</code>, trong đó <code>queries[j] = [time<sub>j</sub>, index<sub>j</sub>]</code>. Câu trả lời cho truy vấn thứ <code>j<sup>th</sup></code> là:</p>

<ul>
	<li><code>nums[index<sub>j</sub>]</code> nếu <code>index<sub>j</sub> &lt; nums.length</code> tại phút <code>time<sub>j</sub></code></li>
	<li><code>-1</code> nếu <code>index<sub>j</sub> &gt;= nums.length</code> tại phút <code>time<sub>j</sub></code></li>
</ul>

<p>Trả về <em>một mảng số nguyên <code>ans</code> có kích thước </em><code>n</code> <em>trong đó </em><code>ans[j]</code><em> là câu trả lời cho truy vấn thứ </em><code>j<sup>th</sup></code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,1,2], queries = [[0,2],[2,0],[3,2],[5,0]]
<strong>Đầu ra:</strong> [2,2,-1,0]
<strong>Giải thích:</strong>
Phút 0: [0,1,2] - Tất cả phần tử đều nằm trong nums.
Phút 1: [1,2]   - Phần tử ngoài cùng bên trái, 0, bị xóa.
Phút 2: [2]     - Phần tử ngoài cùng bên trái, 1, bị xóa.
Phút 3: []      - Phần tử ngoài cùng bên trái, 2, bị xóa.
Phút 4: [0]     - 0 được thêm vào cuối nums.
Phút 5: [0,1]   - 1 được thêm vào cuối nums.

Tại phút 0, nums[2] là 2.
Tại phút 2, nums[0] là 2.
Tại phút 3, nums[2] không tồn tại.
Tại phút 5, nums[0] là 0.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2], queries = [[0,0],[1,0],[2,0],[3,0]]
<strong>Đầu ra:</strong> [2,-1,2,-1]
Phút 0: [2] - Tất cả phần tử đều nằm trong nums.
Phút 1: []  - Phần tử ngoài cùng bên trái, 2, bị xóa.
Phút 2: [2] - 2 được thêm vào cuối nums.
Phút 3: []  - Phần tử ngoài cùng bên trái, 2, bị xóa.

Tại phút 0, nums[0] là 2.
Tại phút 1, nums[0] không tồn tại.
Tại phút 2, nums[0] là 2.
Tại phút 3, nums[0] không tồn tại.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 100</code></li>
	<li><code>n == queries.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>queries[j].length == 2</code></li>
	<li><code>0 &lt;= time<sub>j</sub> &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= index<sub>j</sub> &lt; nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tính trực tiếp

<!-- thinking:start -->

> **Tư duy**
>
> Mảng lặp lại sau mỗi $2n$ giây: $n$ giây đầu xóa các phần tử từ bên trái, $n$ giây tiếp theo khôi phục mảng từ bên trái. Với tối đa $10^5$ truy vấn, không thể tạo trạng thái mảng cho từng mốc thời gian.
>
> Sau khi lấy $t\bmod 2n$, ta chỉ cần phân biệt giai đoạn xóa với giai đoạn khôi phục, rồi ánh xạ chỉ số của truy vấn trở lại $\textit{nums}$. Trong giai đoạn xóa, độ dài là $n-t$ và chỉ số $i$ là chỉ số ban đầu $i+t$; trong giai đoạn khôi phục, độ dài là $t-n$ và chỉ số $i$ là $\textit{nums}[i]$.
>
> Mỗi truy vấn được trả lời trong $O(1)$; các chỉ số nằm ngoài phạm vi vẫn nhận giá trị $-1$.

<!-- thinking:end -->

Đầu tiên, chúng ta khởi tạo mảng $ans$ có độ dài $m$ để lưu các câu trả lời, đồng thời gán tất cả phần tử bằng $-1$.

Tiếp theo, chúng ta duyệt qua mảng $queries$. Với mỗi truy vấn, trước tiên lấy thời điểm hiện tại $t$ và chỉ số $i$. Sau đó, lấy $t$ modulo $2n$ rồi so sánh $t$ với $n$:

- Nếu $t < n$, số phần tử của mảng tại thời điểm $t$ là $n - t$, và các phần tử của mảng là kết quả của việc dịch trái các phần tử trong mảng ban đầu $t$ vị trí. Do đó, nếu $i < n - t$, câu trả lời là $nums[i + t]$;
- Nếu $t > n$, số phần tử của mảng tại thời điểm $t$ là $t - n$, và các phần tử của mảng là $t - n$ phần tử đầu tiên của mảng ban đầu. Do đó, nếu $i < t - n$, câu trả lời là $nums[i]$.

Cuối cùng, trả về mảng $ans$.

Độ phức tạp thời gian là $O(m)$, trong đó $m$ là độ dài của mảng $queries$. Không tính phần bộ nhớ dùng cho mảng kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def elementInNums(self, nums: List[int], queries: List[List[int]]) -> List[int]:
        n, m = len(nums), len(queries)
        ans = [-1] * m
        for j, (t, i) in enumerate(queries):
            t %= 2 * n
            if t < n and i < n - t:
                ans[j] = nums[i + t]
            elif t > n and i < t - n:
                ans[j] = nums[i]
        return ans
```

#### Java

```java
class Solution {
    public int[] elementInNums(int[] nums, int[][] queries) {
        int n = nums.length, m = queries.length;
        int[] ans = new int[m];
        for (int j = 0; j < m; ++j) {
            ans[j] = -1;
            int t = queries[j][0], i = queries[j][1];
            t %= (2 * n);
            if (t < n && i < n - t) {
                ans[j] = nums[i + t];
            } else if (t > n && i < t - n) {
                ans[j] = nums[i];
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
    vector<int> elementInNums(vector<int>& nums, vector<vector<int>>& queries) {
        int n = nums.size(), m = queries.size();
        vector<int> ans(m, -1);
        for (int j = 0; j < m; ++j) {
            int t = queries[j][0], i = queries[j][1];
            t %= (n * 2);
            if (t < n && i < n - t) {
                ans[j] = nums[i + t];
            } else if (t > n && i < t - n) {
                ans[j] = nums[i];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func elementInNums(nums []int, queries [][]int) []int {
	n, m := len(nums), len(queries)
	ans := make([]int, m)
	for j, q := range queries {
		t, i := q[0], q[1]
		t %= (n * 2)
		ans[j] = -1
		if t < n && i < n-t {
			ans[j] = nums[i+t]
		} else if t > n && i < t-n {
			ans[j] = nums[i]
		}
	}
	return ans
}
```

#### TypeScript

```ts
function elementInNums(nums: number[], queries: number[][]): number[] {
    const n = nums.length;
    const m = queries.length;
    const ans: number[] = Array(m).fill(-1);
    for (let j = 0; j < m; ++j) {
        let [t, i] = queries[j];
        t %= 2 * n;
        if (t < n && i < n - t) {
            ans[j] = nums[i + t];
        } else if (t >= n && i < t - n) {
            ans[j] = nums[i];
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
