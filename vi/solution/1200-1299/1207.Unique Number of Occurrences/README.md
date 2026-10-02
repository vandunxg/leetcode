---
comments: true
difficulty: Easy
rating: 1195
source: Weekly Contest 156 Q1
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [1207. Unique Number of Occurrences](https://leetcode.com/problems/unique-number-of-occurrences)

[中文文档](/solution/1200-1299/1207.Unique%20Number%20of%20Occurrences/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>arr</code>, trả về <code>true</code> <em>nếu số lần xuất hiện của mỗi giá trị trong mảng là <strong>duy nhất</strong>; nếu không thì trả về </em><code>false</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> arr = [1,2,2,1,1,3]
<strong>Output:</strong> true
<strong>Giải thích:</strong>&nbsp;Giá trị 1 xuất hiện 3 lần, giá trị 2 xuất hiện 2 lần và giá trị 3 xuất hiện 1 lần. Không có hai giá trị nào xuất hiện cùng số lần.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> arr = [1,2]
<strong>Output:</strong> false
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> arr = [-3,0,1,-3,1,1,1,-3,10,0]
<strong>Output:</strong> true
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 1000</code></li>
	<li><code>-1000 &lt;= arr[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Với $n \le 1000$, chỉ cần đếm số lần xuất hiện của từng giá trị rồi kiểm tra các tần suất có duy nhất hay không. Một lượt duyệt tạo bảng đếm; tập các tần suất có số phần tử bằng số giá trị phân biệt khi và chỉ khi mỗi tần suất chỉ xuất hiện một lần.

<!-- thinking:end -->

Ta dùng hash table $cnt$ để đếm tần suất của từng số trong mảng $arr$, rồi dùng một hash table khác là $vis$ để ghi nhận các tần suất đã gặp. Cuối cùng, kiểm tra kích thước của $cnt$ và $vis$ có bằng nhau hay không.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, với $n$ là độ dài mảng $arr$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def uniqueOccurrences(self, arr: List[int]) -> bool:
        cnt = Counter(arr)
        return len(set(cnt.values())) == len(cnt)
```

#### Java

```java
class Solution {
    public boolean uniqueOccurrences(int[] arr) {
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int x : arr) {
            cnt.merge(x, 1, Integer::sum);
        }
        return new HashSet<>(cnt.values()).size() == cnt.size();
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool uniqueOccurrences(vector<int>& arr) {
        unordered_map<int, int> cnt;
        for (int& x : arr) {
            ++cnt[x];
        }
        unordered_set<int> vis;
        for (auto& [_, v] : cnt) {
            if (vis.count(v)) {
                return false;
            }
            vis.insert(v);
        }
        return true;
    }
};
```

#### Go

```go
func uniqueOccurrences(arr []int) bool {
	cnt := map[int]int{}
	for _, x := range arr {
		cnt[x]++
	}
	vis := map[int]bool{}
	for _, v := range cnt {
		if vis[v] {
			return false
		}
		vis[v] = true
	}
	return true
}
```

#### TypeScript

```ts
function uniqueOccurrences(arr: number[]): boolean {
    const cnt: Map<number, number> = new Map();
    for (const x of arr) {
        cnt.set(x, (cnt.get(x) || 0) + 1);
    }
    return cnt.size === new Set(cnt.values()).size;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
