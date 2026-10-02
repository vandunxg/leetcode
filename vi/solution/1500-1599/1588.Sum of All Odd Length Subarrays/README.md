---
comments: true
difficulty: Easy
rating: 1408
source: Biweekly Contest 35 Q1
tags:
    - Array
    - Math
    - Prefix Sum
---

<!-- problem:start -->

# [1588. Sum of All Odd Length Subarrays](https://leetcode.com/problems/sum-of-all-odd-length-subarrays)

[中文文档](/solution/1500-1599/1588.Sum%20of%20All%20Odd%20Length%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên dương <code>arr</code>, trả về <em>tổng của mọi <strong>mảng con có độ dài lẻ</strong> có thể tạo từ </em><code>arr</code>.</p>

<p><strong>Mảng con</strong> là một dãy con liên tiếp của mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,4,2,5,3]
<strong>Đầu ra:</strong> 58
<strong>Giải thích: </strong>Các mảng con có độ dài lẻ của arr và tổng tương ứng là:
[1] = 1
[4] = 4
[2] = 2
[5] = 5
[3] = 3
[1,4,2] = 7
[4,2,5] = 11
[2,5,3] = 10
[1,4,2,5,3] = 15
Nếu cộng tất cả lại, ta được 1 + 4 + 2 + 5 + 3 + 7 + 11 + 10 + 15 = 58</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,2]
<strong>Đầu ra:</strong> 3
<b>Giải thích: </b>Chỉ có 2 mảng con độ dài lẻ là [1] và [2]. Tổng của chúng là 3.</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [10,11,12]
<strong>Đầu ra:</strong> 66
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 100</code></li>
	<li><code>1 &lt;= arr[i] &lt;= 1000</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong></p>

<p>Bạn có thể giải bài toán với độ phức tạp thời gian O(n) không?</p>

<!-- description:end -->

## Solutions

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Sum every odd-length subarray. $n$ is typically at most $100$, so a triple loop would pass, yet it re-adds the same entries. The numbers of odd- and even-length subarrays ending at $i$ have closed forms.
>
> Let $f[i]$ and $g[i]$ be those two sums. An odd segment is an even segment ending at $i-1$ plus $arr[i]$, and there are $i/2+1$ of them; even segments are symmetric. The answer is the sum of all $f[i]$.

<!-- thinking:end -->

Ta định nghĩa hai mảng $f$ và $g$ độ dài $n$, trong đó $f[i]$ là tổng các mảng con kết thúc tại $\textit{arr}[i]$ có độ dài lẻ, còn $g[i]$ là tổng các mảng con kết thúc tại $\textit{arr}[i]$ có độ dài chẵn. Ban đầu, $f[0] = \textit{arr}[0]$ và $g[0] = 0$. Đáp án là $\sum_{i=0}^{n-1} f[i]$.

Khi $i > 0$, xét sự chuyển trạng thái của $f[i]$ và $g[i]$:

Với trạng thái $f[i]$, phần tử $\textit{arr}[i]$ có thể tạo mảng con độ dài lẻ với các mảng con trước đó thuộc $g[i-1]$. Có $(i / 2) + 1$ mảng như vậy, nên $f[i] = g[i-1] + \textit{arr}[i] \times ((i / 2) + 1)$.

Với trạng thái $g[i]$, khi $i = 0$ không có mảng con độ dài chẵn nào, nên $g[0] = 0$. Khi $i > 0$, phần tử $\textit{arr}[i]$ có thể tạo mảng con độ dài chẵn với các mảng con trước đó thuộc $f[i-1]$. Có $(i + 1) / 2$ mảng như vậy, nên $g[i] = f[i-1] + \textit{arr}[i] \times ((i + 1) / 2)$.

Đáp án cuối cùng là $\sum_{i=0}^{n-1} f[i]$.

