---
comments: true
difficulty: Medium
tags:
    - Stack
    - Array
    - Dynamic Programming
    - Monotonic Stack
---

<!-- problem:start -->

# [907. Sum of Subarray Minimums](https://leetcode.com/problems/sum-of-subarray-minimums)

[中文文档](/solution/0900-0999/0907.Sum%20of%20Subarray%20Minimums/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên arr, hãy tính tổng <code>min(b)</code> với <code>b</code> là mỗi mảng con (liên tiếp) của <code>arr</code>. Vì đáp án có thể lớn, hãy trả về kết quả <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> arr = [3,1,2,4]
<strong>Output:</strong> 17
<strong>Giải thích:</strong> 
Các mảng con là [3], [1], [2], [4], [3,1], [1,2], [2,4], [3,1,2], [1,2,4], [3,1,2,4]. 
Các giá trị nhỏ nhất lần lượt là 3, 1, 2, 4, 1, 1, 2, 1, 1, 1.
Tổng bằng 17.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> arr = [11,81,94,43,3]
<strong>Output:</strong> 444
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= arr[i] &lt;= 3 * 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Monotonic Stack

<!-- thinking:start -->

> **Tư duy**
>
> Nếu liệt kê từng khoảng để cộng giá trị nhỏ nhất của mọi mảng con thì độ phức tạp là bậc hai. Thay vào đó, tính contribution: nhân $arr[i]$ với số mảng con mà nó là giá trị nhỏ nhất.
>
> Monotonic stack tìm giá trị nhỏ hơn nghiêm ngặt gần nhất bên trái và giá trị nhỏ hơn hoặc bằng gần nhất bên phải, nhờ đó các giá trị bằng nhau chỉ được tính một lần. Tích độ dài hai khoảng cho biết số mảng con tương ứng.

<!-- thinking:end -->

Bài toán yêu cầu tính tổng giá trị nhỏ nhất của mỗi mảng con. Tương đương, với mỗi phần tử $arr[i]$, ta đếm số mảng con mà nó là giá trị nhỏ nhất, nhân với $arr[i]$, rồi cộng các kết quả lại.

Vì vậy, trọng tâm là đếm số mảng con mà $arr[i]$ là giá trị nhỏ nhất. Với mỗi $arr[i]$, tìm vị trí đầu tiên $left[i]$ ở bên trái có giá trị nhỏ hơn $arr[i]$, và vị trí đầu tiên $right[i]$ ở bên phải có giá trị nhỏ hơn hoặc bằng $arr[i]$. Số mảng con mà $arr[i]$ là giá trị nhỏ nhất bằng $(i - left[i]) \times (right[i] - i)$.

Vì sao ta tìm vị trí đầu tiên $right[i]$ bên phải có giá trị nhỏ hơn hoặc bằng $arr[i]$, thay vì chỉ tìm giá trị nhỏ hơn $arr[i]$? Nếu chỉ tìm vị trí có giá trị nhỏ hơn, một số mảng con sẽ bị tính trùng.

Xét ví dụ sau để minh họa:

Phần tử ở chỉ số $3$ có giá trị $2$; phần tử đầu tiên bên trái nhỏ hơn $2$ nằm ở chỉ số $0$. Nếu tìm phần tử đầu tiên bên phải nhỏ hơn $2$, ta được chỉ số $7$. Như vậy khoảng của mảng con là $(0, 7)$. Lưu ý đây là khoảng mở.

```
0 4 3 2 5 3 2 1
*     ^       *
```

Tương tự, khoảng của mảng con cho phần tử ở chỉ số $6$ cũng là $(0, 7)$. Như vậy phần tử ở chỉ số $3$ và chỉ số $6$ có cùng khoảng mảng con, dẫn đến tính trùng.

```
0 4 3 2 5 3 2 1
*           ^ *
```

Nếu tìm phần tử đầu tiên bên phải có giá trị nhỏ hơn hoặc bằng giá trị hiện tại thì sẽ không bị trùng: khoảng của phần tử ở chỉ số $3$ trở thành $(0, 6)$, còn khoảng của phần tử ở chỉ số $6$ là $(0, 7)$, nên chúng khác nhau.

Quay lại bài toán, ta duyệt mảng và với mỗi phần tử $arr[i]$, dùng monotonic stack tìm vị trí đầu tiên $left[i]$ bên trái có giá trị nhỏ hơn $arr[i]$, cùng vị trí đầu tiên $right[i]$ bên phải có giá trị nhỏ hơn hoặc bằng $arr[i]$. Số mảng con mà $arr[i]$ là giá trị nhỏ nhất bằng $(i - left[i]) \times (right[i] - i)$. Nhân kết quả này với $arr[i]$, rồi cộng tất cả lại.

Cần chú ý đến tràn số và phép modulo.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, với $n$ là độ dài mảng $arr$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumSubarrayMins(self, arr: List[int]) -> int:
        n = len(arr)
        left = [-1] * n
        right = [n] * n
        stk = []
        for i, v in enumerate(arr):
            while stk and arr[stk[-1]] >= v:
                stk.pop()
            if stk:
                left[i] = stk[-1]
            stk.append(i)

        stk = []
        for i in range(n - 1, -1, -1):
            while stk and arr[stk[-1]] > arr[i]:
                stk.pop()
            if stk:
                right[i] = stk[-1]
            stk.append(i)
        mod = 10**9 + 7
        return sum((i - left[i]) * (right[i] - i) * v for i, v in enumerate(arr)) % mod
```

#### Java

```java
class Solution {
    public int sumSubarrayMins(int[] arr) {
        int n = arr.length;
        int[] left = new int[n];
        int[] right = new int[n];
        Arrays.fill(left, -1);
        Arrays.fill(right, n);
        Deque<Integer> stk = new ArrayDeque<>();
        for (int i = 0; i < n; ++i) {
            while (!stk.isEmpty() && arr[stk.peek()] >= arr[i]) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                left[i] = stk.peek();
            }
            stk.push(i);
        }
        stk.clear();
        for (int i = n - 1; i >= 0; --i) {
            while (!stk.isEmpty() && arr[stk.peek()] > arr[i]) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                right[i] = stk.peek();
            }
            stk.push(i);
        }
        final int mod = (int) 1e9 + 7;
        long ans = 0;
        for (int i = 0; i < n; ++i) {
            ans += (long) (i - left[i]) * (right[i] - i) % mod * arr[i] % mod;
            ans %= mod;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int sumSubarrayMins(vector<int>& arr) {
        int n = arr.size();
        vector<int> left(n, -1);
        vector<int> right(n, n);
        stack<int> stk;
        for (int i = 0; i < n; ++i) {
            while (!stk.empty() && arr[stk.top()] >= arr[i]) {
                stk.pop();
            }
            if (!stk.empty()) {
                left[i] = stk.top();
            }
            stk.push(i);
        }
        stk = stack<int>();
        for (int i = n - 1; i >= 0; --i) {
            while (!stk.empty() && arr[stk.top()] > arr[i]) {
                stk.pop();
            }
            if (!stk.empty()) {
                right[i] = stk.top();
            }
            stk.push(i);
        }
        long long ans = 0;
        const int mod = 1e9 + 7;
        for (int i = 0; i < n; ++i) {
            ans += 1LL * (i - left[i]) * (right[i] - i) * arr[i] % mod;
            ans %= mod;
        }
        return ans;
    }
};
```

#### Go

```go
func sumSubarrayMins(arr []int) (ans int) {
	n := len(arr)
	left := make([]int, n)
	right := make([]int, n)
	for i := range left {
		left[i] = -1
		right[i] = n
	}
	stk := []int{}
	for i, v := range arr {
		for len(stk) > 0 && arr[stk[len(stk)-1]] >= v {
			stk = stk[:len(stk)-1]
		}
		if len(stk) > 0 {
			left[i] = stk[len(stk)-1]
		}
		stk = append(stk, i)
	}
	stk = []int{}
	for i := n - 1; i >= 0; i-- {
		for len(stk) > 0 && arr[stk[len(stk)-1]] > arr[i] {
			stk = stk[:len(stk)-1]
		}
		if len(stk) > 0 {
			right[i] = stk[len(stk)-1]
		}
		stk = append(stk, i)
	}
	const mod int = 1e9 + 7
	for i, v := range arr {
		ans += (i - left[i]) * (right[i] - i) * v % mod
		ans %= mod
	}
	return
}
```

#### TypeScript

```ts
function sumSubarrayMins(arr: number[]): number {
    const n: number = arr.length;
    const left: number[] = Array(n).fill(-1);
    const right: number[] = Array(n).fill(n);
    const stk: number[] = [];
    for (let i = 0; i < n; ++i) {
        while (stk.length > 0 && arr[stk.at(-1)] >= arr[i]) {
            stk.pop();
        }
        if (stk.length > 0) {
            left[i] = stk.at(-1);
        }
        stk.push(i);
    }

    stk.length = 0;
    for (let i = n - 1; ~i; --i) {
        while (stk.length > 0 && arr[stk.at(-1)] > arr[i]) {
            stk.pop();
        }
        if (stk.length > 0) {
            right[i] = stk.at(-1);
        }
        stk.push(i);
    }

    const mod: number = 1e9 + 7;
    let ans: number = 0;
    for (let i = 0; i < n; ++i) {
        ans += ((((i - left[i]) * (right[i] - i)) % mod) * arr[i]) % mod;
        ans %= mod;
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::VecDeque;

impl Solution {
    pub fn sum_subarray_mins(arr: Vec<i32>) -> i32 {
        let n = arr.len();
        let mut left = vec![-1; n];
        let mut right = vec![n as i32; n];
        let mut stk: VecDeque<usize> = VecDeque::new();

        for i in 0..n {
            while !stk.is_empty() && arr[*stk.back().unwrap()] >= arr[i] {
                stk.pop_back();
            }
            if let Some(&top) = stk.back() {
                left[i] = top as i32;
            }
            stk.push_back(i);
        }

        stk.clear();
        for i in (0..n).rev() {
            while !stk.is_empty() && arr[*stk.back().unwrap()] > arr[i] {
                stk.pop_back();
            }
            if let Some(&top) = stk.back() {
                right[i] = top as i32;
            }
            stk.push_back(i);
        }

        let MOD = 1_000_000_007;
        let mut ans: i64 = 0;
        for i in 0..n {
            ans += ((((right[i] - (i as i32)) * ((i as i32) - left[i])) as i64) * (arr[i] as i64))
                % MOD;
            ans %= MOD;
        }
        ans as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
