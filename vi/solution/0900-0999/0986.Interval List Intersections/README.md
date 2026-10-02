---
comments: true
difficulty: Medium
tags:
    - Array
    - Two Pointers
    - Sweep Line
---

<!-- problem:start -->

# [986. Interval List Intersections](https://leetcode.com/problems/interval-list-intersections)

[中文文档](/solution/0900-0999/0986.Interval%20List%20Intersections/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai danh sách đoạn đóng <code>firstList</code> và <code>secondList</code>, trong đó <code>firstList[i] = [start<sub>i</sub>, end<sub>i</sub>]</code> và <code>secondList[j] = [start<sub>j</sub>, end<sub>j</sub>]</code>. Trong mỗi danh sách, các đoạn đôi một <strong>không giao nhau</strong> và được <strong>sắp xếp</strong>.</p>

<p>Hãy trả về <em>giao của hai danh sách đoạn này</em>.</p>

<p><strong>Đoạn đóng</strong> <code>[a, b]</code> (với <code>a &lt;= b</code>) biểu thị tập các số thực <code>x</code> thỏa mãn <code>a &lt;= x &lt;= b</code>.</p>

<p><strong>Giao</strong> của hai đoạn đóng là một tập số thực rỗng hoặc được biểu diễn bằng một đoạn đóng. Ví dụ, giao của <code>[1, 3]</code> và <code>[2, 4]</code> là <code>[2, 3]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0986.Interval%20List%20Intersections/images/interval1.png" style="width: 700px; height: 194px;" />
<pre>
<strong>Đầu vào:</strong> firstList = [[0,2],[5,10],[13,23],[24,25]], secondList = [[1,5],[8,12],[15,24],[25,26]]
<strong>Đầu ra:</strong> [[1,2],[5,5],[8,10],[15,23],[24,24],[25,25]]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> firstList = [[1,3],[5,9]], secondList = []
<strong>Đầu ra:</strong> []
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= firstList.length, secondList.length &lt;= 1000</code></li>
	<li><code>firstList.length + secondList.length &gt;= 1</code></li>
	<li><code>0 &lt;= start<sub>i</sub> &lt; end<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
	<li><code>end<sub>i</sub> &lt; start<sub>i+1</sub></code></li>
	<li><code>0 &lt;= start<sub>j</sub> &lt; end<sub>j</sub> &lt;= 10<sup>9</sup> </code></li>
	<li><code>end<sub>j</sub> &lt; start<sub>j+1</sub></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi danh sách đoạn đóng đều được sắp xếp và các đoạn trong cùng danh sách không giao nhau; ta cần tìm tất cả các giao. Hai pointer lần lượt theo dõi đoạn hiện tại của mỗi danh sách. Phần giao của chúng là $[\max(s_1,s_2),\min(e_1,e_2)]$ và được thêm vào kết quả nếu không rỗng. Di chuyển pointer của đoạn kết thúc trước.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def intervalIntersection(
        self, firstList: List[List[int]], secondList: List[List[int]]
    ) -> List[List[int]]:
        i = j = 0
        ans = []
        while i < len(firstList) and j < len(secondList):
            s1, e1, s2, e2 = *firstList[i], *secondList[j]
            l, r = max(s1, s2), min(e1, e2)
            if l <= r:
                ans.append([l, r])
            if e1 < e2:
                i += 1
            else:
                j += 1
        return ans
```

#### Java

```java
class Solution {
    public int[][] intervalIntersection(int[][] firstList, int[][] secondList) {
        List<int[]> ans = new ArrayList<>();
        int m = firstList.length, n = secondList.length;
        for (int i = 0, j = 0; i < m && j < n;) {
            int l = Math.max(firstList[i][0], secondList[j][0]);
            int r = Math.min(firstList[i][1], secondList[j][1]);
            if (l <= r) {
                ans.add(new int[] {l, r});
            }
            if (firstList[i][1] < secondList[j][1]) {
                ++i;
            } else {
                ++j;
            }
        }
        return ans.toArray(new int[ans.size()][]);
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> intervalIntersection(vector<vector<int>>& firstList, vector<vector<int>>& secondList) {
        vector<vector<int>> ans;
        int m = firstList.size(), n = secondList.size();
        for (int i = 0, j = 0; i < m && j < n;) {
            int l = max(firstList[i][0], secondList[j][0]);
            int r = min(firstList[i][1], secondList[j][1]);
            if (l <= r) ans.push_back({l, r});
            if (firstList[i][1] < secondList[j][1])
                ++i;
            else
                ++j;
        }
        return ans;
    }
};
```

#### Go

```go
func intervalIntersection(firstList [][]int, secondList [][]int) [][]int {
	m, n := len(firstList), len(secondList)
	var ans [][]int
	for i, j := 0, 0; i < m && j < n; {
		l := max(firstList[i][0], secondList[j][0])
		r := min(firstList[i][1], secondList[j][1])
		if l <= r {
			ans = append(ans, []int{l, r})
		}
		if firstList[i][1] < secondList[j][1] {
			i++
		} else {
			j++
		}
	}
	return ans
}
```

#### TypeScript

```ts
function intervalIntersection(firstList: number[][], secondList: number[][]): number[][] {
    const n = firstList.length;
    const m = secondList.length;
    const res = [];
    let i = 0;
    let j = 0;
    while (i < n && j < m) {
        const start = Math.max(firstList[i][0], secondList[j][0]);
        const end = Math.min(firstList[i][1], secondList[j][1]);
        if (start <= end) {
            res.push([start, end]);
        }
        if (firstList[i][1] < secondList[j][1]) {
            i++;
        } else {
            j++;
        }
    }
    return res;
}
```

#### Rust

```rust
impl Solution {
    pub fn interval_intersection(
        first_list: Vec<Vec<i32>>,
        second_list: Vec<Vec<i32>>,
    ) -> Vec<Vec<i32>> {
        let n = first_list.len();
        let m = second_list.len();
        let mut res = Vec::new();
        let (mut i, mut j) = (0, 0);
        while i < n && j < m {
            let start = first_list[i][0].max(second_list[j][0]);
            let end = first_list[i][1].min(second_list[j][1]);
            if start <= end {
                res.push(vec![start, end]);
            }
            if first_list[i][1] < second_list[j][1] {
                i += 1;
            } else {
                j += 1;
            }
        }
        res
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
