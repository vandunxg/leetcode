---
comments: true
difficulty: Medium
rating: 1779
source: Weekly Contest 186 Q3
tags:
    - Array
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1424. Diagonal Traverse II](https://leetcode.com/problems/diagonal-traverse-ii)

[中文文档](/solution/1400-1499/1424.Diagonal%20Traverse%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên 2D <code>nums</code>, hãy trả về <em>tất cả các phần tử của </em><code>nums</code><em> theo thứ tự đường chéo như trong các hình bên dưới</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1424.Diagonal%20Traverse%20II/images/sample_1_1784.png" style="width: 158px; height: 143px;" />
<pre>
<strong>Input:</strong> nums = [[1,2,3],[4,5,6],[7,8,9]]
<strong>Output:</strong> [1,4,2,7,5,3,8,6,9]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1424.Diagonal%20Traverse%20II/images/sample_2_1784.png" style="width: 230px; height: 177px;" />
<pre>
<strong>Input:</strong> nums = [[1,2,3,4,5],[6,7],[8],[9,10,11],[12,13,14,15,16]]
<strong>Output:</strong> [1,6,2,8,7,3,9,4,12,10,5,13,11,14,15,16]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i].length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= sum(nums[i].length) &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i][j] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Các hàng có độ dài khác nhau và có thể có tới $10^5$ phần tử, nên việc duyệt các đường chéo bằng các chỉ số thô khá bất tiện. Các phần tử trên cùng một đường chéo có chung $i+j$; các đường chéo tăng theo $i+j$, còn trong một đường chéo thì $j$ tăng dần.
>
> Lưu các bộ ba $(i+j,j,v)$, sắp xếp chúng rồi xuất các giá trị.

<!-- thinking:end -->

Ta nhận thấy rằng:

- Giá trị của $i + j$ giống nhau trên mỗi đường chéo;
- Giá trị của $i + j$ trên đường chéo tiếp theo lớn hơn đường chéo trước đó;
- Trong cùng một đường chéo, giá trị của $i + j$ giống nhau, còn giá trị của $j$ tăng từ nhỏ đến lớn.

Do đó, ta lưu tất cả các số dưới dạng $(i, j, \textit{nums}[i][j])$ vào \textit{arr}, sau đó sắp xếp theo hai phần tử đầu tiên. Cuối cùng, trả về mảng được tạo bởi các giá trị tại chỉ số 2 của mọi phần tử trong \textit{arr}.

Độ phức tạp thời gian là $O(n \times \log n)$, trong đó $n$ là số phần tử của mảng \textit{nums}. Độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findDiagonalOrder(self, nums: List[List[int]]) -> List[int]:
        arr = []
        for i, row in enumerate(nums):
            for j, v in enumerate(row):
                arr.append((i + j, j, v))
        arr.sort()
        return [v[2] for v in arr]
```

#### Java

```java
class Solution {
    public int[] findDiagonalOrder(List<List<Integer>> nums) {
        List<int[]> arr = new ArrayList<>();
        for (int i = 0; i < nums.size(); ++i) {
            for (int j = 0; j < nums.get(i).size(); ++j) {
                arr.add(new int[] {i + j, j, nums.get(i).get(j)});
            }
        }
        arr.sort((a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
        int[] ans = new int[arr.size()];
        for (int i = 0; i < arr.size(); ++i) {
            ans[i] = arr.get(i)[2];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> findDiagonalOrder(vector<vector<int>>& nums) {
        vector<tuple<int, int, int>> arr;
        for (int i = 0; i < nums.size(); ++i) {
            for (int j = 0; j < nums[i].size(); ++j) {
                arr.push_back({i + j, j, nums[i][j]});
            }
        }
        sort(arr.begin(), arr.end());
        vector<int> ans;
        for (auto& e : arr) {
            ans.push_back(get<2>(e));
        }
        return ans;
    }
};
```

#### Go

```go
func findDiagonalOrder(nums [][]int) []int {
	arr := [][]int{}
	for i, row := range nums {
		for j, v := range row {
			arr = append(arr, []int{i + j, j, v})
		}
	}
	sort.Slice(arr, func(i, j int) bool {
		if arr[i][0] == arr[j][0] {
			return arr[i][1] < arr[j][1]
		}
		return arr[i][0] < arr[j][0]
	})
	ans := []int{}
	for _, v := range arr {
		ans = append(ans, v[2])
	}
	return ans
}
```

#### TypeScript

```ts
function findDiagonalOrder(nums: number[][]): number[] {
    const arr: number[][] = [];
    for (let i = 0; i < nums.length; ++i) {
        for (let j = 0; j < nums[i].length; ++j) {
            arr.push([i + j, j, nums[i][j]]);
        }
    }
    arr.sort((a, b) => (a[0] === b[0] ? a[1] - b[1] : a[0] - b[0]));
    return arr.map(x => x[2]);
}
```

#### C#

```cs
public class Solution {
    public int[] FindDiagonalOrder(IList<IList<int>> nums) {
        List<int[]> arr = new List<int[]>();
        for (int i = 0; i < nums.Count; ++i) {
            for (int j = 0; j < nums[i].Count; ++j) {
                arr.Add(new int[] { i + j, j, nums[i][j] });
            }
        }
        arr.Sort((a, b) => a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
        int[] ans = new int[arr.Count];
        for (int i = 0; i < arr.Count; ++i) {
            ans[i] = arr[i][2];
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
