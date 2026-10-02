---
comments: true
difficulty: Medium
rating: 1524
source: Weekly Contest 188 Q2
tags:
    - Bit Manipulation
    - Array
    - Hash Table
    - Math
    - Prefix Sum
---

<!-- problem:start -->

# [1442. Count Triplets That Can Form Two Arrays of Equal XOR](https://leetcode.com/problems/count-triplets-that-can-form-two-arrays-of-equal-xor)

[中文文档](/solution/1400-1499/1442.Count%20Triplets%20That%20Can%20Form%20Two%20Arrays%20of%20Equal%20XOR/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>arr</code>.</p>

<p>Ta muốn chọn ba chỉ số <code>i</code>, <code>j</code> và <code>k</code> sao cho <code>(0 &lt;= i &lt; j &lt;= k &lt; arr.length)</code>.</p>

<p>Định nghĩa <code>a</code> và <code>b</code> như sau:</p>

<ul>
	<li><code>a = arr[i] ^ arr[i + 1] ^ ... ^ arr[j - 1]</code></li>
	<li><code>b = arr[j] ^ arr[j + 1] ^ ... ^ arr[k]</code></li>
</ul>

<p>Lưu ý rằng <strong>^</strong> biểu diễn phép toán <strong>xor theo bit</strong>.</p>

<p>Trả về <em>số bộ ba</em> (<code>i</code>, <code>j</code> và <code>k</code>) sao cho <code>a == b</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [2,3,1,6,7]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Các bộ ba là (0,1,2), (0,2,2), (2,3,4) và (2,4,4)
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,1,1,1,1]
<strong>Đầu ra:</strong> 10
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 300</code></li>
	<li><code>1 &lt;= arr[i] &lt;= 10<sup>8</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> $a=b$ khi và chỉ khi XOR của $[i,k]$ bằng $0$, khi đó mọi $j\in(i,k]$ đều thỏa mãn ($k-i$ bộ ba). Vì $n\le 300$, ta liệt kê $i$ và $k$ đồng thời duy trì XOR hiện tại.

<!-- thinking:end -->

Theo mô tả bài toán, để tìm các bộ ba $(i, j, k)$ thỏa mãn $a = b$, tương đương với $s = a \oplus b = 0$, ta chỉ cần liệt kê đầu trái $i$, sau đó tính tổng XOR tiền tố của đoạn $[i, k]$ với $k$ là đầu phải. Nếu $s = 0$, thì mọi $j \in [i + 1, k]$ đều thỏa mãn điều kiện $a = b$, nghĩa là $(i, j, k)$ là một bộ ba hợp lệ. Có $k - i$ bộ ba như vậy, ta có thể cộng chúng vào đáp án.

Sau khi hoàn tất việc liệt kê, ta trả về đáp án.

Độ phức tạp thời gian là $O(n^2)$, trong đó $n$ là độ dài của mảng $\textit{arr}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countTriplets(self, arr: List[int]) -> int:
        ans, n = 0, len(arr)
        for i, x in enumerate(arr):
            s = x
            for k in range(i + 1, n):
                s ^= arr[k]
                if s == 0:
                    ans += k - i
        return ans
```

#### Java

```java
class Solution {
    public int countTriplets(int[] arr) {
        int ans = 0, n = arr.length;
        for (int i = 0; i < n; ++i) {
            int s = arr[i];
            for (int k = i + 1; k < n; ++k) {
                s ^= arr[k];
                if (s == 0) {
                    ans += k - i;
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
    int countTriplets(vector<int>& arr) {
        int ans = 0, n = arr.size();
        for (int i = 0; i < n; ++i) {
            int s = arr[i];
            for (int k = i + 1; k < n; ++k) {
                s ^= arr[k];
                if (s == 0) {
                    ans += k - i;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countTriplets(arr []int) (ans int) {
	for i, x := range arr {
		s := x
		for k := i + 1; k < len(arr); k++ {
			s ^= arr[k]
			if s == 0 {
				ans += k - i
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function countTriplets(arr: number[]): number {
    const n = arr.length;
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        let s = arr[i];
        for (let k = i + 1; k < n; ++k) {
            s ^= arr[k];
            if (s === 0) {
                ans += k - i;
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_triplets(arr: Vec<i32>) -> i32 {
        let mut ans = 0;
        let n = arr.len();

        for i in 0..n {
            let mut s = arr[i];
            for k in (i + 1)..n {
                s ^= arr[k];
                if s == 0 {
                    ans += (k - i) as i32;
                }
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
