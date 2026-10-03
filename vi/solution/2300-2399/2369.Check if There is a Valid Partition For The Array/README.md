---
comments: true
difficulty: Medium
rating: 1779
source: Weekly Contest 305 Q3
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [2369. Check if There is a Valid Partition For The Array](https://leetcode.com/problems/check-if-there-is-a-valid-partition-for-the-array)

[中文文档](/solution/2300-2399/2369.Check%20if%20There%20is%20a%20Valid%20Partition%20For%20The%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong>. Bạn phải phân hoạch mảng thành một hoặc nhiều mảng con <strong>liền kề</strong>.</p>

<p>Một phép phân hoạch được gọi là <strong>hợp lệ</strong> nếu mỗi mảng con thu được thỏa mãn <strong>một</strong> trong các điều kiện sau:</p>

<ol>
	<li>Mảng con gồm <strong>chính xác</strong> <code>2,</code> phần tử bằng nhau. Ví dụ, mảng con <code>[2,2]</code> là hợp lệ.</li>
	<li>Mảng con gồm <strong>chính xác</strong> <code>3,</code> phần tử bằng nhau. Ví dụ, mảng con <code>[4,4,4]</code> là hợp lệ.</li>
	<li>Mảng con gồm <strong>chính xác</strong> <code>3</code> phần tử liên tiếp tăng dần, nghĩa là hiệu giữa hai phần tử liền kề bằng <code>1</code>. Ví dụ, mảng con <code>[3,4,5]</code> là hợp lệ, nhưng mảng con <code>[1,3,5]</code> thì không.</li>
</ol>

<p>Trả về <code>true</code><em> nếu mảng có <strong>ít nhất</strong> một phép phân hoạch hợp lệ</em>. Nếu không, trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,4,4,5,6]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Mảng có thể được phân hoạch thành các mảng con [4,4] và [4,5,6].
Phép phân hoạch này hợp lệ, nên ta trả về true.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,1,2]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không có phép phân hoạch hợp lệ nào cho mảng này.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có memoization

<!-- thinking:start -->

> **Tư duy**
>
> Một phép phân hoạch sử dụng hai phần tử bằng nhau, ba phần tử bằng nhau hoặc ba phần tử liên tiếp tăng dần. Với $n \le 10^5$, đệ quy ngây thơ sẽ có các lời gọi bị trùng lặp. Tính hợp lệ bắt đầu từ chỉ số $i$ chỉ phụ thuộc vào phần hậu tố.
>
> Ta dùng memoization cho $dfs(i)$: thử một mảng con hợp lệ có độ dài $2$ hoặc $3$, rồi nhảy đến cuối mảng con đó. Vượt quá $n$ nghĩa là thành công.

<!-- thinking:end -->

Ta xây dựng hàm $dfs(i)$, biểu thị liệu có thể phân hoạch hợp lệ mảng bắt đầu từ chỉ số $i$ hay không. Do đó, đáp án là $dfs(0)$.

Quy trình thực thi của hàm $dfs(i)$ như sau:

- Nếu $i \ge n$, trả về $true$.
- Nếu các phần tử tại chỉ số $i$ và $i+1$ bằng nhau, ta có thể chọn $i$ và $i+1$ làm một mảng con, rồi gọi đệ quy $dfs(i+2)$.
- Nếu các phần tử tại chỉ số $i$, $i+1$ và $i+2$ bằng nhau, ta có thể chọn $i$, $i+1$ và $i+2$ làm một mảng con, rồi gọi đệ quy $dfs(i+3)$.
- Nếu các phần tử tại chỉ số $i$, $i+1$ và $i+2$ lần lượt tăng thêm $1$, ta có thể chọn $i$, $i+1$ và $i+2$ làm một mảng con, rồi gọi đệ quy $dfs(i+3)$.
- Nếu không thỏa mãn điều kiện nào ở trên, trả về $false$; ngược lại, trả về $true$.

Tức là:

$$
dfs(i) = \textit{OR}
\begin{cases}
true,&i \ge n\\
dfs(i+2),&i+1 < n\ \textit{and}\ \textit{nums}[i] = \textit{nums}[i+1]\\
dfs(i+3),&i+2 < n\ \textit{and}\ \textit{nums}[i] = \textit{nums}[i+1] = \textit{nums}[i+2]\\
dfs(i+3),&i+2 < n\ \textit{and}\ \textit{nums}[i+1] - \textit{nums}[i] = 1\ \textit{and}\ \textit{nums}[i+2] - \textit{nums}[i+1] = 1
\end{cases}
$$

