---
comments: true
difficulty: Medium
rating: 1610
source: Biweekly Contest 31 Q2
tags:
    - Array
    - Math
    - Dynamic Programming
    - Prefix Sum
---

<!-- problem:start -->

# [1524. Number of Sub-arrays With Odd Sum](https://leetcode.com/problems/number-of-sub-arrays-with-odd-sum)

[中文文档](/solution/1500-1599/1524.Number%20of%20Sub-arrays%20With%20Odd%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>arr</code>, hãy trả về <em>số lượng mảng con có tổng <strong>lẻ</strong></em>.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về phần dư của đáp án khi chia cho <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> arr = [1,3,5]
<strong>Output:</strong> 4
<strong>Giải thích:</strong> Tất cả mảng con là [[1],[1,3],[1,3,5],[3],[3,5],[5]]
Tổng các mảng con là [1,4,9,3,8,5].
Các tổng lẻ là [1,9,3,5] nên đáp án là 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> arr = [2,4,6]
<strong>Output:</strong> 0
<strong>Giải thích:</strong> Tất cả mảng con là [[2],[2,4],[2,4,6],[4],[4,6],[6]]
Tổng các mảng con là [2,6,12,4,10,6].
Mọi mảng con đều có tổng chẵn nên đáp án là 0.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> arr = [1,2,3,4,5,6,7]
<strong>Output:</strong> 16
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= arr[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng tiền tố + bộ đếm

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các mảng con có tổng lẻ. Có $O(n^2)$ mảng như vậy và $n\le 10^5$, nên không thể tính trực tiếp. Tính chẵn lẻ của tổng mảng con chính là tính chẵn lẻ của hiệu hai tổng tiền tố.
>
> Một tổng tiền tố lẻ ghép được với mọi tổng tiền tố chẵn trước đó; tổng tiền tố chẵn ghép được với mọi tổng tiền tố lẻ. Khi duyệt, ta duy trì hai bộ đếm, cộng số lượng phù hợp với tổng tiền tố hiện tại rồi cập nhật. Cuối cùng lấy đáp án theo modulo $10^9+7$.

<!-- thinking:end -->

Ta định nghĩa mảng đếm $\textit{cnt}$ có độ dài 2, trong đó $\textit{cnt}[0]$ và $\textit{cnt}[1]$ lần lượt biểu diễn số tổng tiền tố chẵn và lẻ. Ban đầu, $\textit{cnt}[0] = 1$ và $\textit{cnt}[1] = 0$.

Tiếp theo, ta duy trì tổng tiền tố hiện tại $s$, ban đầu $s = 0$.

Duyệt mảng $\textit{arr}$. Với mỗi phần tử $x$, cộng $x$ vào $s$, sau đó dựa trên tính chẵn lẻ của $s$, cộng $\textit{cnt}[s \mod 2 \oplus 1]$ vào đáp án rồi tăng $\textit{cnt}[s \mod 2]$ thêm 1.

Sau khi duyệt xong, ta thu được đáp án. Lưu ý thực hiện phép modulo cho đáp án.

Độ phức tạp thời gian là $O(n)$, với $n$ là độ dài mảng $\textit{arr}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numOfSubarrays(self, arr: List[int]) -> int:
        mod = 10**9 + 7
        cnt = [1, 0]
        ans = s = 0
        for x in arr:
            s += x
            ans = (ans + cnt[s & 1 ^ 1]) % mod
            cnt[s & 1] += 1
        return ans
```

#### Java

```java
class Solution {
    public int numOfSubarrays(int[] arr) {
        final int mod = (int) 1e9 + 7;
        int[] cnt = {1, 0};
        int ans = 0, s = 0;
        for (int x : arr) {
            s += x;
            ans = (ans + cnt[s & 1 ^ 1]) % mod;
            ++cnt[s & 1];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numOfSubarrays(vector<int>& arr) {
        const int mod = 1e9 + 7;
        int cnt[2] = {1, 0};
        int ans = 0, s = 0;
        for (int x : arr) {
            s += x;
            ans = (ans + cnt[s & 1 ^ 1]) % mod;
            ++cnt[s & 1];
        }
        return ans;
    }
};
```

#### Go

```go
func numOfSubarrays(arr []int) (ans int) {
	const mod int = 1e9 + 7
	cnt := [2]int{1, 0}
	s := 0
	for _, x := range arr {
		s += x
		ans = (ans + cnt[s&1^1]) % mod
		cnt[s&1]++
	}
	return
}
```

#### TypeScript

```ts
function numOfSubarrays(arr: number[]): number {
    let ans = 0;
    let s = 0;
    const cnt: number[] = [1, 0];
    const mod = 1e9 + 7;
    for (const x of arr) {
        s += x;
        ans = (ans + cnt[(s & 1) ^ 1]) % mod;
        cnt[s & 1]++;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn num_of_subarrays(arr: Vec<i32>) -> i32 {
        const MOD: i32 = 1_000_000_007;
        let mut cnt = [1, 0];
        let mut ans = 0;
        let mut s = 0;
        for &x in arr.iter() {
            s += x;
            ans = (ans + cnt[((s & 1) ^ 1) as usize]) % MOD;
            cnt[(s & 1) as usize] += 1;
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
