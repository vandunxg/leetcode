---
comments: true
difficulty: Medium
rating: 1459
source: Weekly Contest 170 Q2
tags:
    - Bit Manipulation
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [1310. XOR Queries of a Subarray](https://leetcode.com/problems/xor-queries-of-a-subarray)

[中文文档](/solution/1300-1399/1310.XOR%20Queries%20of%20a%20Subarray/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên dương <code>arr</code> và mảng truy vấn <code>queries</code>, trong đó <code>queries[i] = [left<sub>i, </sub>right<sub>i</sub>]</code>.</p>

<p>Với mỗi truy vấn <code>i</code>, hãy tính phép <strong>XOR</strong> các phần tử từ <code>left<sub>i</sub></code> đến <code>right<sub>i</sub></code> (tức là <code>arr[left<sub>i</sub>] XOR arr[left<sub>i</sub> + 1] XOR ... XOR arr[right<sub>i</sub>]</code> ).</p>

<p>Trả về mảng <code>answer</code>, trong đó <code>answer[i]</code> là kết quả của truy vấn thứ <code>i<sup>th</sup></code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,3,4,8], queries = [[0,1],[1,2],[0,3],[3,3]]
<strong>Đầu ra:</strong> [2,7,14,8] 
<strong>Giải thích:</strong> 
Biểu diễn nhị phân của các phần tử trong mảng là:
1 = 0001 
3 = 0011 
4 = 0100 
8 = 1000 
Kết quả XOR của các truy vấn là:
[0,1] = 1 xor 3 = 2 
[1,2] = 3 xor 4 = 7 
[0,3] = 1 xor 3 xor 4 xor 8 = 14 
[3,3] = 8
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [4,8,2,10], queries = [[2,3],[1,3],[0,0],[0,3]]
<strong>Đầu ra:</strong> [8,0,4,4]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length, queries.length &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= arr[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>queries[i].length == 2</code></li>
	<li><code>0 &lt;= left<sub>i</sub> &lt;= right<sub>i</sub> &lt; arr.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix XOR

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi truy vấn yêu cầu tính XOR trên một đoạn. Tính XOR trực tiếp từ $l$ đến $r$ cho từng truy vấn có thể tốn thời gian bậc hai theo số lượng truy vấn. Vì $x \oplus x = 0$, prefix XOR $s[i]=\textit{arr}[0]\oplus\cdots\oplus\textit{arr}[i-1]$ cho phép tính XOR đoạn $[l,r]$ bằng $s[r+1]\oplus s[l]$. Chỉ cần tạo mảng prefix trong thời gian tuyến tính, sau đó mỗi truy vấn mất $O(1)$.

<!-- thinking:end -->

Ta dùng mảng prefix XOR $s$ có độ dài $n+1$ để lưu kết quả XOR prefix của mảng $\textit{arr}$, với $s[i] = s[i-1] \oplus \textit{arr}[i-1]$. Nói cách khác, $s[i]$ là kết quả XOR của $i$ phần tử đầu tiên trong $\textit{arr}$.

Với truy vấn $[l, r]$, ta có thể tính như sau:

$$
\begin{aligned}
\textit{arr}[l] \oplus \textit{arr}[l+1] \oplus \cdots \oplus \textit{arr}[r] &= (\textit{arr}[0] \oplus \textit{arr}[1] \oplus \cdots \oplus \textit{arr}[l-1]) \oplus (\textit{arr}[0] \oplus \textit{arr}[1] \oplus \cdots \oplus \textit{arr}[r]) \\
&= s[l] \oplus s[r+1]
\end{aligned}
$$

Độ phức tạp thời gian là $O(n+m)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ và $m$ lần lượt là độ dài mảng $\textit{arr}$ và mảng truy vấn $\textit{queries}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def xorQueries(self, arr: List[int], queries: List[List[int]]) -> List[int]:
        s = list(accumulate(arr, xor, initial=0))
        return [s[r + 1] ^ s[l] for l, r in queries]
```

#### Java

```java
class Solution {
    public int[] xorQueries(int[] arr, int[][] queries) {
        int n = arr.length;
        int[] s = new int[n + 1];
        for (int i = 1; i <= n; ++i) {
            s[i] = s[i - 1] ^ arr[i - 1];
        }
        int m = queries.length;
        int[] ans = new int[m];
        for (int i = 0; i < m; ++i) {
            int l = queries[i][0], r = queries[i][1];
            ans[i] = s[r + 1] ^ s[l];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> xorQueries(vector<int>& arr, vector<vector<int>>& queries) {
        int n = arr.size();
        int s[n + 1];
        memset(s, 0, sizeof(s));
        for (int i = 1; i <= n; ++i) {
            s[i] = s[i - 1] ^ arr[i - 1];
        }
        vector<int> ans;
        for (auto& q : queries) {
            int l = q[0], r = q[1];
            ans.push_back(s[r + 1] ^ s[l]);
        }
        return ans;
    }
};
```

#### Go

```go
func xorQueries(arr []int, queries [][]int) (ans []int) {
	n := len(arr)
	s := make([]int, n+1)
	for i, x := range arr {
		s[i+1] = s[i] ^ x
	}
	for _, q := range queries {
		l, r := q[0], q[1]
		ans = append(ans, s[r+1]^s[l])
	}
	return
}
```

#### TypeScript

```ts
function xorQueries(arr: number[], queries: number[][]): number[] {
    const n = arr.length;
    const s: number[] = Array(n + 1).fill(0);
    for (let i = 0; i < n; ++i) {
        s[i + 1] = s[i] ^ arr[i];
    }
    return queries.map(([l, r]) => s[r + 1] ^ s[l]);
}
```

#### JavaScript

```js
/**
 * @param {number[]} arr
 * @param {number[][]} queries
 * @return {number[]}
 */
var xorQueries = function (arr, queries) {
    const n = arr.length;
    const s = Array(n + 1).fill(0);
    for (let i = 0; i < n; ++i) {
        s[i + 1] = s[i] ^ arr[i];
    }
    return queries.map(([l, r]) => s[r + 1] ^ s[l]);
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
