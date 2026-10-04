---
comments: true
difficulty: Medium
rating: 1523
source: Weekly Contest 398 Q2
tags:
    - Array
    - Binary Search
    - Prefix Sum
---

<!-- problem:start -->

# [3152. Special Array II](https://leetcode.com/problems/special-array-ii)

[中文文档](/solution/3100-3199/3152.Special%20Array%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Một mảng được coi là <strong>đặc biệt</strong> nếu mọi cặp phần tử liền kề của nó chứa hai số có tính chẵn lẻ khác nhau.</p>

<p>Bạn được cho một mảng số nguyên <code>nums</code> và một ma trận số nguyên 2D <code>queries</code>, trong đó với <code>queries[i] = [from<sub>i</sub>, to<sub>i</sub>]</code>, nhiệm vụ của bạn là kiểm tra xem <span data-keyword="subarray">mảng con</span> <code>nums[from<sub>i</sub>..to<sub>i</sub>]</code> có <strong>đặc biệt</strong> hay không.</p>

<p>Trả về một mảng boolean <code>answer</code> sao cho <code>answer[i]</code> là <code>true</code> nếu <code>nums[from<sub>i</sub>..to<sub>i</sub>]</code> là đặc biệt.<!-- notionvc: e5d6f4e2-d20a-4fbd-9c7f-22fbe52ef730 --></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,4,1,2,6], queries = [[0,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[false]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng con là <code>[3,4,1,2,6]</code>. 2 và 6 đều là số chẵn.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,3,1,6], queries = [[0,2],[2,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[false,true]</span></p>

<p><strong>Giải thích:</strong></p>

<ol>
	<li>Mảng con là <code>[4,3,1]</code>. 3 và 1 đều là số lẻ. Vì vậy, đáp án cho truy vấn này là <code>false</code>.</li>
	<li>Mảng con là <code>[1,6]</code>. Chỉ có một cặp: <code>(1,6)</code> và cặp này chứa hai số có tính chẵn lẻ khác nhau. Vì vậy, đáp án cho truy vấn này là <code>true</code>.</li>
</ol>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>queries[i].length == 2</code></li>
	<li><code>0 &lt;= queries[i][0] &lt;= queries[i][1] &lt;= nums.length - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Ghi lại vị trí bắt đầu nhỏ nhất của mảng đặc biệt cho mỗi vị trí

<!-- thinking:start -->

> **Tư duy**
>
> Nhiều truy vấn yêu cầu kiểm tra xem một mảng con có đặc biệt hay không. Nếu quét từng đoạn, độ phức tạp sẽ là $O(nq)$.
>
> Một đoạn là đặc biệt khi và chỉ khi mọi cặp phần tử liền kề đều đổi tính chẵn lẻ. Nếu $d[i]$ là vị trí bắt đầu nhỏ nhất của dãy đặc biệt bao phủ $i$, truy vấn $[f,t]$ đúng khi $d[t]\le f$.
>
> Một lượt duyệt từ trái sang phải sẽ đặt $d[i]=d[i-1]$ khi tính chẵn lẻ thay đổi và đặt $d[i]=i$ trong trường hợp ngược lại. Khi đó, mỗi truy vấn có thể xử lý trong $O(1)$.

<!-- thinking:end -->

Ta có thể định nghĩa một mảng $d$ để ghi lại vị trí bắt đầu nhỏ nhất của mảng đặc biệt cho mỗi vị trí, ban đầu $d[i] = i$. Sau đó, ta duyệt mảng $nums$ từ trái sang phải. Nếu $nums[i]$ và $nums[i - 1]$ có tính chẵn lẻ khác nhau, thì $d[i] = d[i - 1]$.

Cuối cùng, ta duyệt từng truy vấn và kiểm tra xem $d[to] <= from$ có đúng hay không.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isArraySpecial(self, nums: List[int], queries: List[List[int]]) -> List[bool]:
        n = len(nums)
        d = list(range(n))
        for i in range(1, n):
            if nums[i] % 2 != nums[i - 1] % 2:
                d[i] = d[i - 1]
        return [d[t] <= f for f, t in queries]
```

#### Java

```java
class Solution {
    public boolean[] isArraySpecial(int[] nums, int[][] queries) {
        int n = nums.length;
        int[] d = new int[n];
        for (int i = 1; i < n; ++i) {
            if (nums[i] % 2 != nums[i - 1] % 2) {
                d[i] = d[i - 1];
            } else {
                d[i] = i;
            }
        }
        int m = queries.length;
        boolean[] ans = new boolean[m];
        for (int i = 0; i < m; ++i) {
            ans[i] = d[queries[i][1]] <= queries[i][0];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<bool> isArraySpecial(vector<int>& nums, vector<vector<int>>& queries) {
        int n = nums.size();
        vector<int> d(n);
        iota(d.begin(), d.end(), 0);
        for (int i = 1; i < n; ++i) {
            if (nums[i] % 2 != nums[i - 1] % 2) {
                d[i] = d[i - 1];
            }
        }
        vector<bool> ans;
        for (auto& q : queries) {
            ans.push_back(d[q[1]] <= q[0]);
        }
        return ans;
    }
};
```

#### Go

```go
func isArraySpecial(nums []int, queries [][]int) (ans []bool) {
	n := len(nums)
	d := make([]int, n)
	for i := range d {
		d[i] = i
	}
	for i := 1; i < len(nums); i++ {
		if nums[i]%2 != nums[i-1]%2 {
			d[i] = d[i-1]
		}
	}
	for _, q := range queries {
		ans = append(ans, d[q[1]] <= q[0])
	}
	return
}
```

#### TypeScript

```ts
function isArraySpecial(nums: number[], queries: number[][]): boolean[] {
    const n = nums.length;
    const d: number[] = Array.from({ length: n }, (_, i) => i);
    for (let i = 1; i < n; ++i) {
        if (nums[i] % 2 !== nums[i - 1] % 2) {
            d[i] = d[i - 1];
        }
    }
    return queries.map(([from, to]) => d[to] <= from);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