The time complexity is $O(n)$, and the space complexity is $O(n)$. Here, $n$ is the length of the array $\textit{arr}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumOddLengthSubarrays(self, arr: List[int]) -> int:
        n = len(arr)
        f = [0] * n
        g = [0] * n
        ans = f[0] = arr[0]
        for i in range(1, n):
            f[i] = g[i - 1] + arr[i] * (i // 2 + 1)
            g[i] = f[i - 1] + arr[i] * ((i + 1) // 2)
            ans += f[i]
        return ans
```

#### Java

```java
class Solution {
    public int sumOddLengthSubarrays(int[] arr) {
        int n = arr.length;
        int[] f = new int[n];
        int[] g = new int[n];
        int ans = f[0] = arr[0];
        for (int i = 1; i < n; ++i) {
            f[i] = g[i - 1] + arr[i] * (i / 2 + 1);
            g[i] = f[i - 1] + arr[i] * ((i + 1) / 2);
            ans += f[i];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int sumOddLengthSubarrays(vector<int>& arr) {
        int n = arr.size();
        vector<int> f(n, arr[0]);
        vector<int> g(n);
        int ans = f[0];
        for (int i = 1; i < n; ++i) {
            f[i] = g[i - 1] + arr[i] * (i / 2 + 1);
            g[i] = f[i - 1] + arr[i] * ((i + 1) / 2);
            ans += f[i];
        }
        return ans;
    }
};
```

#### Go

```go
func sumOddLengthSubarrays(arr []int) (ans int) {
	n := len(arr)
	f := make([]int, n)
	g := make([]int, n)
	f[0] = arr[0]
	ans = f[0]
	for i := 1; i < n; i++ {
		f[i] = g[i-1] + arr[i]*(i/2+1)
		g[i] = f[i-1] + arr[i]*((i+1)/2)
		ans += f[i]
	}
	return
}
```

#### TypeScript

```ts
function sumOddLengthSubarrays(arr: number[]): number {
    const n = arr.length;
    const f: number[] = Array(n).fill(arr[0]);
    const g: number[] = Array(n).fill(0);
    let ans = f[0];
    for (let i = 1; i < n; ++i) {
        f[i] = g[i - 1] + arr[i] * ((i >> 1) + 1);
        g[i] = f[i - 1] + arr[i] * ((i + 1) >> 1);
        ans += f[i];
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn sum_odd_length_subarrays(arr: Vec<i32>) -> i32 {
        let n = arr.len();
        let mut f = vec![0; n];
        let mut g = vec![0; n];
        let mut ans = arr[0];
        f[0] = arr[0];
        for i in 1..n {
            f[i] = g[i - 1] + arr[i] * ((i as i32) / 2 + 1);
            g[i] = f[i - 1] + arr[i] * (((i + 1) as i32) / 2);
            ans += f[i];
        }
        ans
    }
}
```

#### C

```c
int sumOddLengthSubarrays(int* arr, int arrSize) {
    int n = arrSize;
    int f[n];
    int g[n];
    int ans = f[0] = arr[0];
    g[0] = 0;
    for (int i = 1; i < n; ++i) {
        f[i] = g[i - 1] + arr[i] * (i / 2 + 1);
        g[i] = f[i - 1] + arr[i] * ((i + 1) / 2);
        ans += f[i];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động (Tối ưu không gian)

<!-- thinking:start -->

> **Tư duy**
>
> Each state reads only the previous $f$ and $g$, so two rolling scalars suffice. Time stays linear and the extra arrays disappear.

<!-- thinking:end -->

Ta nhận thấy giá trị của $f[i]$ và $g[i]$ chỉ phụ thuộc vào $f[i - 1]$ và $g[i - 1]$. Vì vậy, ta có thể dùng hai biến $f$ và $g$ để lần lượt lưu giá trị của $f[i - 1]$ và $g[i - 1]$, nhờ đó tối ưu độ phức tạp không gian.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumOddLengthSubarrays(self, arr: List[int]) -> int:
        ans, f, g = arr[0], arr[0], 0
        for i in range(1, len(arr)):
            ff = g + arr[i] * (i // 2 + 1)
            gg = f + arr[i] * ((i + 1) // 2)
            f, g = ff, gg
            ans += f
        return ans
```

#### Java

```java
class Solution {
    public int sumOddLengthSubarrays(int[] arr) {
        int ans = arr[0], f = arr[0], g = 0;
        for (int i = 1; i < arr.length; ++i) {
            int ff = g + arr[i] * (i / 2 + 1);
            int gg = f + arr[i] * ((i + 1) / 2);
            f = ff;
            g = gg;
            ans += f;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int sumOddLengthSubarrays(vector<int>& arr) {
        int ans = 0, f = 0, g = 0;
        for (int i = 0; i < arr.size(); ++i) {
            int ff = g + arr[i] * (i / 2 + 1);
            int gg = i ? f + arr[i] * ((i + 1) / 2) : 0;
            f = ff;
            g = gg;
            ans += f;
        }
        return ans;
    }
};
```

#### Go

```go
func sumOddLengthSubarrays(arr []int) (ans int) {
	f, g := arr[0], 0
	ans = f
	for i := 1; i < len(arr); i++ {
		ff := g + arr[i]*(i/2+1)
		gg := f + arr[i]*((i+1)/2)
		f, g = ff, gg
		ans += f
	}
	return
}
```

#### TypeScript

```ts
function sumOddLengthSubarrays(arr: number[]): number {
    const n = arr.length;
    let [ans, f, g] = [arr[0], arr[0], 0];
    for (let i = 1; i < n; ++i) {
        const ff = g + arr[i] * (Math.floor(i / 2) + 1);
        const gg = f + arr[i] * Math.floor((i + 1) / 2);
        [f, g] = [ff, gg];
        ans += f;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn sum_odd_length_subarrays(arr: Vec<i32>) -> i32 {
        let mut ans = arr[0];
        let mut f = arr[0];
        let mut g = 0;
        for i in 1..arr.len() {
            let ff = g + arr[i] * ((i as i32) / 2 + 1);
            let gg = f + arr[i] * (((i + 1) as i32) / 2);
            f = ff;
            g = gg;
            ans += f;
        }
        ans
    }
}
```

#### C

```c
int sumOddLengthSubarrays(int* arr, int arrSize) {
    int ans = arr[0], f = arr[0], g = 0;
    for (int i = 1; i < arrSize; ++i) {
        int ff = g + arr[i] * (i / 2 + 1);
        int gg = f + arr[i] * ((i + 1) / 2);
        f = ff;
        g = gg;
        ans += f;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
