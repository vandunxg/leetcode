---
comments: true
difficulty: Easy
rating: 1281
source: Weekly Contest 333 Q1
tags:
    - Array
    - Hash Table
    - Two Pointers
---

<!-- problem:start -->

# [2570. Merge Two 2D Arrays by Summing Values](https://leetcode.com/problems/merge-two-2d-arrays-by-summing-values)

[中文文档](/solution/2500-2599/2570.Merge%20Two%202D%20Arrays%20by%20Summing%20Values/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng số nguyên <strong>2D</strong> <code>nums1</code> và <code>nums2.</code></p>

<ul>
	<li><code>nums1[i] = [id<sub>i</sub>, val<sub>i</sub>]</code>&nbsp;cho biết số có id <code>id<sub>i</sub></code> có giá trị bằng <code>val<sub>i</sub></code>.</li>
	<li><code>nums2[i] = [id<sub>i</sub>, val<sub>i</sub>]</code>&nbsp;cho biết số có id <code>id<sub>i</sub></code> có giá trị bằng <code>val<sub>i</sub></code>.</li>
</ul>

<p>Mỗi mảng chứa các id <strong>không trùng nhau</strong> và được sắp xếp theo thứ tự <strong>tăng dần</strong> của id.</p>

<p>Gộp hai mảng thành một mảng được sắp xếp theo thứ tự tăng dần của id, đồng thời thỏa mãn các điều kiện sau:</p>

<ul>
	<li>Chỉ đưa vào mảng kết quả những id xuất hiện trong ít nhất một trong hai mảng.</li>
	<li>Mỗi id chỉ được đưa vào <strong>một lần</strong> và giá trị của nó là tổng các giá trị của id này trong hai mảng. Nếu id không tồn tại trong một mảng, xem giá trị của nó trong mảng đó là <code>0</code>.</li>
</ul>

<p>Trả về <em>mảng kết quả</em>. Mảng trả về phải được sắp xếp theo thứ tự tăng dần của id.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [[1,2],[2,3],[4,5]], nums2 = [[1,4],[3,2],[4,1]]
<strong>Đầu ra:</strong> [[1,6],[2,3],[3,2],[4,6]]
<strong>Giải thích:</strong> Mảng kết quả chứa:
- id = 1, giá trị của id này là 2 + 4 = 6.
- id = 2, giá trị của id này là 3.
- id = 3, giá trị của id này là 2.
- id = 4, giá trị của id này là 5 + 1 = 6.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [[2,4],[3,6],[5,5]], nums2 = [[1,3],[4,3]]
<strong>Đầu ra:</strong> [[1,3],[2,4],[3,6],[4,3],[5,5]]
<strong>Giải thích:</strong> Không có id chung, nên ta chỉ cần đưa từng id cùng giá trị của nó vào danh sách kết quả.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums1.length, nums2.length &lt;= 200</code></li>
	<li><code>nums1[i].length == nums2[j].length == 2</code></li>
	<li><code>1 &lt;= id<sub>i</sub>, val<sub>i</sub> &lt;= 1000</code></li>
	<li>Hai mảng chứa các id không trùng nhau.</li>
	<li>Cả hai mảng đều được sắp xếp theo thứ tự tăng dần của id.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Gộp hai danh sách các cặp được sắp xếp theo $id$, đồng thời cộng các giá trị có cùng $id$. Có thể dùng two-pointer để trộn, nhưng id không vượt quá $1000$, nên chỉ cần một bộ đếm rồi sắp xếp các phần tử của nó.

<!-- thinking:end -->

Ta có thể dùng hash table hoặc một mảng `cnt` để cộng dồn giá trị của từng id trong hai mảng.

Sau đó, ta duyệt từng id trong `cnt` từ nhỏ đến lớn. Nếu tổng giá trị của một id lớn hơn $0$, ta thêm id đó vào mảng kết quả.

Độ phức tạp thời gian là $O(n + m)$, còn độ phức tạp không gian là $O(M)$. Trong đó, $n$ và $m$ lần lượt là độ dài của hai mảng; còn $M$ là giá trị lớn nhất trong hai mảng, trong bài này, $M = 1000$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def mergeArrays(
        self, nums1: List[List[int]], nums2: List[List[int]]
    ) -> List[List[int]]:
        cnt = Counter()
        for i, v in nums1 + nums2:
            cnt[i] += v
        return sorted(cnt.items())
```

#### Java

```java
class Solution {
    public int[][] mergeArrays(int[][] nums1, int[][] nums2) {
        int[] cnt = new int[1001];
        for (var x : nums1) {
            cnt[x[0]] += x[1];
        }
        for (var x : nums2) {
            cnt[x[0]] += x[1];
        }
        int n = 0;
        for (int i = 0; i < 1001; ++i) {
            if (cnt[i] > 0) {
                ++n;
            }
        }
        int[][] ans = new int[n][2];
        for (int i = 0, j = 0; i < 1001; ++i) {
            if (cnt[i] > 0) {
                ans[j++] = new int[] {i, cnt[i]};
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
    vector<vector<int>> mergeArrays(vector<vector<int>>& nums1, vector<vector<int>>& nums2) {
        int cnt[1001]{};
        for (auto& x : nums1) {
            cnt[x[0]] += x[1];
        }
        for (auto& x : nums2) {
            cnt[x[0]] += x[1];
        }
        vector<vector<int>> ans;
        for (int i = 0; i < 1001; ++i) {
            if (cnt[i]) {
                ans.push_back({i, cnt[i]});
            }
        }
        return ans;
    }
};
```

#### Go

```go
func mergeArrays(nums1 [][]int, nums2 [][]int) (ans [][]int) {
	cnt := [1001]int{}
	for _, x := range nums1 {
		cnt[x[0]] += x[1]
	}
	for _, x := range nums2 {
		cnt[x[0]] += x[1]
	}
	for i, x := range cnt {
		if x > 0 {
			ans = append(ans, []int{i, x})
		}
	}
	return
}
```

#### TypeScript

```ts
function mergeArrays(nums1: number[][], nums2: number[][]): number[][] {
    const n = 1001;
    const cnt = new Array(n).fill(0);
    for (const [a, b] of nums1) {
        cnt[a] += b;
    }
    for (const [a, b] of nums2) {
        cnt[a] += b;
    }
    const ans: number[][] = [];
    for (let i = 0; i < n; ++i) {
        if (cnt[i] > 0) {
            ans.push([i, cnt[i]]);
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn merge_arrays(nums1: Vec<Vec<i32>>, nums2: Vec<Vec<i32>>) -> Vec<Vec<i32>> {
        let mut cnt = vec![0; 1001];

        for x in &nums1 {
            cnt[x[0] as usize] += x[1];
        }

        for x in &nums2 {
            cnt[x[0] as usize] += x[1];
        }

        let mut ans = vec![];
        for i in 0..cnt.len() {
            if cnt[i] > 0 {
                ans.push(vec![i as i32, cnt[i] as i32]);
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
