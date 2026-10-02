---
comments: true
difficulty: Medium
rating: 1597
source: Weekly Contest 157 Q2
tags:
    - Array
    - Hash Table
    - Dynamic Programming
---

<!-- problem:start -->

# [1218. Longest Arithmetic Subsequence of Given Difference](https://leetcode.com/problems/longest-arithmetic-subsequence-of-given-difference)

[中文文档](/solution/1200-1299/1218.Longest%20Arithmetic%20Subsequence%20of%20Given%20Difference/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>arr</code> và số nguyên <code>difference</code>. Hãy trả về độ dài của dãy con dài nhất trong <code>arr</code> mà các phần tử tạo thành cấp số cộng, tức hiệu giữa hai phần tử liền kề trong dãy con bằng <code>difference</code>.</p>

<p><strong>Dãy con</strong> là một dãy có thể thu được từ <code>arr</code> bằng cách xóa một số phần tử hoặc không xóa phần tử nào, nhưng vẫn giữ nguyên thứ tự của các phần tử còn lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> arr = [1,2,3,4], difference = 1
<strong>Output:</strong> 4
<strong>Giải thích: </strong>Dãy con cấp số cộng dài nhất là [1,2,3,4].</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> arr = [1,3,5,7], difference = 1
<strong>Output:</strong> 1
<strong>Giải thích: </strong>Dãy con cấp số cộng dài nhất có thể là bất kỳ phần tử đơn lẻ nào.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> arr = [1,5,7,8,5,3,4,2,1], difference = -2
<strong>Output:</strong> 4
<strong>Giải thích: </strong>Dãy con cấp số cộng dài nhất là [7,5,3,1].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>4</sup> &lt;= arr[i], difference &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Dãy con phải giữ nguyên thứ tự và có cùng $difference$ giữa các phần tử liên tiếp. Với $n \le 10^5$, không thể liệt kê mọi tập con. Dãy dài nhất kết thúc tại $x$ chỉ phụ thuộc vào dãy kết thúc tại $x-difference$; nếu dãy đó tồn tại thì nó đã được xét trước đó.
>
> Duyệt từ trái sang phải và đặt $f[x]=f[x-difference]+1$. Hash map tra cứu phần tử trước theo giá trị; một lượt duyệt tính độ dài dãy kết thúc tại mỗi phần tử, sau đó lấy giá trị lớn nhất.

<!-- thinking:end -->

Ta có thể dùng hash table $f$ để lưu độ dài của dãy con cấp số cộng dài nhất kết thúc tại $x$.

Duyệt mảng $\textit{arr}$; với mỗi phần tử $x$, cập nhật $f[x]$ thành $f[x - \textit{difference}] + 1$.

Sau khi duyệt xong, trả về giá trị lớn nhất trong $f$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, với $n$ là độ dài mảng $\textit{arr}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestSubsequence(self, arr: List[int], difference: int) -> int:
        f = defaultdict(int)
        for x in arr:
            f[x] = f[x - difference] + 1
        return max(f.values())
```

#### Java

```java
class Solution {
    public int longestSubsequence(int[] arr, int difference) {
        Map<Integer, Integer> f = new HashMap<>();
        int ans = 0;
        for (int x : arr) {
            f.put(x, f.getOrDefault(x - difference, 0) + 1);
            ans = Math.max(ans, f.get(x));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestSubsequence(vector<int>& arr, int difference) {
        unordered_map<int, int> f;
        int ans = 0;
        for (int x : arr) {
            f[x] = f[x - difference] + 1;
            ans = max(ans, f[x]);
        }
        return ans;
    }
};
```

#### Go

```go
func longestSubsequence(arr []int, difference int) (ans int) {
	f := map[int]int{}
	for _, x := range arr {
		f[x] = f[x-difference] + 1
		ans = max(ans, f[x])
	}
	return
}
```

#### TypeScript

```ts
function longestSubsequence(arr: number[], difference: number): number {
    const f: Map<number, number> = new Map();
    for (const x of arr) {
        f.set(x, (f.get(x - difference) ?? 0) + 1);
    }
    return Math.max(...f.values());
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn longest_subsequence(arr: Vec<i32>, difference: i32) -> i32 {
        let mut f = HashMap::new();
        let mut ans = 0;
        for &x in &arr {
            let count = f.get(&(x - difference)).unwrap_or(&0) + 1;
            f.insert(x, count);
            ans = ans.max(count);
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} arr
 * @param {number} difference
 * @return {number}
 */
var longestSubsequence = function (arr, difference) {
    const f = new Map();
    for (const x of arr) {
        f.set(x, (f.get(x - difference) || 0) + 1);
    }
    return Math.max(...f.values());
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
