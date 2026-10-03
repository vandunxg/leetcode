---
comments: true
difficulty: Medium
rating: 1702
source: Weekly Contest 268 Q3
tags:
    - Design
    - Segment Tree
    - Array
    - Hash Table
    - Binary Search
---

<!-- problem:start -->

# [2080. Range Frequency Queries](https://leetcode.com/problems/range-frequency-queries)

[Tài liệu tiếng Trung](/solution/2000-2099/2080.Range%20Frequency%20Queries/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế một cấu trúc dữ liệu để tìm <strong>số lần xuất hiện</strong> của một giá trị cho trước trong một mảng con cho trước.</p>

<p><strong>Số lần xuất hiện</strong> của một giá trị trong một mảng con là số lần giá trị đó xuất hiện trong mảng con.</p>

<p>Hãy cài đặt lớp <code>RangeFreqQuery</code>:</p>

<ul>
	<li><code>RangeFreqQuery(int[] arr)</code> Khởi tạo một thể hiện của lớp với mảng số nguyên <code>arr</code> được <strong>đánh chỉ số từ 0</strong>.</li>
	<li><code>int query(int left, int right, int value)</code> Trả về <strong>số lần xuất hiện</strong> của <code>value</code> trong mảng con <code>arr[left...right]</code>.</li>
</ul>

<p><strong>Mảng con</strong> là một dãy liên tiếp các phần tử trong một mảng. <code>arr[left...right]</code> biểu thị mảng con chứa các phần tử của <code>nums</code> từ chỉ số <code>left</code> đến chỉ số <code>right</code> (<strong>bao gồm cả hai đầu</strong>).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;RangeFreqQuery&quot;, &quot;query&quot;, &quot;query&quot;]
[[[12, 33, 4, 56, 22, 2, 34, 33, 22, 12, 34, 56]], [1, 2, 4], [0, 11, 33]]
<strong>Đầu ra</strong>
[null, 1, 2]

<strong>Giải thích</strong>
RangeFreqQuery rangeFreqQuery = new RangeFreqQuery([12, 33, 4, 56, 22, 2, 34, 33, 22, 12, 34, 56]);
rangeFreqQuery.query(1, 2, 4); // return 1. The value 4 occurs 1 time in the subarray [33, 4]
rangeFreqQuery.query(0, 11, 33); // return 2. The value 33 occurs 2 times in the whole array.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= arr[i], value &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= left &lt;= right &lt; arr.length</code></li>
	<li>Có nhiều nhất <code>10<sup>5</sup></code> lần gọi đến <code>query</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Có nhiều truy vấn đếm số lần xuất hiện trên một mảng cố định. $n$ và số lượng truy vấn đều là $10^5$; segment tree có thể giải quyết, nhưng với mỗi giá trị ta chỉ cần danh sách các chỉ số của nó.
>
> Một hash map lưu các chỉ số theo thứ tự tăng dần; hai lần tìm kiếm nhị phân trên $[left,right]$ sẽ cho số lượng cần tìm.

<!-- thinking:end -->

Ta sử dụng một bảng băm $g$ để lưu danh sách các chỉ số trong mảng tương ứng với mỗi giá trị. Trong hàm khởi tạo, ta duyệt qua mảng $\textit{arr}$ và thêm chỉ số tương ứng với mỗi giá trị vào bảng băm.

Trong hàm query, trước tiên ta kiểm tra xem giá trị đã cho có tồn tại trong bảng băm hay không. Nếu không tồn tại, nghĩa là giá trị đó không có trong mảng, nên ta trả về $0$. Nếu có, ta lấy mảng chỉ số $\textit{idx}$ tương ứng với giá trị đó. Sau đó, ta dùng tìm kiếm nhị phân để tìm chỉ số đầu tiên $l$ lớn hơn hoặc bằng $\textit{left}$ và chỉ số đầu tiên $r$ lớn hơn $\textit{right}$. Cuối cùng, ta trả về $r - l$.

Về độ phức tạp thời gian, hàm khởi tạo có độ phức tạp $O(n)$ và hàm query có độ phức tạp $O(\log n)$. Độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class RangeFreqQuery:

    def __init__(self, arr: List[int]):
        self.g = defaultdict(list)
        for i, x in enumerate(arr):
            self.g[x].append(i)

    def query(self, left: int, right: int, value: int) -> int:
        idx = self.g[value]
        l = bisect_left(idx, left)
        r = bisect_left(idx, right + 1)
        return r - l


# Your RangeFreqQuery object will be instantiated and called as such:
# obj = RangeFreqQuery(arr)
# param_1 = obj.query(left,right,value)
```

#### Java

```java
class RangeFreqQuery {
    private Map<Integer, List<Integer>> g = new HashMap<>();

    public RangeFreqQuery(int[] arr) {
        for (int i = 0; i < arr.length; ++i) {
            g.computeIfAbsent(arr[i], k -> new ArrayList<>()).add(i);
        }
    }

    public int query(int left, int right, int value) {
        if (!g.containsKey(value)) {
            return 0;
        }
        var idx = g.get(value);
        int l = Collections.binarySearch(idx, left);
        l = l < 0 ? -l - 1 : l;
        int r = Collections.binarySearch(idx, right + 1);
        r = r < 0 ? -r - 1 : r;
        return r - l;
    }
}

