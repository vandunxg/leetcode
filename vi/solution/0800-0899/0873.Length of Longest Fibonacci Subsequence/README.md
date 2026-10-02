---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - Dynamic Programming
---

<!-- problem:start -->

# [873. Length of Longest Fibonacci Subsequence](https://leetcode.com/problems/length-of-longest-fibonacci-subsequence)

[中文文档](/solution/0800-0899/0873.Length%20of%20Longest%20Fibonacci%20Subsequence/README.md)

## Mô tả

<!-- description:start -->

<p>Dãy <code>x<sub>1</sub>, x<sub>2</sub>, ..., x<sub>n</sub></code> được gọi là <em>kiểu Fibonacci</em> nếu:</p>

<ul>
	<li><code>n &gt;= 3</code></li>
	<li><code>x<sub>i</sub> + x<sub>i+1</sub> == x<sub>i+2</sub></code> với mọi <code>i + 2 &lt;= n</code></li>
</ul>

<p>Cho mảng <code>arr</code> gồm các số nguyên dương tạo thành một dãy <b>tăng nghiêm ngặt</b>. Hãy trả về <em><strong>độ dài</strong> của dãy con kiểu Fibonacci dài nhất trong</em> <code>arr</code>. Nếu không tồn tại dãy như vậy, trả về <code>0</code>.</p>

<p><strong>Dãy con</strong> được tạo từ một dãy <code>arr</code> bằng cách xóa tùy ý số phần tử (có thể không xóa phần tử nào) mà không thay đổi thứ tự các phần tử còn lại. Ví dụ, <code>[3, 5, 8]</code> là dãy con của <code>[3, 4, 5, 6, 7, 8]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,2,3,4,5,6,7,8]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Dãy con kiểu Fibonacci dài nhất là [1,2,3,5,8].</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,3,7,11,12,14,18]
<strong>Đầu ra:</strong> 3
<strong>Giải thích</strong>:<strong> </strong>Các dãy con kiểu Fibonacci dài nhất là [1,11,12], [3,11,14] hoặc [7,11,18].</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= arr.length &lt;= 1000</code></li>
	<li><code>1 &lt;= arr[i] &lt; arr[i + 1] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Tìm dãy con Fibonacci dài nhất trong một mảng tăng nghiêm ngặt. $n\le 1000$, nên có thể thử mở rộng từ mọi cặp phần tử, nhưng như vậy sẽ lặp lại công việc cho cùng một cặp kết thúc.
>
> $f[i][j]$ là độ dài dãy dài nhất kết thúc bằng $arr[j],arr[i]$. Nếu $arr[i]-arr[j]$ xuất hiện trước vị trí $j$, ta có thể nối dài cặp đó. Chỉ cập nhật đáp án khi độ dài đạt ít nhất $3$.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là độ dài dãy con kiểu Fibonacci dài nhất có phần tử cuối là $\textit{arr}[i]$ và phần tử kế cuối là $\textit{arr}[j]$. Ban đầu, với mọi $i \in [0, n)$ và $j \in [0, i)$, ta có $f[i][j] = 2$. Các phần tử còn lại bằng $0$.

Ta dùng hash table $d$ để lưu chỉ số của từng phần tử trong mảng $\textit{arr}$.

Sau đó, duyệt $\textit{arr}[i]$ và $\textit{arr}[j]$, với $i \in [2, n)$ và $j \in [1, i)$. Với cặp phần tử đang xét $\textit{arr}[i]$ và $\textit{arr}[j]$, tính $\textit{arr}[i] - \textit{arr}[j]$ và gọi kết quả là $t$. Nếu $t$ có trong mảng $\textit{arr}$ và chỉ số $k$ của nó thỏa mãn $k < j$, ta có thể tạo dãy con kiểu Fibonacci có hai phần tử cuối là $\textit{arr}[j]$ và $\textit{arr}[i]$, với độ dài $f[i][j] = \max(f[i][j], f[j][k] + 1)$. Cập nhật $f[i][j]$ theo cách này rồi cập nhật đáp án.

Sau khi duyệt xong, trả về đáp án.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là độ dài mảng $\textit{arr}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def lenLongestFibSubseq(self, arr: List[int]) -> int:
        n = len(arr)
        f = [[0] * n for _ in range(n)]
        d = {x: i for i, x in enumerate(arr)}
        for i in range(n):
            for j in range(i):
                f[i][j] = 2
        ans = 0
        for i in range(2, n):
            for j in range(1, i):
                t = arr[i] - arr[j]
                if t in d and (k := d[t]) < j:
                    f[i][j] = max(f[i][j], f[j][k] + 1)
                    ans = max(ans, f[i][j])
        return ans
