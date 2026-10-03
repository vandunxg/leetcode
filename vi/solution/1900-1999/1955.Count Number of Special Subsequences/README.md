---
comments: true
difficulty: Hard
rating: 2125
source: Weekly Contest 252 Q4
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [1955. Count Number of Special Subsequences](https://leetcode.com/problems/count-number-of-special-subsequences)

[中文文档](/solution/1900-1999/1955.Count%20Number%20of%20Special%20Subsequences/README.md)

## Mô tả

<!-- description:start -->

<p>Một dãy được gọi là <strong>đặc biệt</strong> nếu nó gồm một số lượng <strong>dương</strong> các số <code>0</code>, tiếp theo là một số lượng <strong>dương</strong> các số <code>1</code>, rồi đến một số lượng <strong>dương</strong> các số <code>2</code>.</p>

<ul>
	<li>Ví dụ, <code>[0,1,2]</code> và <code>[0,0,1,1,1,2]</code> là các dãy đặc biệt.</li>
	<li>Ngược lại, <code>[2,1,0]</code>, <code>[1]</code> và <code>[0,1,2,0]</code> không phải là các dãy đặc biệt.</li>
</ul>

<p>Cho một mảng <code>nums</code> ( <strong>chỉ</strong> gồm các số nguyên <code>0</code>, <code>1</code> và <code>2</code>), hãy trả về <em><strong>số lượng dãy con khác nhau</strong> là dãy đặc biệt</em>. Vì đáp án có thể rất lớn, hãy <strong>trả về đáp án modulo </strong><code>10<sup>9</sup> + 7</code>.</p>

<p>Một <strong>dãy con</strong> của một mảng là một dãy có thể thu được từ mảng bằng cách xóa một số hoặc không xóa phần tử nào mà không thay đổi thứ tự của các phần tử còn lại. Hai dãy con <strong>khác nhau</strong> nếu <strong>tập chỉ số</strong> được chọn là khác nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,1,2,2]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Các dãy con đặc biệt được in đậm là [<strong><u>0</u></strong>,<strong><u>1</u></strong>,<strong><u>2</u></strong>,2], [<strong><u>0</u></strong>,<strong><u>1</u></strong>,2,<strong><u>2</u></strong>] và [<strong><u>0</u></strong>,<strong><u>1</u></strong>,<strong><u>2</u></strong>,<strong><u>2</u></strong>].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,2,0,0]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có dãy con đặc biệt nào trong [2,2,0,0].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,1,2,0,1,2]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Các dãy con đặc biệt được in đậm:
- [<strong><u>0</u></strong>,<strong><u>1</u></strong>,<strong><u>2</u></strong>,0,1,2]
- [<strong><u>0</u></strong>,<strong><u>1</u></strong>,2,0,1,<strong><u>2</u></strong>]
- [<strong><u>0</u></strong>,<strong><u>1</u></strong>,<strong><u>2</u></strong>,0,1,<strong><u>2</u></strong>]
- [<strong><u>0</u></strong>,<strong><u>1</u></strong>,2,0,<strong><u>1</u></strong>,<strong><u>2</u></strong>]
- [<strong><u>0</u></strong>,1,2,<strong><u>0</u></strong>,<strong><u>1</u></strong>,<strong><u>2</u></strong>]
- [<strong><u>0</u></strong>,1,2,0,<strong><u>1</u></strong>,<strong><u>2</u></strong>]
- [0,1,2,<strong><u>0</u></strong>,<strong><u>1</u></strong>,<strong><u>2</u></strong>]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 2</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Một dãy con đặc biệt gồm các số $0$, tiếp theo là các số $1$, rồi đến các số $2$. Có số lượng dãy con tăng theo cấp số mũ, nên chúng ta đếm theo giá trị kết thúc.
>
> $f[i][j]$ là số dãy con trong $i$ phần tử đầu tiên kết thúc bằng $j$. Một số $0$ làm tăng gấp đôi các dãy kết thúc bằng $0$ trước đó và tạo thêm một dãy chỉ gồm một phần tử; số $1$ và $2$ được nối vào giai đoạn trước đó hoặc cùng giai đoạn.
>
> Đáp án là $f[n-1][2]$ modulo $10^9+7$.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là số dãy con đặc biệt kết thúc bằng $j$ trong $i+1$ phần tử đầu tiên. Ban đầu, $f[i][j]=0$; nếu $nums[0]=0$ thì $f[0][0]=1$.