/**
 * Your RangeFreqQuery object will be instantiated and called as such:
 * RangeFreqQuery obj = new RangeFreqQuery(arr);
 * int param_1 = obj.query(left,right,value);
 */
```

#### C++

```cpp
class RangeFreqQuery {
public:
    RangeFreqQuery(vector<int>& arr) {
        for (int i = 0; i < arr.size(); ++i) {
            g[arr[i]].push_back(i);
        }
    }

    int query(int left, int right, int value) {
        if (!g.contains(value)) {
            return 0;
        }
        auto& idx = g[value];
        auto l = lower_bound(idx.begin(), idx.end(), left);
        auto r = lower_bound(idx.begin(), idx.end(), right + 1);
        return r - l;
    }

private:
    unordered_map<int, vector<int>> g;
};

/**
 * Your RangeFreqQuery object will be instantiated and called as such:
 * RangeFreqQuery* obj = new RangeFreqQuery(arr);
 * int param_1 = obj->query(left,right,value);
 */
```

#### Go

```go
type RangeFreqQuery struct {
	g map[int][]int
}

func Constructor(arr []int) RangeFreqQuery {
	g := make(map[int][]int)
	for i, v := range arr {
		g[v] = append(g[v], i)
	}
	return RangeFreqQuery{g}
}

func (this *RangeFreqQuery) Query(left int, right int, value int) int {
	if idx, ok := this.g[value]; ok {
		l := sort.SearchInts(idx, left)
		r := sort.SearchInts(idx, right+1)
		return r - l
	}
	return 0
}

/**
 * Your RangeFreqQuery object will be instantiated and called as such:
 * obj := Constructor(arr);
 * param_1 = obj.Query(left,right,value);
 */
```

#### TypeScript

```ts
class RangeFreqQuery {
    private g: Map<number, number[]> = new Map();

    constructor(arr: number[]) {
        for (let i = 0; i < arr.length; ++i) {
            if (!this.g.has(arr[i])) {
                this.g.set(arr[i], []);
            }
            this.g.get(arr[i])!.push(i);
        }
    }

    query(left: number, right: number, value: number): number {
        const idx = this.g.get(value);
        if (!idx) {
            return 0;
        }
        const l = _.sortedIndex(idx, left);
        const r = _.sortedIndex(idx, right + 1);
        return r - l;
    }
}

/**
 * Your RangeFreqQuery object will be instantiated and called as such:
 * var obj = new RangeFreqQuery(arr)
 * var param_1 = obj.query(left,right,value)
 */
```

#### Rust

```rust
use std::collections::HashMap;

struct RangeFreqQuery {
    g: HashMap<i32, Vec<usize>>,
}

impl RangeFreqQuery {
    fn new(arr: Vec<i32>) -> Self {
        let mut g = HashMap::new();
        for (i, &value) in arr.iter().enumerate() {
            g.entry(value).or_insert_with(Vec::new).push(i);
        }
        RangeFreqQuery { g }
    }

    fn query(&self, left: i32, right: i32, value: i32) -> i32 {
        if let Some(idx) = self.g.get(&value) {
            let l = idx.partition_point(|&x| x < left as usize);
            let r = idx.partition_point(|&x| x <= right as usize);
            return (r - l) as i32;
        }
        0
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} arr
 */
var RangeFreqQuery = function (arr) {
    this.g = new Map();

    for (let i = 0; i < arr.length; ++i) {
        if (!this.g.has(arr[i])) {
            this.g.set(arr[i], []);
        }
        this.g.get(arr[i]).push(i);
    }
};

/**
 * @param {number} left
 * @param {number} right
 * @param {number} value
 * @return {number}
 */
RangeFreqQuery.prototype.query = function (left, right, value) {
    const idx = this.g.get(value);
    if (!idx) {
        return 0;
    }
    const l = _.sortedIndex(idx, left);
    const r = _.sortedIndex(idx, right + 1);
    return r - l;
};

/**
 * Your RangeFreqQuery object will be instantiated and called as such:
 * var obj = new RangeFreqQuery(arr)
 * var param_1 = obj.query(left,right,value)
 */
```

#### C#

```cs
public class RangeFreqQuery {
    private Dictionary<int, List<int>> g;

    public RangeFreqQuery(int[] arr) {
        g = new Dictionary<int, List<int>>();
        for (int i = 0; i < arr.Length; ++i) {
            if (!g.ContainsKey(arr[i])) {
                g[arr[i]] = new List<int>();
            }
            g[arr[i]].Add(i);
        }
    }

    public int Query(int left, int right, int value) {
        if (g.ContainsKey(value)) {
            var idx = g[value];
            int l = idx.BinarySearch(left);
            int r = idx.BinarySearch(right + 1);
            l = l < 0 ? -l - 1 : l;
            r = r < 0 ? -r - 1 : r;
            return r - l;
        }
        return 0;
    }
}

/**
 * Your RangeFreqQuery object will be instantiated and called as such:
 * RangeFreqQuery obj = new RangeFreqQuery(arr);
 * int param_1 = obj.Query(left, right, value);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