```

#### Java

```java
class Solution {
    public int lenLongestFibSubseq(int[] arr) {
        int n = arr.length;
        int[][] f = new int[n][n];
        Map<Integer, Integer> d = new HashMap<>();
        for (int i = 0; i < n; ++i) {
            d.put(arr[i], i);
            for (int j = 0; j < i; ++j) {
                f[i][j] = 2;
            }
        }
        int ans = 0;
        for (int i = 2; i < n; ++i) {
            for (int j = 1; j < i; ++j) {
                int t = arr[i] - arr[j];
                Integer k = d.get(t);
                if (k != null && k < j) {
                    f[i][j] = Math.max(f[i][j], f[j][k] + 1);
                    ans = Math.max(ans, f[i][j]);
                }
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
    int lenLongestFibSubseq(vector<int>& arr) {
        int n = arr.size();
        int f[n][n];
        memset(f, 0, sizeof(f));
        unordered_map<int, int> d;
        for (int i = 0; i < n; ++i) {
            d[arr[i]] = i;
            for (int j = 0; j < i; ++j) {
                f[i][j] = 2;
            }
        }

        int ans = 0;
        for (int i = 2; i < n; ++i) {
            for (int j = 1; j < i; ++j) {
                int t = arr[i] - arr[j];
                auto it = d.find(t);
                if (it != d.end() && it->second < j) {
                    int k = it->second;
                    f[i][j] = max(f[i][j], f[j][k] + 1);
                    ans = max(ans, f[i][j]);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func lenLongestFibSubseq(arr []int) (ans int) {
	n := len(arr)
	f := make([][]int, n)
	for i := range f {
		f[i] = make([]int, n)
	}

	d := make(map[int]int)
	for i, x := range arr {
		d[x] = i
		for j := 0; j < i; j++ {
			f[i][j] = 2
		}
	}

	for i := 2; i < n; i++ {
		for j := 1; j < i; j++ {
			t := arr[i] - arr[j]
			if k, ok := d[t]; ok && k < j {
				f[i][j] = max(f[i][j], f[j][k]+1)
				ans = max(ans, f[i][j])
			}
		}
	}

	return
}
```

#### TypeScript

```ts
function lenLongestFibSubseq(arr: number[]): number {
    const n = arr.length;
    const f: number[][] = Array.from({ length: n }, () => Array(n).fill(0));
    const d: Map<number, number> = new Map();
    for (let i = 0; i < n; ++i) {
        d.set(arr[i], i);
        for (let j = 0; j < i; ++j) {
            f[i][j] = 2;
        }
    }
    let ans = 0;
    for (let i = 2; i < n; ++i) {
        for (let j = 1; j < i; ++j) {
            const t = arr[i] - arr[j];
            const k = d.get(t);
            if (k !== undefined && k < j) {
                f[i][j] = Math.max(f[i][j], f[j][k] + 1);
                ans = Math.max(ans, f[i][j]);
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;
impl Solution {
    pub fn len_longest_fib_subseq(arr: Vec<i32>) -> i32 {
        let n = arr.len();
        let mut f = vec![vec![0; n]; n];
        let mut d = HashMap::new();
        for i in 0..n {
            d.insert(arr[i], i);
            for j in 0..i {
                f[i][j] = 2;
            }
        }
        let mut ans = 0;
        for i in 2..n {
            for j in 1..i {
                let t = arr[i] - arr[j];
                if let Some(&k) = d.get(&t) {
                    if k < j {
                        f[i][j] = f[i][j].max(f[j][k] + 1);
                        ans = ans.max(f[i][j]);
                    }
                }
            }
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} arr
 * @return {number}
 */
var lenLongestFibSubseq = function (arr) {
    const n = arr.length;
    const f = Array.from({ length: n }, () => Array(n).fill(0));
    const d = new Map();
    for (let i = 0; i < n; ++i) {
        d.set(arr[i], i);
        for (let j = 0; j < i; ++j) {
            f[i][j] = 2;
        }
    }
    let ans = 0;
    for (let i = 2; i < n; ++i) {
        for (let j = 1; j < i; ++j) {
            const t = arr[i] - arr[j];
            const k = d.get(t);
            if (k !== undefined && k < j) {
                f[i][j] = Math.max(f[i][j], f[j][k] + 1);
                ans = Math.max(ans, f[i][j]);
            }
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
