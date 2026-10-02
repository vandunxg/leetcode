---
comments: true
difficulty: Hard
rating: 1872
source: Biweekly Contest 31 Q4
tags:
    - Stack
    - Greedy
    - Array
    - Dynamic Programming
    - Monotonic Stack
---

<!-- problem:start -->

# [1526. Minimum Number of Increments on Subarrays to Form a Target Array](https://leetcode.com/problems/minimum-number-of-increments-on-subarrays-to-form-a-target-array)

[中文文档](/solution/1500-1599/1526.Minimum%20Number%20of%20Increments%20on%20Subarrays%20to%20Form%20a%20Target%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>target</code>. Có một mảng số nguyên <code>initial</code> cùng kích thước với <code>target</code>, trong đó mọi phần tử ban đầu đều bằng 0.</p>

<p>Trong một thao tác, bạn có thể chọn <strong>bất kỳ</strong> mảng con nào của <code>initial</code> và tăng mỗi giá trị lên 1.</p>

<p>Trả về <em>số thao tác ít nhất để tạo mảng </em><code>target</code><em> từ </em><code>initial</code>.</p>

<p>Các test case được tạo sao cho đáp án vừa trong số nguyên 32-bit.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> target = [1,2,3,2,1]
<strong>Output:</strong> 3
<strong>Giải thích:</strong> Cần ít nhất 3 thao tác để tạo mảng target từ mảng initial.
[<strong><u>0,0,0,0,0</u></strong>] increment 1 from index 0 to 4 (inclusive).
[1,<strong><u>1,1,1</u></strong>,1] increment 1 from index 1 to 3 (inclusive).
[1,2,<strong><u>2</u></strong>,2,1] increment 1 at index 2.
[1,2,3,2,1] target array is formed.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> target = [3,1,1,2]
<strong>Output:</strong> 4
<strong>Giải thích:</strong> [<strong><u>0,0,0,0</u></strong>] -&gt; [1,1,1,<strong><u>1</u></strong>] -&gt; [<strong><u>1</u></strong>,1,1,2] -&gt; [<strong><u>2</u></strong>,1,1,2] -&gt; [3,1,1,2]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> target = [3,1,5,4,2]
<strong>Output:</strong> 7
<strong>Giải thích:</strong> [<strong><u>0,0,0,0,0</u></strong>] -&gt; [<strong><u>1</u></strong>,1,1,1,1] -&gt; [<strong><u>2</u></strong>,1,1,1,1] -&gt; [3,1,<strong><u>1,1,1</u></strong>] -&gt; [3,1,<strong><u>2,2</u></strong>,2] -&gt; [3,1,<strong><u>3,3</u></strong>,2] -&gt; [3,1,<strong><u>4</u></strong>,4,2] -&gt; [3,1,5,4,2].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= target.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= target[i] &lt;= 10<sup>5</sup></code></li>
	<li>​​​​​​​Dữ liệu đầu vào được tạo sao cho đáp án vừa trong số nguyên 32 bit.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác tăng một đoạn liên tiếp; ta phải biến các số 0 thành $target$. Cả $n$ và $target[i]$ đều có thể bằng $10^5$, nên không thể xây dựng mảng theo từng lớp.
>
> Một lần tăng phủ đoạn $[i,j]$ chỉ cần bổ sung vào tiền tố $target[0..i]$ khi $target[i]$ lớn hơn phần tử bên trái. Do đó $f[i]=f[i-1]+\max(0,target[i]-target[i-1])$ với $f[0]=target[0]$. Công thức chỉ phụ thuộc giá trị trước đó, nên chỉ cần duyệt các mức tăng giữa hai phần tử kề nhau.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là số thao tác ít nhất cần để thu được $target[0,..i]$, ban đầu đặt $f[0] = target[0]$.

For $target[i]$, if $target[i] \leq target[i-1]$, then $f[i] = f[i-1]$; otherwise, $f[i] = f[i-1] + target[i] - target[i-1]$.

Đáp án cuối cùng là $f[n-1]$.

Ta nhận thấy $f[i]$ chỉ phụ thuộc vào $f[i-1]$, nên có thể duy trì số thao tác chỉ bằng một biến.

Độ phức tạp thời gian là $O(n)$, với $n$ là độ dài mảng $target$. Độ phức tạp không gian là $O(1)$.

Similar problems:

- [3229. Minimum Operations to Make Array Equal to Target](https://github.com/doocs/leetcode/blob/main/solution/3200-3299/3229.Minimum%20Operations%20to%20Make%20Array%20Equal%20to%20Target/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minNumberOperations(self, target: List[int]) -> int:
        return target[0] + sum(max(0, b - a) for a, b in pairwise(target))
```

#### Java

```java
class Solution {
    public int minNumberOperations(int[] target) {
        int f = target[0];
        for (int i = 1; i < target.length; ++i) {
            if (target[i] > target[i - 1]) {
                f += target[i] - target[i - 1];
            }
        }
        return f;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minNumberOperations(vector<int>& target) {
        int f = target[0];
        for (int i = 1; i < target.size(); ++i) {
            if (target[i] > target[i - 1]) {
                f += target[i] - target[i - 1];
            }
        }
        return f;
    }
};
```

#### Go

```go
func minNumberOperations(target []int) int {
	f := target[0]
	for i, x := range target[1:] {
		if x > target[i] {
			f += x - target[i]
		}
	}
	return f
}
```

#### TypeScript

```ts
function minNumberOperations(target: number[]): number {
    let f = target[0];
    for (let i = 1; i < target.length; ++i) {
        if (target[i] > target[i - 1]) {
            f += target[i] - target[i - 1];
        }
    }
    return f;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_number_operations(target: Vec<i32>) -> i32 {
        let mut f = target[0];
        for i in 1..target.len() {
            if target[i] > target[i - 1] {
                f += target[i] - target[i - 1];
            }
        }
        f
    }
}
```

#### C#

```cs
public class Solution {
    public int MinNumberOperations(int[] target) {
        int f = target[0];
        for (int i = 1; i < target.Length; ++i) {
            if (target[i] > target[i - 1]) {
                f += target[i] - target[i - 1];
            }
        }
        return f;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
