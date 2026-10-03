---
comments: true
difficulty: Medium
rating: 1445
source: Weekly Contest 296 Q3
tags:
    - Array
    - Hash Table
    - Simulation
---

<!-- problem:start -->

# [2295. Replace Elements in an Array](https://leetcode.com/problems/replace-elements-in-an-array)

[中文文档](/solution/2200-2299/2295.Replace%20Elements%20in%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <strong>được đánh chỉ số từ 0</strong> <code>nums</code> gồm <code>n</code> số nguyên dương <strong>phân biệt</strong>. Thực hiện <code>m</code> thao tác trên mảng này, trong đó ở thao tác <code>i<sup>th</sup></code>, thay số <code>operations[i][0]</code> bằng <code>operations[i][1]</code>.</p>

<p>Đảm bảo rằng trong thao tác <code>i<sup>th</sup></code>:</p>

<ul>
	<li><code>operations[i][0]</code> <strong>tồn tại</strong> trong <code>nums</code>.</li>
	<li><code>operations[i][1]</code> <strong>không tồn tại</strong> trong <code>nums</code>.</li>
</ul>

<p>Trả về <em>mảng thu được sau khi thực hiện tất cả các thao tác</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,4,6], operations = [[1,3],[4,7],[6,1]]
<strong>Đầu ra:</strong> [3,2,7,1]
<strong>Giải thích:</strong> Ta thực hiện các thao tác sau trên nums:
- Thay số 1 bằng 3. nums trở thành [<u><strong>3</strong></u>,2,4,6].
- Thay số 4 bằng 7. nums trở thành [3,2,<u><strong>7</strong></u>,6].
- Thay số 6 bằng 1. nums trở thành [3,2,7,<u><strong>1</strong></u>].
Ta trả về mảng cuối cùng [3,2,7,1].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2], operations = [[1,3],[2,1],[3,2]]
<strong>Đầu ra:</strong> [2,1]
<strong>Giải thích:</strong> Ta thực hiện các thao tác sau trên nums:
- Thay số 1 bằng 3. nums trở thành [<u><strong>3</strong></u>,2].
- Thay số 2 bằng 1. nums trở thành [3,<u><strong>1</strong></u>].
- Thay số 3 bằng 2. nums trở thành [<u><strong>2</strong></u>,1].
Ta trả về mảng cuối cùng [2,1].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>m == operations.length</code></li>
	<li><code>1 &lt;= n, m &lt;= 10<sup>5</sup></code></li>
	<li>Tất cả các giá trị trong <code>nums</code> đều <strong>phân biệt</strong>.</li>
	<li><code>operations[i].length == 2</code></li>
	<li><code>1 &lt;= nums[i], operations[i][0], operations[i][1] &lt;= 10<sup>6</sup></code></li>
	<li><code>operations[i][0]</code> sẽ tồn tại trong <code>nums</code> khi thực hiện thao tác <code>i<sup>th</sup></code>.</li>
	<li><code>operations[i][1]</code> sẽ không tồn tại trong <code>nums</code> khi thực hiện thao tác <code>i<sup>th</sup></code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác thay $x$ trong mảng bằng một $y$ mới; có tới $10^5$ thao tác. Nếu tìm kiếm tuyến tính $x$ thì sẽ quá chậm. Vì các giá trị là duy nhất, chỉ cần một hash map ánh xạ từ giá trị đến chỉ số.
>
> Với $[x,y]$, ghi $y$ vào $nums[d[x]]$ và đặt $d[y]$ thành chỉ số đó. Có thể giữ lại key cũ $d[x]$ vì nó sẽ không được truy vấn lại.

<!-- thinking:end -->

Đầu tiên, ta dùng một hash table $d$ để lưu chỉ số của mỗi số trong mảng $\textit{nums}$. Sau đó, ta duyệt qua mảng thao tác $\textit{operations}$. Với mỗi thao tác $[x, y]$, ta thay số tại chỉ số $d[x]$ trong $\textit{nums}$ bằng $y$, đồng thời cập nhật chỉ số của $y$ trong $d$ thành $d[x]$.

Cuối cùng, ta trả về $\textit{nums}$.

Độ phức tạp thời gian là $O(n + m)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ và $m$ lần lượt là độ dài của mảng $\textit{nums}$ và mảng thao tác $\textit{operations}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def arrayChange(self, nums: List[int], operations: List[List[int]]) -> List[int]:
        d = {x: i for i, x in enumerate(nums)}
        for x, y in operations:
            nums[d[x]] = y
            d[y] = d[x]
        return nums
```

#### Java

```java
class Solution {
    public int[] arrayChange(int[] nums, int[][] operations) {
        int n = nums.length;
        Map<Integer, Integer> d = new HashMap<>(n);
        for (int i = 0; i < n; ++i) {
            d.put(nums[i], i);
        }
        for (var op : operations) {
            int x = op[0], y = op[1];
            nums[d.get(x)] = y;
            d.put(y, d.get(x));
        }
        return nums;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> arrayChange(vector<int>& nums, vector<vector<int>>& operations) {
        unordered_map<int, int> d;
        for (int i = 0; i < nums.size(); ++i) {
            d[nums[i]] = i;
        }
        for (auto& op : operations) {
            int x = op[0], y = op[1];
            nums[d[x]] = y;
            d[y] = d[x];
        }
        return nums;
    }
};
```

#### Go

```go
func arrayChange(nums []int, operations [][]int) []int {
	d := map[int]int{}
	for i, x := range nums {
		d[x] = i
	}
	for _, op := range operations {
		x, y := op[0], op[1]
		nums[d[x]] = y
		d[y] = d[x]
	}
	return nums
}
```

#### TypeScript

```ts
function arrayChange(nums: number[], operations: number[][]): number[] {
    const d: Map<number, number> = new Map(nums.map((x, i) => [x, i]));
    for (const [x, y] of operations) {
        nums[d.get(x)!] = y;
        d.set(y, d.get(x)!);
    }
    return nums;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