Với $i \gt 0$, ta xét giá trị của $nums[i]$:

Nếu $nums[i] = 0$: Nếu không chọn $nums[i]$ thì $f[i][0] = f[i-1][0]$; nếu chọn $nums[i]$ thì $f[i][0]=f[i-1][0]+1$, vì ta có thể thêm một số $0$ vào cuối bất kỳ dãy con đặc biệt nào kết thúc bằng $0$ để tạo thành một dãy con đặc biệt mới, hoặc dùng riêng $nums[i]$ làm một dãy con đặc biệt. Do đó, $f[i][0] = 2 \times f[i - 1][0] + 1$. Các $f[i][j]$ còn lại bằng $f[i-1][j]$.

Nếu $nums[i] = 1$: Nếu không chọn $nums[i]$ thì $f[i][1] = f[i-1][1]$; nếu chọn $nums[i]$ thì $f[i][1]=f[i-1][1]+f[i-1][0]$, vì ta có thể thêm một số $1$ vào cuối bất kỳ dãy con đặc biệt nào kết thúc bằng $0$ hoặc $1$ để tạo thành một dãy con đặc biệt mới. Do đó, $f[i][1] = f[i-1][0] + 2 \times f[i - 1][1]$. Các $f[i][j]$ còn lại bằng $f[i-1][j]$.

Nếu $nums[i] = 2$: Nếu không chọn $nums[i]$ thì $f[i][2] = f[i-1][2]$; nếu chọn $nums[i]$ thì $f[i][2]=f[i-1][2]+f[i-1][1]$, vì ta có thể thêm một số $2$ vào cuối bất kỳ dãy con đặc biệt nào kết thúc bằng $1$ hoặc $2$ để tạo thành một dãy con đặc biệt mới. Do đó, $f[i][2] = f[i-1][1] + 2 \times f[i - 1][2]$. Các $f[i][j]$ còn lại bằng $f[i-1][j]$.

Tóm lại, ta có các phương trình chuyển trạng thái sau:

$$
\begin{aligned}
f[i][0] &= 2 \times f[i - 1][0] + 1, \quad nums[i] = 0 \\
f[i][1] &= f[i-1][0] + 2 \times f[i - 1][1], \quad nums[i] = 1 \\
f[i][2] &= f[i-1][1] + 2 \times f[i - 1][2], \quad nums[i] = 2 \\
f[i][j] &= f[i-1][j], \quad nums[i] \neq j
\end{aligned}
$$

Đáp án cuối cùng là $f[n-1][2]$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$.

Đã tìm thấy mã tương tự với 1 loại giấy phép

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSpecialSubsequences(self, nums: List[int]) -> int:
        mod = 10**9 + 7
        n = len(nums)
        f = [[0] * 3 for _ in range(n)]
        f[0][0] = nums[0] == 0
        for i in range(1, n):
            if nums[i] == 0:
                f[i][0] = (2 * f[i - 1][0] + 1) % mod
                f[i][1] = f[i - 1][1]
                f[i][2] = f[i - 1][2]
            elif nums[i] == 1:
                f[i][0] = f[i - 1][0]
                f[i][1] = (f[i - 1][0] + 2 * f[i - 1][1]) % mod
                f[i][2] = f[i - 1][2]
            else:
                f[i][0] = f[i - 1][0]
                f[i][1] = f[i - 1][1]
                f[i][2] = (f[i - 1][1] + 2 * f[i - 1][2]) % mod
        return f[n - 1][2]
