---
comments: true
difficulty: Easy
rating: 1270
source: Biweekly Contest 84 Q1
tags:
    - Array
    - Hash Table
    - Ordered Set
    - Sorting
---

<!-- problem:start -->

# [2363. Merge Similar Items](https://leetcode.com/problems/merge-similar-items)

[中文文档](/solution/2300-2399/2363.Merge%20Similar%20Items/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng số nguyên 2D, <code>items1</code> và <code>items2</code>, biểu diễn hai tập hợp vật phẩm. Mỗi mảng <code>items</code> có các tính chất sau:</p>

<ul>
	<li><code>items[i] = [value<sub>i</sub>, weight<sub>i</sub>]</code>, trong đó <code>value<sub>i</sub></code> biểu diễn <strong>giá trị</strong> và <code>weight<sub>i</sub></code> biểu diễn <strong>trọng lượng </strong> của vật phẩm thứ <code>i<sup>th</sup></code>.</li>
	<li>Giá trị của mỗi vật phẩm trong <code>items</code> là <strong>duy nhất</strong>.</li>
</ul>

<p>Trả về <em>một mảng số nguyên 2D</em> <code>ret</code> <em>trong đó</em> <code>ret[i] = [value<sub>i</sub>, weight<sub>i</sub>]</code><em>,</em> <em>với</em> <code>weight<sub>i</sub></code> <em>là <strong>tổng trọng lượng</strong> của tất cả vật phẩm có giá trị</em> <code>value<sub>i</sub></code>.</p>

<p><strong>Lưu ý:</strong> <code>ret</code> phải được trả về theo thứ tự <strong>tăng dần</strong> của giá trị.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> items1 = [[1,1],[4,5],[3,8]], items2 = [[3,1],[1,5]]
<strong>Đầu ra:</strong> [[1,6],[3,9],[4,5]]
<strong>Giải thích:</strong>
Vật phẩm có value = 1 xuất hiện trong items1 với weight = 1 và trong items2 với weight = 5, tổng weight = 1 + 5 = 6.
Vật phẩm có value = 3 xuất hiện trong items1 với weight = 8 và trong items2 với weight = 1, tổng weight = 8 + 1 = 9.
Vật phẩm có value = 4 xuất hiện trong items1 với weight = 5, tổng weight = 5.
Vì vậy, ta trả về [[1,6],[3,9],[4,5]].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> items1 = [[1,1],[3,2],[2,3]], items2 = [[2,1],[3,2],[1,3]]
<strong>Đầu ra:</strong> [[1,4],[2,4],[3,4]]
<strong>Giải thích:</strong>
Vật phẩm có value = 1 xuất hiện trong items1 với weight = 1 và trong items2 với weight = 3, tổng weight = 1 + 3 = 4.
Vật phẩm có value = 2 xuất hiện trong items1 với weight = 3 và trong items2 với weight = 1, tổng weight = 3 + 1 = 4.
Vật phẩm có value = 3 xuất hiện trong items1 với weight = 2 và trong items2 với weight = 2, tổng weight = 2 + 2 = 4.
Vì vậy, ta trả về [[1,4],[2,4],[3,4]].</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> items1 = [[1,3],[2,2]], items2 = [[7,1],[2,2],[1,4]]
<strong>Đầu ra:</strong> [[1,7],[2,4],[7,1]]
<strong>Giải thích:
</strong>Vật phẩm có value = 1 xuất hiện trong items1 với weight = 3 và trong items2 với weight = 4, tổng weight = 3 + 4 = 7.
Vật phẩm có value = 2 xuất hiện trong items1 với weight = 2 và trong items2 với weight = 2, tổng weight = 2 + 2 = 4.
Vật phẩm có value = 7 xuất hiện trong items2 với weight = 1, tổng weight = 1.
Vì vậy, ta trả về [[1,7],[2,4],[7,1]].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= items1.length, items2.length &lt;= 1000</code></li>
	<li><code>items1[i].length == items2[i].length == 2</code></li>
	<li><code>1 &lt;= value<sub>i</sub>, weight<sub>i</sub> &lt;= 1000</code></li>
	<li>Mỗi <code>value<sub>i</sub></code> trong <code>items1</code> là <strong>duy nhất</strong>.</li>
	<li>Mỗi <code>value<sub>i</sub></code> trong <code>items2</code> là <strong>duy nhất</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table hoặc Mảng

<!-- thinking:start -->

> **Tư duy**
>
> Gộp hai danh sách theo giá trị, đồng thời cộng các trọng lượng. Giá trị và độ dài đều không vượt quá $1000$, nên chỉ cần một map cùng với thao tác sắp xếp.
>
> Thêm mọi cặp $(value,weight)$ vào một bộ đếm, sau đó xuất các vật phẩm theo thứ tự giá trị tăng dần.

<!-- thinking:end -->

Ta có thể dùng hash table hoặc mảng `cnt` để đếm tổng trọng lượng của mỗi vật phẩm trong `items1` và `items2`. Sau đó, ta duyệt các giá trị theo thứ tự tăng dần, thêm từng giá trị cùng tổng trọng lượng tương ứng vào mảng kết quả.

Độ phức tạp thời gian là $O(n + m)$ và độ phức tạp không gian là $O(n + m)$, trong đó $n$ và $m$ lần lượt là độ dài của `items1` và `items2`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def mergeSimilarItems(
        self, items1: List[List[int]], items2: List[List[int]]
    ) -> List[List[int]]:
        cnt = Counter()
        for v, w in chain(items1, items2):
            cnt[v] += w
        return sorted(cnt.items())
```

#### Java

```java
class Solution {
    public List<List<Integer>> mergeSimilarItems(int[][] items1, int[][] items2) {
        int[] cnt = new int[1010];
        for (var x : items1) {
            cnt[x[0]] += x[1];
        }
        for (var x : items2) {
            cnt[x[0]] += x[1];
        }
        List<List<Integer>> ans = new ArrayList<>();
        for (int i = 0; i < cnt.length; ++i) {
            if (cnt[i] > 0) {
                ans.add(List.of(i, cnt[i]));
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
    vector<vector<int>> mergeSimilarItems(vector<vector<int>>& items1, vector<vector<int>>& items2) {
        int cnt[1010]{};
        for (auto& x : items1) {
            cnt[x[0]] += x[1];
        }
        for (auto& x : items2) {
            cnt[x[0]] += x[1];
        }
        vector<vector<int>> ans;
        for (int i = 0; i < 1010; ++i) {
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
func mergeSimilarItems(items1 [][]int, items2 [][]int) (ans [][]int) {
	cnt := [1010]int{}
	for _, x := range items1 {
		cnt[x[0]] += x[1]
	}
	for _, x := range items2 {
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
function mergeSimilarItems(items1: number[][], items2: number[][]): number[][] {
    const count = new Array(1001).fill(0);
    for (const [v, w] of items1) {
        count[v] += w;
    }
    for (const [v, w] of items2) {
        count[v] += w;
    }
    return [...count.entries()].filter(v => v[1] !== 0);
}
```

#### Rust

```rust
impl Solution {
    pub fn merge_similar_items(items1: Vec<Vec<i32>>, items2: Vec<Vec<i32>>) -> Vec<Vec<i32>> {
        let mut count = [0; 1001];
        for item in items1.iter() {
            count[item[0] as usize] += item[1];
        }
        for item in items2.iter() {
            count[item[0] as usize] += item[1];
        }
        count
            .iter()
            .enumerate()
            .filter_map(|(i, &v)| {
                if v == 0 {
                    return None;
                }
                Some(vec![i as i32, v])
            })
            .collect()
    }
}
```

#### C

```c
/**
 * Return an array of arrays of size *returnSize.
 * The sizes of the arrays are returned as *returnColumnSizes array.
 * Note: Both returned array and *columnSizes array must be malloced, assume caller calls free().
 */
int** mergeSimilarItems(int** items1, int items1Size, int* items1ColSize, int** items2, int items2Size,
    int* items2ColSize, int* returnSize, int** returnColumnSizes) {
    int count[1001] = {0};
    for (int i = 0; i < items1Size; i++) {
        count[items1[i][0]] += items1[i][1];
    }
    for (int i = 0; i < items2Size; i++) {
        count[items2[i][0]] += items2[i][1];
    }
    int** ans = malloc(sizeof(int*) * (items1Size + items2Size));
    *returnColumnSizes = malloc(sizeof(int) * (items1Size + items2Size));
    int size = 0;
    for (int i = 0; i < 1001; i++) {
        if (count[i]) {
            ans[size] = malloc(sizeof(int) * 2);
            ans[size][0] = i;
            ans[size][1] = count[i];
            (*returnColumnSizes)[size] = 2;
            size++;
        }
    }
    *returnSize = size;
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
