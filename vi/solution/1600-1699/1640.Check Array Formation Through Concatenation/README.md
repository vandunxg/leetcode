---
comments: true
difficulty: Easy
rating: 1524
source: Weekly Contest 213 Q1
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [1640. Check Array Formation Through Concatenation](https://leetcode.com/problems/check-array-formation-through-concatenation)

[中文文档](/solution/1600-1699/1640.Check%20Array%20Formation%20Through%20Concatenation/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một mảng số nguyên <strong>khác nhau</strong> <code>arr</code> và một mảng các mảng số nguyên <code>pieces</code>, trong đó các số nguyên trong <code>pieces</code> cũng <strong>khác nhau</strong>. Hãy tạo <code>arr</code> bằng cách nối các mảng trong <code>pieces</code> theo <strong>bất kỳ thứ tự nào</strong>. Tuy nhiên, bạn <strong>không được</strong> sắp xếp lại các số nguyên trong mỗi mảng <code>pieces[i]</code>.</p>

<p>Trả về <code>true</code><em> </em><em> </em><em> </em> nếu có thể tạo mảng <code>arr</code> từ <code>pieces</code>. Nếu không, trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> arr = [15,88], pieces = [[88],[15]]
<strong>Output:</strong> true
<strong>Explanation:</strong> Nối [15] rồi đến [88]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> arr = [49,18,16], pieces = [[16,18,49]]
<strong>Output:</strong> false
<strong>Explanation:</strong> Dù các số khớp nhau, ta không thể sắp xếp lại pieces[0].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> arr = [91,4,64,78], pieces = [[78],[4,64],[91]]
<strong>Output:</strong> true
<strong>Explanation:</strong> Nối [91], rồi [4,64], sau đó [78]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= pieces.length &lt;= arr.length &lt;= 100</code></li>
	<li><code>sum(pieces[i].length) == arr.length</code></li>
	<li><code>1 &lt;= pieces[i].length &lt;= arr.length</code></li>
	<li><code>1 &lt;= arr[i], pieces[i][j] &lt;= 100</code></li>
	<li>Các số nguyên trong <code>arr</code> là <strong>khác nhau</strong>.</li>
	<li>Các số nguyên trong <code>pieces</code> là <strong>khác nhau</strong> (tức là nếu làm phẳng pieces thành một mảng 1D, mọi số nguyên trong mảng này đều khác nhau).</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi piece đã có thứ tự và các giá trị là duy nhất, nên ta duyệt $arr$ và tìm piece bắt đầu bằng giá trị hiện tại. Với $n \le 100$, có thể tìm tuyến tính piece đó.
>
> Con trỏ $i$ duyệt $arr$ và tìm piece có phần tử đầu tiên là $arr[i]$; nếu không tìm được thì không thể tạo mảng.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canFormArray(self, arr: List[int], pieces: List[List[int]]) -> bool:
        i = 0
        while i < len(arr):
            k = 0
            while k < len(pieces) and pieces[k][0] != arr[i]:
                k += 1
            if k == len(pieces):
                return False
            j = 0
            while j < len(pieces[k]) and arr[i] == pieces[k][j]:
                i, j = i + 1, j + 1
        return True
```

#### Java

```java
class Solution {
    public boolean canFormArray(int[] arr, int[][] pieces) {
        for (int i = 0; i < arr.length;) {
            int k = 0;
            while (k < pieces.length && pieces[k][0] != arr[i]) {
                ++k;
            }
            if (k == pieces.length) {
                return false;
            }
            int j = 0;
            while (j < pieces[k].length && arr[i] == pieces[k][j]) {
                ++i;
                ++j;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canFormArray(vector<int>& arr, vector<vector<int>>& pieces) {
        for (int i = 0; i < arr.size();) {
            int k = 0;
            while (k < pieces.size() && pieces[k][0] != arr[i]) {
                ++k;
            }
            if (k == pieces.size()) {
                return false;
            }
            int j = 0;
            while (j < pieces[k].size() && arr[i] == pieces[k][j]) {
                ++i;
                ++j;
            }
        }
        return true;
    }
};
```

#### Go

```go
func canFormArray(arr []int, pieces [][]int) bool {
	for i := 0; i < len(arr); {
		k := 0
		for k < len(pieces) && pieces[k][0] != arr[i] {
			k++
		}
		if k == len(pieces) {
			return false
		}
		j := 0
		for j < len(pieces[k]) && arr[i] == pieces[k][j] {
			i, j = i+1, j+1
		}
	}
	return true
}
```

#### TypeScript

```ts
function canFormArray(arr: number[], pieces: number[][]): boolean {
    const n = arr.length;
    let i = 0;
    while (i < n) {
        const target = arr[i];
        const items = pieces.find(v => v[0] === target);
        if (items == null) {
            return false;
        }
        for (const item of items) {
            if (item !== arr[i]) {
                return false;
            }
            i++;
        }
    }
    return true;
}
```

#### Rust

```rust
use std::collections::HashMap;
impl Solution {
    pub fn can_form_array(arr: Vec<i32>, pieces: Vec<Vec<i32>>) -> bool {
        let n = arr.len();
        let mut map = HashMap::new();
        for (i, v) in pieces.iter().enumerate() {
            map.insert(v[0], i);
        }
        let mut i = 0;
        while i < n {
            match map.get(&arr[i]) {
                None => {
                    return false;
                }
                Some(&j) => {
                    for &item in pieces[j].iter() {
                        if item != arr[i] {
                            return false;
                        }
                        i += 1;
                    }
                }
            }
        }
        true
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} arr
 * @param {number[][]} pieces
 * @return {boolean}
 */
var canFormArray = function (arr, pieces) {
    const d = new Map();
    for (const p of pieces) {
        d.set(p[0], p);
    }
    for (let i = 0; i < arr.length;) {
        if (!d.has(arr[i])) {
            return false;
        }
        const p = d.get(arr[i]);
        for (const v of p) {
            if (arr[i++] != v) {
                return false;
            }
        }
    }
    return true;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 duyệt mọi piece ở mỗi bước. Vì các giá trị là duy nhất, ánh xạ từ phần tử đầu tiên đến piece giúp tra cứu trong $O(1)$.
>
> Lưu $d[p[0]]=p$ rồi so sánh $arr[i:i+|p|]$ với $p$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canFormArray(self, arr: List[int], pieces: List[List[int]]) -> bool:
        d = {p[0]: p for p in pieces}
        i, n = 0, len(arr)
        while i < n:
            if arr[i] not in d:
                return False
            p = d[arr[i]]
            if arr[i : i + len(p)] != p:
                return False
            i += len(p)
        return True
```

#### Java

```java
class Solution {
    public boolean canFormArray(int[] arr, int[][] pieces) {
        Map<Integer, int[]> d = new HashMap<>();
        for (var p : pieces) {
            d.put(p[0], p);
        }
        for (int i = 0; i < arr.length;) {
            if (!d.containsKey(arr[i])) {
                return false;
            }
            for (int v : d.get(arr[i])) {
                if (arr[i++] != v) {
                    return false;
                }
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canFormArray(vector<int>& arr, vector<vector<int>>& pieces) {
        unordered_map<int, vector<int>> d;
        for (auto& p : pieces) {
            d[p[0]] = p;
        }
        for (int i = 0; i < arr.size();) {
            if (!d.count(arr[i])) {
                return false;
            }
            for (int& v : d[arr[i]]) {
                if (arr[i++] != v) {
                    return false;
                }
            }
        }
        return true;
    }
};
```

#### Go

```go
func canFormArray(arr []int, pieces [][]int) bool {
	d := map[int][]int{}
	for _, p := range pieces {
		d[p[0]] = p
	}
	for i := 0; i < len(arr); {
		p, ok := d[arr[i]]
		if !ok {
			return false
		}
		for _, v := range p {
			if arr[i] != v {
				return false
			}
			i++
		}
	}
	return true
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