```

#### Java

```java
class Solution {
    public int countSpecialSubsequences(int[] nums) {
        final int mod = (int) 1e9 + 7;
        int n = nums.length;
        int[][] f = new int[n][3];
        f[0][0] = nums[0] == 0 ? 1 : 0;
        for (int i = 1; i < n; ++i) {
            if (nums[i] == 0) {
                f[i][0] = (2 * f[i - 1][0] % mod + 1) % mod;
                f[i][1] = f[i - 1][1];
                f[i][2] = f[i - 1][2];
            } else if (nums[i] == 1) {
                f[i][0] = f[i - 1][0];
                f[i][1] = (f[i - 1][0] + 2 * f[i - 1][1] % mod) % mod;
                f[i][2] = f[i - 1][2];
            } else {
                f[i][0] = f[i - 1][0];
                f[i][1] = f[i - 1][1];
                f[i][2] = (f[i - 1][1] + 2 * f[i - 1][2] % mod) % mod;
            }
        }
        return f[n - 1][2];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countSpecialSubsequences(vector<int>& nums) {
        const int mod = 1e9 + 7;
        int n = nums.size();
        int f[n][3];
        memset(f, 0, sizeof(f));
        f[0][0] = nums[0] == 0;
        for (int i = 1; i < n; ++i) {
            if (nums[i] == 0) {
                f[i][0] = (2 * f[i - 1][0] % mod + 1) % mod;
                f[i][1] = f[i - 1][1];
                f[i][2] = f[i - 1][2];
            } else if (nums[i] == 1) {
                f[i][0] = f[i - 1][0];
                f[i][1] = (f[i - 1][0] + 2 * f[i - 1][1] % mod) % mod;
                f[i][2] = f[i - 1][2];
            } else {
                f[i][0] = f[i - 1][0];
                f[i][1] = f[i - 1][1];
                f[i][2] = (f[i - 1][1] + 2 * f[i - 1][2] % mod) % mod;
            }
        }
        return f[n - 1][2];
    }
};
```

#### Go

```go
func countSpecialSubsequences(nums []int) int {
	const mod = 1e9 + 7
	n := len(nums)
	f := make([][3]int, n)
	if nums[0] == 0 {
		f[0][0] = 1
	}
	for i := 1; i < n; i++ {
		if nums[i] == 0 {
			f[i][0] = (2*f[i-1][0] + 1) % mod
			f[i][1] = f[i-1][1]
			f[i][2] = f[i-1][2]
		} else if nums[i] == 1 {
			f[i][0] = f[i-1][0]
			f[i][1] = (f[i-1][0] + 2*f[i-1][1]) % mod
			f[i][2] = f[i-1][2]
		} else {
			f[i][0] = f[i-1][0]
			f[i][1] = f[i-1][1]
			f[i][2] = (f[i-1][1] + 2*f[i-1][2]) % mod
		}
	}
	return f[n-1][2]
}
```

#### TypeScript

```ts
function countSpecialSubsequences(nums: number[]): number {
    const mod = 1e9 + 7;
    const n = nums.length;
    const f: number[][] = Array(n)
        .fill(0)
        .map(() => Array(3).fill(0));
    f[0][0] = nums[0] === 0 ? 1 : 0;
    for (let i = 1; i < n; ++i) {
        if (nums[i] === 0) {
            f[i][0] = (((2 * f[i - 1][0]) % mod) + 1) % mod;
            f[i][1] = f[i - 1][1];
            f[i][2] = f[i - 1][2];
        } else if (nums[i] === 1) {
            f[i][0] = f[i - 1][0];
            f[i][1] = (f[i - 1][0] + ((2 * f[i - 1][1]) % mod)) % mod;
            f[i][2] = f[i - 1][2];
        } else {
            f[i][0] = f[i - 1][0];
            f[i][1] = f[i - 1][1];
            f[i][2] = (f[i - 1][1] + ((2 * f[i - 1][2]) % mod)) % mod;
        }
    }
    return f[n - 1][2];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động (Tối ưu không gian)

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ cần ba bộ đếm của trạng thái trước đó, nên dùng một mảng có độ dài $3$ và cập nhật tại chỗ sẽ giảm không gian phụ xuống hằng số.

<!-- thinking:end -->

Ta nhận thấy trong các phương trình chuyển trạng thái trên, giá trị của $f[i][j]$ chỉ liên quan đến $f[i-1][j]$. Vì vậy, ta có thể loại bỏ chiều thứ nhất và tối ưu độ phức tạp không gian xuống $O(1)$.

Ta có thể dùng một mảng $f$ có độ dài 3 để lần lượt biểu diễn số dãy con đặc biệt kết thúc bằng 0, 1 và 2. Với mỗi phần tử trong mảng, ta cập nhật mảng $f$ dựa trên giá trị của phần tử hiện tại.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSpecialSubsequences(self, nums: List[int]) -> int:
        mod = 10**9 + 7
        n = len(nums)
        f = [0] * 3
        f[0] = nums[0] == 0
        for i in range(1, n):
            if nums[i] == 0:
                f[0] = (2 * f[0] + 1) % mod
            elif nums[i] == 1:
                f[1] = (f[0] + 2 * f[1]) % mod
            else:
                f[2] = (f[1] + 2 * f[2]) % mod
        return f[2]
```

#### Java

```java
class Solution {
    public int countSpecialSubsequences(int[] nums) {
        final int mod = (int) 1e9 + 7;
        int n = nums.length;
        int[] f = new int[3];
        f[0] = nums[0] == 0 ? 1 : 0;
        for (int i = 1; i < n; ++i) {
            if (nums[i] == 0) {
                f[0] = (2 * f[0] % mod + 1) % mod;
            } else if (nums[i] == 1) {
                f[1] = (f[0] + 2 * f[1] % mod) % mod;
            } else {
                f[2] = (f[1] + 2 * f[2] % mod) % mod;
            }
        }
        return f[2];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countSpecialSubsequences(vector<int>& nums) {
        const int mod = 1e9 + 7;
        int n = nums.size();
        int f[3]{0};
        f[0] = nums[0] == 0;
        for (int i = 1; i < n; ++i) {
            if (nums[i] == 0) {
                f[0] = (2 * f[0] % mod + 1) % mod;
            } else if (nums[i] == 1) {
                f[1] = (f[0] + 2 * f[1] % mod) % mod;
            } else {
                f[2] = (f[1] + 2 * f[2] % mod) % mod;
            }
        }
        return f[2];
    }
};
```

#### Go

```go
func countSpecialSubsequences(nums []int) int {
	const mod = 1e9 + 7
	n := len(nums)
	f := [3]int{}
	if nums[0] == 0 {
		f[0] = 1
	}
	for i := 1; i < n; i++ {
		if nums[i] == 0 {
			f[0] = (2*f[0] + 1) % mod
		} else if nums[i] == 1 {
			f[1] = (f[0] + 2*f[1]) % mod
		} else {
			f[2] = (f[1] + 2*f[2]) % mod
		}
	}
	return f[2]
}
```

#### TypeScript

```ts
function countSpecialSubsequences(nums: number[]): number {
    const mod = 1e9 + 7;
    const n = nums.length;
    const f: number[] = [0, 0, 0];
    f[0] = nums[0] === 0 ? 1 : 0;
    for (let i = 1; i < n; ++i) {
        if (nums[i] === 0) {
            f[0] = (((2 * f[0]) % mod) + 1) % mod;
            f[1] = f[1];
            f[2] = f[2];
        } else if (nums[i] === 1) {
            f[0] = f[0];
            f[1] = (f[0] + ((2 * f[1]) % mod)) % mod;
            f[2] = f[2];
        } else {
            f[0] = f[0];
            f[1] = f[1];
            f[2] = (f[1] + ((2 * f[2]) % mod)) % mod;
        }
    }
    return f[2];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
