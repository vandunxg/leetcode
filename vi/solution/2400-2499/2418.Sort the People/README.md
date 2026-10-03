---
comments: true
difficulty: Easy
rating: 1193
source: Weekly Contest 312 Q1
tags:
    - Array
    - Hash Table
    - String
    - Sorting
---

<!-- problem:start -->

# [2418. Sort the People](https://leetcode.com/problems/sort-the-people)

[中文文档](/solution/2400-2499/2418.Sort%20the%20People/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng chuỗi <code>names</code> và một mảng <code>heights</code> gồm các số nguyên dương <strong>phân biệt</strong>. Cả hai mảng đều có độ dài <code>n</code>.</p>

<p>Với mỗi chỉ số <code>i</code>, <code>names[i]</code> và <code>heights[i]</code> lần lượt là tên và chiều cao của người thứ <code>i<sup>th</sup></code>.</p>

<p>Trả về <code>names</code><em> được sắp xếp theo thứ tự <strong>giảm dần</strong> dựa trên chiều cao của mọi người</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> names = [&quot;Mary&quot;,&quot;John&quot;,&quot;Emma&quot;], heights = [180,165,170]
<strong>Đầu ra:</strong> [&quot;Mary&quot;,&quot;Emma&quot;,&quot;John&quot;]
<strong>Giải thích:</strong> Mary cao nhất, tiếp theo là Emma và John.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> names = [&quot;Alice&quot;,&quot;Bob&quot;,&quot;Bob&quot;], heights = [155,185,150]
<strong>Đầu ra:</strong> [&quot;Bob&quot;,&quot;Alice&quot;,&quot;Bob&quot;]
<strong>Giải thích:</strong> Bob thứ nhất cao nhất, tiếp theo là Alice và Bob thứ hai.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == names.length == heights.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>3</sup></code></li>
	<li><code>1 &lt;= names[i].length &lt;= 20</code></li>
	<li><code>1 &lt;= heights[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>names[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường và viết hoa.</li>
	<li>Tất cả giá trị trong <code>heights</code> đều phân biệt.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Với $n\le 10^3$, hãy sắp xếp names theo chiều cao giảm dần. Vì các chiều cao là duy nhất nên khóa sắp xếp là đủ để xác định thứ tự. Sắp xếp các chỉ số theo thứ tự giảm dần của $heights$, sau đó lấy lần lượt $names[i]$.

<!-- thinking:end -->

Theo mô tả bài toán, ta có thể tạo một mảng chỉ số $idx$ có độ dài $n$, trong đó $idx[i]=i$. Sau đó, ta sắp xếp từng chỉ số trong $idx$ theo thứ tự giảm dần của chiều cao tương ứng trong $heights$. Cuối cùng, ta duyệt từng chỉ số $i$ trong $idx$ đã sắp xếp và thêm $names[i]$ vào mảng kết quả.

Ta cũng có thể tạo một mảng $arr$ có độ dài $n$, trong đó mỗi phần tử là một tuple $(heights[i], i)$. Sau đó, ta sắp xếp $arr$ theo thứ tự giảm dần của chiều cao. Cuối cùng, ta duyệt từng phần tử $(heights[i], i)$ trong $arr$ đã sắp xếp và thêm $names[i]$ vào mảng kết quả.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của các mảng $names$ và $heights$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sortPeople(self, names: List[str], heights: List[int]) -> List[str]:
        idx = list(range(len(heights)))
        idx.sort(key=lambda i: -heights[i])
        return [names[i] for i in idx]
```

#### Java

```java
class Solution {
    public String[] sortPeople(String[] names, int[] heights) {
        int n = names.length;
        Integer[] idx = new Integer[n];
        Arrays.setAll(idx, i -> i);
        Arrays.sort(idx, (i, j) -> heights[j] - heights[i]);
        String[] ans = new String[n];
        for (int i = 0; i < n; ++i) {
            ans[i] = names[idx[i]];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> sortPeople(vector<string>& names, vector<int>& heights) {
        int n = names.size();
        vector<int> idx(n);
        iota(idx.begin(), idx.end(), 0);
        sort(idx.begin(), idx.end(), [&](int i, int j) { return heights[j] < heights[i]; });
        vector<string> ans;
        for (int i : idx) {
            ans.push_back(names[i]);
        }
        return ans;
    }
};
```

#### Go

```go
func sortPeople(names []string, heights []int) (ans []string) {
	n := len(names)
	idx := make([]int, n)
	for i := range idx {
		idx[i] = i
	}
	sort.Slice(idx, func(i, j int) bool { return heights[idx[j]] < heights[idx[i]] })
	for _, i := range idx {
		ans = append(ans, names[i])
	}
	return
}
```

#### TypeScript

```ts
function sortPeople(names: string[], heights: number[]): string[] {
    const n = names.length;
    const idx = new Array(n);
    for (let i = 0; i < n; ++i) {
        idx[i] = i;
    }
    idx.sort((i, j) => heights[j] - heights[i]);
    const ans: string[] = [];
    for (const i of idx) {
        ans.push(names[i]);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn sort_people(names: Vec<String>, heights: Vec<i32>) -> Vec<String> {
        let mut combine: Vec<(String, i32)> = names.into_iter().zip(heights.into_iter()).collect();
        combine.sort_by(|a, b| b.1.cmp(&a.1));
        combine.iter().map(|s| s.0.clone()).collect()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 đã sắp xếp thông qua một mảng chỉ số. Ghép chiều cao với tên thành từng cặp rồi sắp xếp các cặp đó theo thứ tự giảm dần sẽ loại bỏ mảng chỉ số bổ sung; thứ tự thu được vẫn giống nhau.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sortPeople(self, names: List[str], heights: List[int]) -> List[str]:
        return [name for _, name in sorted(zip(heights, names), reverse=True)]
```

#### Java

```java
class Solution {
    public String[] sortPeople(String[] names, int[] heights) {
        int n = names.length;
        int[][] arr = new int[n][2];
        for (int i = 0; i < n; ++i) {
            arr[i] = new int[] {heights[i], i};
        }
        Arrays.sort(arr, (a, b) -> b[0] - a[0]);
        String[] ans = new String[n];
        for (int i = 0; i < n; ++i) {
            ans[i] = names[arr[i][1]];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> sortPeople(vector<string>& names, vector<int>& heights) {
        int n = names.size();
        vector<pair<int, int>> arr;
        for (int i = 0; i < n; ++i) {
            arr.emplace_back(-heights[i], i);
        }
        sort(arr.begin(), arr.end());
        vector<string> ans;
        for (int i = 0; i < n; ++i) {
            ans.emplace_back(names[arr[i].second]);
        }
        return ans;
    }
};
```

#### Go

```go
func sortPeople(names []string, heights []int) []string {
	n := len(names)
	arr := make([][2]int, n)
	for i, h := range heights {
		arr[i] = [2]int{h, i}
	}
	sort.Slice(arr, func(i, j int) bool { return arr[i][0] > arr[j][0] })
	ans := make([]string, n)
	for i, x := range arr {
		ans[i] = names[x[1]]
	}
	return ans
}
```

#### TypeScript

```ts
function sortPeople(names: string[], heights: number[]): string[] {
    return names
        .map<[string, number]>((s, i) => [s, heights[i]])
        .sort((a, b) => b[1] - a[1])
        .map(([v]) => v);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