Để tránh tính toán lặp lại, ta sử dụng phương pháp tìm kiếm có memoization.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def validPartition(self, nums: List[int]) -> bool:
        @cache
        def dfs(i: int) -> bool:
            if i >= n:
                return True
            a = i + 1 < n and nums[i] == nums[i + 1]
            b = i + 2 < n and nums[i] == nums[i + 1] == nums[i + 2]
            c = (
                i + 2 < n
                and nums[i + 1] - nums[i] == 1
                and nums[i + 2] - nums[i + 1] == 1
            )
            return (a and dfs(i + 2)) or ((b or c) and dfs(i + 3))

        n = len(nums)
        return dfs(0)
```

#### Java

```java
class Solution {
    private int n;
    private int[] nums;
    private Boolean[] f;

    public boolean validPartition(int[] nums) {
        n = nums.length;
        this.nums = nums;
        f = new Boolean[n];
        return dfs(0);
    }

    private boolean dfs(int i) {
        if (i >= n) {
            return true;
        }
        if (f[i] != null) {
            return f[i];
        }
        boolean a = i + 1 < n && nums[i] == nums[i + 1];
        boolean b = i + 2 < n && nums[i] == nums[i + 1] && nums[i + 1] == nums[i + 2];
        boolean c = i + 2 < n && nums[i + 1] - nums[i] == 1 && nums[i + 2] - nums[i + 1] == 1;
        return f[i] = ((a && dfs(i + 2)) || ((b || c) && dfs(i + 3)));
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool validPartition(vector<int>& nums) {
        n = nums.size();
        this->nums = nums;
        f.assign(n, -1);
        return dfs(0);
    }

private:
    int n;
    vector<int> f;
    vector<int> nums;

    bool dfs(int i) {
        if (i >= n) {
            return true;
        }
        if (f[i] != -1) {
            return f[i] == 1;
        }
        bool a = i + 1 < n && nums[i] == nums[i + 1];
        bool b = i + 2 < n && nums[i] == nums[i + 1] && nums[i + 1] == nums[i + 2];
        bool c = i + 2 < n && nums[i + 1] - nums[i] == 1 && nums[i + 2] - nums[i + 1] == 1;
        f[i] = ((a && dfs(i + 2)) || ((b || c) && dfs(i + 3))) ? 1 : 0;
        return f[i] == 1;
    }
};
```

#### Go

```go
func validPartition(nums []int) bool {
	n := len(nums)
	f := make([]int, n)
	for i := range f {
		f[i] = -1
	}
	var dfs func(int) bool
	dfs = func(i int) bool {
		if i == n {
			return true
		}
		if f[i] != -1 {
			return f[i] == 1
		}
		a := i+1 < n && nums[i] == nums[i+1]
		b := i+2 < n && nums[i] == nums[i+1] && nums[i+1] == nums[i+2]
		c := i+2 < n && nums[i+1]-nums[i] == 1 && nums[i+2]-nums[i+1] == 1
		f[i] = 0
		if a && dfs(i+2) || b && dfs(i+3) || c && dfs(i+3) {
			f[i] = 1
		}
		return f[i] == 1
	}
	return dfs(0)
}
```

#### TypeScript

```ts
function validPartition(nums: number[]): boolean {
    const n = nums.length;
    const f: number[] = Array(n).fill(-1);
    const dfs = (i: number): boolean => {
        if (i >= n) {
            return true;
        }
        if (f[i] !== -1) {
            return f[i] === 1;
        }
        const a = i + 1 < n && nums[i] == nums[i + 1];
        const b = i + 2 < n && nums[i] == nums[i + 1] && nums[i + 1] == nums[i + 2];
        const c = i + 2 < n && nums[i + 1] - nums[i] == 1 && nums[i + 2] - nums[i + 1] == 1;
        f[i] = (a && dfs(i + 2)) || ((b || c) && dfs(i + 3)) ? 1 : 0;
        return f[i] == 1;
    };
    return dfs(0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Memoization vẫn sử dụng đệ quy. Gọi $f[i]$ là việc tiền tố có độ dài $i$ có hợp lệ hay không, rồi chuyển trạng thái từ $f[i-2]$ và $f[i-3]$ với cùng ba loại mảng con.

<!-- thinking:end -->

Ta có thể chuyển phép tìm kiếm có memoization trong Lời giải 1 thành quy hoạch động.

Gọi $f[i]$ là việc có thể phân hoạch hợp lệ $i$ phần tử đầu tiên của mảng hay không. Ban đầu, $f[0] = true$, và đáp án là $f[n]$.

Công thức chuyển trạng thái như sau:

$$
f[i] = \textit{OR}
\begin{cases}
true,&i = 0\\
f[i-2],&i-2 \ge 0\ \textit{and}\ \textit{nums}[i-1] = \textit{nums}[i-2]\\
f[i-3],&i-3 \ge 0\ \textit{and}\ \textit{nums}[i-1] = \textit{nums}[i-2] = \textit{nums}[i-3]\\
f[i-3],&i-3 \ge 0\ \textit{and}\ \textit{nums}[i-1] - \textit{nums}[i-2] = 1\ \textit{and}\ \textit{nums}[i-2] - \textit{nums}[i-3] = 1
\end{cases}
$$

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def validPartition(self, nums: List[int]) -> bool:
        n = len(nums)
        f = [True] + [False] * n
        for i, x in enumerate(nums, 1):
            a = i - 2 >= 0 and nums[i - 2] == x
            b = i - 3 >= 0 and nums[i - 3] == nums[i - 2] == x
            c = i - 3 >= 0 and x - nums[i - 2] == 1 and nums[i - 2] - nums[i - 3] == 1
            f[i] = (a and f[i - 2]) or ((b or c) and f[i - 3])
        return f[n]
```

#### Java

```java
class Solution {
    public boolean validPartition(int[] nums) {
        int n = nums.length;
        boolean[] f = new boolean[n + 1];
        f[0] = true;
        for (int i = 1; i <= n; ++i) {
            boolean a = i - 2 >= 0 && nums[i - 1] == nums[i - 2];
            boolean b = i - 3 >= 0 && nums[i - 1] == nums[i - 2] && nums[i - 2] == nums[i - 3];
            boolean c
                = i - 3 >= 0 && nums[i - 1] - nums[i - 2] == 1 && nums[i - 2] - nums[i - 3] == 1;
            f[i] = (a && f[i - 2]) || ((b || c) && f[i - 3]);
        }
        return f[n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool validPartition(vector<int>& nums) {
        int n = nums.size();
        vector<bool> f(n + 1);
        f[0] = true;
        for (int i = 1; i <= n; ++i) {
            bool a = i - 2 >= 0 && nums[i - 1] == nums[i - 2];
            bool b = i - 3 >= 0 && nums[i - 1] == nums[i - 2] && nums[i - 2] == nums[i - 3];
            bool c = i - 3 >= 0 && nums[i - 1] - nums[i - 2] == 1 && nums[i - 2] - nums[i - 3] == 1;
            f[i] = (a && f[i - 2]) || ((b || c) && f[i - 3]);
        }
        return f[n];
    }
};
```

#### Go

```go
func validPartition(nums []int) bool {
	n := len(nums)
	f := make([]bool, n+1)
	f[0] = true
	for i := 1; i <= n; i++ {
		x := nums[i-1]
		a := i-2 >= 0 && nums[i-2] == x
		b := i-3 >= 0 && nums[i-3] == nums[i-2] && nums[i-2] == x
		c := i-3 >= 0 && x-nums[i-2] == 1 && nums[i-2]-nums[i-3] == 1
		f[i] = (a && f[i-2]) || ((b || c) && f[i-3])
	}
	return f[n]
}
```

#### TypeScript

```ts
function validPartition(nums: number[]): boolean {
    const n = nums.length;
    const f: boolean[] = Array(n + 1).fill(false);
    f[0] = true;
    for (let i = 1; i <= n; ++i) {
        const a = i - 2 >= 0 && nums[i - 1] === nums[i - 2];
        const b = i - 3 >= 0 && nums[i - 1] === nums[i - 2] && nums[i - 2] === nums[i - 3];
        const c = i - 3 >= 0 && nums[i - 1] - nums[i - 2] === 1 && nums[i - 2] - nums[i - 3] === 1;
        f[i] = (a && f[i - 2]) || ((b || c) && f[i - 3]);
    }
    return f[n];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
