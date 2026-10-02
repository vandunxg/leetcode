---
comments: true
difficulty: Medium
rating: 1787
source: Weekly Contest 195 Q2
tags:
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [1497. Check If Array Pairs Are Divisible by k](https://leetcode.com/problems/check-if-array-pairs-are-divisible-by-k)

[中文文档](/solution/1400-1499/1497.Check%20If%20Array%20Pairs%20Are%20Divisible%20by%20k/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>arr</code> có độ dài chẵn <code>n</code> và một số nguyên <code>k</code>.</p>

<p>Ta muốn chia mảng thành chính xác <code>n / 2</code> cặp sao cho tổng của mỗi cặp chia hết cho <code>k</code>.</p>

<p>Trả về <code>true</code><em> nếu có thể thực hiện được, ngược lại trả về </em><code>false</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> arr = [1,2,3,4,5,10,6,7,8,9], k = 5
<strong>Output:</strong> true
<strong>Giải thích:</strong> Các cặp là (1,9),(2,8),(3,7),(4,6) và (5,10).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> arr = [1,2,3,4,5,6], k = 7
<strong>Output:</strong> true
<strong>Giải thích:</strong> Các cặp là (1,6),(2,5) và (3,4).
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> arr = [1,2,3,4,5,6], k = 10
<strong>Output:</strong> false
<strong>Giải thích:</strong> Có thể thử mọi cặp để thấy rằng không có cách nào chia arr thành 3 cặp mà tổng của mỗi cặp đều chia hết cho 10.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>arr.length == n</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>n</code> là số chẵn.</li>
	<li><code>-10<sup>9</sup> &lt;= arr[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm phần dư

<!-- thinking:start -->

> **Tư duy**
>
> Hai giá trị có tổng là bội của $k$ khi và chỉ khi tổng phần dư của chúng bằng $0$ hoặc $k$. Vì $n\le 10^5$, ta đếm $x\bmod k$: số phần tử có phần dư $0$ phải là số chẵn, còn phần dư $i$ phải khớp với $k-i$ (bao gồm $k/2$ khi $k$ chẵn).

<!-- thinking:end -->

Tổng của hai số $a$ và $b$ chia hết cho $k$ khi và chỉ khi tổng phần dư của chúng khi chia cho $k$ chia hết cho $k$.

Do đó, ta có thể đếm phần dư của mỗi số trong mảng khi chia cho $k$ và lưu chúng trong một mảng $\textit{cnt}$. Sau đó, ta duyệt mảng $\textit{cnt}$. Với mỗi số $i$ trong đoạn $[1,..k-1]$, nếu giá trị của $\textit{cnt}[i]$ và $\textit{cnt}[k-i]$ không bằng nhau, điều đó có nghĩa là ta không thể chia các số trong mảng thành $n/2$ cặp sao cho tổng của mỗi cặp chia hết cho $k$. Tương tự, nếu giá trị của $\textit{cnt}[0]$ không chẵn, ta cũng không thể chia các số trong mảng thành $n/2$ cặp sao cho tổng của mỗi cặp chia hết cho $k$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{arr}$. Độ phức tạp không gian là $O(k)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canArrange(self, arr: List[int], k: int) -> bool:
        cnt = Counter(x % k for x in arr)
        return cnt[0] % 2 == 0 and all(cnt[i] == cnt[k - i] for i in range(1, k))
```

#### Java

```java
class Solution {
    public boolean canArrange(int[] arr, int k) {
        int[] cnt = new int[k];
        for (int x : arr) {
            ++cnt[(x % k + k) % k];
        }
        for (int i = 1; i < k; ++i) {
            if (cnt[i] != cnt[k - i]) {
                return false;
            }
        }
        return cnt[0] % 2 == 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canArrange(vector<int>& arr, int k) {
        vector<int> cnt(k);
        for (int& x : arr) {
            ++cnt[((x % k) + k) % k];
        }
        for (int i = 1; i < k; ++i) {
            if (cnt[i] != cnt[k - i]) {
                return false;
            }
        }
        return cnt[0] % 2 == 0;
    }
};
```

#### Go

```go
func canArrange(arr []int, k int) bool {
	cnt := make([]int, k)
	for _, x := range arr {
		cnt[(x%k+k)%k]++
	}
	for i := 1; i < k; i++ {
		if cnt[i] != cnt[k-i] {
			return false
		}
	}
	return cnt[0]%2 == 0
}
```

#### TypeScript

```ts
function canArrange(arr: number[], k: number): boolean {
    const cnt: number[] = Array(k).fill(0);
    for (const x of arr) {
        ++cnt[((x % k) + k) % k];
    }
    for (let i = 1; i < k; ++i) {
        if (cnt[i] !== cnt[k - i]) {
            return false;
        }
    }
    return cnt[0] % 2 === 0;
}
```

#### Rust

```rust
impl Solution {
    pub fn can_arrange(arr: Vec<i32>, k: i32) -> bool {
        let k = k as usize;
        let mut cnt = vec![0; k];
        for &x in &arr {
            cnt[((x % k as i32 + k as i32) % k as i32) as usize] += 1;
        }
        for i in 1..k {
            if cnt[i] != cnt[k - i] {
                return false;
            }
        }
        cnt[0] % 2 == 0
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} arr
 * @param {number} k
 * @return {boolean}
 */
var canArrange = function (arr, k) {
    const cnt = Array(k).fill(0);
    for (const x of arr) {
        ++cnt[((x % k) + k) % k];
    }
    for (let i = 1; i < k; ++i) {
        if (cnt[i] !== cnt[k - i]) {
            return false;
        }
    }
    return cnt[0] % 2 === 0;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
